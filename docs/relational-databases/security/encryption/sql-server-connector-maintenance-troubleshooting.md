---
title: Mantenimiento y solución de problemas del Conector de SQL Server
description: Obtenga información sobre las instrucciones de mantenimiento y los pasos de solución de problemas comunes para el Conector de SQL Server.
ms.custom: seo-lt-2019
ms.date: 10/08/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Connector, appendix
ms.assetid: 7f5b73fc-e699-49ac-a22d-f4adcfae62b1
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: 7bbd8aee97ccd31649661032c76729ad89b7fe5a
ms.sourcegitcommit: 81ee3cd57526d255de93afb84186074a3fb9885f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622761"
---
# <a name="sql-server-connector-maintenance--troubleshooting"></a>Conector de SQL Server, apéndice

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  En este tema se proporciona información adicional acerca del conector de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Para obtener más información acerca del conector de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], consulte [Administración extensible de claves con el Almacén de claves de Azure & #40; SQL Server & #41;](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md), [Setup Steps for Extensible Key Management Using the Azure Key Vault](../../../relational-databases/security/encryption/setup-steps-for-extensible-key-management-using-the-azure-key-vault.md) (Pasos de instalación de Administración extensible de claves con el Almacén de claves de Azure) y [Use SQL Server Connector with SQL Encryption Features](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md) (Usar el conector de SQL Server con características de cifrado de SQL).  
  
##  <a name="a-maintenance-instructions-for-ssnoversion-connector"></a><a name="AppendixA"></a> A. Instrucciones de mantenimiento para el conector de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  
  
### <a name="key-rollover"></a>Sustitución incremental de claves  
  
> [!IMPORTANT]  
> El conector de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] requiere que el nombre de clave solo use los caracteres "a-z", "A-Z", "0-9" y "-", con un límite de 26 caracteres.
> No funcionará el uso de diferentes versiones de claves bajo el mismo nombre de clave en el Almacén de claves de Azure con el conector de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Para girar una clave de Azure Key Vault que está usando [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], se debe crear una clave con un nombre de clave nuevo.  
  
 Normalmente, es necesario crear versiones de las claves asimétricas de servidor para el cifrado de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] cada uno o dos años. Es importante tener en cuenta que aunque el Almacén de claves permite versiones de clave, los clientes no deben usar esta característica para implementar el control de versiones. El conector de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] no se puede ocupar de los cambios en la versión de clave del Almacén de claves. Para implementar el control de versiones de claves, cree una nueva clave en Key Vault y vuelva a cifrar la clave de cifrado de datos en [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)].  
  
 Para TDE, así es cómo se lograría:  
  
