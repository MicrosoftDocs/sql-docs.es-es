---
description: Información general sobre seguridad (Integration Services)
title: Información general sobre seguridad (Integration Services) | Microsoft Docs
ms.custom: security
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- SSIS packages, security
- Integration Services, security
- security [Integration Services], about security
- passwords [Integration Services]
- packages [Integration Services], security
- SQL Server Integration Services, security
- SSIS, security
- Integration Services packages, security
- SQL Server Integration Services packages, security
ms.assetid: 01aa0b88-d477-4581-9a3b-2efc3de2b133
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6b757c7c0a60ab8cff5c496c2a0774646da4c7f8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340777"
---
# <a name="security-overview-integration-services"></a>Información general sobre seguridad (Integration Services)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La seguridad en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] consta de varios niveles que proporcionan un entorno de seguridad sofisticado y flexible. Estos niveles de seguridad incluyen el uso de firmas digitales, propiedades de paquete, roles de la base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y permisos del sistema operativo. La mayoría de estas características de seguridad pertenecen a las categorías de identidad y control de acceso.  

## <a name="threat-and-vulnerability-mitigation"></a>Mitigar amenazas y vulnerabilidades
  Aunque [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] incluye diversos mecanismos de seguridad, los paquetes y los archivos que los paquetes crean o usan se pueden utilizar con fines malintencionados.  
  
 En la tabla siguiente se describen estos riesgos y los pasos proactivos que se pueden dar para reducirlos.  
  
