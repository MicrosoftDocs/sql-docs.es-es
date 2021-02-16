---
title: Solución de problemas de escalabilidad horizontal de SSIS | Microsoft Docs
description: Solucione problemas comunes de Escalabilidad horizontal de SSIS como errores de configuración y falta de permisos de acceso.
ms.custom: performance
ms.date: 01/09/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: 7d9c02b6912019304b37d3d536312c6a5707f769
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337412"
---
# <a name="troubleshoot-scale-out"></a>Solución de problemas de escalabilidad horizontal

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



La escalabilidad horizontal de SSIS implica la comunicación entre la base de datos del catálogo de SSIS`SSISDB`, el Servicio principal de escalabilidad horizontal y el servicio de trabajo de escalabilidad horizontal. A veces, la comunicación se interrumpe debido a errores de configuración, falta de permisos de acceso y otros motivos. Este artículo puede ayudarle a solucionar problemas relacionados con la configuración de la escalabilidad horizontal.

Para investigar los síntomas que experimente, realice los pasos siguientes uno por uno hasta que se resuelva el problema.

## <a name="scale-out-master-fails"></a>Error del Servicio principal de escalabilidad horizontal

### <a name="symptoms"></a>Síntomas 
-   El patrón de escalabilidad horizontal no se puede conectar a SSISDB. 

-   Las propiedades del patrón no se pueden mostrar en el Administrador de escalabilidad horizontal.

-   Las propiedades principales no aparecen rellenadas en la vista `[catalog].[master_properties]`.

### <a name="solution"></a>Solución
1.  Compruebe si la opción Escalabilidad horizontal está habilitada.

    En SSMS, en el Explorador de objetos, haga clic con el botón derecho en **SSISDB** y active **La característica Escalabilidad horizontal está habilitada**.

    ![Escalabilidad horizontal habilitada](media/isenabled.PNG)

    Si el valor de la propiedad es "false", habilite la escalabilidad horizontal llamando al procedimiento almacenado `[catalog].[enable_scaleout]`.

2.  Compruebe si el nombre de SQL Server especificado en el archivo de configuración del Servicio principal de escalabilidad horizontal es correcto y reinicie el servicio.

## <a name="scale-out-worker-fails"></a>Error de trabajo de escalabilidad horizontal

### <a name="symptoms"></a>Síntomas 
-   El trabajo de escalabilidad horizontal no puede conectarse al Servicio principal de escalabilidad horizontal.

-   El trabajo de escalabilidad horizontal no se muestra después de agregarlo en el Administrador de escalabilidad horizontal.

-   El trabajo de escalabilidad horizontal no se muestra en la vista `[catalog].[worker_agents]`.

-   El servicio del trabajo de escalabilidad horizontal se está ejecutando, pero el trabajo de escalabilidad horizontal está desconectado.

### <a name="solution"></a>Solución
Compruebe los mensajes de error en el registro del servicio de trabajo de escalabilidad horizontal en `\<drive\>:\Users\\*[account running worker service]*\AppData\Local\SSIS\Cluster\Agent`.

## <a name="no-endpoint-listening"></a>No hay ningún extremo escuchando

### <a name="symptoms"></a>Síntomas

*"System.ServiceModel.EndpointNotFoundException: No había ningún punto de conexión escuchando en https://* [NombreEquipo]:[Puerto] */ClusterManagement/ que pudiera aceptar el mensaje".*

### <a name="solution"></a>Solución

1.  Compruebe si el número de puerto especificado en el archivo de configuración del Servicio principal de escalabilidad horizontal es correcto y reinicie el servicio. 

2.  Compruebe si el punto de conexión principal especificado en la configuración del Servicio principal de escalabilidad horizontal es correcto y reinicie el trabajo.

3.  Compruebe si el puerto del firewall está abierto en el nodo del Servicio principal de escalabilidad horizontal.

4.  Resuelva cualquier otro problema de conexión entre el nodo del Servicio principal de escalabilidad horizontal y el nodo del trabajo de escalabilidad horizontal.

## <a name="could-not-establish-trust-relationship"></a>No se pudo establecer una relación de confianza

### <a name="symptoms"></a>Síntomas
*""System.ServiceModel.Security.SecurityNegotiationException: No se pudo establecer una relación de confianza para el canal seguro SSL/TLS con la autoridad "[NombreEquipo]:[Puerto]"".*

*"System.Net.WebException: Se ha cerrado la conexión subyacente: No se pudo establecer una relación de confianza para el canal seguro SSL/TLS".*

*"System.Security.Authentication.AuthenticationException: El certificado remoto no es válido según el procedimiento de validación".*