- **En PowerShell:** cree una nueva clave asimétrica (con un nombre diferente de la clave asimétrica de TDE actual) en Key Vault.  
  
    ```powershell  
    Add-AzKeyVaultKey -VaultName 'ContosoDevKeyVault' `  
      -Name 'Key2' -Destination 'Software'  
    ```  
  
- **Con [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] o sqlcmd.exe:** use las siguientes instrucciones, como se muestra en el paso 3 de la sección 3.  
  
     Importe la nueva clave asimétrica.  
  
    ```sql  
    USE master  
    CREATE ASYMMETRIC KEY [MASTER_KEY2]
    FROM PROVIDER [EKM]
    WITH PROVIDER_KEY_NAME = 'Key2',
    CREATION_DISPOSITION = OPEN_EXISTING
    GO  
    ```  
  
     Cree un nuevo inicio de sesión que se asociará con la nueva clave asimétrica (como se muestra en las instrucciones de TDE).  
  
    ```sql  
    USE master  
    CREATE LOGIN TDE_Login2
    FROM ASYMMETRIC KEY [MASTER_KEY2]  
    GO  
    ```  
  
     Cree una nueva credencial para asignarse al inicio de sesión.  
  
    ```sql  
    CREATE CREDENTIAL Azure_EKM_TDE_cred2  
        WITH IDENTITY = 'ContosoDevKeyVault',
       SECRET = 'EF5C8E094D2A4A769998D93440D8115DAADsecret123456789='
    FOR CRYPTOGRAPHIC PROVIDER EKM;  
  
    ALTER LOGIN TDE_Login2  
    ADD CREDENTIAL Azure_EKM_TDE_cred2;  
    GO  
    ```  
  
     Elija la base de datos cuya clave de cifrado de base de datos le gustaría volver a cifrar.  
  
    ```sql  
    USE [database]  
    GO  
    ```  
  
     Vuelva a cifrar la clave de cifrado de la base de datos.  
  
    ```sql  
    ALTER DATABASE ENCRYPTION KEY
    ENCRYPTION BY SERVER ASYMMETRIC KEY [MASTER_KEY2];  
    GO  
    ```  
  
### <a name="upgrade-of-ssnoversion-connector"></a>Actualización del conector de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  

Las versiones 1.0.0.440 y anteriores se han reemplazado y ya no se admiten en entornos de producción. Las versiones 1.0.1.0 y posteriores se admiten en los entornos de producción. Use las instrucciones siguientes para actualizar a la versión más reciente disponible en el [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=45344).

### <a name="upgrade"></a>Actualizar

1. Detenga el servicio SQL Server mediante el Administrador de configuración de SQL Server.
1. Desinstale la versión anterior mediante Panel de control\Programas\Programas y características.
    1. Nombre de la aplicación: Conector de SQL Server para Azure Key Vault
    1. Versión: 15.0.300.96 (o anterior)
    1. Fecha del archivo DLL: 30/01/2018 15:00 (o anterior)
1. Instale (actualice) un nuevo Conector de SQL Server para Microsoft Azure Key Vault.
    1. Versión: 15.0.2000.367
    1. Fecha del archivo DLL: 11/09/2020 5:17
1. Inicie el servicio SQL Server.
1. Pruebe que se pueda a las bases de datos cifradas.

### <a name="rollback"></a>Reversión

1. Detenga el servicio SQL Server mediante el Administrador de configuración de SQL Server.

1. Desinstale la nueva versión mediante Panel de control\Programas\Programas y características.
    1. Nombre de la aplicación: Conector de SQL Server para Azure Key Vault
    1. Versión: 15.0.2000.367
    1. Fecha del archivo DLL: 11/09/2020 5:17

1. Instale la versión anterior del Conector de SQL Server para Microsoft Azure Key Vault.
    1. Versión: 15.0.300.96
    1. Fecha del archivo DLL: 30/01/2018 15:00
1. Inicie el servicio SQL Server.

1. Compruebe que las bases de datos con TDE sean accesibles.  
  
1. Después de comprobar que la actualización funciona, puede eliminar la antigua carpeta del conector de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (si decide cambiar el nombre en lugar de desinstalar en el paso 3).

### <a name="older-versions-of-the-sql-server-connector"></a>Versiones anteriores del Conector de SQL Server
  
Vínculos profundos a versiones anteriores del Conector de SQL Server

- Corriente: [1.0.5.0 (versión 15.0.2000.367); fecha de archivo del 11 de septiembre de 2020](https://download.microsoft.com/download/8/0/9/809494F2-BAC9-4388-AD07-7EAF9745D77B/1033_15.0.2000.367/SQLServerConnectorforMicrosoftAzureKeyVault.msi)
- [1.0.5.0 (versión 15.0.300.96); fecha de archivo del 30 de enero de 2018](https://download.microsoft.com/download/8/0/9/809494F2-BAC9-4388-AD07-7EAF9745D77B/ENU/SQLServerConnectorforMicrosoftAzureKeyVault.msi)
- [1.0.4.0 (versión 13.0.811.168)](https://download.microsoft.com/download/8/0/9/809494F2-BAC9-4388-AD07-7EAF9745D77B/SQLServerConnectorforMicrosoftAzureKeyVault.msi)

### <a name="rolling-the-ssnoversion-service-principal"></a>Puesta en marcha de la entidad de servicio de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]

 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] usa entidades de servicio creadas en Azure Active Directory como credenciales para acceder al Almacén de claves. La entidad de servicio tiene identificador de cliente y clave de autenticación. Una credencial de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se configura con el **nombre de almacén**, el **identificador de cliente** y la **clave de autenticación**. La **clave de autenticación** es válida durante un determinado período de tiempo (uno o dos años). Antes de que expire el período de tiempo se debe generar una nueva clave en Azure AD para la entidad de servicio. Después, la credencial debe cambiarse en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] mantiene una caché para la credencial en la sesión actual, por lo que cuando se cambia una credencial, [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] se debe reiniciar.  
  
### <a name="key-backup-and-recovery"></a>Copia de seguridad y recuperación de claves

Se debe realizar una copia de seguridad del almacén de claves con cierta frecuencia. Si se pierde una clave asimétrica en el almacén, se puede restaurar desde la copia de seguridad. La clave debe restaurarse con el mismo nombre que antes, que es lo que hará el comando de PowerShell Restore (consulte los pasos siguientes).  
Si el almacén se ha perdido, tendrá que volver a crear un almacén y restaurar la clave asimétrica en el almacén usando el mismo nombre que antes. El nombre del almacén puede ser diferente (o el mismo que antes). Debe establecer los permisos de acceso en el nuevo almacén para conceder a la entidad de servicio de SQL Server el acceso necesario a los escenarios de cifrado de SQL Server y ajustar la credencial de SQL Server para que se refleje el nuevo nombre del almacén.

En resumen, estos son los pasos:  
  
- Realice una copia de seguridad de la clave del almacén (con el cmdlet de PowerShell Backup-AzureKeyVaultKey).  
- En caso de error del almacén, cree un nuevo almacén en la misma región geográfica. El usuario que crea el almacén debería estar en el mismo directorio predeterminado que el programa de instalación de la entidad de servicio para SQL Server.  
- Restaure la clave en el nuevo almacén mediante el cmdlet Restore-AzureKeyVaultKey de PowerShell, que restaura la clave con el mismo nombre que antes. Si ya existe una clave con el mismo nombre, se producirá un error en la restauración.  
- Conceda permisos a la entidad de servicio de SQL Server para que use este nuevo almacén.
- Modifique la credencial de SQL Server que usa el motor de base de datos para reflejar el nuevo nombre de almacén (si es necesario).  
  
Las copias de seguridad de claves se pueden restaurar entre regiones de Azure, siempre que permanezcan en la misma región geográfica o nube nacional: Estados Unidos, Canadá, Japón, Australia, India, APAC, Europa, Brasil, China, US Government o Alemania.  
  
##  <a name="b-frequently-asked-questions"></a><a name="AppendixB"></a> B. Preguntas más frecuentes

### <a name="on-azure-key-vault"></a>En el Almacén de claves de Azure
  
**¿Cómo funcionan las operaciones de clave con el Almacén de claves de Azure?**  
 La clave asimétrica en el Almacén de claves se usa para proteger las claves de cifrado de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . La parte pública de la clave asimétrica es la única que sale del almacén: el almacén no exporta nunca la parte privada. Todas las operaciones de cifrado en las que se usa la clave asimétrica se realizan en el servicio Azure Key Vault y se protegen con la seguridad del servicio.  
  
 **¿Qué es un URI de clave?**  
 Todas las claves del Almacén de claves de Azure tienen un identificador uniforme de recursos, (URI), que se puede usar para hacer referencia a la clave en la aplicación. Use el formato `https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey` para obtener la versión actual y el formato `https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87` para obtener una versión específica.  
  
### <a name="on-configuring-ssnoversion"></a>Configuración [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  

**¿Qué son los extremos a los que necesita tener acceso el conector de SQL Server?**
El conector se comunica con dos puntos de conexión, que deben estar permitidos. El único puerto requerido para la comunicación saliente a estos otros servicios es el puerto 443 para Https:

- login.microsoftonline.com/*:443
- *.vault.azure.net/* :443

**¿Cómo conectarse a Azure Key Vault a través de un servidor proxy HTTP(S)?**
El conector usa la configuración de proxy de Internet Explorer. Esta configuración se puede controlar por medio de la [directiva de grupo](/archive/blogs/askie/how-to-configure-proxy-settings-for-ie10-and-ie11-as-iem-is-not-available) o el registro, pero es importante tener en cuenta que no son opciones de configuración de todo el sistema y deben estar destinadas a la cuenta de servicio que ejecuta la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Si un administrador de bases de datos ve o edita la configuración en Internet Explorer, solo afectará a la cuenta del administrador de la base de datos en lugar del motor de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. No se recomienda iniciar sesión en el servidor de forma interactiva mediante la cuenta de servicio; esta opción está bloqueada en muchos entornos seguros. Los cambios en la configuración de proxy configurado pueden requerir que se reinicie la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] para que surtan efecto, ya que se almacenan en caché cuando el conector intenta conectarse por primera vez a un almacén de claves.

**¿Qué tamaños de clave de Azure Key Vault son compatibles con el Conector de SQL Server?**
La última compilación del Conector de SQL Server admite claves de Azure Key Vault de los tamaños 2048 y 3072.
  
 > [!NOTE] 
 > La vista "sys.asymmetric_keys" notifica el tamaño de clave como 2048 aunque se use el tamaño de clave 3072. Es un problema conocido de esta vista y el equipo de producto de SQL Server lo solucionará en una versión futura.

**¿Cuáles son los niveles de permiso mínimos necesarios para cada paso de configuración en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]?**  
 Aunque puede realizar todos los pasos de configuración como miembro del rol fijo de servidor sysadmin, [!INCLUDE[msCoName](../../../includes/msconame-md.md)] le anima a minimizar los permisos que usa. En la lista siguiente se define el nivel de permiso mínimo para cada acción.  
  
- Para crear un proveedor de servicios criptográficos, son necesarios el permiso `CONTROL SERVER` o la pertenencia al rol fijo de servidor **sysadmin** .  
  
- Para cambiar una opción de configuración y ejecutar la instrucción de `RECONFIGURE` , debe tener concedido el permiso `ALTER SETTINGS` de nivel de servidor. Los roles fijos de servidor sysadmin y `ALTER SETTINGS` serveradmin **tienen el permiso** de forma implícita.  
  
- Para cread una credencial, necesita el permiso `ALTER ANY CREDENTIAL` .  
  
- Para agregar una credencial a un inicio de sesión, necesita el permiso `ALTER ANY LOGIN` .  
  
- Para crear una clave asimétrica, necesita el permiso `CREATE ASYMMETRIC KEY` .  

**¿Cómo puedo cambiar el Active Directory predeterminado para que mi almacén de claves se cree en la misma suscripción y Active Directory de la entidad de servicio que creé para el Conector de [!INCLUDE[ssNoVersion_md](../../../includes/ssnoversion-md.md)] ?**

![Pasos para cambiar el directorio predeterminado de AAD](../../../relational-databases/security/encryption/media/aad-change-default-directory-helpsteps.png)

1. Vaya al portal clásico de Azure: [https://manage.windowsazure.com](https://manage.windowsazure.com).  
2. Seleccione **Configuración** en el menú izquierdo.
3. Seleccione la suscripción a Azure que usa actualmente y haga clic en **Editar directorio** en los comandos que aparecen en la parte inferior de la pantalla.
4. En la ventana emergente, use el menú desplegable **Directorio** para seleccionar el Active Directory que desea usar. Esto lo convertirá en el directorio predeterminado.
5. Asegúrese de ser el administrador global del Active Directory recién seleccionado. Si no es el administrador global, podría perder permisos de administración después del cambio de directorios.
6. Una vez que se cierra la ventana emergente, si no ve ninguna de las suscripciones, puede que tenga que actualizar el filtro **Filtrar por directorio** del filtro **Suscripciones** en el menú superior derecho de la pantalla para ver las suscripciones que usa la instancia de Active Directory recién seleccionada.

    > [!NOTE] 
    > Puede que no tenga los permisos para realmente cambiar el directorio predeterminado de la suscripción a Azure. En este caso, cree la entidad de servicio de AAD dentro del directorio predeterminado para que esté en el mismo directorio que Azure Key Vault que se usará más adelante.

Para más información sobre Active Directory, consulte [Asociación de las suscripciones de Azure con Azure Active Directory](/azure/active-directory/fundamentals/active-directory-how-subscriptions-associated-directory).
  
##  <a name="c-error-code-explanations-for-ssnoversion-connector"></a><a name="AppendixC"></a> C. Explicación de los códigos de error para el conector de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]

**Códigos de error del proveedor:**  
  
Código de error  |Símbolo  |Descripción
---------|---------|---------  
0 | scp_err_Success | La operación se ha realizado correctamente.
1 | scp_err_Failure | Se produjeron errores en la operación.
2 | scp_err_InsufficientBuffer | Este error indica al motor que asigne más memoria para el búfer.
3 | scp_err_NotSupported | La operación no es compatible. Por ejemplo, el tipo de clave o el algoritmo especificado no es compatible con el proveedor EKM.
4 | scp_err_NotFound | El proveedor EKM no ha podido encontrar el algoritmo o la clave especificada.
5 | scp_err_AuthFailure | Error durante la autenticación con el proveedor EKM.
6 | scp_err_InvalidArgument | El argumento proporcionado no es válido.
7 | scp_err_ProviderError | Se ha producido un error sin especificar en el proveedor EKM que ha detectado el motor de SQL.
401 | acquireToken | El servidor ha respondido con el error 401 a la solicitud. Asegúrese de que el secreto y el id. de cliente son correctos, y que la cadena de credenciales es una concatenación de secreto e id. de cliente de AAD sin guiones.
404 | getKeyByName | El servidor respondió con un error 404 porque no se encontró el nombre de clave. Asegúrese de que el nombre de clave existe en el almacén.
2049 | scp_err_KeyNameDoesNotFitThumbprint | El nombre de la clave es demasiado largo para caber en la huella digital del motor de SQL. El nombre de la clave no debe superar los 26 caracteres.
2050 | scp_err_PasswordTooShort | La cadena de secreto, que es la concatenación del identificador de cliente y el secreto de AAD, no llega a 32 caracteres.
2051 | scp_err_OutOfMemory | El motor de SQL se ha quedado sin memoria y no se ha podido asignar la memoria para el proveedor EKM.
2052 | scp_err_ConvertKeyNameToThumbprint | No se ha podido convertir el nombre de la clave en una huella digital.
2053 | scp_err_ConvertThumbprintToKeyName|  No se ha podido convertir la huella digital en un nombre de clave.
2058 | scp_err_FailureInRegistry|  No se pudo realizar la operación en el Registro. La cuenta del servicio SQL Server no tiene permiso para crear la clave del Registro.
3000 | ErrorSuccess | La operación de AKV se ha realizado correctamente.
3001 | ErrorUnknown | Se ha producido un error en la operación de AKV con un error sin especificar.
3002 | ErrorHttpCreateHttpClientOutOfMemory | No se puede crear un elemento HttpClient para la operación de AKV debido a que no hay memoria suficiente.
3003 | ErrorHttpOpenSession | No se puede abrir una sesión Http debido a un error de red.
3004 | ErrorHttpConnectSession | No se puede conectar una sesión Http debido a un error de red.
3005 | ErrorHttpAttemptConnect | No se puede intentar una conexión debido a un error de red.
3006 | ErrorHttpOpenRequest | No se puede abrir una solicitud debido a un error de red.
3007 | ErrorHttpAddRequestHeader | No se puede agregar un encabezado de solicitud.
3008 | ErrorHttpSendRequest | No se puede enviar una solicitud debido a un error de red.
3009 | ErrorHttpGetResponseCode | No se puede obtener un código de respuesta debido a un error de red.
3010 | ErrorHttpResponseCodeUnauthorized | El servidor ha respondido con el error 401 a la solicitud.
3011 | ErrorHttpResponseCodeThrottled | El servidor ha limitado la solicitud.
3012 | ErrorHttpResponseCodeClientError | La solicitud que se envió desde el conector no es válida. Normalmente esto significa que el nombre de clave no es válido o contiene caracteres no válidos.
3013 | ErrorHttpResponseCodeServerError | El servidor ha respondido con un código de respuesta entre 500 y 600.
3014 | ErrorHttpQueryHeader | No se puede realizar una consulta para el encabezado de respuesta.
3015 | ErrorHttpQueryHeaderOutOfMemoryCopyHeader | No se puede copiar el encabezado de respuesta debido a que no hay memoria suficiente.
3016 | ErrorHttpQueryHeaderOutOfMemoryReallocBuffer | No se puede realizar una consulta al encabezado de respuesta debido a que no hay memoria suficiente al reasignar un búfer.
3017 | ErrorHttpQueryHeaderNotFound | No se encuentra el encabezado de consulta en la respuesta.
3018 | ErrorHttpQueryHeaderUpdateBufferLength | No se puede actualizar la longitud del búfer al realizar una consulta del encabezado de respuesta.
3019 | ErrorHttpReadData | No se pueden leer los datos de respuesta debido a un error de red.
3076 | ErrorHttpResourceNotFound | El servidor respondió con un error 404 porque no se encontró el nombre de clave. Asegúrese de que el nombre de la clave existe en el almacén.
3077 | ErrorHttpOperationForbidden | El servidor respondió con un error 403 porque el usuario no tiene el permiso adecuado para realizar la acción. Asegúrese de que tiene el permiso para la operación especificada. Como mínimo, el conector requiere los permisos 'get, list, wrapKey, unwrapKey' para funcionar adecuadamente.
3100 | ErrorHttpCreateHttpClientOutOfMemory               | No se puede crear un HttpClient para la operación de AKV debido a que no hay memoria suficiente.
3101 | ErrorHttpOpenSession                               | No se puede abrir una sesión HTTP debido a un error de red.
3102 | ErrorHttpConnectSession                            | No se puede conectar una sesión HTTP debido a un error de red.
3103 | ErrorHttpAttemptConnect                            | No se puede intentar realizar una conexión debido a un error de red.
3104 | ErrorHttpOpenRequest                               | No se puede abrir una solicitud debido a un error de red.
3105 | ErrorHttpAddRequestHeader                          | No se puede agregar un encabezado de solicitud.
3106 | ErrorHttpSendRequest                               | No se puede enviar una solicitud debido a un error de red.
3107 | ErrorHttpGetResponseCode                           | No se puede obtener un código de respuesta debido a un error de red.
3108 | ErrorHttpResponseCodeUnauthorized                  | El servidor ha respondido con el error 401 a la solicitud. Asegúrese de que el secreto y el identificador de cliente son correctos, y que la cadena de credenciales es una concatenación de secreto e identificador de cliente de AAD sin guiones.
3109 | ErrorHttpResponseCodeThrottled                     | El servidor ha limitado la solicitud.
3110 | ErrorHttpResponseCodeClientError                    | La solicitud no es válida. Normalmente esto significa que el nombre de clave no es válido o contiene caracteres no válidos.
3111 | ErrorHttpResponseCodeServerError                   | El servidor ha respondido con un código de respuesta entre 500 y 600.
3112 | ErrorHttpResourceNotFound                          | El servidor respondió con un error 404 porque no se encontró el nombre de clave. Asegúrese de que el nombre de clave existe en el almacén.
3113 | ErrorHttpOperationForbidden                         | El servidor respondió con un error 403 porque el usuario no tiene el permiso adecuado para realizar la acción. Asegúrese de que tiene el permiso para la operación especificada. Como mínimo, se necesitan los permisos "get, wrapKey, unwrapKey".
3114 | ErrorHttpQueryHeader                               | No se puede realizar una consulta para el encabezado de respuesta.
3115 | ErrorHttpQueryHeaderOutOfMemoryCopyHeader          | No se puede copiar el encabezado de respuesta debido a que no hay memoria suficiente.
3116 | ErrorHttpQueryHeaderOutOfMemoryReallocBuffer       | No se puede realizar una consulta al encabezado de respuesta debido a que no hay memoria suficiente al reasignar un búfer.
3117 | ErrorHttpQueryHeaderNotFound                       | No se encuentra el encabezado de consulta en la respuesta.
3118 | ErrorHttpQueryHeaderUpdateBufferLength             | No se puede actualizar la longitud del búfer al realizar una consulta del encabezado de respuesta.
3119 | ErrorHttpReadData                                  | No se pueden leer los datos de respuesta debido a un error de red.
3120 | ErrorHttpGetResponseOutOfMemoryCreateTempBuffer    | No se puede obtener el cuerpo de respuesta debido a memoria insuficiente al crear un búfer temporal.
3121 | ErrorHttpGetResponseOutOfMemoryGetResultString     | No se puede obtener el cuerpo de respuesta debido a memoria insuficiente al obtener la cadena de resultado.
3122 | ErrorHttpGetResponseOutOfMemoryAppendResponse      | No se puede obtener el cuerpo de respuesta debido a memoria insuficiente al anexar la respuesta.
3200 | ErrorGetAADValuesOutOfMemoryConcatPath | No se pueden obtener valores de encabezado de desafío de Azure Active Directory debido a memoria insuficiente al concatenar la ruta de acceso.
3201 | ErrorGetAADDomainUrlStartPosition | No se puede encontrar la posición inicial de la dirección URL de dominio de Azure Active Directory en el encabezado de desafío de respuesta con formato incorrecto.
3202 | ErrorGetAADDomainUrlStopPosition | No se puede encontrar la posición final de la dirección URL de dominio de Azure Active Directory en el encabezado de desafío de respuesta con formato incorrecto.
3203 | ErrorGetAADDomainUrlMalformatted | El encabezado de desafío de respuesta de Azure Active Directory tiene un formato incorrecto y no contiene la dirección URL de dominio de AAD.
3204 | ErrorGetAADDomainUrlOutOfMemoryAlloc | Memoria insuficiente al asignar el búfer para la dirección URL del dominio de Azure Active Directory.
3205 | ErrorGetAADTenantIdOutOfMemoryAlloc | Memoria insuficiente al asignar el búfer para el valor tenantId de Azure Active Directory.
3206 | ErrorGetAKVResourceUrlStartPosition | No se puede encontrar la posición inicial de la dirección URL del recurso de Azure Key Vault en el encabezado de desafío de respuesta con formato incorrecto.
3207 | ErrorGetAKVResourceUrlStopPosition | No se puede encontrar la posición final de la dirección URL del recurso de Azure Key Vault en el encabezado de desafío de respuesta con formato incorrecto.
3208 | ErrorGetAKVResourceUrlOutOfMemoryAlloc | Memoria insuficiente al asignar el búfer para la dirección URL del recurso de Azure Key Vault.
3300 | ErrorGetTokenOutOfMemoryConcatPath | No se puede obtener el token debido a memoria insuficiente al concatenar la ruta de acceso de la solicitud.
3301 | ErrorGetTokenOutOfMemoryConcatBody | No se puede obtener el token debido a memoria insuficiente al concatenar el cuerpo de la respuesta.
3302 | ErrorGetTokenOutOfMemoryConvertResponseString | No se puede obtener el token debido a memoria insuficiente al convertir la cadena de respuesta.
3303 | ErrorGetTokenBadCredentials | No se puede obtener el token debido a credenciales incorrectas. Asegúrese de que la cadena de credenciales o el certificado sean válidos.
3304 | ErrorGetTokenFailedToGetToken | Aunque las credenciales son correctas, la operación todavía no pudo obtener un token válido.
3305 | ErrorGetTokenRejected | El token es válido, pero lo rechaza el servidor.
3306 | ErrorGetTokenNotFound | No se encuentra el token en la respuesta.
3307 | ErrorGetTokenJsonParser | No se puede analizar la respuesta JSON del servidor.
3308 | ErrorGetTokenExtractToken | No se puede extraer el token de la respuesta JSON.
3400 | ErrorGetKeyByNameOutOfMemoryConvertResponseString | No se puede obtener la clave por nombre debido a memoria insuficiente al convertir la cadena de respuesta.
3401 | ErrorGetKeyByNameOutOfMemoryConcatPath | No se puede obtener la clave por nombre debido a memoria insuficiente al concatenar la ruta de acceso.
3402 | ErrorGetKeyByNameOutOfMemoryConcatHeader | No se puede obtener la clave por nombre debido a memoria insuficiente al concatenar el encabezado.
3403 | ErrorGetKeyByNameNoResponse | No se puede obtener la clave por nombre debido a que no hay respuesta del servidor.
3404 | ErrorGetKeyByNameJsonParser | No se puede obtener la clave por nombre debido a un error al analizar la respuesta JSON.
3405 | ErrorGetKeyByNameExtractKeyNode | No se puede obtener la clave por nombre debido a un error al extraer el nodo de clave de la respuesta.
3406 | ErrorGetKeyByNameExtractKeyId | No se puede obtener la clave por nombre debido a un error al extraer el identificador de clave de la respuesta.
3407 | ErrorGetKeyByNameExtractKeyType | No se puede obtener la clave por nombre debido a un error al extraer el tipo de clave de la respuesta.
3408 | ErrorGetKeyByNameExtractKeyN | No se puede obtener la clave por nombre debido a un error al extraer la clave N de la respuesta.
3409 | ErrorGetKeyByNameBase64DecodeN | No se puede obtener la clave por nombre debido a un error al decodificar la N en base64.
3410 | ErrorGetKeyByNameExtractKeyE | No se puede obtener la clave por nombre debido a un error al extraer la clave E de la respuesta.
3411 | ErrorGetKeyByNameBase64DecodeE | No se puede obtener la clave por nombre debido a un error al decodificar la E en base64.
3412 | ErrorGetKeyByNameExtractKeyUri | No se puede extraer el URI de clave de la respuesta.
3500 | ErrorBackupKeyOutOfMemoryConvertResponseString | No se puede realizar una copia de seguridad de la clave debido a memoria insuficiente al convertir la cadena de respuesta.
3501 | ErrorBackupKeyOutOfMemoryConcatPath | No se puede realizar una copia de seguridad de la clave debido a memoria insuficiente al concatenar la ruta de acceso.
3502 | ErrorBackupKeyOutOfMemoryConcatHeader | No se puede realizar una copia de seguridad de la clave debido a memoria insuficiente al concatenar el encabezado de la solicitud.
3503 | ErrorBackupKeyNoResponse | No se puede realizar una copia de seguridad de la clave debido a que no hay respuesta del servidor.
3504 | ErrorBackupKeyJsonParser | No se puede realizar una copia de seguridad de la clave debido a un error al analizar la respuesta JSON.
3505 | ErrorBackupKeyExtractValue | No se puede realizar una copia de seguridad de la clave debido a un error al extraer el valor de la respuesta JSON.
3506 | ErrorBackupKeyBase64DecodeValue | No se puede realizar la copia de seguridad de la clave debido a un error al descodificar el campo de valor en Base64.
3600 | ErrorWrapKeyOutOfMemoryConvertResponseString | No se puede ajustar la clave debido a memoria insuficiente al convertir la cadena de respuesta.
3601 | ErrorWrapKeyOutOfMemoryConcatPath | No se puede ajustar la clave debido a memoria insuficiente al concatenar la ruta de acceso.
3602 | ErrorWrapKeyOutOfMemoryConcatHeader | No se puede ajustar la clave debido a memoria insuficiente al concatenar el encabezado.
3603 | ErrorWrapKeyOutOfMemoryConcatBody | No se puede ajustar la clave debido a memoria insuficiente al concatenar el cuerpo.
3604 | ErrorWrapKeyOutOfMemoryConvertEncodedBody | No se puede ajustar la clave debido a memoria insuficiente al convertir el cuerpo codificado.
3605 | ErrorWrapKeyBase64EncodeKey | No se puede ajustar la clave debido a un error al codificar la clave en base64.
3606 | ErrorWrapKeyBase64DecodeValue | No se puede ajustar la clave debido a un error al descodificar el valor de respuesta en base64.
3607 | ErrorWrapKeyJsonParser | No se puede ajustar la clave debido a un error al analizar la respuesta JSON.
3608 | ErrorWrapKeyExtractValue | No se puede ajustar la clave debido a un error al extraer el valor de la respuesta.
3609 | ErrorWrapKeyNoResponse | No se puede ajustar la clave debido a que no hay respuesta del servidor.
3700 | ErrorUnwrapKeyOutOfMemoryConvertResponseString | No se puede desajustar la clave debido a memoria insuficiente al convertir la cadena de respuesta.
3701 | ErrorUnwrapKeyOutOfMemoryConcatPath | No se puede desajustar la clave debido a memoria insuficiente al concatenar la ruta de acceso.
3702 | ErrorUnwrapKeyOutOfMemoryConcatHeader | No se puede desajustar la clave debido a memoria insuficiente al concatenar el encabezado.
3703 | ErrorUnwrapKeyOutOfMemoryConcatBody | No se puede desajustar la clave debido a memoria insuficiente al concatenar el cuerpo.
3704 | ErrorUnwrapKeyOutOfMemoryConvertEncodedBody | No se puede desajustar la clave debido a memoria insuficiente al convertir el cuerpo codificado.
3705 | ErrorUnwrapKeyBase64EncodeKey | No se puede desajustar la clave debido a un error al codificar la clave en base64.
3706 | ErrorUnwrapKeyBase64DecodeValue | No se puede desajustar la clave debido a un error al descodificar el valor de respuesta en base64.
3707 | ErrorUnwrapKeyJsonParser | No se puede desajustar la clave debido a un error al extraer el valor de la respuesta.
3708 | ErrorUnwrapKeyExtractValue | No se puede desajustar la clave debido a un error al extraer el valor de la respuesta.
3709 | ErrorUnwrapKeyNoResponse | No se puede desajustar la clave debido a que no hay respuesta del servidor.
3800 | ErrorSecretAuthParamsGetRequestBody | Error al crear el cuerpo de la solicitud con el valor clientId y el secreto de AAD.
3801 | ErrorJWTTokenCreateHeader | Error al crear el encabezado de token JWT para la autenticación con AAD.
3802 | ErrorJWTTokenCreatePayloadGUID | Error al crear el GUID para la carga de token JWT para la autenticación con AAD.
3803 | ErrorJWTTokenCreatePayload | Error al crear la carga de token JWT para la autenticación con AAD.
3804 | ErrorJWTTokenCreateSignature | Error al crear la firma de token de JWT para la autenticación con AAD.
3805 | ErrorJWTTokenSignatureHashAlg | Error al obtener el algoritmo hash SHA256 para la autenticación con AAD.
3806 | ErrorJWTTokenSignatureHash | Error al crear el hash SHA256 para la autenticación del token JWT con AAD.
3807 | ErrorJWTTokenSignatureSignHash | Error al firmar el hash del token JWT para la autenticación con AAD.
3808 | ErrorJWTTokenCreateToken | Error al crear el token JWT para la autenticación con AAD.
3809 | ErrorPfxCertAuthParamsImportPfx | Error al importar el certificado pfx para la autenticación con AAD.
3810 | ErrorPfxCertAuthParamsGetThumbprint | Error al obtener la huella digital del certificado pfx para la autenticación con AAD.
3811 | ErrorPfxCertAuthParamsGetPrivateKey | Error al obtener la clave privada del certificado pfx para la autenticación con AAD.
3812 | ErrorPfxCertAuthParamsSignAlg | Error al obtener el algoritmo de firma RSA para la autenticación del certificado pfx con AAD.
3813 | ErrorPfxCertAuthParamsImportForSign | Error al importar la clave privada de pfx para la firma RSA para la autenticación con AAD.
3814 | ErrorPfxCertAuthParamsCreateRequestBody | Error al crear el cuerpo de la solicitud desde el certificado pfx para la autenticación con AAD.
3815 | ErrorPEMCertAuthParamsGetThumbprint | Error de descodificación de la huella digital en base64 para la autenticación con AAD.
3816 | ErrorPEMCertAuthParamsGetPrivateKey | Error al obtener la clave privada RSA de PEM para la autenticación con AAD.
3817 | ErrorPEMCertAuthParamsSignAlg | Error al obtener el algoritmo de firma RSA para la autenticación de clave privada de PEM con AAD.
3818 | ErrorPEMCertAuthParamsImportForSign | Error al importar la clave privada de PEM para la firma RSA para la autenticación con AAD.
3819 | ErrorPEMCertAuthParamsCreateRequestBody | Error al crear el cuerpo de la solicitud a partir de la clave privada de PEM para la autenticación con AAD.
3820 | ErrorLegacyPrivateKeyAuthParamsSignAlg | Error al obtener el algoritmo de firma RSA para la autenticación de clave privada heredada con AAD.
3821 | ErrorLegacyPrivateKeyAuthParamsImportForSign | Error al importar la clave privada heredada para la firma RSA para la autenticación con AAD.
3822 | ErrorLegacyPrivateKeyAuthParamsCreateRequestBody        | Error al crear el cuerpo de la solicitud a partir de la clave privada heredada para la autenticación con AAD.
3900 | ErrorAKVDoesNotExist | Error de nombre de Internet no resuelto. Esto suele indicar que se ha eliminado Azure Key Vault.
4000 | ErrorCreateKeyVaultRetryManagerOutOfMemory | No se puede crear un RetryManager para la operación de AKV debido a que no hay memoria suficiente.

Si no ve el código de error en esta tabla, estas son algunas razones más por las que se puede estar produciendo el error:
  
- Es posible que no disponga de acceso a Internet y no pueda acceder a Azure Key Vault. Compruebe la conexión a Internet.  
  
- Es posible que el servicio del Almacén de claves de Azure esté fuera de servicio. Vuelva a intentarlo en otro momento.  
  
- Es posible que haya quitado la clave asimétrica del Almacén de datos de Azure o [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Restaure la clave.  
  
- Si recibe un error de "No se puede cargar la biblioteca", asegúrese de tener la versión adecuada de Visual Studio C++ Redistribuible instalada en función de la versión de SQL Server que se esté ejecutando. En la tabla siguiente se especifica qué versión instalar desde el Centro de descarga de Microsoft.

El registro de eventos de Windows también registra los errores asociados con el Conector de SQL Server, lo que puede ayudar proporcionando contexto adicional sobre el motivo por el que se produce el error. El origen del registro de eventos de la aplicación Windows será "Conector de SQL Server para Microsoft Azure Key Vault".
  
SQL Server Version  |Vínculo de instalación redistribuible
---------|---------
2008, 2008 R2, 2012, 2014 | [Paquetes redistribuibles de Visual C++ para Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=40784)
2016 | [Visual C++ Redistributable para Visual Studio 2015](https://www.microsoft.com/download/details.aspx?id=48145)
  
## <a name="additional-references"></a>Referencias adicionales

 Más información acerca de la Administración extensible de claves:  
  
- [Administración extensible de claves &#40;EKM&#41;](../../../relational-databases/security/encryption/extensible-key-management-ekm.md)  
  
 Cifrados de SQL compatibles con EKM:  
  
- [Habilitar TDE en SQL Server con EKM](../../../relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm.md)  
  
- [Cifrado de copia de seguridad](../../../relational-databases/backup-restore/backup-encryption.md)  
  
- [Crear una copia de seguridad cifrada](../../../relational-databases/backup-restore/create-an-encrypted-backup.md)  
  
 Comandos de [!INCLUDE[tsql](../../../includes/tsql-md.md)] relacionados:  
  
- [sp_configure &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
- [CREATE CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../../t-sql/statements/create-cryptographic-provider-transact-sql.md)  
  
- [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../../t-sql/statements/create-credential-transact-sql.md)  
  
- [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-asymmetric-key-transact-sql.md)  
  
- [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
- [CREATE LOGIN &#40;Transact-SQL&#41;](../../../t-sql/statements/create-login-transact-sql.md)  
  
- [ALTER LOGIN &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-login-transact-sql.md)  
  
 Documentación del Almacén de claves de Azure:  
  
- [¿Qué es Azure Key Vault?](/azure/key-vault/general/basic-concepts)  
  
- [Introducción al Almacén de claves de Azure](/azure/key-vault/general/overview)  
  
- Referencia de [cmdlets del Almacén de claves de Azure](/powershell/module/azurerm.keyvault/) de PowerShell  
  
## <a name="see-also"></a>Consulte también

 [Administración extensible de claves con Azure Key Vault](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  
 [Usar el conector de SQL Server con características de cifrado de SQL](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md)  
 [EKM provider enabled (opción de configuración del servidor)](../../../database-engine/configure-windows/ekm-provider-enabled-server-configuration-option.md)  
 [Setup Steps for Extensible Key Management Using the Azure Key Vault](../../../relational-databases/security/encryption/setup-steps-for-extensible-key-management-using-the-azure-key-vault.md)

Para obtener otros scripts de ejemplo, vea el blog en [Cifrado de datos transparente de SQL Server y administración extensible de claves con Azure Key Vault](https://techcommunity.microsoft.com/t5/sql-server/intro-sql-server-transparent-data-encryption-and-extensible-key/ba-p/1427549).
