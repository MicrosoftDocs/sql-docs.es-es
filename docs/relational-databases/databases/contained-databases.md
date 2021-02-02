---
title: Bases de datos independientes | Microsoft Docs
description: Obtenga información sobre bases de datos independientes, las ventajas y limitaciones de las bases de datos parcialmente independientes y cómo funciona SQL Server 2019 para aislar bases de datos.
ms.custom: ''
ms.date: 08/24/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- contained database
- database_uncontained_usage event
- partially contained database
- contained database, understanding
ms.assetid: 36af59d7-ce96-4a02-8598-ffdd78cdc948
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: cd8b60ec5b302183fdb1a0713e71568821dae2fe
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813096"
---
# <a name="contained-databases"></a>Bases de datos independientes
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Una *base de datos independiente* es una base de datos que está aislada de otras bases de datos y de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que hospeda la base de datos.  [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] ayuda al usuario a aislar su base de datos de la instancia de 4 maneras.  
  
-   Muchos de los metadatos que describen una base de datos se mantienen en la base de datos. (Además, o en lugar de, mantenerse los metadatos en la base de datos maestra).  
  
-   Todos los metadatos se definen utilizando la misma intercalación.  
  
-   La autenticación de usuario la puede realizar la base de datos, reduciendo la dependencia de las bases de datos en los inicios de sesión de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   El entorno de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (DMV, XEvents, etc.) informa y puede actuar en la información de contención.  
  
 Algunas características de las bases de datos parcialmente independientes, como el almacenamiento de la base de datos, se aplican a todas las bases de datos de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] . Algunas ventajas de las bases de datos parcialmente independientes, como la autenticación en el nivel de base de datos y la intercalación de catálogo, se deben habilitar para que estén disponibles. La contención parcial se habilita mediante las instrucciones **CREATE DATABASE** y **ALTER DATABASE** o mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Para obtener más información acerca de cómo habilitar la contención parcial de bases de datos, vea [Migrate to a Partially Contained Database](../../relational-databases/databases/migrate-to-a-partially-contained-database.md).  
  
