---
description: sp_dropextendedproperty (Transact-SQL)
title: sp_dropextendedproperty (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dropextendedproperty_TSQL
- sp_dropextendedproperty
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dropextendedproperty
ms.assetid: 4851865a-86ca-4823-991a-182dd1934075
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c42236cc1f0157d774ea38d356ba8f122407c2c3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205135"
---
# <a name="sp_dropextendedproperty-transact-sql"></a>sp_dropextendedproperty (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Quita una propiedad extendida existente.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_dropextendedproperty   
    [ @name = ] { 'property_name' }  
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
```  
  
## <a name="arguments"></a>Argumentos  
 [ @name =] {'*property_name*'}  
 Es el nombre de la propiedad que se va a quitar. *property_name* es de **tipo sysname** y no puede ser null.  
  
 [ @level0type =] {'*level0_object_type*'}  
 Nombre del tipo de objeto de nivel 0 especificado. *level0_object_type* es de tipo **VARCHAR (128)** y su valor predeterminado es NULL.  
  
 Las entradas válidas son ASSEMBLY, CONTRACT, EVENT NOTIFICATION, FILEGROUP, MESSAGE TYPE, PARTITION FUNCTION, PARTITION SCHEME, REMOTE SERVICE BINDING, ROUTE, SCHEMA, SERVICE, USER, TRIGGER, TYPE y NULL.  
  
> [!IMPORTANT]  
>  USER y TYPE como tipos de nivel 0 se quitarán en una versión futura de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evite utilizar estas características en los nuevos trabajos de programación y planee modificar las aplicaciones que actualmente las utilizan. En lugar de USER, use SCHEMA como tipo de nivel 0. Para TYPE, use SCHEMA como tipo de nivel 0 y TYPE como tipo de nivel 1.  
  
 [ @level0name =] {'*level0_object_name*'}  
 Nombre del tipo de objeto de nivel 0 especificado. *level0_object_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
 [ @level1type =] {'*level1_object_type*'}  
 Tipo de objeto de nivel 1. *level1_object_type* es de tipo **VARCHAR (128)** y su valor predeterminado es NULL. Las entradas válidas son AGGREGATE, DEFAULT, FUNCTION, LOGICAL FILE NAME, PROCEDURE, QUEUE, RULE, SYNONYM, TABLE, TABLE_TYPE, TYPE, VIEW, XML SCHEMA COLLECTION y NULL.  
  
 [ @level1name =] {'*level1_object_name*'}  
 Nombre del tipo de objeto de nivel 1 especificado. *level1_object_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
 [ @level2type =] {'*level2_object_type*'}  
 Es el tipo de objeto de nivel 2. *level2_object_type* es de tipo **VARCHAR (128)** y su valor predeterminado es NULL. Las entradas válidas son COLUMN, CONSTRAINT, EVENT NOTIFICATION, INDEX, PARAMETER, TRIGGER y NULL.  
  
 [ @level2name =] {'*level2_object_name*'}  
 Es el nombre del tipo de objeto de nivel 2 especificado. *level2_object_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 Con el fin de especificar las propiedades extendidas, los objetos de una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se clasifican en tres niveles: 0, 1y 2. El nivel 0 es el nivel más elevado y se define como los objetos incluidos en el ámbito de la base de datos. Los objetos de nivel 1 se incluyen en un ámbito de esquema o de usuario, y los objetos de nivel 2 están incluidos en los objetos de nivel 1. Se pueden definir propiedades extendidas para los objetos de cualquiera de estos niveles. Las referencias a un objeto de un nivel deben completarse con los tipos y nombres de los objetos de nivel superior.  
  
 Dado un *property_name* válido, si todos los tipos y nombres de objetos son NULL y existe una propiedad en la base de datos actual, se elimina esa propiedad. Vea el ejemplo B más adelante en este tema.  
  
## <a name="permissions"></a>Permisos  
 Los miembros de los roles fijos de base de datos db_owner y db_ddladmin pueden eliminar las propiedades extendidas de cualquier objeto con la siguiente excepción: db_ddladmin no puede agregar propiedades a la base de datos, a los usuarios o a los roles.  
  
 Los usuarios pueden quitar propiedades extendidas para los objetos que poseen o en los que tienen permisos ALTER o CONTROL.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-dropping-an-extended-property-on-a-column"></a>A. Quitar una propiedad extendida de una columna  
 En el ejemplo siguiente se quita la propiedad `caption` de la columna `id` de la tabla `T1` contenida en el esquema `dbo`.  
  
```  
CREATE TABLE T1 (id int , name char (20));  
GO  
EXEC sp_addextendedproperty   
     @name = 'caption'   
    ,@value = 'Employee ID'   
    ,@level0type = 'schema'   
    ,@level0name = dbo  
    ,@level1type = 'table'  
    ,@level1name = 'T1'  
    ,@level2type = 'column'  
    ,@level2name = id;  
GO  
EXEC sp_dropextendedproperty   
     @name = 'caption'   
    ,@level0type = 'schema'   
    ,@level0name = dbo  
    ,@level1type = 'table'  
    ,@level1name = 'T1'  
    ,@level2type = 'column'  
    ,@level2name = id;  
GO  
DROP TABLE T1;  
GO  
```  
  
### <a name="b-dropping-an-extended-property-on-a-database"></a>B. Quitar una propiedad extendida de una base de datos  
 En el ejemplo siguiente se quita la propiedad denominada `MS_Description` de la [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] base de datos de ejemplo. La propiedad se encuentra en la propia base de datos, por lo que no se especifican tipos ni nombres de objeto.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_dropextendedproperty   
@name = N'MS_Description';  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Motor de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sys.fn_listextendedproperty &#40;&#41;de Transact-SQL ](../../relational-databases/system-functions/sys-fn-listextendedproperty-transact-sql.md)   
 [sp_addextendedproperty &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql.md)   
 [sp_updateextendedproperty &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-updateextendedproperty-transact-sql.md)   
 [sys.extended_properties &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/extended-properties-catalog-views-sys-extended-properties.md)  
  
  
