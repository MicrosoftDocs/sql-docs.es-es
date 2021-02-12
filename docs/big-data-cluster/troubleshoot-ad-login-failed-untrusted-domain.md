---
title: 'Error de inicio de sesión en el modo de AD: dominio que no es de confianza'
titleSuffix: SQL Server Big Data Cluster
description: 'Corregir comportamiento: los clientes no se pueden autenticar cuando las entradas DNS de los puntos de conexión están configuradas con un valor de CNAME que apunta a un nombre de alias.'
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
ms.date: 05/01/2020
ms.topic: how-to
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 41145555b16dc437a4cb8f47ec490077f8aa8db3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045555"
---
# <a name="symptom-ad-mode-login-fails---untrusted-domain-big-data-clusters"></a>Síntoma: Error de inicio de sesión en el modo de AD: dominio que no es de confianza (clústeres de macrodatos)

En un clúster de macrodatos (BDC) de SQL Server en el modo de Active Directory, se puede producir un error al tratar de iniciar conexión, y se devuelve el siguiente error:

`Login failed. The login is from an untrusted domain and cannot be used with Integrated authentication.`

Esto puede ocurrir cuando las entradas DNS se han configurado con un valor de CNAME que apunta a un nombre de alias del proxy inverso que distribuye el tráfico a los nodos de Kubernetes.

## <a name="root-cause"></a>Causa principal

Cuando los puntos de conexión se configuran con entradas DNS con un valor de CNAME que apunta a un nombre de alias del proxy inverso que distribuye el tráfico a los nodos de Kubernetes, sucede lo siguiente:

- El proceso de autenticación de Kerberos busca un nombre de entidad de seguridad de servicio (SPN) que coincida con la entrada de CNAME, y no el SPN real registrado por BDC en Active Directory.
- Se produce un error en la autenticación. 

## <a name="confirm-root-cause"></a>Confirmar la causa raíz

Cuando se produzca un error en la autenticación, compruebe la memoria caché de vales de Kerberos. 

Para comprobar la memoria caché de vales, use el comando `klist`. 

Busque un vale con un SPN que coincida con el punto de conexión al que intentó conectarse.

El vale previsto no está ahí.

En este ejemplo, el registro DNS de un punto de conexión principal, `bdc-sql`, es un CNAME establecido en el proxy inverso denominado `ServerReverseProxy`.

```PowerShell
Resolve-DnsName bdc-sql
```

En la siguiente sección se muestran los resultados del comando anterior.

```
Name                           Type   TTL   Section    NameHost
----                           ----   ---   -------    --------
bdc-sql.mydomain.com           CNAME  3600  Answer     ReverseProxyServer.mydomain.com

Name       : ReverseProxyServer.mydomain.com
QueryType  : A
TTL        : 3600
Section    : Answer
IP4Address : 193.168.5.10
```