##  <a name="partially-contained-database-concepts"></a><a name="Concepts"></a> Conceptos de las bases de datos parcialmente independientes  
 Una base de datos totalmente independiente incluye todos los metadatos y opciones de configuración necesarios para definirla y no tiene dependencias de configuración en la instancia de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] donde esté instalada. En versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la separación de una base de datos de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] podría necesitar mucho tiempo y un conocimiento detallado de la relación entre la base de datos y la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Las bases de datos parcialmente independientes facilitan la separación de una base de datos de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y de otras bases de datos.  
  
 La base de datos independiente considera las características en lo que se refiere a la contención. Cualquier entidad definida por el usuario que solo se base en las funciones que residen dentro de la base de datos se considera totalmente independiente. Cualquier entidad definida por el usuario que se base en funciones que residen fuera de la base de datos se considera dependiente. (Para obtener más información, vea la sección [Contención](#containment) más adelante en este tema).  
  
 Las siguientes condiciones se aplican al modelo de base de datos independiente.  
  
 Límite de la base de datos  
 El límite entre el modelo una base de datos y la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El límite entre una base de datos y otras bases de datos.  
  
 Contenida  
 Un elemento que está completamente en el límite de la base de datos.  
  
 No contenida  
 Un elemento que cruza el límite de la base de datos.  
  
 Base de datos dependiente  
 Una base de datos que tiene establecida la contención en **NONE**. Todas las bases de datos de versiones anteriores a [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] son dependientes. De forma predeterminada, todas las bases de datos de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y posteriores tienen establecida la contención en **NONE**.  
  
 Base de datos parcialmente independiente  
 Una base de datos parcialmente independiente es una base de datos independiente que puede permitir algunas características que cruzan el límite de la base de datos. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] incluye la capacidad de determinar cuándo se traspasa el límite de contención.  
  
 Usuario contenido  
 Hay dos tipos de usuarios para las bases de datos independientes.  
  
-   **Usuario de la base de datos independiente con contraseña**  
  
     La base de datos autentica a los usuarios de la base de datos independiente con contraseñas. Para obtener más información, vea [Usuarios de base de datos independiente: hacer que la base de datos sea portátil](../../relational-databases/security/contained-database-users-making-your-database-portable.md).  
  
-   **Entidades de seguridad de Windows**  
  
     Los usuarios de Windows autorizados y los miembros de grupos de Windows autorizados pueden conectarse directamente a la base de datos y no requieren inicios de sesión en la base de datos **maestra** . La base de datos confía en la autenticación de Windows.  
  
 A los usuarios basados en inicios de sesión en la base de datos **maestra** se les puede conceder acceso a una base de datos independiente, pero eso crearía una dependencia en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Por lo tanto, para crear usuarios basados en inicios de sesión se necesita contención parcial.
  
> [!IMPORTANT]  
>  La habilitación de bases de datos parcialmente independientes delega el control sobre el acceso a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en los propietarios de la base de datos. Para más información, vea [Security Best Practices with Contained Databases](../../relational-databases/databases/security-best-practices-with-contained-databases.md).  
  
 Límite de la base de datos  
 Dado que las bases de datos parcialmente independientes separan la funcionalidad de la base de datos de las de la instancia, hay una línea definida claramente entre estos dos elementos que se conoce como *límite de la base de datos*.  
  
 Dentro del límite de la base de datos está el *modelo de base de datos*, donde las bases de datos se desarrollan y administran. Ejemplos de entidades situadas dentro de la base de datos son las tablas del sistema como **sys.tables**, los usuarios de bases de datos independientes con contraseñas y las tablas de usuario de la base de datos actual a la que se hace referencia mediante un nombre de dos partes.  
  
 Fuera del límite de la base de datos está el *modelo de administración*, que tiene que ver con la administración y las funciones del nivel de instancia. Algunos ejemplos de entidades situadas fuera del límite de la base de datos son las tablas del sistema como **sys.endpoints**, los usuarios asignados a los inicios de sesión y las tablas de usuario de otra base de datos a la que se hace referencia mediante un nombre de tres partes.  
  
##  <a name="containment"></a><a name="containment"></a> Contención  
 Las entidades de usuario que residen completamente dentro de la base de datos se consideran *independientes*. Cualquier entidad de usuario que resida fuera de la base de datos o se base en la interacción con funciones externas a la base de datos se considera *dependiente*.  
  
 En general, las entidades de usuario entran en las siguientes categorías de contención:  
  
-   Las entidades de usuario totalmente independientes (aquellas que nunca cruzan el límite de la base de datos), por ejemplo, sys.indexes. Cualquier código que utilice estas características o cualquier objeto que haga referencia únicamente a estas entidades también está totalmente contenido.  
  
-   Las entidades de usuario dependientes (aquellas que cruzan el límite de la base de datos), por ejemplo sys.server_principals o una entidad de seguridad del servidor (inicio de sesión) propiamente dicha. Cualquier código que utilice estas entidades o cualquier función que haga referencia a estas entidades también es dependiente.  
  
###  <a name="partially-contained-database"></a><a name="partial"></a> Partially Contained Database  
 La característica de base de datos independiente solo está disponible actualmente en un estado contenido de forma parcial. Una base de datos parcialmente independiente es una base de datos independiente que permite el uso de características no contenidas.  
  
 Use las vistas [sys.dm_db_uncontained_entities](../../relational-databases/system-dynamic-management-views/sys-dm-db-uncontained-entities-transact-sql.md) y [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md) para devolver información sobre características u objetos dependientes. Mediante la determinación del estado de contención de los elementos de la base de datos, se puede detectar qué objetos o características deben reemplazarse o alterarse para promover la contención.  
  
> [!IMPORTANT]  
>  Dado que ciertos objetos tienen la configuración de contención predeterminada **NONE**, esta vista puede devolver falsos positivos.  
  
 El comportamiento de las bases de datos parcialmente independientes difiere del que se observa en las bases de datos dependientes en lo que respecta a la intercalación. Para obtener más información acerca de los problemas de intercalación, vea [Contained Database Collations](../../relational-databases/databases/contained-database-collations.md).  
  
##  <a name="benefits-of-using-partially-contained-databases"></a><a name="benefits"></a> Ventajas del uso de bases de datos parcialmente independientes  
 Las bases de datos dependientes tienen problemas y complicaciones asociados que se pueden resolver utilizando una base de datos parcialmente independiente.  
  
### <a name="database-movement"></a>Movimiento de la base de datos  
 Uno de los problemas que se produce al mover bases de datos, es que determinada información importante puede no estar disponible cuando una base de datos se mueve de una instancia a otra. Por ejemplo, la información de inicio de sesión se almacena en la instancia en lugar de en la base de datos. Al mover una base de datos dependiente de una instancia a otra de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], esta información se queda atrás. Debe identificar la información que falta y moverla con la base de datos a la nueva instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Este proceso puede ser difícil y tardar mucho tiempo.  
  
 La base de datos parcialmente independiente puede almacenar información importante en la base de datos, de modo que esta todavía tiene la información después de moverse.  
  
> [!NOTE]  
>  Una base de datos parcialmente independiente puede proporcionar documentación que describe las características que se utilizan en una base de datos que no se puede separar de la instancia. Esto incluye una lista de otras bases de datos interrelacionadas, la configuración del sistema que la base de datos requiere pero no se puede contener, etc.  
  
