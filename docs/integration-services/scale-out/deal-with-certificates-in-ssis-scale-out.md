---
title: Usar certificados en la escalabilidad horizontal de SQL Server Integration Services | Microsoft Docs
description: En este artículo se describe cómo administrar certificados para proteger las comunicaciones entre el Patrón de escalado horizontal y el Trabajador de escalado horizontal de SSIS.
ms.date: 12/19/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.custom: performance
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: 0577e6f0e0d1bd78b71b2945977c313ff6a36afc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347437"
---
# <a name="manage-certificates-for-sql-server-integration-services-scale-out"></a>Usar certificados en la escalabilidad horizontal de SQL Server Integration Services

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Para proteger la comunicación entre el patrón de escalabilidad horizontal y los trabajadores de escalabilidad horizontal, la escalabilidad horizontal de SSIS usa dos certificados: uno para el patrón y otro para los trabajadores. 

## <a name="scale-out-master-certificate"></a>Certificado del patrón de escalabilidad horizontal

En la mayoría de los casos, el certificado del patrón de escalabilidad horizontal se configura durante la instalación de este patrón.

En la página **Configuración de escalabilidad horizontal de Integration Services: nodo principal** del asistente de instalación de SQL Server, puede elegir crear un nuevo certificado TLS/SSL autofirmado o usar un certificado TLS/SSL ya existente.

![Configuración principal](media/master-config.PNG)

**Nuevo certificado** Si no tiene requisitos especiales para los certificados, puede crear un nuevo certificado TLS/SSL autofirmado. Además, puede especificar los nombres comunes en el certificado. Asegúrese de que el nombre de host del punto de conexión principal que los trabajadores de escalabilidad horizontal van a usar se incluya en el apartado de nombres comunes. De forma predeterminada, se incluyen el nombre del equipo y la dirección IP del nodo principal. 

**Certificado existente** Si decide usar un certificado existente, haga clic en **Examinar** para seleccionar un certificado TLS/SSL desde el almacén de certificados **raíz** del equipo local.

### <a name="change-the-scale-out-master-certificate"></a>Cambiar el certificado del patrón de escalabilidad horizontal

Puede que quiera cambiar el certificado del patrón de escalabilidad horizontal porque está a punto de expirar o por otras razones. Para cambiar el certificado del patrón de escalabilidad horizontal, siga estos pasos:

#### <a name="1-create-a-tlsssl-certificate"></a>1. Cree un certificado TLS/SSL.
Cree e instale un nuevo certificado TLS/SSL en el nodo principal mediante el siguiente comando:

```dos
MakeCert.exe -n CN={master endpoint host} SSISScaleOutMaster.cer -r -ss Root -sr LocalMachine -a sha1
```
Por ejemplo:

```dos
MakeCert.exe -n CN=MasterMachine SSISScaleOutMaster.cer -r -ss Root -sr LocalMachine -a sha1
```

#### <a name="2-bind-the-certificate-to-the-master-port"></a>2. Enlazar el certificado al puerto principal
Compruebe el enlace original mediante el siguiente comando:

```dos
netsh http show sslcert ipport=0.0.0.0:{Master port}
```

Por ejemplo:

```dos
netsh http show sslcert ipport=0.0.0.0:8391
```

Elimine el enlace original y configure el nuevo enlace mediante los siguientes comandos:

```dos
netsh http delete sslcert ipport=0.0.0.0:{Master port}
netsh http add sslcert ipport=0.0.0.0:{Master port} certhash={TLS/SSL Certificate Thumbprint} certstorename=Root appid={original appid}
```

Por ejemplo:

```dos
netsh http delete sslcert ipport=0.0.0.0:8391
netsh http add sslcert ipport=0.0.0.0:8391 certhash=01d207b300ca662f479beb884efe6ce328f77d53 certstorename=Root appid={a1f96506-93e0-4c91-9171-05a2f6739e13}
```

#### <a name="3-update-the-scale-out-master-service-configuration-file"></a>3. Actualizar el archivo de configuración del servicio de patrón de escalabilidad horizontal
Actualice el archivo de configuración del servicio de patrón de escalabilidad horizontal, `\<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn\MasterSettings.config`, en el nodo del patrón. Actualice **SSLCertThumbprint** a la huella digital del certificado TLS/SSL nuevo.