### <a name="solution"></a>Solución
1.  Si el certificado del Servicio principal de escalabilidad horizontal todavía no está instalado en el almacén de certificados raíz del equipo local del nodo del servicio de trabajo de escalabilidad horizontal, instálelo y reinicie el servicio de trabajo.

2.  Compruebe si el nombre de host del punto de conexión principal está incluido en los nombres comunes del certificado del Servicio principal de escalabilidad horizontal. Si no es así, restablezca el punto de conexión principal del archivo de configuración del trabajo de escalabilidad horizontal y reinicie el Servicio principal de escalabilidad horizontal. 

    > [!NOTE]
    > Si no es posible cambiar el nombre de host del punto de conexión principal debido a la configuración de DNS, debe cambiar el certificado del Servicio principal de escalabilidad horizontal. Vea [Administración de certificados para la escalabilidad horizontal de SSIS](deal-with-certificates-in-ssis-scale-out.md).

3.  Compruebe si la huella digital del servicio especificada en la configuración del trabajo de escalabilidad horizontal coincide con el certificado del Servicio principal de escalabilidad horizontal de la huella digital. 

## <a name="could-not-establish-secure-channel"></a>No se pudo establecer un canal seguro

### <a name="symptoms"></a>Síntomas

*"System.ServiceModel.Security.SecurityNegotiationException: No se pudo establecer un canal seguro para SSL/TLS con la autoridad '[NombreDeEquipo]:[Puerto]'".*

*"System.Net.WebException: Se anuló la solicitud: No se puede crear un canal seguro SSL/TLS".*

### <a name="solution"></a>Solución
Compruebe si la cuenta que ejecuta el servicio del trabajo de escalabilidad horizontal accede al certificado del trabajo de escalabilidad horizontal mediante el siguiente comando:

```dos
winhttpcertcfg.exe -l -c LOCAL_MACHINE\MY -s {CN of the worker certificate}
```

Si la cuenta no tiene acceso, concédaselo mediante la ejecución del comando siguiente y reinicie el servicio del trabajo de escalabilidad horizontal.

```dos
winhttpcertcfg.exe -g -c LOCAL_MACHINE\My -s {CN of the worker certificate} -a {the account running Scale Out Worker service}
```

## <a name="http-request-forbidden"></a>Solicitud HTTP prohibida

### <a name="symptoms"></a>Síntomas

*"System.ServiceModel.Security.MessageSecurityException: Se prohibió la solicitud HTTP con el esquema de autenticación de cliente 'Anonymous'".*

*"System.Net.WebException: Error en el servidor remoto: (403) Prohibido".*

### <a name="solution"></a>Solución
1.  Si el certificado de trabajo de escalabilidad horizontal todavía no está instalado en el almacén de certificados raíz del equipo local del nodo del Servicio principal de escalabilidad horizontal, instálelo y reinicie el servicio de trabajo.

2.  Limpie los certificados obsoletos del almacén de certificados raíz del equipo local en el nodo del servicio principal de escalabilidad horizontal.

3.  Configure SChannel para que deje de enviar la lista de entidades de certificación raíz de confianza durante el proceso del protocolo de enlace TLS/SSL agregando la siguiente entrada del registro en el nodo del Servicio principal de escalabilidad horizontal.

    `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL`

    Nombre del valor: **SendTrustedIssuerList** 

    Tipo de valor: **REG_DWORD** 

    Datos del valor: **0 (False)**