>[!NOTE]
>En la siguiente sección se hace referencia a [`tshark`](https://www.wireshark.org/docs/man-pages/tshark.html). `tshark` es una utilidad de la línea de comandos que se instala como parte de la utilidad de seguimiento de red [Wireshark](https://www.wireshark.org/docs/).

Para ver el SPN solicitado desde Active Directory, use `tshark`. El siguiente comando limita la captura de seguimiento de red a la comunicación del protocolo Kerberos, y muestra solo los mensajes `krb-error (30)`. Estos mensajes deben contener mensajes de solicitudes de SPN erróneas.

```bash
tshark -Y "kerberos && kerberos.msg_type == 30" -T fields -e kerberos.error_code -e kerberos.SNameString
```

Desde un shell de comandos diferente, intente conectarse al pod maestro:

```bash
klist purge

sqlcmd -S bdc-sql.mydomain.com,31433 -E
```

Vea la siguiente salida de ejemplo.

```bash
klist purge

Current LogonId is 0:0xf6b58
        Deleting all tickets:
        Ticket(s) purged!

sqlcmd -S bdc-sql.mydomain.com,31433 -E
sqlcmd: Error: Microsoft ODBC Driver 17 for SQL Server : Login failed. The login is from an untrusted domain and cannot be used with Integrated authentication.
```

Compruebe la salida `tshark`. 

```bash
Capturing on 'Ethernet 3'
25      krbtgt,RLAZURE.COM
7       MSSQLSvc,ReverseProxyServer.mydomain.com:31433
2 packets captured
```

Fíjese en que el cliente solicita `SPN MSSQLSvc,ReverseProxyServer.mydomain.com:31433`, que no existe. El intento de conexión acaba generando un error 7. El error 7 significa `KRB5KDC_ERR_S_PRINCIPAL_UNKNOWN Server not found in Kerberos database`.

En la configuración correcta, el cliente solicita el SPN registrado por BDC. En el ejemplo, el SPN correcto habría sido `MSSQLSvc,bdc-sql.mydomain.com:31433`.

>[!NOTE]
>El error 25 significa `KDC_ERR_PREAUTH_REQUIRED`, es decir, se requiere autenticación previa adicional. Se puede ignorar con seguridad. `KDC_ERR_PREAUTH_REQUIRED` se devuelve en la solicitud de AD de Kerberos inicial. El cliente de Kerberos de Windows no incluye de forma predeterminada información de autenticación previa en esta primera solicitud.

Para ver la lista de SPN registrados por BDC en relación con el punto de conexión principal, ejecute `setspn -L mssql-master`. 

Vea la siguiente salida de ejemplo:

```bash
Registered ServicePrincipalNames for CN=mssql-master,OU=bdc,DC=mydomain,DC=com:
        MSSQLSvc/bdc-sqlread.mydomain.com:31436
        MSSQLSvc/-sqlread:31436
        MSSQLSvc/bdc-sqlread.mydomain.com
        MSSQLSvc/bdc-sqlread
        MSSQLSvc/bdc-sql.mydomain.com:31433
        MSSQLSvc/bdc-sql:31433
        MSSQLSvc/bdc-sql.mydomain.com
        MSSQLSvc/bdc-sql
        MSSQLSvc/master-p-svc.mydomain.com:1533
        MSSQLSvc/master-p-svc:1533
        MSSQLSvc/master-p-svc.mydomain.com:1433
        MSSQLSvc/master-p-svc:1433
        MSSQLSvc/master-p-svc.mydomain.com
        MSSQLSvc/master-p-svc
        MSSQLSvc/master-svc.mydomain.com:1533
        MSSQLSvc/master-svc:1533
        MSSQLSvc/master-svc.mydomain.com:1433
        MSSQLSvc/master-svc:1433
        MSSQLSvc/master-svc.mydomain.com
        MSSQLSvc/master-svc
```

En los resultados anteriores, la dirección del proxy inverso no debe estar registrada.

## <a name="resolve"></a>Resolver

En esta sección se muestran dos maneras de resolver el problema. Después de realizar los cambios adecuados, ejecute `ipconfig -flushdns` y `klist purge` en el cliente e intente conectarse de nuevo.

### <a name="option-1"></a>Opción 1

Quite el registro CNAME de cada punto de conexión de BDC en DNS y reemplácelo por varios registros `A` que apunten a cada nodo de Kubernetes o a cada maestro de Kubernetes, si hay más de uno.

>[!TIP]
>El script descrito abajo usa PowerShell. Para obtener más información, vea [Instalación de PowerShell en Linux](/powershell/scripting/install/installing-powershell-core-on-linux).

Puede usar el siguiente script de PowerShell para actualizar los registros de los puntos de conexión DNS. Ejecute el script desde cualquier equipo conectado al mismo dominio:

```powershell
#Specify the DNS server, example contoso.local
$Domain_DNS_name=mydomain.com'

#DNS records for bdc endpoints
$Controller_DNS_name = 'bdc-control'
$Managment_proxy_DNS_name= 'bdc-proxy'
$Master_Primary_DNS_name = 'bdc-sql'
$Master_Secondary_DNS_name = 'bdc-sqlread'
$Gateway_DNS_name = 'bdc-gateway'
$AppProxy_DNS_name = 'bdc-appproxy'

#Performing Endpoint DNS records Checks..

#Build array of endpoints 
$BdcEndpointsDns = New-Object System.Collections.ArrayList

[void]$BdcEndpointsDns.Add($Controller_DNS_name)
[void]$BdcEndpointsDns.Add($Managment_proxy_DNS_name)
[void]$BdcEndpointsDns.Add($Master_Primary_DNS_name)
[void]$BdcEndpointsDns.Add($Master_Secondary_DNS_name)
[void]$BdcEndpointsDns.Add($Gateway_DNS_name)
[void]$BdcEndpointsDns.Add($AppProxy_DNS_name)

#Build arrary for results 
$BdcEndpointsDns_Result = New-Object System.Collections.ArrayList

foreach ($DnsName in $BdcEndpointsDns) {
    try {
        $endpoint_DNS_record = Resolve-DnsName $DnsName -Type A -Server $Domain_DNS_IP_address -ErrorAction Stop 
        foreach ($ip in $endpoint_DNS_record.IPAddress) {
            [void]$BdcEndpointsDns_Result.Add("OK - $DnsName is an A record with an IP $ip")
        }
    }
    catch {
        [void]$BdcEndpointsDns_Result.Add("MisConfiguration - $DnsName is not an A record or does not exists")
    }  
}

#show the results 
$BdcEndpointsDns_Result
```

### <a name="option-2"></a>Opción 2

Como alternativa, el problema se puede solucionar modificando el CNAME para que apunte a la dirección IP del proxy inverso, en lugar de al nombre del proxy inverso.

## <a name="confirm-resolution"></a>Confirmar la resolución

Después de resolver el problema con una de las opciones anteriores, confirme que todo funciona estableciendo una conexión al clúster de macrodatos con Active Directory.

## <a name="next-steps"></a>Pasos siguientes

[Comprobación de la entrada de DNS inverso (registro PTR) para el controlador de dominio](active-directory-deploy.md#verify-reverse-dns-entry-for-domain-controller).
