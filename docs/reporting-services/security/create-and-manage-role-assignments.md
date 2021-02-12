---
description: Crear y administrar asignaciones de roles
title: Crear y administrar asignaciones de roles | Microsoft Docs
ms.date: 05/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- removing role assignments
- roles [Reporting Services], assignments
- security [Reporting Services], role assignments
- modifying role assignments
- deleting role assignments
ms.assetid: 086d0987-b43c-4834-8372-e08fb4b432f8
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: e1be919afaea2693329ae6fe957fb6fc5ff3357f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015205"
---
# <a name="create-and-manage-role-assignments"></a>Crear y administrar asignaciones de roles

Un *asignación de roles* es una directiva de seguridad que determina los permisos de un usuario o grupo. Los permisos deciden si el usuario o grupo puede acceder a un elemento específico del servidor de informes o modificarlo, así como realizar una tarea. Una asignación de roles consiste en un nombre de cuenta de usuario o de grupo y una o más definiciones de roles.

El ámbito de las asignaciones de roles es el *nivel de elemento* o el *nivel de sistema*.

- Una asignación de roles de nivel de elemento se crea para un elemento o rama específicos de la jerarquía de carpetas del servidor de informes. Navegue a una carpeta o un elemento específico para crear una asignación de roles para él.

- Las asignaciones de roles del sistema proporcionan a usuarios seleccionados la capacidad de realizar tareas que afectan en conjunto al sitio del servidor de informes. Entre las tareas, se incluyen las siguientes:
  - Crear programaciones compartidas
  - Administración de trabajos
  - Procesar informes
  - Configurar propiedades

La seguridad de nivel del sistema no otorga acceso a elementos en la jerarquía de carpetas del servidor de informes.

## <a name="creating-an-item-level-role-assignment"></a>Crear una asignación de roles de nivel de elemento

A partir de aquí, puede crear una asignación de roles independiente para cada cuenta de grupo o de usuario que requiera acceso al servidor de informes. Si la cuenta se encuentra en un dominio diferente del que contiene el servidor de informes, incluya el nombre de dominio. Después de especificar una cuenta, elija una o varias definiciones de roles. Las definiciones de roles son aditivas. La asignación admite el conjunto combinado de todas las tareas de todas las definiciones, para un usuario o grupo en particular.

Para permitir un acceso generalizado, elija un elemento en una posición alta de la jerarquía de carpetas (por ejemplo, la carpeta raíz Inicio). Después, puede crear las asignaciones de roles para restringir áreas específicas de la jerarquía de carpetas.

Debe ser miembro del grupo local Administradores del equipo del servidor de informes para crear una asignación de roles. Puede delegar esa responsabilidad asignando otros usuarios al rol **Administrador de contenido** .

Para crear o administrar asignaciones de roles, o para obtener más información, consulte [Conceder a un usuario acceso a un servidor de informes](../../reporting-services/security/grant-user-access-to-a-report-server.md).
  
## <a name="creating-a-system-level-role-assignment"></a>Crear una asignación de roles de nivel de sistema

Las asignaciones de roles de nivel de sistema y de nivel de elemento van juntas. Cree una asignación de roles del sistema para cada usuario o grupo que tenga una asignación de roles de nivel de elemento.

Las asignaciones de roles del sistema incluyen una amplia variedad de permisos, pero no incluyen los que forman parte de una asignación de roles de nivel de elemento.

A diferencia de los permisos del sistema de un equipo, los roles del sistema de los servidores de informes no otorgan permisos generales que incluyan todas las tareas posibles. En su lugar, las asignaciones de roles de nivel de sistema simplemente son un conjunto de tareas cuyo ámbito es el sitio del servidor de informes. Las asignaciones de roles del sistema determinan si los usuarios pueden ver las propiedades de la aplicación (como la imagen o el título de la página Inicio), ver o administrar las programaciones compartidas o usar el Generador de informes.

Para crear o administrar asignaciones de roles, o para obtener más información, consulte [Conceder a un usuario acceso a un servidor de informes](../../reporting-services/security/grant-user-access-to-a-report-server.md) o [Roles predefinidos](../../reporting-services/security/role-definitions-predefined-roles.md).  

## <a name="modifying-a-role-assignment"></a>Modificar una asignación de roles

Puede modificar las asignaciones de roles en cualquier momento. Los cambios surtirán efecto cuando guarde la asignación de roles. Las sesiones de usuarios no se ven afectadas por cambios en la asignación de roles. Si un usuario tiene un informe abierto y se modifica una asignación de roles para denegar el acceso, el usuario puede continuar usando el informe durante una sesión activa.

Si se agrega una cuenta de usuario a un grupo que ya forma parte de una asignación de roles, habrá un retardo antes de que la cuenta de usuario pueda obtener acceso a los elementos desde la modificación. Este retardo se debe a que Internet Information Services (IIS) de Microsoft almacena en caché los tokens de autenticación. También puede esperar a que se actualicen los tokens (tarea que suele tardar 15 minutos) o puede restablecer IIS para actualizar la caché de forma inmediata.

Solo puede modificar una asignación de roles cada vez. No puede llevar a cabo una operación de búsqueda y reemplazo global para cambiar nombres de definiciones de roles o configuraciones de asignaciones de roles, o bien para buscar todas las asignaciones de roles que incluyan un usuario o grupo específico.

## <a name="deleting-a-role-assignment"></a>Eliminar una asignación de roles

Puede eliminar asignaciones de roles activando la casilla de cada asignación que desee eliminar y haciendo clic en **Eliminar**. También puede eliminarlas haciendo clic en **Volver a la seguridad del elemento primario**. Cuando se selecciona este botón, las asignaciones de roles existentes para el elemento se eliminan y se reemplazan con las asignaciones heredadas del elemento primario.

## <a name="see-also"></a>Consulte también

[Concesión a un usuario de acceso a un servidor de informes](../../reporting-services/security/grant-user-access-to-a-report-server.md)  
[Asignaciones de roles](../../reporting-services/security/role-assignments.md)  
[Definiciones de roles](../../reporting-services/security/role-definitions.md)  
[Roles predefinidos](../../reporting-services/security/role-definitions-predefined-roles.md)  
[Concesión de permisos en un servidor de informes en modo nativo](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)