4.  Si no se pueden limpiar todos los certificados no autofirmados tal y como se describe en el paso 2, establezca el valor de la siguiente clave del Registro en 2.

    `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL`

    Nombre del valor: **ClientAuthTrustMode** 

    Tipo de valor: **REG_DWORD** 

    Datos del valor: **2**

    > [!NOTE]
    > Si tiene certificados no autofirmados en el almacén de certificados raíz, se produce un error de autenticación de certificado cliente. Para obtener más información, consulte [Internet Information Services (IIS) 8 may reject client certificate requests with HTTP 403.7 or 403.16 errors](https://support.microsoft.com/help/2802568/internet-information-services-iis-8-may-reject-client-certificate-requ) (Internet Information Services (IIS) 8 puede rechazar solicitudes de certificado de cliente con errores HTTP 403.7 o 403.16).

## <a name="http-request-error"></a>Error de solicitud HTTP

### <a name="symptoms"></a>Síntomas

*"System.ServiceModel.CommunicationException: Error al realizar la solicitud HTTP a https://[NombreEquipo]:[Puerto]/ClusterManagement/. Este error puede deberse a que el certificado del servidor no está configurado correctamente con HTTP.SYS en la instrucción 'case' HTTPS. La causa puede ser también una falta de coincidencia del enlace de seguridad entre el cliente y el servidor".*

### <a name="solution"></a>Solución
1.  Para comprobar si el certificado del Servicio principal de escalabilidad horizontal está enlazado correctamente al puerto en el punto de conexión del nodo principal, ejecute el comando siguiente:

    ```dos
    netsh http show sslcert ipport=0.0.0.0:{Master port}
    ```

    Compruebe si el hash del certificado que se muestra coincide con la huella digital del certificado del Servicio principal de escalabilidad horizontal. Si el enlace no es correcto, restablézcalo con los siguientes comandos y reinicie el servicio del trabajo de escalabilidad horizontal.

    ```dos
    netsh http delete sslcert ipport=0.0.0.0:{Master port}
    netsh http add sslcert ipport=0.0.0.0:{Master port} certhash={Master certificate thumbprint} certstorename=Root  appid={random guid}
    ```

## <a name="cannot-open-certificate-store"></a>No se puede abrir el almacén de certificados

### <a name="symptoms"></a>Síntomas
Error de validación al conectar el trabajo de escalabilidad horizontal al Servicio principal de escalabilidad horizontal en el Administrador de escalabilidad horizontal. Mensaje de error: *"No se puede abrir el almacén de certificados en la máquina"* .

### <a name="solution"></a>Solución

1.  Ejecute el Administrador de escalabilidad horizontal como administrador. Si abre el Administrador de escalabilidad horizontal con SSMS, tendrá que ejecutar SSMS como administrador.

2.  Inicie el servicio de Registro remoto en el equipo, si no se está ejecutando.

## <a name="execution-doesnt-start"></a>No se inicia la ejecución

### <a name="symptoms"></a>Síntomas
No se inicia la ejecución en la escalabilidad horizontal.

### <a name="solution"></a>Solución

Compruebe el estado de los equipos seleccionados para ejecutar el paquete en la vista `[catalog].[worker_agents]`. Debe haber un trabajo en línea y habilitado como mínimo.

## <a name="no-log"></a>No hay ningún registro

### <a name="symptoms"></a>Síntomas 
Los paquetes se ejecutan correctamente, pero no hay ningún mensaje registrado.

### <a name="solution"></a>Solución

Compruebe si la instancia de SQL Server que hospeda SSISDB permite la autenticación de SQL Server.

> [!NOTE]  
> Si cambió la cuenta para el registro de escalabilidad horizontal, consulte [Change the Account for Scale Out Logging](change-logdb-account.md) (Cambiar la cuenta para el registro de escalabilidad horizontal) y compruebe la cadena de conexión usada para el registro.

## <a name="error-messages-arent-helpful"></a>Mensajes de error que no resultan útiles

### <a name="symptoms"></a>Síntomas
Los mensajes de error del informe de ejecución del paquete no son suficientes para resolver los problemas.

### <a name="solution"></a>Solución
Puede encontrar más registros de ejecución en la carpeta `TasksRootFolder` configurada en `WorkerSettings.config`. De forma predeterminada, esta carpeta es `\<drive\>:\Users\\[account]\AppData\Local\SSIS\ScaleOut\Tasks`. *[cuenta]* es la cuenta que ejecuta el servicio de trabajo de escalabilidad horizontal con el valor predeterminado `SSISScaleOutWorker140`.

Para localizar el registro de la ejecución del paquete con *[id. de ejecución]* , ejecute el comando Transact-SQL siguiente para obtener el *[id. de tarea]* . A continuación, busque el nombre de la carpeta que contenga el *[id. de tarea]* en `TasksRootFolder`.

```sql
SELECT [TaskId]
FROM [SSISDB].[internal].[tasks] tasks, [SSISDB].[internal].[executions] executions 
WHERE executions.execution_id = *Your Execution Id* AND tasks.JobId = executions.job_id
```

> [!WARNING]
> Esta consulta solo está pensada para solucionar problemas. Las vistas internas a las que se hace referencia en la consulta pueden cambiar en el futuro. 

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información, vea los siguientes artículos sobre cómo instalar y configurar la escalabilidad horizontal de SSIS:
-   [Introducción a la escalabilidad horizontal de Integration Services (SSIS) en un único equipo](get-started-with-ssis-scale-out-onebox.md)
-   [Tutorial: Configuración de escalabilidad horizontal de Integration Services (SSIS)](walkthrough-set-up-integration-services-scale-out.md)
