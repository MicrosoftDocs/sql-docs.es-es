---
title: Configuración de la cuenta de servicio del servidor de informes (Administrador de configuración) | Microsoft Docs
description: Reporting Services se implementa como un servicio único que contiene un servicio web del servidor de informes, el portal web y una aplicación de procesamiento en segundo plano que se usa para el procesamiento programado de informes y la entrega de suscripciones.
author: maggiesMSFT
ms.author: maggies
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
ms.custom: seo-lt-2019, seo-mmd-2019
ms.date: 06/09/2020
ms.openlocfilehash: 530478c30885e603d73a5bd35fe3a759b607b836
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100014926"
---
# <a name="configure-the-report-server-service-account-report-server-configuration-manager"></a>Configurar la cuenta de servicio del servidor de informes (Administrador de configuración del servidor de informes)

  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] se implementa como un servicio único que contiene el servicio web del servidor de informes, el [!INCLUDE[ssRSWebPortal-Non-Markdown](../../includes/ssrswebportal-non-markdown-md.md)]y una aplicación de procesamiento en segundo plano que se usa para el procesamiento programado de informes y la entrega de suscripciones. En este tema se explica cómo se configura inicialmente la cuenta de servicio y cómo modificar la cuenta o la contraseña con la herramienta Configuración de Reporting Services.  
  
## <a name="initial-configuration"></a>Configuración inicial

 La cuenta del servicio Servidor de informes se define durante la instalación. Puede ejecutar el servicio en una cuenta de usuario de dominio o en una cuenta integrada, como **cuenta de servicio virtual**. No hay ninguna cuenta predeterminada; la que se especifique en la página **Cuentas de servicio** del Asistente para la instalación se convierte en la cuenta inicial del servicio del servidor de informes.  
  
> [!IMPORTANT]  
> Aunque el servicio web del servidor de informes y [!INCLUDE[ssRSWebPortal-Non-Markdown](../../includes/ssrswebportal-non-markdown-md.md)] son aplicaciones independientes de [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] , se ejecutan en una sola arquitectura de servicio dentro de la misma identidad de proceso del servidor de informes.

> [!NOTE]  
> Las cuentas de servicio de Windows integradas (Servicio local o Servicio de red) no se admiten como cuentas de servicio del servidor de informes en un equipo que sea controlador de dominio.
  
## <a name="changing-the-service-account"></a>Cambiar la cuenta de servicio

 Para ver y reconfigurar la información de la cuenta del servicio, use siempre el Administrador de configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . La información de la identidad del servicio se almacena internamente en varias ubicaciones. El uso de la herramienta garantiza que todas las referencias se actualizan en consecuencia siempre que se cambia la cuenta o la contraseña. El Administrador de configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] lleva a cabo los siguientes pasos adicionales para garantizar que el servidor de informes sigue disponible:  
  
- Agrega automáticamente la cuenta nueva al grupo de servidores de informes que se crea en el equipo local. Este grupo se especifica en las listas de control de acceso (ACL) que protegen los archivos de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
- Actualiza automáticamente los permisos de inicio de sesión en la instancia del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] que se utiliza para hospedar la base de datos del servidor de informes. La cuenta nueva se agrega a **RSExecRole**.  
  
     El inicio de sesión de base de datos de la cuenta anterior no se quita automáticamente. Asegúrese de quitar las cuentas que ya no se usen. Para más información, vea [Administrar una base de datos del servidor de informes &#40;modo nativo de SSRS&#41;](../../reporting-services/report-server/administer-a-report-server-database-ssrs-native-mode.md).  
  
     Solo se conceden permisos de base de datos a una cuenta de servicio nueva si la conexión de base de datos del servidor de informes se configuró para usar la cuenta de servicio en primer lugar. Si la conexión de base de datos del servidor de informes se configuró para utilizar una cuenta de usuario de dominio o un inicio de sesión de base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la actualización de la cuenta de servicio no afecta a la información de conexión.  
  
- Actualiza automáticamente la clave de cifrado para incluir la información de perfil de la nueva cuenta.  
  
    > [!NOTE]  
    > Si el servidor de informes forma parte de la implementación escalada, solo resultará afectado el servidor de informes que se está actualizando. El cambio de cuenta de servicio no afecta a las claves de cifrado de otros servidores de informes de la implementación.  

## <a name="to-configure-the-report-server-service-account"></a>Configurar la cuenta del servicio Servidor de informes  
  
1. Inicie el Administrador de configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] y conéctese al servidor de informes.  
  
