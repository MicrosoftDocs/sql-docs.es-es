---
title: Administración de una base de datos del servidor de informes (Modo nativo) | Microsoft Docs
description: Obtenga información sobre cómo administrar una implementación de Reporting Services, incluida la copia de seguridad y restauración de bases de datos del servidor de informes y la administración de claves de cifrado.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- report servers [Reporting Services], databases
- renaming databases
- report server database
- databases [Reporting Services], administering
- reportservertempdb
- reportserver database
ms.assetid: 97b2e1b5-3869-4766-97b9-9bf206b52262
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 2ae57de490fcb516b0014ea2484824028c4c1a38
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100022090"
---
# <a name="administer-a-report-server-database-ssrs-native-mode"></a>Administrar una base de datos del servidor de informes (Modo nativo de SSRS)
  Una implementación de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] usa dos bases de datos relacionales de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para almacenamiento interno. De manera predeterminada, las bases de datos tienen los nombres ReportServer y ReportServerTempdb. ReportServerTempdb se crea con la base de datos principal del servidor de informes y se usa para almacenar datos temporales, información de sesión e informes almacenados en caché.  
  
 En [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], entre las tareas de administración de bases de datos se incluyen la copia de seguridad y restauración de bases de datos del servidor de informes y la administración de las claves de cifrado que se usan para cifrar y descifrar datos confidenciales.  
  
 Para administrar las bases de datos del servidor de informes, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proporciona una amplia variedad de herramientas.  
  
-   Para realizar una copia de seguridad, restaurar, mover o recuperar una base de datos del servidor de informes, puede usar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], los comandos [!INCLUDE[tsql](../../includes/tsql-md.md)] o las utilidades de símbolo del sistema de la base de datos. Para obtener instrucciones, consulte [Mover las bases de datos del servidor de informes a otro equipo (Modo nativo de SSRS)](../../reporting-services/report-server/moving-the-report-server-databases-to-another-computer-ssrs-native-mode.md).  
  
-   Para copiar el contenido de una base de datos existente en otra base de datos del servidor de informes, puede adjuntar una copia de una base de datos del servidor de informes y utilizarla con una instancia distinta del servidor de informes. También puede crear y ejecutar un script que utilice llamadas SOAP para volver a crear contenido del servidor de informes en una nueva base de datos. Puede usar la utilidad **rs** para ejecutar el script.  
  
-   Para administrar las conexiones entre el servidor de informes y la base de datos del servidor de informes, y para averiguar qué base de datos se utiliza para una instancia concreta del servidor de informes, puede utilizarse la página Instalación de base de datos de la herramienta de configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Para más información sobre la conexión del servidor de informes a la base de datos del servidor de informes, vea [Configurar una conexión a la base de datos del servidor de informes &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).  
  
## <a name="sql-server-login-and-database-permissions"></a>Permisos de inicio de sesión y de base de datos de SQL Server  
 Las bases de datos del servidor de informes son utilizadas internamente por este. El servicio del servidor de informes realiza conexiones a cualquier base de datos. Puede usar la herramienta de configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para configurar la conexión del servidor de informes a la base de datos del servidor de informes.  
  
 Las credenciales para la conexión del servidor de informes a la base de datos pueden ser la cuenta de servicio, una cuenta de usuario de dominio o local de Windows o un usuario de base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Debe elegir una cuenta existente para la conexión, ya que [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] no crea cuentas por usted.  
  
 Automáticamente se creará para usted un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a la base de datos del servidor de informes para la cuenta especificada.  
  
 Los permisos a la base de datos también se configuran automáticamente. La herramienta de configuración de Reporting Services asignará la cuenta o usuario de la base de datos a los roles **Public** y **RSExecRole** para las bases de datos del servidor de informes. **RSExecRole** otorga permisos para tener acceso a las tablas de base de datos y para ejecutar procedimientos almacenados. El rol **RSExecRole** se crea en master y msdb al crear la base de datos del servidor de informes. **RSExecRole** es miembro del rol **db_owner** para las bases de datos del servidor de informes, lo que permite al servidor de informes actualizar su propio esquema que admite un proceso de actualización automática.  
  
