---
description: sp_addextendedproperty (Transact-SQL)
title: sp_addextendedproperty (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addextendedproperty
- sp_addextendedproperty_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addextendedproperty
ms.assetid: 565483ea-875b-4133-b327-d0006d2d7b4c
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 448f2c67d7e7c09f0e2768cc0d6c359224d15b45
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182778"
---
# <a name="sp_addextendedproperty-transact-sql"></a>sp_addextendedproperty (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Agrega una nueva propiedad extendida a un objeto de base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_addextendedproperty  
    [ @name = ] { 'property_name' }  
    [ , [ @value = ] { 'value' }   
        [ , [ @level0type = ] { 'level0_object_type' }   
          , [ @level0name = ] { 'level0_object_name' }   
                [ , [ @level1type = ] { 'level1_object_type' }   
                  , [ @level1name = ] { 'level1_object_name' }   
                        [ , [ @level2type = ] { 'level2_object_type' }   
                          , [ @level2name = ] { 'level2_object_name' }   
                        ]   
                ]  
        ]   
    ]   
[;]  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @name ] = {'*property_name*'}  
 Es el nombre de la propiedad que se va a agregar. *property_name* es de **tipo sysname** y no puede ser null. Los nombres también pueden incluir espacios en blanco o cadenas de caracteres no alfanuméricos, y valores binarios.  
  
 [ @value =] {'*valor*'}  
 Es el valor que se va a asociar a la propiedad. el *valor* es **sql_variant** y su valor predeterminado es NULL. El tamaño de *value* no puede ser superior a 7.500 bytes.  
  
 [ @level0type =] {'*level0_object_type*'}  
 Tipo de objeto de nivel 0. *level0_object_type* es de tipo **VARCHAR (128)** y su valor predeterminado es NULL.  
  
 Las entradas válidas son ASSEMBLY, CONTRACT, EVENT NOTIFICATION, FILEGROUP, MESSAGE TYPE, PARTITION FUNCTION, PARTITION SCHEME, REMOTE SERVICE BINDING, ROUTE, SCHEMA, SERVICE, USER, TRIGGER, TYPE, PLAN GUIDE y NULL.  
  
> [!IMPORTANT]  
>  La capacidad de especificar USER como tipo de nivel 0 en una propiedad extendida de un objeto de tipo de nivel 1 se quitará en una versión futura de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En su lugar, utilice SCHEMA como tipo de nivel 0. Por ejemplo, al definir una propiedad extendida en una tabla, especifique el esquema de la tabla en lugar de un nombre de usuario. La capacidad de especificar TYPE como tipo de nivel 0 se quitará en una versión futura de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para TYPE, use SCHEMA como tipo de nivel 0 y TYPE como tipo de nivel 1.  
  
 [ @level0name =] {'*level0_object_name*'}  
 Nombre del tipo de objeto de nivel 0 especificado. *level0_object_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
 [ @level1type =] {'*level1_object_type*'}  
 Tipo de objeto de nivel 1. *level1_object_type* es de tipo **VARCHAR (128)** y su valor predeterminado es NULL. Las entradas válidas son Aggregate, DEFAULT, FUNCTION, LOGICal FILE NAME, PROCEDURE, QUEUe, RULE, SEQUENCE, SYNONYM, TABLE, TABLE_TYPE, TYPE, VIEW, XML SCHEMA COLLECTION y NULL.    
 [ @level1name =] {'*level1_object_name*'}  
 Nombre del tipo de objeto de nivel 1 especificado. *level1_object_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
 [ @level2type =] {'*level2_object_type*'}  
 Es el tipo de objeto de nivel 2. *level2_object_type* es de tipo **VARCHAR (128)** y su valor predeterminado es NULL. Las entradas válidas son COLUMN, CONSTRAINT, EVENT NOTIFICATION, INDEX, PARAMETER, TRIGGER y NULL.  
  
 [ @level2name =] {'*level2_object_name*'}  
 Es el nombre del tipo de objeto de nivel 2 especificado. *level2_object_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 Para especificar propiedades extendidas, los objetos de una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se clasifican en tres niveles: 0, 1 y 2. El nivel 0 es el más alto y corresponde a los objetos contenidos en el ámbito de la base de datos. Los objetos de nivel 1 se incluyen en un ámbito de esquema o de usuario, y los objetos de nivel 2 están incluidos en los objetos de nivel 1. Se pueden definir propiedades extendidas para los objetos de cualquiera de estos niveles.  
  
 Las referencias a un objeto de un nivel deben estar calificadas con los nombres de los objetos del nivel superior que son sus propietarios o que los contienen. Por ejemplo, cuando se agrega una propiedad extendida a una columna de tabla (nivel 2), también se debe especificar el nombre de la tabla (nivel 1) que contiene la columna y el esquema (nivel 0) que contiene la tabla.  
  
 Si todos los tipos y nombres de objetos son NULL, la propiedad pertenece a la base de datos actual.  
  
 No se permiten propiedades extendidas en los objetos del sistema, en los objetos que se encuentren fuera del ámbito de una base de datos definida por el usuario o en los objetos que no estén incluidos como entradas válidas en la sección Argumentos.  
  
 No se permiten propiedades extendidas en tablas optimizadas para memoria.  
  
