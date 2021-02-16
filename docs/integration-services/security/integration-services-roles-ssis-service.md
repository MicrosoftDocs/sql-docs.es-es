---
description: Roles de Integration Services (servicio SSIS)
title: Roles de Integration Services (servicio SSIS) | Microsoft Docs
ms.custom: security
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.dtsserver.packageroles.f1
helpviewer_keywords:
- security [Integration Services], roles
- db_ssisoperator role
- db_ssisadmin role
- fixed database roles [Integration Services]
- packages [Integration Services], security
- roles [Integration Services]
- db_ssisltduser role
ms.assetid: 9702e90c-fada-4978-a473-1b1423017d80
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d8801c1a4f7387f1ff5bc815b45897d5a4f68fdb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340786"
---
# <a name="integration-services-roles-ssis-service"></a>Roles de Integration Services (servicio SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] proporciona determinados roles fijos a nivel de base de datos para ayudar a proteger el acceso a los paquetes almacenados en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los roles disponibles son diferentes, según si va a guardar paquetes en la base de datos de catálogo de SSIS (SSISDB) o en la base de datos msdb.  
  
## <a name="roles-in-the-ssis-catalog-database-ssisdb"></a>Roles de la base de datos de catálogo de SSIS (SSISDB)  
 La base de datos de catálogo de SSIS (SSISDB) proporciona los siguientes roles fijos a nivel de base de datos para ayudar a proteger el acceso a paquetes y a la información sobre ellos.  
  
-   **ssis_admin**. Este rol proporciona acceso administrativo completo a la base de datos de catálogo de SSIS.  
  
-   **ssis_logreader** Este rol proporciona permisos para acceder a todas las vistas relacionadas con registros operativos de SSISDB.  
  
     La lista de vistas incluye: [catalog].[projects], [catalog].[packages], [catalog].[operations], [catalog].[extended_operation_info], [catalog].[operation_messages], [catalog].[event_messages], [catalog].[execution_data_statistics], [catalog].[execution_component_phases], [catalog].[execution_data_taps], [catalog].[event_message_context], [catalog].[executions], [catalog].[executables], [catalog].[executable_statistics], [catalog].[validations], [catalog].[execution_parameter_values] y [catalog].[execution_property_override_values].  
  
## <a name="roles-in-the-msdb-database"></a>Roles en la base de datos msdb  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] incluye los tres roles fijos de nivel de base de datos, **db_ssisadmin**, **db_ssisltduser** y **db_ssisoperator**, para controlar el acceso a los paquetes almacenados en la base de datos **msdb**. Para asignar roles a un paquete se utiliza [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Las asignaciones de roles se guardan en la base de datos **msdb** .  
  
### <a name="read-and-write-actions"></a>Acciones de lectura y escritura  
 En la tabla siguiente se describen las acciones de lectura y escritura de Windows y los roles fijos de nivel de base de datos en [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
|Rol|Acción de lectura|Acción de escritura|  
|----------|-----------------|------------------|  
|**db_ssisadmin**<br /><br /> o bien<br /><br /> **sysadmin**|Enumerar los paquetes propios.<br /><br /> Enumerar todos los paquetes.<br /><br /> Ver los paquetes propios.<br /><br /> Ver todos los paquetes.<br /><br /> Ejecutar los paquetes propios.<br /><br /> Ejecutar todos los paquetes.<br /><br /> Exportar los paquetes propios.<br /><br /> Exportar todos los paquetes.<br /><br /> Ejecutar todos los paquetes del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|Importar paquetes.<br /><br /> Eliminar los paquetes propios.<br /><br /> Eliminar todos los paquetes.<br /><br /> Cambiar los roles de los paquetes propios.<br /><br /> Cambiar los roles de todos los paquetes.<br /><br /> <br /><br /> **\*\* Advertencia \*\*** Los miembros del rol db_ssisadmin y del rol dc_admin quizá puedan elevar sus privilegios a sysadmin. Esta elevación de privilegio se puede producir porque estos roles pueden modificar los paquetes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] y los paquetes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] los puede ejecutar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizando el contexto de seguridad de sysadmin del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para protegerse contra esta elevación de privilegio al ejecutar planes de mantenimiento, conjuntos de recopilación de datos y otros paquetes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , configure los trabajos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que ejecutan paquetes para utilizar una cuenta de proxy con privilegios limitados o agregar solo los miembros de sysadmin a los roles dc_admin y db_ssisadmin.|  
|**db_ssisltduser**|Enumerar los paquetes propios.<br /><br /> Enumerar todos los paquetes.<br /><br /> Ver los paquetes propios.<br /><br /> Ejecutar los paquetes propios.<br /><br /> Exportar los paquetes propios.|Importar paquetes.<br /><br /> Eliminar los paquetes propios.<br /><br /> Cambiar los roles de los paquetes propios.|  
|**db_ssisoperator**|Enumerar todos los paquetes.<br /><br /> Ver todos los paquetes.<br /><br /> Ejecutar todos los paquetes.<br /><br /> Exportar todos los paquetes.<br /><br /> Ejecutar todos los paquetes del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|Ninguno|  
|**Administradores de Windows**|Ver los detalles de ejecución de todos los paquetes que se están ejecutando.|Detener todos los paquetes en ejecución en ese momento.|  
  