|Amenaza o vulnerabilidad|Definición|Mitigación|  
|-----------------------------|----------------|----------------|  
|Origen del paquete|El origen de un paquete es el individuo o la organización que creó el paquete. La ejecución de un paquete desde un origen desconocido o que no sea de confianza puede ser arriesgado.|Identifique el origen de un paquete utilizando una firma digital y solo ejecute paquetes que procedan de orígenes conocidos y de confianza. Para más información, vea [Identificar el origen de paquetes con firmas digitales](../../integration-services/security/identify-the-source-of-packages-with-digital-signatures.md).|  
|Contenido del paquete|El contenido del paquete incluye los elementos del paquete y sus propiedades. Las propiedades pueden contener datos confidenciales, como una contraseña o una cadena de conexión. Los elementos del paquete tales como una instrucción SQL pueden revelar la estructura de la base de datos.|Controle el acceso a un paquete y al contenido del mismo realizando los pasos siguientes:<br /><br /> 1) Para controlar el acceso al propio paquete, aplique las características de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a los paquetes guardados en la base de datos **msdb** en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. A los paquetes que están guardados en el sistema de archivos, aplique características de seguridad del sistema de archivos, como por ejemplo listas de control de acceso (ACL).<br /><br /> 2) Para controlar el acceso al contenido del paquete, establezca el nivel de protección del paquete.<br /><br /> Para más información, vea [Información general sobre seguridad &#40;Integration Services&#41;](../../integration-services/security/security-overview-integration-services.md) y [Control del acceso a la información confidencial en paquetes](../../integration-services/security/access-control-for-sensitive-data-in-packages.md).|  
|Salida del paquete|Cuando se configura un paquete para utilizar configuraciones, puntos de comprobación y registro, el paquete almacena esta información fuera del paquete. La información que se almacena fuera del paquete puede contener datos confidenciales.|Para proteger las configuraciones y los registros que el paquete guarda en las tablas de base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , utilice las características de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> Para controlar el acceso a los archivos, utilice las listas de control de acceso (ACL) disponibles en el sistema de archivos.<br /><br /> Para más información, vea [Acceso a los archivos usados por los paquetes](#files)|  
  
## <a name="identity-features"></a>Características de identidad  
 Si implementa características de identidad en los paquetes, puede lograr el objetivo siguiente:  
  
 **Asegurarse de que solo se abren y se ejecutan paquetes de orígenes de confianza**.  
  
 Para asegurarse de que solamente se abren y se ejecutan paquetes de orígenes de confianza, primero debe identificar el origen de los paquetes. Para ello, firme los paquetes con certificados. De esta forma, al abrir o ejecutar los paquetes, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] puede comprobar la presencia y la validez de las firmas digitales. Para más información, vea [Identificar el origen de paquetes con firmas digitales](../../integration-services/security/identify-the-source-of-packages-with-digital-signatures.md).  
  
## <a name="access-control-features"></a>Características de control de acceso  
 Si implementa características de identidad en los paquetes, puede lograr el objetivo siguiente:  
  
 **Asegurarse de que solo los usuarios autorizados abren y ejecutan paquetes**.  
  
 Para asegurarse de que solo los usuarios autorizados abren y ejecutan paquetes, debe controlar el acceso a la información siguiente:  
  
-   El contenido de los paquetes, sobre todo si son datos confidenciales.  
  
-   Los paquetes y las configuraciones de paquetes almacenadas en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Los paquetes y los archivos relacionados, como configuraciones, registros y archivos de punto de comprobación, almacenados en el sistema de archivos.  
  
-   El servicio de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] y la información sobre paquetes que muestra dicho servicio en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
### <a name="controlling-access-to-the-contents-of-packages"></a>Controlar el acceso al contenido de los paquetes  
 Para ayudar a restringir el acceso al contenido de un paquete, puede cifrar el paquete estableciendo su propiedad ProtectionLevel. Puede establecer esta propiedad en el nivel de protección que necesita el paquete. Por ejemplo, en un entorno de desarrollo de equipo, puede cifrar un paquete con una contraseña que solo conozcan los miembros del equipo que trabajan en el paquete.  
  
 Cuando se establece la propiedad ProtectionLevel de un paquete, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] detecta automáticamente las propiedades confidenciales y las trata de acuerdo con el nivel de protección especificado para el paquete. Por ejemplo, la propiedad ProtectionLevel de un paquete se establece en un nivel que cifra la información confidencial mediante una contraseña. En este paquete, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] cifra automáticamente los valores de todas las propiedades confidenciales y no mostrará los datos correspondientes a no ser que se proporcione la contraseña correcta.  
  
 Normalmente, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] identifica las propiedades como confidenciales si estas contienen información, como una contraseña o una cadena de conexión, o si corresponden a variables o nodos XML generados por tareas. El que [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] considere una propiedad como confidencial dependerá de si el programador del componente de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , como un administrador de conexiones o una tarea, ha designado la propiedad como tal. Los usuarios no pueden agregar propiedades a la lista de propiedades consideradas como confidenciales, ni tampoco quitarlas de dicha lista. Si escribe tareas, administradores de conexión o componentes de flujo de datos personalizados, puede especificar qué propiedades debe tratar [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] como confidenciales.  
  
 Para más información, consulte [Access Control for Sensitive Data in Packages](../../integration-services/security/access-control-for-sensitive-data-in-packages.md).  
  
### <a name="controlling-access-to-packages"></a>Controlar el acceso a los paquetes  
 En una instancia de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , los paquetes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]se pueden guardar en la base de datos msdb o en el sistema de archivos como archivos XML con la extensión de nombre de archivo .dtsx. Para más información, vea [Guardar paquetes](../../integration-services/save-packages.md).  
  
