---
description: sp_updateextendedproperty (Transact-SQL)
title: sp_updateextendedproperty (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/12/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_updateextendedproperty_TSQL
- sp_updateextendedproperty
dev_langs:
- TSQL
helpviewer_keywords:
- sp_updateextendedproperty
ms.assetid: 7f02360f-cb9e-48b4-b75f-29b4bc9ea304
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a4ba9f50620dfb623edd3154600c956d1d168c16
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189513"
---
# <a name="sp_updateextendedproperty-transact-sql"></a>sp_updateextendedproperty (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Actualiza el valor de una propiedad extendida existente.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_updateextendedproperty  
    [ @name = ]{ 'property_name' }   
    [ , [ @value = ]{ 'value' }  
        [, [ @level0type = ]{ 'level0_object_type' }  
         , [ @level0name = ]{ 'level0_object_name' }  
              [, [ @level1type = ]{ 'level1_object_type' }  
               , [ @level1name = ]{ 'level1_object_name' }  
                     [, [ @level2type = ]{ 'level2_object_type' }  
                      , [ @level2name = ]{ 'level2_object_name' }  
                     ]  
              ]  
        ]  
    ]  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @name =] {'*property_name*'}  
 Es el nombre de la propiedad que se va a actualizar. *property_name* es de **tipo sysname** y no puede ser null.  
  
 [ @value =] {'*valor*'}  
 Es el valor asociado a la propiedad. el *valor* es **sql_variant** y su valor predeterminado es NULL. El tamaño del *valor* no puede ser superior a 7.500 bytes.  
  
 [ @level0type =] {'*level0_object_type*'}  
 Es el usuario o el tipo definido por el usuario. *level0_object_type* es de tipo **VARCHAR (128)** y su valor predeterminado es NULL. Las entradas válidas son ASSEMBLy, CONTRACT, EVENT NOTIFICATION, FILEGROUP, MESSAGE TYPE, PARTITION FUNCTION, PARTITION SCHEME, PLAN GUIDE, Remote SERVICE BINDING, ROUTE, SCHEMA, SERVICE, USER, TRIGGER, TYPE y NULL.  
  
> [!IMPORTANT]  
>  USER y TYPE como tipos de nivel 0 se quitarán en una versión futura de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evite utilizar estas características en los nuevos trabajos de programación y planee modificar las aplicaciones que actualmente las utilizan. En lugar de USER, use SCHEMA como tipo de nivel 0. Para TYPE, use SCHEMA como tipo de nivel 0 y TYPE como tipo de nivel 1.  
  
 [ @level0name =] {'*level0_object_name*'}  
 Nombre del tipo de objeto de nivel 1 especificado. *level0_object_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
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
 Los objetos de una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se clasifican en tres niveles (0, 1, 2) con el fin de especificar las propiedades extendidas. El nivel 0 es el nivel más elevado y se define como los objetos incluidos en el ámbito de la base de datos. Los objetos de nivel 1 se incluyen en un ámbito de esquema o de usuario, y los objetos de nivel 2 están incluidos en los objetos de nivel 1. Se pueden definir propiedades extendidas para los objetos de cualquiera de estos niveles. Las referencias a un objeto de un nivel deben estar calificadas con los nombres de los objetos del nivel superior que son sus propietarios o que los contienen.  
  
 Dado una *property_name* y un *valor* válidos, si todos los tipos y nombres de objetos son NULL, la propiedad actualizada pertenece a la base de datos actual.  
  
## <a name="permissions"></a>Permisos  
 Los miembros de los roles fijos de base de datos db_owner y db_ddladmin pueden actualizar las propiedades extendidas de cualquier objeto con la siguiente excepción: db_ddladmin no puede agregar propiedades a la base de datos, a los usuarios ni a los roles.  
  
 Los usuarios pueden actualizar las propiedades extendidas de los objetos de los que sean propietarios, o para los que tengan permisos ALTER o CONTROL.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-updating-an-extended-property-on-a-column"></a>A. Actualizar una propiedad extendida de una columna  
 En el siguiente ejemplo se actualiza el valor de la propiedad `Caption` en la columna `ID` de la tabla `T1`.  
  
```  
USE AdventureWorks2012;  
GO  
CREATE TABLE T1 (id int , name char (20));  
GO  
EXEC sp_addextendedproperty   
    @name = N'Caption'  
    ,@value = N'Employee ID'  
    ,@level0type = N'Schema', @level0name = dbo  
    ,@level1type = N'Table',  @level1name = T1  
    ,@level2type = N'Column', @level2name = id;  
GO  
--Update the extended property.  
EXEC sp_updateextendedproperty   
    @name = N'Caption'  
    ,@value = 'Employee ID must be unique.'  
    ,@level0type = N'Schema', @level0name = dbo  
    ,@level1type = N'Table',  @level1name = T1  
    ,@level2type = N'Column', @level2name = id;  
GO  
```  
  
### <a name="b-updating-an-extended-property-on-a-database"></a>B. Actualizar una propiedad extendida de una base de datos  
 El siguiente ejemplo crea primero una propiedad extendida en la base de datos de muestra [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] y después actualiza el valor de esa propiedad.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_addextendedproperty   
@name = N'NewCaption', @value = 'AdventureWorks2012 Sample OLTP Database';  
GO  
USE AdventureWorks2012;  
GO  
EXEC sp_updateextendedproperty   
@name = N'NewCaption', @value = 'AdventureWorks2012 Sample Database';  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Motor de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sys.fn_listextendedproperty &#40;&#41;de Transact-SQL ](../../relational-databases/system-functions/sys-fn-listextendedproperty-transact-sql.md)   
 [sp_addextendedproperty &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql.md)   
 [sp_dropextendedproperty &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropextendedproperty-transact-sql.md)   
 [sys.extended_properties &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/extended-properties-catalog-views-sys-extended-properties.md)  
  
  