### <a name="sysssispackages-table"></a>Tabla Sysssispackages  
 La tabla **sysssispackages** en **msdb** contiene los paquetes que están guardados en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información, vea [sysssispackages &#40;Transact-SQL&#41;](../../relational-databases/system-tables/sysssispackages-transact-sql.md).  
  
 La tabla **sysssispackages** incluye columnas que contienen información sobre los roles asignados a los paquetes.  
  
-   La columna **readerrole** especifica el rol que tiene acceso de lectura al paquete.  
  
-   La columna **writerrole** especifica el rol que tiene acceso de escritura al paquete.  
  
-   La columna **ownersid** contiene el identificador de seguridad único del usuario que creó el paquete. Esta columna define el propietario del paquete.  
  
### <a name="permissions"></a>Permisos  
 De forma predeterminada, los permisos de los roles fijos de nivel de base de datos **db_ssisadmin** y **db_ssisoperator** y el identificador de seguridad único del usuario que ha creado el paquete se aplican al rol de lector para paquetes, y los permisos del rol **db_ssisadmin** y el identificador de seguridad único del usuario que ha creado el paquete se aplican al rol de escritor. Un usuario debe ser miembro del rol **db_ssisadmin**, **db_ssisltduser** o **db_ssisoperator** para tener acceso de lectura al paquete. Un usuario debe ser miembro del rol **db_ssisadmin** para tener acceso de escritura.  
  
### <a name="access-to-packages"></a>Acceso a paquetes  
 Los roles fijos de nivel de base de datos trabajan conjuntamente con los roles definidos por el usuario. Los roles definidos por el usuario son los que se crean en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] y posteriormente se usan para asignar permisos a los paquetes. Para tener acceso a un paquete, un usuario debe ser miembro del rol definido por el usuario y del rol fijo de nivel de base de datos [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] pertinente. Por ejemplo, si los usuarios son miembros del rol definido por el usuario **AuditUsers** asignado a un paquete, también deberán ser miembros del rol **db_ssisadmin**, **db_ssisltduser** o **db_ssisoperator** para tener acceso de lectura al paquete.  
  
 Si no asigna roles definidos por el usuario a los paquetes, el acceso a dichos paquetes se determina con los roles fijos de nivel de base de datos.  
  
 Si quiere usar roles definidos por el usuario, debe agregarlos a la base de datos **msdb** antes de asignarlos a los paquetes. Puede crear roles de base de datos en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 Los roles de nivel de base de datos de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] conceden derechos sobre las tablas del sistema de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en la base de datos msdb.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe iniciarse (el servicio MSSQLSERVER) antes de conectarse al motor de base de datos y tener acceso a la base de datos **msdb** .  
  
 Para asignar roles a paquetes, debe completar las tareas siguientes.  
  