#### <a name="saving-packages-to-the-msdb-database"></a>Guardar paquetes en la base de datos msdb  
 Guardar los paquetes en la base de datos msdb proporciona seguridad en los siguientes niveles: servidor, base de datos y tabla. En la base de datos msdb, los paquetes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] se almacenan en la tabla sysssispackages. Dado que los paquetes se guardan en las tablas sysssispackages y sysdtspackages de la base de datos msdb, se realiza una copia de seguridad de dichos paquetes de forma automática al crear una copia de seguridad de la base de datos msdb.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] los paquetes almacenados en la base de datos msdb también se pueden proteger aplicando los roles de nivel de base de datos [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] incluye tres roles fijos de nivel de base de datos db_ssisadmin, db_ssisltduser y db_ssisoperator para controlar el acceso a los paquetes. Se puede asociar un rol de lector y de escritor a cada paquete. También se pueden definir roles personalizados de nivel de base de datos para usarlos en los paquetes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Los roles solo se pueden implementar en los paquetes que se guardan en la base de datos msdb en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información, vea [Roles de Integration Services &#40;servicio SSIS&#41;](../../integration-services/security/integration-services-roles-ssis-service.md).  
  
#### <a name="saving-packages-to-the-file-system"></a>Guardar paquetes en el sistema de archivos  
 Si almacena los paquetes en el sistema de archivos en lugar de en la base de datos msdb, no olvide asegurar los archivos de los paquetes y las carpetas que contienen los archivos de los paquetes.  
  
### <a name="controlling-access-to-files-used-by-packages"></a>Controlar el acceso a los archivos usados por los paquetes  
 Los paquetes configurados para usar configuraciones, puntos de comprobación y registros generan información que se almacena fuera del paquete. Esta información puede ser confidencial y debe protegerse. Los archivos de puntos de comprobación únicamente se pueden guardar en el sistema de archivos, pero las configuraciones y los registros se pueden guardar en el sistema de archivos o en tablas de una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Las configuraciones y los registros que se guardan en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] están sujetos a la configuración de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , pero la información guardada en el sistema de archivos requiere una seguridad adicional.  
  
 Para más información, vea [Acceso a los archivos usados por los paquetes](#files).  
  
#### <a name="storing-package-configurations-securely"></a>Almacenar configuraciones de paquetes de forma segura  
 Las configuraciones de paquetes se pueden guardar en una tabla de una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o en el sistema de archivos.  
  
 Las configuraciones se pueden guardar en cualquier base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , no solo en la base de datos msdb. Por lo tanto, puede especificar qué base de datos actúa como repositorio de las configuraciones de paquetes. También puede especificar el nombre de la tabla que contendrá las configuraciones; [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] creará la tabla de forma automática con la estructura correcta. Guardar las configuraciones en una tabla permite proporcionar seguridad en tres niveles: servidor, base de datos y tabla. Además, las configuraciones guardadas en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se agregan automáticamente a la copia de seguridad al crear una copia de seguridad de la base de datos.  
  
 Si almacena las configuraciones en el sistema de archivos en lugar de en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], no olvide asegurar las carpetas que contienen los archivos de configuración de paquetes.  
  
 Para obtener más información acerca de las configuraciones, vea [Package Configurations](../packages/legacy-package-deployment-ssis.md).  
  
