---
description: sp_help (Transact-SQL)
title: sp_help (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/24/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help
- sp_help_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help
ms.assetid: 913cd5d4-39a3-4a4b-a926-75ed32878884
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e97e2118c90521881dbf55d686910cff01ef79e7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176571"
---
# <a name="sp_help-transact-sql"></a>sp_help (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Proporciona información sobre un objeto de base de datos (cualquier objeto enumerado en la vista de compatibilidad de **objetossys.sys** ), un tipo de datos definido por el usuario o un tipo de datos.  
  
 
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_help [ [ @objname = ] 'name' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @objname = ] 'name'` Es el nombre de cualquier objeto, en **sysobjects** o cualquier tipo de datos definido por el usuario en la tabla **systypes** . *Name* es de tipo **nvarchar (** 776 **)** y su valor predeterminado es NULL. No se aceptan nombres de bases de datos.  Se deben delimitar dos o tres nombres de partes, como "Person.AddressType" o [Person.AddressType].   
   
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 Los conjuntos de resultados que se devuelven dependen de si se especifica *el nombre* , Cuándo se especifica y qué objeto de base de datos es.  
  
1.  Si se ejecuta **sp_help** sin argumentos, se devuelve información de Resumen de los objetos de todos los tipos que existen en la base de datos actual.  
  
    |Nombre de la columna|Tipo de datos|Descripción|  
    |-----------------|---------------|-----------------|  
    |**Nombre**|**nvarchar (** 128 **)**|Nombre del objeto|  
    |**Propietario**|**nvarchar (** 128 **)**|Propietario del objeto (esta es la entidad de seguridad de base de datos que posee el objeto. De forma predeterminada, es el propietario del esquema que contiene el objeto).|  
    |**Object_type**|**nvarchar (** 31 **)**|Tipo de objeto|  
  
2.  Si *Name* es un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipo de datos o un tipo de datos definido por el usuario, **sp_help** devuelve este conjunto de resultados.  
  
    |Nombre de la columna|Tipo de datos|Descripción|  
    |-----------------|---------------|-----------------|  
    |**Type_name**|**nvarchar (** 128 **)**|Nombre del tipo de datos.|  
    |**Storage_type**|**nvarchar (** 128 **)**|Nombre del tipo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
    |**Duración**|**smallint**|Longitud física del tipo de datos (en bytes).|  
    |**Prec**|**int**|Precisión (número total de dígitos).|  
    |**Escalar**|**int**|Número de dígitos a la derecha del separador decimal.|  
    |**Admisión de valores NULL**|**VARCHAR (** 35 **)**|Indica si se permiten valores NULL: Yes o No.|  
    |**Default_name**|**nvarchar (** 128 **)**|Nombre de un valor predeterminado enlazado a este tipo.<br /><br /> NULL = No hay un valor predeterminado enlazado.|  
    |**Rule_name**|**nvarchar (** 128 **)**|Nombre de una regla enlazada a este tipo.<br /><br /> NULL = No hay un valor predeterminado enlazado.|  
    |**Intercalación**|**sysname**|Intercalación del tipo de datos. NULL para tipos de datos que no sean de caracteres.|  
  
3.  Si *Name* es cualquier objeto de base de datos que no sea un tipo de datos, **sp_help** devuelve este conjunto de resultados y también los conjuntos de resultados adicionales, en función del tipo de objeto especificado.  

    |Nombre de la columna|Tipo de datos|Descripción|  
    |-----------------|---------------|-----------------|  
    |**Nombre**|**nvarchar (** 128 **)**|Nombre de la tabla|  
    |**Propietario**|**nvarchar (** 128 **)**|Propietario de la tabla.|  
    |**Type**|**nvarchar (** 31 **)**|Tipo de tabla.|  
    |**Created_datetime**|**datetime**|Tabla de fechas de creación|  
  
     Dependiendo del objeto de base de datos especificado, **sp_help** devuelve conjuntos de resultados adicionales.  
  
     Si *Name* es una tabla del sistema, una tabla de usuario o una vista, **sp_help** devuelve los siguientes conjuntos de resultados. No obstante, el conjunto de resultados que describe la ubicación del archivo de datos en un grupo de archivos no se devuelve para una vista.  
  
    -   Conjunto de resultados adicional devuelto en los objetos de columna:  
  
        |Nombre de la columna|Tipo de datos|Descripción|  
        |-----------------|---------------|-----------------|  
        |**Column_name**|**nvarchar (** 128 **)**|Nombre de la columna.|  
        |**Type**|**nvarchar (** 128 **)**|Tipo de datos de la columna.|  
        |**Calculada**|**VARCHAR (** 35 **)**|Indica si los valores de la columna son calculados: Yes o No.|  
        |**Duración**|**int**|Longitud de la columna en bytes.<br /><br /> Nota: Si el tipo de datos de la columna es un tipo de valor grande (**VARCHAR (Max)**, **nvarchar (Max)**, **varbinary (Max)** o **XML**), el valor se mostrará como-1.|  
        |**Prec**|**Char (** 5 **)**|Precisión de la columna.|  
        |**Escalar**|**Char (** 5 **)**|Escala de columnas.|  
        |**Admisión de valores NULL**|**VARCHAR (** 35 **)**|Indica si se permiten valores NULL en la columna: Yes o No.|  
        |**TrimTrailingBlanks**|**VARCHAR (** 35 **)**|Recorta los espacios en blanco finales. Devuelve Yes o No.|  
        |**FixedLenNullInSource**|**VARCHAR (** 35 **)**|Se conserva únicamente por compatibilidad con versiones anteriores.|  
        |**Intercalación**|**sysname**|Intercalación de la columna. NULL para los tipos de datos que no son de caracteres.|  
  
    -   Conjunto de resultados adicional devuelto en las columnas de identidad:  
  
        |Nombre de la columna|Tipo de datos|Descripción|  
        |-----------------|---------------|-----------------|  
        |**Identidad**|**nvarchar (** 128 **)**|Nombre de la columna cuyo tipo de datos se declara como identidad.|  
        |**Seed**|**numeric**|Valor inicial de la columna de identidad.|  
        |**Incremento**|**numeric**|Incremento que se va a utilizar en los valores de esta columna.|  
        |**No disponible para replicación**|**int**|La propiedad IDENTITY no se aplica cuando un inicio de sesión de replicación, como **sqlrepl**, inserta datos en la tabla:<br /><br /> 1 = True<br /><br /> 0 = False|  
  
    -   Conjunto de resultados adicional devuelto en las columnas:  
  
        |Nombre de la columna|Tipo de datos|Descripción|  
        |-----------------|---------------|-----------------|  
        |**RowGuidCol**|**sysname**|Nombre de la columna de identificador único global.|  
  
    -   Conjunto de resultados adicional devuelto en los grupos de archivos:  
  
        |Nombre de la columna|Tipo de datos|Descripción|  
        |-----------------|---------------|-----------------|  
        |**Data_located_on_filegroup**|**nvarchar (** 128 **)**|Grupo de archivos en el que se encuentran los datos: Principal, Secundario o Registro de transacciones.|  
  
    -   Conjunto de resultados adicional devuelto en los índices:  
  
        |Nombre de la columna|Tipo de datos|Descripción|  
        |-----------------|---------------|-----------------|  
        |**index_name**|**sysname**|Nombre del índice.|  
        |**Index_description**|**VARCHAR (** 210 **)**|Descripción del índice.|  
        |**index_keys**|**nvarchar (** 2078 **)**|Nombres de las columnas en las que se ha generado el índice. Devuelve NULL para los índices de almacén de columnas optimizados de memoria xVelocity.|  
  
    -   Conjunto de resultados adicional devuelto en las restricciones:  
  
        |Nombre de la columna|Tipo de datos|Descripción|  
        |-----------------|---------------|-----------------|  
        |**constraint_type**|**nvarchar (** 146 **)**|Tipo de restricción.|  
        |**constraint_name**|**nvarchar (** 128 **)**|Nombre de la restricción.|  
        |**delete_action**|**nvarchar (** 9 **)**|Indica si la acción DELETE es: NO_ACTION, CASCADE, SET_NULL, SET_DEFAULT o N/A.<br /><br /> Solo se aplica a las restricciones FOREIGN KEY.|  
        |**update_action**|**nvarchar (** 9 **)**|Indica si la acción UPDATE es: NO_ACTION, CASCADE, SET_NULL, SET_DEFAULT o N/A.<br /><br /> Solo se aplica a las restricciones FOREIGN KEY.|  
        |**status_enabled**|**VARCHAR (** 8 **)**|Indica si la restricción está habilitada: Habilitada, Deshabilitada o N/A.<br /><br /> Solo se aplica a las restricciones CHECK y FOREIGN KEY.|  
        |**status_for_replication**|**VARCHAR (** 19 **)**|Indica si la restricción es para replicación.<br /><br /> Solo se aplica a las restricciones CHECK y FOREIGN KEY.|  
        |**constraint_keys**|**nvarchar (** 2078 **)**|Nombres de las columnas que componen la restricción o, en el caso de valores predeterminados y reglas, el texto que define el valor predeterminado o la regla.|  
  
    -   Conjunto de resultados adicional devuelto en los objetos de referencia:  
  
        |Nombre de la columna|Tipo de datos|Descripción|  
        |-----------------|---------------|-----------------|  
        |**Table is referenced by**|**nvarchar (** 516 **)**|Identifica otros objetos de base de datos que hacen referencia a la tabla.|  
  
    -   Conjunto de resultados adicional devuelto en los procedimientos almacenados, las funciones o los procedimientos almacenados extendidos.  
  
        |Nombre de la columna|Tipo de datos|Descripción|  
        |-----------------|---------------|-----------------|  
        |**Parameter_name**|**nvarchar (** 128 **)**|Nombre del parámetro del procedimiento almacenado.|  
        |**Type**|**nvarchar (** 128 **)**|Tipo de datos del parámetro del procedimiento almacenado.|  
        |**Duración**|**smallint**|Longitud máxima de almacenamiento físico en bytes.|  
        |**Prec**|**int**|Precisión o número total de dígitos.|  
        |**Escalar**|**int**|Número de dígitos a la derecha del separador decimal.|  
        |**Param_order**|**smallint**|Orden del parámetro.|  
  
## <a name="remarks"></a>Observaciones  
 El procedimiento **sp_help** solo busca un objeto en la base de datos actual.  
  
 Cuando no se especifica *Name* , **sp_help** enumera los nombres de objeto, los propietarios y los tipos de objeto para todos los objetos de la base de datos actual. **sp_helptrigger** proporciona información acerca de los desencadenadores.  
  
 **sp_help** solo expone columnas de índice ordenable; por lo tanto, no expone información sobre índices XML o índices espaciales.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** . El usuario debe tener al menos un permiso en *objName*. Para ver claves de restricción de columna, valores predeterminados o reglas, debe tener el permiso VIEW DEFINITION en la tabla.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-returning-information-about-all-objects"></a>A. Devolver información acerca de todos los objetos  
 En el siguiente ejemplo se presenta información acerca de cada objeto de la base de datos `master`.  
  
```  
USE master;  
GO  
EXEC sp_help;  
GO  
```  
  
### <a name="b-returning-information-about-a-single-object"></a>B. Devolver información acerca de un solo objeto  
 En el siguiente ejemplo se presenta información acerca de la tabla `Person`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_help 'Person.Person';  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Motor de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sp_helpindex &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpindex-transact-sql.md)   
 [sp_helprotect &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helprotect-transact-sql.md)   
 [sp_helpserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md)   
 [sp_helptrigger &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptrigger-transact-sql.md)   
 [sp_helpuser &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpuser-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [ Objetossys.sys&#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-sysobjects-transact-sql.md)  
  
  