-   **Abrir el Explorador de objetos y conectarse a Integration Services**  
  
     Antes de asignar roles a los paquetes con [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], debe abrir el Explorador de objetos en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] y conectarse a [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
     Debe iniciar el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] antes de conectarse a [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
-   **Asignar roles de lector y escritor a los paquetes**  
  
     Puede asignar un rol de lector y un rol de escritor a cada paquete.  

## <a name="assign-a-reader-and-writer-role-to-a-package"></a><a name="assign"></a> Asignar roles de lector y escritor a un paquete
  Puede asignar un rol de lector y un rol de escritor a cada paquete.  
  
### <a name="assign-a-reader-and-writer-role-to-a-package"></a>Asignar roles de lector y escritor a un paquete  
  
1.  En el Explorador de objetos, localice la conexión a [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
2.  Expanda la carpeta Paquetes almacenados y, a continuación, la subcarpeta que contiene el paquete al que desea asignar roles.  
  
3.  Haga clic con el botón secundario en el paquete al que desea asignar roles.  
  
4.  En el cuadro de diálogo **Roles de paquete** , seleccione un rol de lector de la lista **Rol de lector** y un rol de escritor de la lista **Rol de escritor** .  
  
5.  Haga clic en **OK**.

## <a name="create-a-user-defined-role"></a><a name="create"></a> Crear un rol definido por el usuario
    
### <a name="to-create-a-user-defined-role"></a>Para crear un rol definido por el usuario  
  
1.  Abra [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
2.  Haga clic en **Explorador de objetos** , en el menú **Ver** .  
  
3.  En la barra de herramientas del Explorador de objetos, haga clic en **Conectar** y, a continuación, en **Motor de base de datos**.  
  
4.  En el cuadro de diálogo **Conectar al servidor** , indique el nombre del servidor y seleccione un modo de autenticación. Puede usar un punto (.), (local) o **localhost** para indicar el servidor local.  
  
5.  Haga clic en **Conectar**.  
  
6.  Expanda Bases de datos, Bases de datos del sistema, msdb, Seguridad y Roles.  
  
7.  En el nodo Roles, haga clic con el botón derecho en Roles de base de datos y, luego, haga clic en **Nuevo rol de base de datos**.  
  
8.  En la página General, indique un nombre y si lo desea, especifique un propietario, esquemas de propiedad y agregue miembros de roles.  
  
9. Opcionalmente, haga clic en **Permisos** y configure permisos de objetos.  
  
10. Opcionalmente, haga clic en **Propiedades extendidas** y configure las propiedades extendidas.  
  
11. Haga clic en **OK**.

## <a name="package-roles-dialog-box-ui-reference"></a><a name="roles_dialog"></a> Referencia de la interfaz de usuario del cuadro de diálogo Roles del paquete
  Use el cuadro de diálogo **Roles de paquete** , disponible en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], para especificar los roles de base de datos que tienen acceso de lectura al paquete y los roles de base de datos que tienen acceso de escritura al paquete. Los roles de base de datos se aplican solo a paquetes almacenados en la base de datos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **msdb** database.  
  
 Los roles enumerados en el cuadro de diálogo son los roles de base de datos actuales de la base de datos del sistema **msdb** . Si no se seleccionan roles, se aplican los roles de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] predeterminados. De manera predeterminada, el rol de lector incluye **db_ssisadmin**, **db_ssisoperator** y el usuario que creó el paquete. Un usuario que sea miembro de uno de estos roles o que haya creado los paquetes puede enumerar, ver, exportar y ejecutar paquetes. De manera predeterminada, el rol de escritor incluye **db_ssisadmin** y el usuario que creó el paquete. Un usuario que sea miembro de este rol y el usuario que creó los paquetes pueden importar, eliminar y cambiar paquetes.  
  
 La columna **ownersid** de la tabla **sysssispackages** contiene el identificador de seguridad único del usuario que creó el paquete.  
  
### <a name="options"></a>Opciones  
 **Nombre del paquete**  
 Especifique el nombre del paquete.  
  
 **Rol de lector**  
 Seleccione un rol de la lista.  
  
 **Rol de escritor**  
 Seleccione un rol de la lista.  
