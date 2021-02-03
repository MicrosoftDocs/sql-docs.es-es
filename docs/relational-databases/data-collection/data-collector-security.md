---
description: Seguridad del recopilador de datos
title: Seguridad del recopilador de datos | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- data collection [SQL Server]
- security [data collector]
- data collector [SQL Server], security
ms.assetid: e75d6975-641e-440a-a642-cb39a583359a
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a5e968978dd641f78dfb17d515ab07e3b7e27485
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250161"
---
# <a name="data-collector-security"></a>Seguridad del recopilador de datos
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  El recopilador de datos utiliza el modelo de seguridad basada en roles implementado por el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Este modelo permite al administrador de bases de datos ejecutar las distintas tareas de recopilador de datos en un contexto de seguridad que solo tiene los permisos necesarios realizar dicha tarea. Este enfoque también se utiliza para operaciones que implican tablas internas, a las que solamente se puede tener acceso utilizando un procedimiento almacenado o vista. No se concede ningún permiso a las tablas internas. En lugar de ello, los permisos se comprueban en el usuario del procedimiento almacenado o vista que se utiliza para tener acceso a una tabla.  
  
> [!IMPORTANT]  
>  Otro aspecto clave de este modelo de seguridad son los permisos concéntricos. Con los permisos concéntricos, los roles con más privilegios heredan los permisos de los roles con menos privilegios sobre los objetos (incluidos alertas, operadores, trabajos, programaciones y servidores proxy). Para más información, consulte [SQL Server Agent Fixed Database Roles](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 En las secciones siguientes se describe la seguridad de la recopilación de datos en general, así como los roles que debe conceder a los usuarios para que puedan configurar y utilizar el recopilador de datos y llevar a cabo tareas asociadas al almacén de administración de datos.  
  
## <a name="general-security"></a>Seguridad general  
 El recopilador de datos se instala según los estándares especificados para [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
### <a name="network-security"></a>Seguridad de redes  
 Se puede pasar información confidencial entre las instancias de destino, la instancia relacional asociada al servidor de configuración, los conjuntos de recopilación que se están ejecutando y el servidor que hospeda el almacén de administración de datos.  
  
 Para proteger los datos que se transfieren a través de una red, se implementan mecanismos de seguridad estándar, como el cifrado de protocolos para [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="permissions-for-configuring-and-using-the-data-collector"></a>Permisos para configurar y utilizar el recopilador de datos  
 En función de la tarea, los usuarios deben ser miembros de uno o varios de los roles fijos de base de datos que se proporcionan para el recopilador de datos. En orden de acceso con más privilegios a acceso con menos privilegios, los roles son las siguientes:  
  
-   **dc_admin**  
  
-   **dc_operator**  
  
-   **dc_proxy**  
  
 Estos roles se almacenan en la base de datos msdb. De manera predeterminada, ningún usuario es miembro de estos roles de base de datos. La pertenencia de los usuarios a estos roles se debe conceder explícitamente.  
  
 Los usuarios que son miembros del rol fijo de servidor **sysadmin** tienen acceso total a los objetos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y a las vistas del recopilador de datos. No obstante, deben agregarse explícitamente a los roles del recopilador de datos.  
  
> [!IMPORTANT]  
>  Los miembros del rol db_ssisadmin y del rol dc_admin quizá puedan elevar sus privilegios a sysadmin. Esta elevación de privilegio se puede producir porque estos roles pueden modificar los paquetes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] y los paquetes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] los puede ejecutar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizando el contexto de seguridad de sysadmin del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para protegerse contra esta elevación de privilegio al ejecutar planes de mantenimiento, conjuntos de recopilación de datos y otros paquetes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , configure los trabajos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que ejecutan paquetes para utilizar una cuenta de proxy con privilegios limitados o agregar solo los miembros de sysadmin a los roles dc_admin y db_ssisadmin.  
  
### <a name="dc_admin-role"></a>Rol dc_admin  
 Los usuarios asignados al rol **dc_admin** tienen acceso de administrador completo (crear, leer, actualizar y eliminar) a la configuración del recopilador de datos de una instancia del servidor. Los miembros de este rol pueden realizar las operaciones siguientes:  
  
-   Establecer propiedades de nivel del recopilador.  
  
-   Agregar nuevos conjuntos de recopilación.  
  
-   Instalar nuevos tipos de recopilación.  
  
-   Realizar todas las operaciones permitidas al rol **dc_operator** .  
  
 El rol **dc_admin** es miembro de los roles siguientes:  
  
-   **SQLAgentUserRole**. Este rol es necesario para crear programaciones y ejecutar trabajos.  
  
    > [!NOTE]  
    >  Los servidores proxy creados para el recopilador de datos deben conceder acceso a **dc_admin** para crearlos y usarlos en los pasos de trabajo que exijan un servidor proxy.  
  
-   **dc_operator**. Los miembros de la función **dc_admin** heredan los permisos concedidos a **dc_operator**.  
  
### <a name="dc_operator-role"></a>Rol dc_operator  
 Los miembros del rol **dc_operator** tienen acceso de lectura y actualización. Este rol admite tareas de operaciones relacionadas con la ejecución y configuración de conjuntos de recopilación. Los miembros de este rol pueden realizar las operaciones siguientes:  
  
-   Iniciar o detener un conjunto de recopilación.  
  
-   Enumerar conjuntos de recopilación existentes.  
  
-   Ver la información detallada (por ejemplo, elementos de recopilación y frecuencia de recopilación) asociada a un conjunto de recopilación.  
  
-   Cambiar la frecuencia de la carga para los conjuntos de recopilación existentes.  
  
-   Cambiar la frecuencia de recopilación de los elementos de recopilación que forman parte de un conjunto de recopilación existente.  
  
 El rol **dc_operator** es miembro de los roles siguientes que son necesarios para enumerar y ver paquetes del recopilador de datos:  
  
-   **db_ssisltduser**  
  
-   **db_ssisoperator**  
  
 Para obtener más información, vea [Roles de Integration Services &#40;servicio SSIS&#41;](../../integration-services/security/integration-services-roles-ssis-service.md).  
  
### <a name="dc_proxy-role"></a>Rol dc_proxy  
 Los miembros del rol **dc_proxy** tienen acceso de lectura a los conjuntos de recopilación y a las propiedades de nivel de recopilador. Los miembros de este rol también pueden ejecutar los trabajos que les pertenecen, así como crear pasos de trabajos que se ejecuten como una cuenta de proxy existente.  
  
 Los miembros de este rol pueden realizar las operaciones siguientes:  
  
-   Ver la información de configuración de conjuntos de recopilación (por ejemplo, parámetros de entrada para los elementos de recopilación, y la frecuencia de recopilación de estos elementos).  
  
-   Obtener información cifrada interna a la que solo se puede tener acceso por un procedimiento almacenado firmado (por ejemplo, información de conexión de almacenamiento de datos utilizada para las cargas de datos).  
  
-   Registrar eventos en tiempo de ejecución de los conjuntos de recopilación.  
  
 El rol **dc_proxy** es miembro de los roles siguientes que son necesarios para enumerar y ver paquetes del recopilador de datos:  
  
-   **db_ssisltduser**.  
  
-   **db_ssisoperator**  
  
 Para obtener más información, vea [Roles de Integration Services &#40;servicio SSIS&#41;](../../integration-services/security/integration-services-roles-ssis-service.md).  
  
## <a name="permissions-for-configuring-and-using-the-management-data-warehouse"></a>Permisos para configurar y utilizar el almacén de administración de datos  
 En función de la tarea, los usuarios deben ser miembros de uno o varios de los roles fijos de base de datos que se proporcionan para tener acceso al almacén de administración de datos. En orden de acceso con más privilegios a acceso con menos privilegios, los roles son las siguientes:  
  
-   **mdw_admin**  
  
-   **mdw_writer**  
  
-   **mdw_reader**  
  
 Estos roles se almacenan en la base de datos msdb. De manera predeterminada, ningún usuario es miembro de estos roles de base de datos. La pertenencia de los usuarios a estos roles se debe conceder explícitamente.  
  
 Los usuarios que son miembros del rol fijo de servidor **sysadmin** tienen acceso total a las vistas del recopilador de datos. No obstante, deben agregarse explícitamente a los roles de la base de datos para realizar otras operaciones.  
  
### <a name="mdw_admin-role"></a>Rol mdw_admin  
 Los miembros del rol **mdw_admin** tienen acceso de lectura, escritura, actualización y eliminación al almacén de administración de datos.  
  
 Los miembros de este rol pueden realizar las operaciones siguientes:  
  
-   Cambiar el esquema del almacén de administración de datos cuando se requiere (por ejemplo, agregando una nueva tabla cuando se instala un nuevo tipo de recopilador).  
  
    > [!NOTE]  
    >  Si hay un cambio del esquema, el usuario también debe ser miembro del rol **dc_admin** para instalar un nuevo tipo de recopilador, puesto que esta acción exige permiso para actualizar la configuración del recopilador de datos en msdb.  
  
-   Ejecutar trabajos de mantenimiento en el almacén de administración de datos, por ejemplo archivado o limpieza.  
  
### <a name="mdw_writer-role"></a>Rol mdw_writer  
 Los miembros del rol **mdw_writer** pueden cargar y escribir datos en el almacén de administración de datos. Cualquier recopilador de datos que almacene datos en el almacén de administración de datos tiene que ser miembro de este rol.  
  
### <a name="mdw_reader-role"></a>Rol mdw_reader  
 Los miembros del rol **mdw_reader** tienen acceso de lectura al almacén de administración de datos. Dado que la finalidad de este rol es permitir solucionar problemas proporcionando acceso a datos históricos, los miembros de este rol no pueden ver otros elementos del esquema de almacén de administración de datos.  
  
## <a name="see-also"></a>Consulte también  
 [Implementar la seguridad del Agente SQL Server](../../ssms/agent/implement-sql-server-agent-security.md)  
  
  