#### <a name="4-restart-the-scale-out-master-service"></a>4. Reiniciar el servicio del patrón de escalabilidad horizontal

#### <a name="5-reconnect-scale-out-workers-to-scale-out-master"></a>5. Reconectar los trabajadores de escalabilidad horizontal con el patrón de escalabilidad horizontal
Para cada trabajador de escalabilidad horizontal, elimine el trabajador y vuelva a agregarlo con [Scale Out Manager](integration-services-ssis-scale-out-manager.md) o siga estos pasos:

a.  Instale el certificado TLS/SSL de cliente en el almacén raíz del equipo local en el nodo del trabajador.

b.  Actualice el archivo de configuración de mantenimiento del trabajador de escalabilidad horizontal.

Actualice el archivo de configuración del servicio del trabajador de escalabilidad horizontal, `\<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn\WorkerSettings.config`, en el nodo del trabajador. Actualice **MasterHttpsCertThumbprint** a la huella digital del certificado TLS/SSL nuevo.

c.  Reinicie el servicio del trabajador de escalabilidad horizontal.

## <a name="scale-out-worker-certificate"></a>Certificado de trabajo de escalabilidad horizontal

El certificado de trabajo de escalabilidad horizontal se crea automáticamente durante la instalación del trabajo de escalabilidad horizontal en cuestión. 

### <a name="change-the-scale-out-worker-certificate"></a>Cambiar el certificado del trabajador de escalabilidad horizontal

Si quiere cambiar el certificado del trabajador de escalabilidad horizontal, siga estos pasos:

#### <a name="1-create-a-certificate"></a>1. Crear un certificado
Cree e instale un certificado mediante el siguiente comando:

```dos
MakeCert.exe -n CN={worker machine name};CN={worker machine ip} SSISScaleOutWorker.cer -r -ss My -sr LocalMachine
```

Por ejemplo:

```dos
MakeCert.exe -n CN=WorkerMachine;CN=10.0.2.8 SSISScaleOutWorker.cer -r -ss My -sr LocalMachine
```

#### <a name="2-install-the-client-certificate-to-the-root-store-of-the-local-computer-on-the-worker-node"></a>2. Instalar el certificado de cliente en el almacén raíz del equipo local en el nodo del trabajador

#### <a name="3-grant-service-access-to-the-certificate"></a>3. Conceder acceso de servicio al certificado
Elimine el certificado antiguo y conceda acceso de servicio del trabajador de escalabilidad horizontal al certificado nuevo con el siguiente comando:

```dos
certmgr.exe /del /c /s /r localmachine My /n {CN of the old certificate}
winhttpcertcfg.exe -g -c LOCAL_MACHINE\My -s {CN of the new certificate} -a {the account running Scale Out Worker service}
```

Por ejemplo:

```dos
certmgr.exe /del /c /s /r localmachine My /n WorkerMachine
winhttpcertcfg.exe -g -c LOCAL_MACHINE\My -s WorkerMachine -a SSISScaleOutWorker140
```

#### <a name="4-update-the-scale-out-worker-service-configuration-file"></a>4. Actualizar el archivo de configuración de mantenimiento del trabajador de escalabilidad horizontal
Actualice el archivo de configuración del servicio del trabajador de escalabilidad horizontal, `\<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn\WorkerSettings.config`, en el nodo del trabajador. Actualice **WorkerHttpsCertThumbprint** a la huella digital del certificado nuevo.

#### <a name="5-install-the-client-certificate-to-the-root-store-of-the-local-computer-on-the-master-node"></a>5. Instalar el certificado de cliente en el almacén raíz del equipo local en el nodo principal

#### <a name="6-restart-the-scale-out-worker-service"></a>6. Reiniciar el servicio de trabajador de escalabilidad horizontal

## <a name="next-steps"></a>Pasos siguientes
Para más información, consulte los siguientes artículos:
-   [Servicio principal de escalabilidad horizontal de Integration Services (SSIS)](integration-services-ssis-scale-out-master.md)
-   [Trabajo de escalabilidad horizontal de Integration Services (SSIS)](integration-services-ssis-scale-out-worker.md)