## <a name="naming-conventions-for-the-report-server-databases"></a>Convenciones de nomenclatura para las bases de datos del servidor de informes  
 Cuando se crea la base de datos principal, su nombre debe seguir las reglas especificadas para los [Identificadores de bases de datos](../../relational-databases/databases/database-identifiers.md). El nombre de la base de datos temporal utiliza siempre el mismo nombre que la base de datos principal del servidor de informes pero con el sufijo Tempdb. No puede elegir un nombre diferente para la base de datos temporal.  
  
 No se permite cambiar el nombre de una base de datos del servidor de informes porque las bases de datos del servidor de informes se consideran componentes internos. Si se cambia el nombre de las bases de datos del servidor de informes, se producen errores. Específicamente, si se cambia el nombre de la base de datos principal, un mensaje de error explica que los nombres de las bases de datos no están sincronizados. Si cambia el nombre de la base de datos ReportServerTempdb, se produce el siguiente error interno más adelante al ejecutar informes:  
  
 "Error interno en el servidor de informes. Vea el registro de errores para obtener más detalles. (rsInternalError)  
  
 El nombre de objeto 'ReportServerTempDB.dbo.PersistedStream' no es válido."  
  
 Este error se debe a que el nombre ReportServerTempdb se almacena internamente y lo utilizan los procedimientos almacenados para realizar operaciones internas. Si se cambia el nombre de la base de datos temporal, no funcionarán correctamente los procedimientos almacenados.  
  
## <a name="enabling-snapshot-isolation-on-the-report-server-database"></a>Habilitar el aislamiento de instantánea en la base de datos del servidor de informes  
 No puede habilitar el aislamiento de instantánea en la base de datos del servidor de informes. Si está activado el aislamiento de instantánea, se producirá el siguiente error: "El informe seleccionado no está listo para su visualización. Aún se está representando o no hay disponible una instantánea de informe".  
  
 Si no habilitó explícitamente el aislamiento de instantánea, puede que el atributo lo estableciera otra aplicación o que la base de datos **modelo** tenga el aislamiento de instantánea habilitado, haciendo que todas las bases de datos nuevas heredaran el valor.  
  
 Para desactivar el aislamiento de instantánea en la base de datos del servidor de informes, inicie Management Studio, abra una nueva ventana de consulta y pegue y después ejecute el script siguiente:  
  
```  
ALTER DATABASE ReportServer  
SET ALLOW_SNAPSHOT_ISOLATION OFF  
ALTER DATABASE ReportServerTempdb  
SET ALLOW_SNAPSHOT_ISOLATION OFF  
ALTER DATABASE ReportServer  
SET READ_COMMITTED_SNAPSHOT OFF  
ALTER DATABASE ReportServerTempDb  
SET READ_COMMITTED_SNAPSHOT OFF  
```  
  
## <a name="about-database-versions"></a>Acerca de las versiones de base de datos  
 En [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]no se dispone de información explícita acerca de la versión de la base de datos. Sin embargo, como las versiones de la base de datos siempre están sincronizadas con las versiones de los productos, se puede utilizar la información de la versión del producto para saber cuándo ha cambiado la versión de la base de datos. La información de versión de producto de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] se indica a través de la información de versión de archivo que aparece en los archivos de registro, en los encabezados de todas las llamadas SOAP y al conectarse con la dirección URL del servidor de informes (por ejemplo, al abrir un explorador en `https://localhost/reportserver`).  
  
## <a name="see-also"></a>Consulte también  

 [Crear una base de datos del servidor de informes en modo nativo &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/ssrs-report-server-create-a-native-mode-report-server-database.md)   
 [Configurar la cuenta de servicio del servidor de informes &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md)   
 [Configurar una conexión de base de datos del servidor de informes &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md)   
 [Crear una base de datos del servidor de informes &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/ssrs-report-server-create-a-report-server-database.md)   
 [Administrador de configuración del servidor de informes &#40;modo nativo&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
 [Operaciones de copia de seguridad y restauración de Reporting Services](../../reporting-services/install-windows/backup-and-restore-operations-for-reporting-services.md)   
 [Base de datos del servidor de informes &#40;Modo nativo de SSRS&#41;](../../reporting-services/report-server/report-server-database-ssrs-native-mode.md)   
 [Servidor de informes de Reporting Services &#40;modo nativo&#41;](../../reporting-services/report-server/reporting-services-report-server-native-mode.md)   
 [Almacenar datos cifrados del servidor de informes &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-store-encrypted-report-server-data.md)   
 [Configurar y administrar claves de cifrado &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md)  
  
  