## <a name="replicating-extended-properties"></a>Replicar propiedades extendidas  
 Las propiedades extendidas solo se replican en la sincronización inicial entre el publicador y el suscriptor. Si agrega o modifica una propiedad extendida después de la sincronización inicial, el cambio no se replica. Para obtener más información sobre cómo replicar objetos de base de datos, vea [publicar datos y objetos de base de datos](../../relational-databases/replication/publish/publish-data-and-database-objects.md).  
  
## <a name="schema-vs-user"></a>Esquema frente Usuario  
 No se recomienda especificar USER como tipo de nivel 0 cuando se aplica una propiedad extendida a un objeto de base de datos, ya que esto puede producir ambigüedad en la resolución de nombres. Supongamos, por ejemplo, que el usuario Mary posee dos esquemas, Mary y MySchema, y que cada uno contiene una tabla denominada MyTable. Si Mary agrega una propiedad extendida a la tabla MyTable y especifica **@level0type = N'USER '**, **@level0name = Mary**, no queda claro a qué tabla se aplica la propiedad extendida. Por compatibilidad con versiones anteriores, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aplicará la propiedad a la tabla contenida en el esquema Mary.  
  
## <a name="permissions"></a>Permisos  
 Los miembros de los roles fijos de base de datos db_owner y db_ddladmin pueden agregar las propiedades extendidas a cualquier objeto con la siguiente excepción: db_ddladmin no puede agregar propiedades a la base de datos, a los usuarios o a los roles.  
  
 Los usuarios pueden agregar propiedades extendidas a los objetos que poseen o en los que tienen permisos ALTER o CONTROL.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-adding-an-extended-property-to-a-database"></a>A. Agregar una propiedad extendida a una base de datos  
 En el ejemplo siguiente se agrega el nombre de la propiedad `'Caption'` con el valor `'AdventureWorks2012 Sample OLTP Database'` a la base de datos de ejemplo `AdventureWorks2012` .  
  
```  
USE AdventureWorks2012;  
GO  
--Add a caption to the AdventureWorks2012 Database object itself.  
EXEC sp_addextendedproperty   
@name = N'Caption',   
@value = 'AdventureWorks2012 Sample OLTP Database';  
```  
  
### <a name="b-adding-an-extended-property-to-a-column-in-a-table"></a>B. Agregar una propiedad extendida a una columna de una tabla  
 En el ejemplo siguiente se agrega una propiedad de descripción (Caption) a la columna `PostalCode` de la tabla `Address`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_addextendedproperty   
@name = N'Caption',   
@value = 'Postal code is a required column.',  
@level0type = N'Schema', @level0name = 'Person',  
@level1type = N'Table',  @level1name = 'Address',  
@level2type = N'Column', @level2name = 'PostalCode';  
GO  
```  
  
### <a name="c-adding-an-input-mask-property-to-a-column"></a>C. Agregar una propiedad de máscara de entrada a una columna  
 En el ejemplo siguiente se agrega una propiedad de máscara de entrada '`99999 or 99999-9999 or #### ###`' a la columna `PostalCode` de la tabla `Address`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_addextendedproperty   
