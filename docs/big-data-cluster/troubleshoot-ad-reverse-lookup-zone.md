---
title: 'Implementación del modo de AD detenida: falta la entrada de la zona de búsqueda inversa del controlador de dominio'
titleSuffix: SQL Server Big Data Cluster
description: Se bloqueó la implementación del BDC con el modo de AD debido a que falta la entrada de la zona de búsqueda inversa para el controlador de dominio en el servidor DNS del controlador de dominio.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
ms.date: 04/21/2020
ms.topic: how-to
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 02276cee62edd604df5e018bc22cf86cddaaec03
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043955"
---
# <a name="ad-mode-deployment-stopped---missing-reverse-lookup-zone-entry-for-dc"></a>Implementación del modo de AD detenida: falta la entrada de la zona de búsqueda inversa del controlador de dominio

La implementación en el modo de Active Directory (AD) se inmoviliza. Compruebe los síntomas para saber si la causa es que falta la entrada de la zona de búsqueda inversa en el servidor DNS del controlador de dominio. 

## <a name="symptom"></a>Síntoma

Ha empezado a implementar BDC con el modo de AD, pero la implementación está bloqueada y no avanza.

En el ejemplo siguiente se muestran los resultados de la implementación en un shell de Bash.

```
The privacy statement can be viewed at:
https://go.microsoft.com/fwlink/?LinkId=853010
 
The license terms for SQL Server Big Data Cluster can be viewed at:
Enterprise: https://go.microsoft.com/fwlink/?linkid=2104292
Standard: https://go.microsoft.com/fwlink/?linkid=2104294
Developer: https://go.microsoft.com/fwlink/?linkid=2104079
 
Cluster deployment documentation can be viewed at:
https://aka.ms/bdc-deploy
 
NOTE: Cluster creation can take a significant amount of time depending on
configuration, network speed, and the number of nodes in the cluster.
 
Starting cluster deployment.
Cluster controller endpoint is available at bdc-control.contoso.com:30080, 193.168.5.14:30080.
Waiting for control plane to be ready after 5 minutes.
Waiting for control plane to be ready after 10 minutes.
Waiting for control plane to be ready after 15 minutes.
Waiting for control plane to be ready after 20 minutes.
Waiting for control plane to be ready after 25 minutes.
```

Compruebe los pods implementados actualmente.

```bash
kubectl get pods -n mssql-cluster
```

Los resultados siguientes indican que solo se han implementado los pods que pertenecen al controlador. No se crean los pods de proceso, datos o almacenamiento.

```
NAME              READY   STATUS    RESTARTS   AGE
control-rts5t     3/3     Running   0          18m
controldb-0       2/2     Running   0          18m
controlwd-csgst   1/1     Running   0          16m
dns-7kfnz         2/2     Running   0          16m
logsdb-0          1/1     Running   0          16m
logsui-2pc29      1/1     Running   0          16m
metricsdb-0       1/1     Running   0          16m
metricsdc-4rtm4   1/1     Running   0          16m
metricsdc-6lr2t   1/1     Running   0          16m
metricsdc-ftx9m   1/1     Running   0          16m
metricsdc-h59jb   1/1     Running   0          16m
metricsui-lvdpt   1/1     Running   0          16m
mgmtproxy-mkmxp   2/2     Running   0          16m
```

Inspeccione los registros de contenedor de compatibilidad de seguridad. Busque errores de LDAP. 

## <a name="check-security-support-container"></a>Comprobación del contenedor de compatibilidad de seguridad 

Revise los registros de contenedor de compatibilidad de seguridad.

El comando siguiente recopila los registros de compatibilidad de seguridad en un clúster en el espacio de nombres `mssql-cluster`.

```bash
azdata bdc debug copy-logs -n mssql-cluster -c security-support
```

Extraiga los registros y busque `\mssql-cluster\control-<identifier>\controller\control-rts5t-controller-stdout.log`.

> [!TIP]
> Hay varias formas de recopilar los registros. En lugar de copiar los registros con [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)], puede usar un cuaderno en Azure Data Studio.
> En Azure Data Studio, conéctese al clúster de Kubernetes y ejecute un cuaderno de solución de problemas adecuado. A continuación, se muestran ejemplos de cuadernos.
>
> - TSG027: observación de la implementación del clúster
> - TSG061: obtención del final de todos los registros de contenedor de los pods del espacio de nombres del BDC
> - TSG001: Ejecución de `azdata copy-logs`
>

## <a name="inspect-the-logs"></a>Inspección de los registros

Busque el registro. En el ejemplo siguiente se apunta a un registro de implementación de controlador. 

`<folderOfDebugCopyLog>\debuglogs-mssql-cluster-YYYYMMDD-HHMMSS\<namespace>\control-<identifier>\controller\control-<identifier>-controller-stdout.log`"

```
YYYY-MM-DD HH:MM:SS.ms | ERROR | Failed to create AD user account 'cntrl-controller'. Error code: 53. Message: Failed to create user object: Failed to add object 'CN=cntrl-controller,OU=bdc, DC=CONTOSO, DC=com' to 'CONTOSO.COM': Server is unwilling to perform. 
YYYY-MM-DD HH:MM:SS.ms | ERROR | Failed to create AD user account 'ldap-user'. Error code: 53. Message: Failed to create user object: Failed to add object 'CN=ldap-user,OU=bdc, DC=CONTOSO, DC=com' to 'CONTOSO.COM': Server is unwilling to perform. 
YYYY-MM-DD HH:MM:SS.ms | ERROR | Failed to create AD user account 'nginx-mgmtproxy'. Error code: 53. Message: Failed to create user object: Failed to add object 'CN=nginx-mgmtproxy,OU=bdc, DC=CONTOSO, DC=com' to 'CONTOSO.COM': Server is unwilling to perform. 
```

## <a name="cause"></a>Causa

Falta la entrada de la zona de búsqueda inversa para el controlador de dominio en la entrada DNS del controlador de dominio. 

## <a name="resolution"></a>Solución

Ejecute el script de PowerShell siguiente para confirmar si tiene configurada una entrada de DNS inverso (registro PTR).

```powershell
#Domain Controller FQDN 'DCserver01.contoso.local'
$Domain_controller_FQDN = 'DCserver01.contoso.local'

#Performing Domain Controller DNS record, reverse PTR Checks...
$DcControllerDnsPtr_Result = New-Object System.Collections.ArrayList
try {
    $Domain_controller_DNS_Record = Resolve-DnsName $Domain_controller_FQDN -Type A -Server $Domain_DNS_IP_address -ErrorAction Stop
    foreach ($ip in $Domain_controller_DNS_Record.IPAddress) {
        #resolving hostname by IP address to make sure we have reverse PTR record 
        if ((Resolve-DnsName $ip).NameHost -eq $Domain_controller_FQDN) {
            [void]$DcControllerDnsPtr_Result.add("OK - $Domain_controller_FQDN has an A record with an IP $ip, Reverse PTR record is in place") 
        }
        else {
            [void]$DcControllerDnsPtr_Result.add("Missing - $Domain_controller_FQDN has an A record with an IP $ip, But no reverse PTR record was found for the host")
        }
    }
}
catch {
    [void]$DcControllerDnsPtr_Result.add("Error - " + $_.exception.message)
}

#show the results 
$DcControllerDnsPtr_Result
```

## <a name="next-steps"></a>Pasos siguientes

[Comprobación de la entrada de DNS inverso (registro PTR) para el controlador de dominio](active-directory-deploy.md#verify-reverse-dns-entry-for-domain-controller).