2. En la página Cuenta de servicio, seleccione la opción que describa el tipo de cuenta que desea utilizar.  
  
3. Si seleccionó una cuenta de usuario de Windows, especifique la nueva cuenta y la contraseña. La cuenta no puede tener más de 20 caracteres y no puede contener los caracteres especiales " / \ [ ] : ; | = , + * ? < > ' según las reglas de nomenclatura de cuentas de usuario de Windows.  
  
     Si el servidor de informes se implementa en una red que admite la autenticación Kerberos, debe registrar el Nombre principal de servicio (SPN) del servidor de informes con la cuenta de usuario de dominio especificada. Para más información, vea [Registrar un nombre de entidad de seguridad de servicio &#40;SPN&#41; para un servidor de informes](../../reporting-services/report-server/register-a-service-principal-name-spn-for-a-report-server.md).  
  
4. Haga clic en **Aplicar**.  
  
5. Cuando el sistema pida que cree una copia de seguridad de la clave simétrica, escriba un nombre de archivo y una ubicación para la copia de seguridad, escriba una contraseña para bloquear y desbloquear el archivo y haga clic en **Aceptar**.  
  
6. Si el servidor de informes usa la cuenta de servicio para conectarse a su base de datos, la información de la conexión se actualiza para usar la nueva cuenta o contraseña. Para actualizar la información de la conexión se requiere que se conecte a la base de datos. Si aparece el cuadro de diálogo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **Conexión de base de datos**, escriba las credenciales que tienen permiso para conectarse a la base de datos y haga clic en **Aceptar**.  
  
7. Cuando el sistema pida confirmación para restaurar la clave simétrica, escriba la contraseña que especificó en el paso 5 y haga clic en **Aceptar**.  
  
8. Revise los mensajes de estado en el panel Resultados para comprobar que todas las tareas se completaron correctamente.  
  
## <a name="choosing-an-account"></a>Elegir una cuenta

 Para obtener los mejores resultados, especifique una cuenta que tenga permisos de conexión de red, con acceso a los controladores de dominio de la red y a servidores SMTP corporativos o puertas de enlace. En la tabla siguiente se resumen las cuentas y se proporcionan recomendaciones para utilizarlas.  
  
|Cuenta|Explicación|  
|-------------|-----------------|  
|Cuentas de usuario de dominio|Si tiene una cuenta de usuario de dominio de Windows que tenga los permisos mínimos requeridos para las operaciones del servidor de informes, debería utilizarla.<br /><br /> Se recomienda emplear una cuenta de usuario de dominio porque aísla el servicio Servidor de informes de otras aplicaciones. Al ejecutar varias aplicaciones en una cuenta compartida, como Servicio de red, se aumenta el riesgo de que un usuario malintencionado tome el control del servidor de informes gracias a que una infracción de seguridad de una aplicación se pueda extender con facilidad a todas las aplicaciones que se ejecutan en la misma cuenta.<br /><br /> Si se usa una cuenta de usuario de dominio, será necesario cambiar la contraseña periódicamente si la organización exige una directiva de expiración de las contraseñas. También es posible que tenga que registrar el servicio con la cuenta de usuario. Para más información, vea [Registrar un nombre de entidad de seguridad de servicio &#40;SPN&#41; para un servidor de informes](../../reporting-services/report-server/register-a-service-principal-name-spn-for-a-report-server.md).<br /><br /> Evite utilizar una cuenta de usuario de Windows local. Las cuentas locales no suelen contar con los permisos suficientes para acceder a los recursos de otros equipos. Para obtener más información sobre el modo en que el uso de una cuenta local limita la funcionalidad del servidor de informes, vea [Consideraciones para usar cuentas locales](#localaccounts) en este tema.| 
| **Cuenta de servicio administrada de grupo (gMSA)** | Las cuentas de servicio administradas independientes se introdujeron en Windows Server 2008 R2 y Windows 7. Son cuentas de dominio administradas que proporcionan administración automática de contraseñas y administración simplificada de SPN, lo que incluye la delegación de la administración a otros administradores. La **cuenta de servicio administrada de grupo** proporciona la misma funcionalidad dentro del dominio, pero también la amplía en varios servidores. |
|**Cuenta de servicio virtual**|**Cuenta de servicio virtual** representa el servicio de Windows. Se trata de una cuenta integrada con privilegios mínimos que tiene permisos de inicio de sesión en red. Esta cuenta se recomienda si no tiene una cuenta de usuario de dominio disponible o si quiere evitar cualquier interrupción del servicio que podría producirse como resultado de las directivas de expiración de las contraseñas.|  
|**Servicio de red**|**Servicio de red** es una cuenta integrada con privilegios mínimos que tiene permisos de inicio de sesión en red. <br /><br /> Si selecciona **Servicio de red**, intente reducir el número de servicios adicionales que se ejecutan en la misma cuenta. Una infracción de seguridad de cualquier aplicación pone en peligro la seguridad de las demás aplicaciones que se ejecuten en la misma cuenta.|  
|**Servicio local**|**Servicio local** es una cuenta integrada similar a una cuenta de usuario de Windows local autenticada. Los servicios que se ejecutan como cuenta **Servicio local** tienen acceso a los recursos de red como una sesión nula sin credenciales. Esta cuenta no es adecuada para los escenarios de implementación con intranets en los que el servidor de informes deba conectarse a una base de datos del servidor de informes remota o a un controlador de dominio de la red para autenticar a un usuario antes de abrir un informe o procesar una suscripción.|  
|**Sistema local**|**Sistema local** es una cuenta con muchos privilegios que no es necesaria para ejecutar un servidor de informes. No utilice esta cuenta para instalaciones de servidor de informes. En su lugar, elija una cuenta de dominio o **Servicio de red** .|  
  
## <a name="considerations-for-using-local-accounts"></a><a name="localaccounts"></a> Consideraciones para usar cuentas locales

 La consideración principal para utilizar cuentas locales es si el servidor de informes requiere acceso a controladores de dominio, servidores de correo y servidores de bases de datos remotos. Si configura el servidor de informes para ejecutarse en una cuenta de usuario de Windows local, Servicio local o Sistema local, introduce consideraciones que deben tenerse en cuenta para establecer otra configuración, y en la creación y entrega de las suscripciones:  
  
- Si el servicio se ejecuta con una cuenta local, se limitan las opciones posteriormente si configura una conexión a la base de datos de un servidor de informes remoto. En concreto, si usa la base de datos de un servidor de informes remoto, tendrá que configurar la conexión para usar una cuenta de usuario de dominio o un usuario de base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que tenga permiso para iniciar sesión en la instancia remota de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
- Si el servicio se ejecuta con una cuenta local, se imponen nuevos requisitos a la creación de suscripciones. El servidor de informes almacena información acerca del usuario que crea la suscripción. Si el usuario crea la suscripción tras iniciar sesión con una cuenta de dominio, el servicio Servidor de informes intenta conectarse a un controlador de dominio para autenticar al usuario cuando la suscripción se procese. Si el servicio se ejecuta con una cuenta local, se produce un error en la solicitud de autenticación cuando el servidor de informes intenta enviarla a un controlador de dominio remoto. Para subsanar esta limitación se puede usar una extensión de autenticación basada en formularios personalizada, o bien hacer que todos los usuarios se conecten a un servidor de informes con una cuenta de usuario local.  
  
- Si el servicio se ejecuta con una cuenta local, se imponen nuevos requisitos a la entrega de suscripciones. Algunas extensiones de entrega tienen información de cuenta de usuario en la definición de la suscripción. Si se envían informes a direcciones de correo electrónico que se basen en cuentas de usuario de dominio y se ejecuta el servicio Servidor de informes con una cuenta local, este no puede acceder a un controlador de dominio remoto para resolver la cuenta de correo electrónico de destino.  
  
- Las cuentas de servicio de Windows integradas (Servicio local o Servicio de red) no se admiten como cuentas de servicio del servidor de informes en un equipo que sea controlador de dominio.  
  
Las siguientes directrices y los vínculos de esta sección pueden ayudarle a elegir la solución que mejor se adapte a su implementación.  
  
- [Configuración de permisos y cuentas de servicio de Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
- [Guía de planeamiento de seguridad para servicios y cuentas de servicio](https://www.zubairalexander.com/blog/services-and-service-accounts-security-planning-guide/).  
  
## <a name="updating-an-expired-password"></a>Actualizar una contraseña que ha expirado

 Si el servicio Servidor de informes se ejecuta en una cuenta de dominio y la contraseña expira antes de que se pueda actualizar en el Administrador de configuración del servidor de informes, el servicio no se inicia hasta que se especifique una contraseña nueva.  
  
 Si la contraseña de la cuenta de servicio del [!INCLUDE[ssDE](../../includes/ssde-md.md)] expira, se producirá el error **rsReportServerDatabaseUnavailable** al intentar conectarse al servidor de informes. Restablecer la contraseña resuelve este error.  
  
## <a name="troubleshooting-service-identity-update-errors"></a>Solucionar los errores de actualización de la identidad de servicio

 Al cambiar la identidad del servicio, si usa la cuenta de servicio para conectarse a la base de datos del servidor de informes, se inicia una serie de eventos que incluyen el reinicio del servicio, la actualización de la clave de cifrado protegida mediante contraseña, la actualización de las reservas de direcciones URL y la actualización de la información de conexión de base de datos del servidor de informes. Puede supervisar el estado de estos eventos viendo las notificaciones del panel Resultados en la parte inferior de la página. Si se producen errores durante este proceso, puede intentar resolverlos utilizando las técnicas siguientes:  
  
- Si no se puede restaurar la clave simétrica, puede intentar restaurarla manualmente mediante la opción **Restaurar** de la página Claves de cifrado. Si eso no funciona, considere la posibilidad de eliminar el contenido cifrado. Tendrá que volver a crear la información de la conexión al origen de datos y las suscripciones, pero el resto del contenido sigue estando disponible. Para obtener más información, vea [Hacer copia de seguridad y restaurar claves de cifrado de Reporting Services](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md).  
  
- Si el servicio no se inicia, reinícielo manualmente con la aplicación de consola Servicios en Herramientas del administrador.  
  
- Pueden producirse errores de la reserva de direcciones URL al actualizar la cuenta de servicio. Cada reserva de direcciones URL incluye un descriptor de seguridad que incluye una Lista de control de acceso discrecional (DACL) que concede permisos a la cuenta de servicio para aceptar las solicitudes en la dirección URL. Al actualizar la cuenta, la dirección URL se debe volver a crear para actualizar la DACL con la información de la nueva cuenta. Si no se puede volver a crear la reserva de direcciones URL, y sabe que la cuenta es válida, intente reiniciar el equipo. Si el error persiste, intente utilizar una cuenta diferente.  
  
## <a name="next-steps"></a>Pasos siguientes

 [Configurar las direcciones URL del servidor de informes &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md) [Administrador de configuración del servidor de informes &#40;modo nativo&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