### <a name="controlling-access-to-the-integration-services-service"></a>Controlar el acceso al servicio Integration Services  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] usa el servicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para mostrar los paquetes almacenados. Restrinja el acceso a los equipos que ejecutan el servicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para evitar que los usuarios no autorizados tengan acceso a la información sobre los paquetes almacenados en equipos locales y remotos, y obtengan información privada.  
  
 Para más información, vea [Acceso al servicio Integration Services](#service).  

## <a name="access-to-files-used-by-packages"></a><a name="files"></a> Acceso a los archivos usados por los paquetes
  El nivel de protección de paquetes no protege los archivos almacenados fuera del paquete. Entre los archivos figuran los siguientes:  
  
-   Archivos de configuración  
  
-   punto de comprobación, archivos  
  
-   Archivos de registro  
  
 Estos archivos se deben proteger por separado, especialmente si incluyen información confidencial.  
  
### <a name="configuration-files"></a>Archivos de configuración  
 Si una configuración contiene información confidencial, como información de inicio de sesión o de la contraseña, puede guardar la configuración en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]o usar una lista de control de acceso (ACL) para restringir el acceso a la ubicación o carpeta en la que ha almacenado los archivos y para permitir el acceso solo a determinadas cuentas. Por lo general, otorgará acceso a las cuentas a las que permite ejecutar paquetes, y a las cuentas que administran paquetes y que solucionan problemas de paquetes, que pueden incluir la revisión del contenido de archivos de configuración, de punto de comprobación y de registro. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proporciona el almacenamiento más seguro dado que ofrece protección en los niveles de servidor y base de datos. Para guardar configuraciones en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], utilice el tipo de configuración [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para guardar configuraciones en el sistema de archivos, utilice el tipo de configuración XML.  
  
 Para más información, vea [Configuraciones de paquetes](../packages/legacy-package-deployment-ssis.md), [Crear configuraciones de paquetes](../packages/legacy-package-deployment-ssis.md)y [Consideraciones de seguridad para una instalación de SQL Server](../../sql-server/install/security-considerations-for-a-sql-server-installation.md).  
  
### <a name="checkpoint-files"></a>punto de comprobación, archivos  
 De igual modo, si el archivo de punto de comprobación que utiliza el paquete incluye información confidencial, debe utilizar una lista de control de acceso (ACL) para asegurar la ubicación o carpeta en la que ha almacenado el archivo. Los archivos de punto de comprobación guardan la información de estado actual acerca del progreso del paquete y los valores actuales de las variables. Por ejemplo, el paquete puede incluir una variable personalizada que contiene un número de teléfono. Para obtener más información, vea [Restart Packages by Using Checkpoints](../../integration-services/packages/restart-packages-by-using-checkpoints.md).  
  
### <a name="log-files"></a>Archivos de registro  
 También es necesario proteger las entradas del registro escritas en el sistema de archivos mediante una lista de control de acceso (ACL). Las entradas del registro pueden almacenarse también en tablas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y protegerse con la seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Las entradas del registro pueden incluir información confidencial. Por ejemplo, si el paquete contiene una tarea Ejecutar SQL que crea una instrucción SQL que hace referencia a un número de teléfono, la entrada del registro de la instrucción SQL incluye el número de teléfono. La instrucción SQL puede revelar información privada acerca de los nombres de tablas y columnas de las bases de datos. Para obtener más información, vea [Registro de Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  

## <a name="access-to-the-integration-services-service"></a><a name="service"></a> Acceso al servicio Integration Services
  Los niveles de protección de paquetes pueden limitar quién puede editar y ejecutar un paquete. Es necesaria protección adicional para limitar quién puede ver la lista de paquetes actualmente en ejecución en un servidor y quién puede detener dicha ejecución en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] utiliza el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para mostrar los paquetes que se están ejecutando. Los miembros del grupo Administradores de Windows pueden ver y detener todos los paquetes en ejecución. Los usuarios que no sean miembros del grupo Administradores solo pueden ver y detener los paquetes que han iniciado ellos mismos.  
  
 Es importante restringir el acceso a los equipos que ejecutan un servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , especialmente un servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que puede mostrar carpetas remotas. Cualquier usuario autenticado puede solicitar la enumeración de los paquetes. Aunque el servicio no encuentre el servicio, el servicio enumera las carpetas. Estos nombres de carpeta pueden ser útiles para un usuario malintencionado. Si un administrador configura el servicio para que enumere las carpetas en un equipo remoto, los usuarios también podrán ver los nombres de las carpetas que normalmente no podrían ver.  

## <a name="related-tasks"></a>Related Tasks  
 La lista siguiente contiene vínculos a temas que muestran cómo realizar una determinada tarea relacionada con la seguridad.  
  
-   [Crear un rol definido por el usuario](../../integration-services/security/integration-services-roles-ssis-service.md#create)  
  
-   [Asignar roles de lector y escritor a un paquete](../../integration-services/security/integration-services-roles-ssis-service.md#assign)  
  
-   [Implementar una directiva de firma estableciendo un valor del Registro](../../integration-services/security/identify-the-source-of-packages-with-digital-signatures.md#registry)  
  
-   [Firmar un paquete mediante un certificado digital](../../integration-services/security/identify-the-source-of-packages-with-digital-signatures.md#cert)  
  
-   [Establecer o cambiar el nivel de protección de los paquetes](../../integration-services/security/access-control-for-sensitive-data-in-packages.md#set_protection)