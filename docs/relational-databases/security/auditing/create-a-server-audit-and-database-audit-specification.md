---
title: Creación de una especificación de auditoría de servidor y de base de datos
description: Aprenda a crear una especificación de auditoría de SQL Server y de base de datos mediante SQL Server Management Studio o Transact-SQL (T-SQL).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.sqlaudit.dbaudit.general.f1
helpviewer_keywords:
- audits [SQL Server], creating database specification
- database audit [SQL Server]
ms.assetid: 26ee85de-6e97-4318-b526-900924d96e62
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: d015a1adf0bdd5e5f47b49a8dabb6783b8538b7e
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250091"
---
# <a name="create-a-server-audit-and-database-audit-specification"></a>Creación de una especificación de auditoría de servidor y de base de datos
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  En este artículo se describe cómo crear una especificación de auditoría de servidor y de base de datos en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 La auditoría de una instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] o de una base de datos de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] implica el seguimiento y registro de los eventos que se producen en el sistema. El objeto *SQL Server Audit* recopila una única instancia de acciones y grupos de acciones de nivel de servidor o de nivel de base de datos para su supervisión. La auditoría se realiza en el nivel de instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Es posible tener varias auditorías por cada instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . El objeto *Especificación de auditoría de base de datos* pertenece a una auditoría. Puede crear una única especificación de auditoría de base de datos para cada base de datos de SQL Server y cada auditoría. Para más información, vea [SQL Server Audit (motor de base de datos)](../../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
 ##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de empezar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
 Las especificaciones de auditoría de base de datos son objetos no protegibles que residen en una base de datos determinada. Al crear una especificación de auditoría de base de datos, está en un estado deshabilitado.  
  
 Cuando cree o modifique una especificación de auditoría de base de datos en una base de datos de usuario, no incluya acciones de auditoría en objetos del ámbito de servidor, como las vistas del sistema. Si se incluyen objetos del ámbito de servidor, se creará la auditoría. Sin embargo, los objetos del ámbito de servidor no se incluirán y no se devolverá ningún error. Para auditar objetos del ámbito de servidor, utilice una especificación de auditoría de base de datos en la base de datos maestra.  
  
 Las especificaciones de auditoría de base de datos residen en la base de datos en la que se crearon, excepto en el caso de la base de datos del sistema **TempDB**.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
  
-   Los usuarios con el permiso ALTER ANY DATABASE AUDIT pueden crear especificaciones de auditoría de base de datos y enlazarlas a cualquier auditoría.  
  
-   Una vez creada una especificación de auditoría de base de datos, las entidades de seguridad que cuenten con los permisos CONTROL SERVER o ALTER ANY DATABASE AUDIT podrán verla. La cuenta de administrador del sistema también podrá verla.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-create-a-server-audit"></a>Para crear una auditoría de servidor  
  
1.  En el Explorador de objetos, expanda la carpeta **Seguridad** .  
  
2.  Haga clic con el botón derecho en la carpeta **Auditorías** y seleccione **Nueva auditoría**. Para más información, vea [Crear una auditoría de servidor y una especificación de auditoría de servidor](../../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md).  
  
3.  Cuando termine de elegir las opciones, seleccione **Aceptar**.  

#### <a name="to-create-a-database-level-audit-specification"></a>Para crear una especificación de auditoría de nivel de base de datos  
  
1.  En el Explorador de objetos, expanda la base de datos donde quiera crear la especificación de auditoría.  
  
2.  Expanda la carpeta **Seguridad** .  
  
3.  Haga clic con el botón derecho en la carpeta **Especificaciones de auditoría de base de datos** y seleccione **Nueva especificación de auditoría de base de datos**.  
  
     Estas opciones están disponibles en el cuadro de diálogo **Crear especificación de auditoría de base de datos**:  
  
     **Nombre**  
     El nombre de la especificación de auditoría de base de datos. Al crear una especificación de auditoría de servidor, se genera automáticamente un nombre. El nombre se puede editar.  
  
     **Auditoría**  
     Nombre de un objeto de auditoría de servidor existente. Escriba el nombre de la auditoría o selecciónelo en la lista.  
  
     **Tipo de acción de auditoría**  
     Especifica los grupos de acciones de auditoría y las acciones de auditoría en el nivel de base de datos que se desean capturar. Para obtener una lista de grupos de acciones de auditoría y de acciones de auditoría de nivel de base de datos, así como una descripción de los eventos que contienen, vea [Grupos de acciones y acciones de SQL Server Audit](../../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md).  
  
     **Esquema de objeto**  
     Muestra el esquema para el **Nombre de objeto** especificado.  
  
     **Nombre de objeto**  
     Nombre del objeto que se va a auditar. Esta opción solo está disponible para las acciones de auditoría. No se aplica a los grupos de auditoría.  
  
     **Puntos suspensivos (...)**  
     Abre el cuadro de diálogo **Seleccionar objetos** para buscar y seleccionar un objeto disponible, en función del **Tipo de acción de auditoría** especificado.  
  
     **Nombre de la entidad**  
     La cuenta por la que se va filtrar la auditoría para el objeto que se va a auditar.  
  
     **Puntos suspensivos (...)**  
     Abre el cuadro de diálogo **Seleccionar objetos** para buscar y seleccionar un objeto disponible, en función del **Nombre de objeto** especificado.  
  
4.  Cuando termine de elegir las opciones, seleccione **Aceptar**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-create-a-server-audit"></a>Para crear una auditoría de servidor  
  
1.  En el Explorador de objetos, conéctese a una instancia del [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, seleccione **Nueva consulta**.  
  
3.  Pegue el ejemplo siguiente en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    USE master ;  
    GO  
    -- Create the server audit.   
    CREATE SERVER AUDIT Payrole_Security_Audit  
        TO FILE ( FILEPATH =   
    'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA' ) ;   
    GO  
    -- Enable the server audit.   
    ALTER SERVER AUDIT Payrole_Security_Audit   
    WITH (STATE = ON) ;  
    ```  
  
#### <a name="to-create-a-database-level-audit-specification"></a>Para crear una especificación de auditoría de nivel de base de datos  
  
1.  En el Explorador de objetos, conéctese a una instancia del [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, seleccione **Nueva consulta**.  
  
3.  Pegue el ejemplo siguiente en la ventana de consulta y haga clic en **Ejecutar**. En el ejemplo siguiente se crea una especificación de auditoría de servidor denominada `Audit_Pay_Tables`. Audita las instrucciones SELECT e INSERT del usuario `dbo` de la tabla `HumanResources.EmployeePayHistory`, en función de la auditoría de servidor definida en la sección anterior.  
  
    ```  
    USE AdventureWorks2012 ;   
    GO  
    -- Create the database audit specification.   
    CREATE DATABASE AUDIT SPECIFICATION Audit_Pay_Tables  
    FOR SERVER AUDIT Payrole_Security_Audit  
    ADD (SELECT , INSERT  
         ON HumanResources.EmployeePayHistory BY dbo )   
    WITH (STATE = ON) ;   
    GO  
  
    ```  
  
 Para obtener más información, vea [CREATE SERVER AUDIT &#40;Transact-SQL&#41;](../../../t-sql/statements/create-server-audit-transact-sql.md) y [CREATE DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../../t-sql/statements/create-database-audit-specification-transact-sql.md).  
  
  