@name = N'Input Mask ', @value = '99999 or 99999-9999 or #### ###',  
@level0type = N'Schema', @level0name = 'Person',  
@level1type = N'Table', @level1name = 'Address',   
@level2type = N'Column',@level2name = 'PostalCode';  
GO  
```  
  
### <a name="d-adding-an-extended-property-to-a-filegroup"></a>D. Agregar una propiedad extendida a un grupo de archivos  
 En el ejemplo siguiente se agrega una propiedad extendida al grupo de archivos `PRIMARY` .  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sys.sp_addextendedproperty   
@name = N'MS_DescriptionExample',   
@value = N'Primary filegroup for the AdventureWorks2012 sample database.',   
@level0type = N'FILEGROUP', @level0name = 'PRIMARY';  
GO  
```  
  
### <a name="e-adding-an-extended-property-to-a-schema"></a>E. Agregar una propiedad extendida a un esquema  
 En el ejemplo siguiente se agrega una propiedad extendida al esquema `HumanResources` .  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_addextendedproperty   
@name = N'MS_DescriptionExample',  
@value = N'Contains objects related to employees and departments.',  
@level0type = N'SCHEMA',   
@level0name = 'HumanResources';  
```  
  
### <a name="f-adding-an-extended-property-to-a-table"></a>F. Agregar una propiedad extendida a una tabla  
 En el siguiente ejemplo se agrega una propiedad extendida a la tabla `Address` del esquema `Person` .  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sys.sp_addextendedproperty   
@name = N'MS_DescriptionExample',   
@value = N'Street address information for customers, employees, and vendors.',   
@level0type = N'SCHEMA', @level0name = 'Person',  
@level1type = N'TABLE',  @level1name = 'Address';  
GO  
```  
  
### <a name="g-adding-an-extended-property-to-a-role"></a>G. Agregar una propiedad extendida a un rol  
 En el ejemplo siguiente se crea un rol de aplicación y se agrega una propiedad extendida al rol.  
  
```  
USE AdventureWorks2012;   
GO  
CREATE APPLICATION ROLE Buyers  
WITH Password = '987G^bv876sPY)Y5m23';   
GO  
EXEC sys.sp_addextendedproperty   
@name = N'MS_Description',   
@value = N'Application Role for the Purchasing Department.',  
@level0type = N'USER',  
@level0name = 'Buyers';  
```  
  
### <a name="h-adding-an-extended-property-to-a-type"></a>H. Agregar una propiedad extendida a un tipo  
 En el ejemplo siguiente se agrega una propiedad extendida a un tipo.  
  
```  
USE AdventureWorks2012;   
GO  
EXEC sys.sp_addextendedproperty   
@name = N'MS_Description',   
@value = N'Data type (alias) to use for any column that represents an order number. For example a sales order number or purchase order number.',   
@level0type = N'SCHEMA',   
@level0name = N'dbo',   
@level1type = N'TYPE',   
@level1name = N'OrderNumber';  
```  
  
### <a name="i-adding-an-extended-property-to-a-user"></a>I. Agregar una propiedad extendida a un usuario  
 En el ejemplo siguiente se crea un usuario y se agrega una propiedad extendida al usuario.  
  
```  
USE AdventureWorks2012;   
GO  
CREATE USER CustomApp WITHOUT LOGIN ;   
GO  
EXEC sys.sp_addextendedproperty   
@name = N'MS_Description',   
@value = N'User for an application.',   
@level0type = N'USER',   
@level0name = N'CustomApp';  
```  
  
## <a name="see-also"></a>Consulte también  
 [Motor de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sys.fn_listextendedproperty &#40;&#41;de Transact-SQL ](../../relational-databases/system-functions/sys-fn-listextendedproperty-transact-sql.md)   
 [sp_dropextendedproperty &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropextendedproperty-transact-sql.md)   
 [sp_updateextendedproperty &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-updateextendedproperty-transact-sql.md)  
  
  