### <a name="benefit-of-contained-database-users-with-always-on"></a>Ventajas de usuarios de bases de datos independientes con AlwaysOn  
 Mediante la reducción de los vínculos a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], las bases de datos parcialmente independientes pueden ser útiles durante la conmutación por error cuando se use [!INCLUDE[ssHADR](../../includes/sshadr-md.md)].  
  
 La creación de usuarios contenidos permite al usuario conectarse directamente a la base de datos independiente. Esta es una característica muy importante en escenarios de alta disponibilidad y recuperación ante desastres, como en una solución de AlwaysOn. Cuando los usuarios son usuarios contenidos, si que se produce una conmutación por error, podrán conectarse al servidor secundario sin crear inicios de sesión en la instancia que hospeda a este servidor. Esto proporciona una ventaja inmediata. Para obtener más información, consulte [Información general de los grupos de disponibilidad AlwaysOn (SQL Server)](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md) y [Requisitos previos, restricciones y recomendaciones para Grupos de disponibilidad AlwaysOn (SQL Server)](../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
### <a name="initial-database-development"></a>Desarrollo inicial de la base de datos  
 Dado que un desarrollador puede no saber dónde se implementará una nueva base de datos, la limitación de los impactos del entorno implementados en la base de datos disminuye el trabajo y la carga del desarrollador. En el modelo dependiente, el desarrollador debe tener en cuenta los posibles impactos del entorno en la nueva base de datos y programar en consecuencia. Sin embargo, las bases de datos parcialmente independientes permiten a los desarrolladores detectar los efectos de nivel de instancia en la base de datos y aspectos en el nivel de instancia que les preocupan.  
  
### <a name="database-administration"></a>Administración de bases de datos  
 El mantenimiento de los valores de configuración de la base de datos en la base de datos, en lugar de en la base de datos maestra, permite a cada propietario de la base de datos tener un mayor control sobre ella, sin proporcionar al propietario de la base de datos el permiso **sysadmin** .  
  
##  <a name="limitations"></a><a name="Limitations"></a> Limitaciones  
 Las bases de datos parcialmente independientes no permiten las siguientes características.  
  
-   Replicación, captura de datos modificados o seguimiento de cambios  
  
-   Procedimientos numerados  
  
-   Objetos enlazados a esquema que dependen de funciones integradas con cambios de intercalación  
  
-   Cambios de enlace que son el resultado de los cambios de intercalación, incluidas las referencias a los objetos, columnas, símbolos o tipos.
  
> [!WARNING]  
>  Los procedimientos almacenados temporales se admiten ahora. Dado que los procedimientos almacenados temporales incumplen la contención, no está previsto que se admitan en las versiones futuras de una base de datos independiente.  
  
##  <a name="identifying-database-containment"></a><a name="Identifying"></a> Identificar la contención de base de datos  
 Hay dos herramientas para ayudar a identificar el estado de contención de la base de datos. [sys.dm_db_uncontained_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-uncontained-entities-transact-sql.md)es una vista que muestra todas las entidades potencialmente no contenidas de la base de datos. El evento database_uncontained_usage se produce cuando se identifica una entidad real no contenida en tiempo de ejecución.  
  
### <a name="sysdm_db_uncontained_entities"></a>sys.dm_db_uncontained_entities  
 Esta vista muestra las entidades de la base de datos que tienen el potencial de ser dependientes, como por ejemplo, las que cruzan los límites de la base de datos. Esto incluye las entidades del usuario que pueden utilizar objetos fuera del modelo de base de datos. Sin embargo, dado que la contención de ciertas entidades (por ejemplo, aquellas que usan SQL dinámico) no se puede determinar hasta el tiempo de ejecución, la vista puede mostrar algunas entidades que realmente no están contenidas. Para obtener más información, vea [sys.dm_db_uncontained_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-uncontained-entities-transact-sql.md).  
  
### <a name="database_uncontained_usage-event"></a>database_uncontained_usage, evento  
 Este XEvent tiene lugar siempre que una entidad dependiente se identifica en tiempo de ejecución. Esto incluye las entidades originadas en el código de cliente. Este Xevent solo tendrá lugar para las entidades dependientes reales. Sin embargo, el evento solo se produce en tiempo de ejecución. Por consiguiente, este XEvent no identificará las entidades de usuario dependientes que no se hayan ejecutado.  
  
## <a name="see-also"></a>Consulte también  
 [Características modificadas &#40;base de datos independiente&#41;](../../relational-databases/databases/modified-features-contained-database.md)   
 [Intercalaciones de bases de datos independientes](../../relational-databases/databases/contained-database-collations.md)   
 [Prácticas recomendadas de seguridad con bases de datos independientes](../../relational-databases/databases/security-best-practices-with-contained-databases.md)   
 [Migrate to a Partially Contained Database](../../relational-databases/databases/migrate-to-a-partially-contained-database.md)   
 [Usuarios de base de datos independientes: hacer que la base de datos sea portátil](../../relational-databases/security/contained-database-users-making-your-database-portable.md)  
  
  
