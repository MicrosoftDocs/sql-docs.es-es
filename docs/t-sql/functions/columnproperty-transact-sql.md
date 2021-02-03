---
description: COLUMNPROPERTY (Transact-SQL)
title: COLUMNPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- COLUMNPROPERTY
- COLUMNPROPERTY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- column properties [SQL Server]
- parameters [SQL Server], properties
- COLUMNPROPERTY function
ms.assetid: 2408c264-6eca-4120-bb71-df043c7c2792
author: cawrites
ms.author: chadam
ms.openlocfilehash: 320b9c78fadcef3ba70585def88f744bf16e7767
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237431"
---
# <a name="columnproperty-transact-sql"></a>COLUMNPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Esta función devuelve información de la columna o el parámetro.
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
COLUMNPROPERTY ( id , column , property )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*id*  
Una [expresión](../../t-sql/language-elements/expressions-transact-sql.md) que contiene el identificador (Id.) de la tabla o del procedimiento.
  
*column*  
Una expresión que contiene el nombre de la columna o del parámetro.
  
*property*  
Para el argumento *id*, el argumento *propiedad* especifica el tipo de información que devolverá la función `COLUMNPROPERTY`. El argumento *propiedad* puede tener uno de estos valores:
  
|Value|Descripción|Valor devuelto|  
|---|---|---|
|**AllowsNull**|Permite valores NULL.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: entrada no válida|  
|**ColumnId**|Valor del Id. de columna correspondiente a **sys.columns.column_id**.|Identificador de columna<br /><br /> **Nota:** Cuando se consultan varias columnas, pueden aparecer espacios en la secuencia de valores de los Id. de columna.|  
|**FullTextTypeColumn**|El valor TYPE COLUMN de la tabla que contiene la información del tipo de documento de la *columna*.|Id. de TYPE COLUMN de texto completo para la expresión de nombre de columna que se pasa como segundo parámetro de esta función.|  
|**GeneratedAlwaysType**|Es el valor de columna generado por el sistema. Corresponde a **sys.columns.generated_always_type**|**Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.<br /><br /> 0: no siempre se genera.<br /><br /> 1: se genera siempre como comienzo de fila.<br /><br /> 2: se genera siempre como fin de fila.|  
|**IsColumnSet**|La columna es un conjunto de columnas. Para obtener más información, vea [Usar conjuntos de columnas](../../relational-databases/tables/use-column-sets.md).|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: entrada no válida|  
|**IsComputed**|La columna es una columna calculada.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: entrada no válida|  
|**IsCursorType**|El parámetro de procedimiento es del tipo CURSOR.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: entrada no válida|  
|**IsDeterministic**|La columna es determinista. Esta propiedad solo se aplica a columnas calculadas y columnas de vistas.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: entrada no válida No es una columna calculada o una columna de vista.|  
|**IsFulltextIndexed**|La columna se registra para la indización de texto completo.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: entrada no válida|  
|**IsHidden**|Es el valor de columna generado por el sistema. Corresponde a **sys.columns.is_hidden**|**Válido para** : [!INCLUDE[ssCurrentLong](../../includes/sscurrent-md.md)] y versiones posteriores.<br /><br /> 0: no está oculto<br /><br /> 1: oculto|  
|**IsIdentity**|La columna utiliza la propiedad IDENTITY.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: entrada no válida|  
|**IsIdNotForRepl**|La columna comprueba el valor IDENTITY_INSERT.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: entrada no válida|  
|**IsIndexable**|La columna se puede indizar.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: entrada no válida|  
|**IsOutParam**|El parámetro de procedimiento es un parámetro de salida.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: entrada no válida|  
|**IsPrecise**|La columna es precisa. Esta propiedad solo se aplica a columnas deterministas.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: entrada no válida No es una columna determinista|  
|**IsRowGuidCol**|La columna es del tipo de datos **uniqueidentifier** y se define con la propiedad ROWGUIDCOL.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: entrada no válida|  
|**IsSparse**|La columna es una columna dispersa. Para obtener más información, vea [Usar columnas dispersas](../../relational-databases/tables/use-sparse-columns.md).|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: entrada no válida|  
|**IsSystemVerified**|El [!INCLUDE[ssDE](../../includes/ssde-md.md)] puede comprobar las propiedades de determinismo y precisión de la columna. Esta propiedad solo se aplica a columnas calculadas y columnas de vistas.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: entrada no válida|  
|**IsXmlIndexable**|La columna XML se puede utilizar en un índice XML.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: entrada no válida|  
|**Precisión**|Longitud del tipo de datos de la columna o el parámetro.|Longitud del tipo de datos especificado para la columna<br /><br /> -1: **xml** o tipos de valor grandes<br /><br /> NULL: entrada no válida|  
|**Escala**|Escala del tipo de datos de la columna o del parámetro.|El valor de escala.<br /><br /> NULL: entrada no válida|  
|**StatisticalSemantics**|La columna está habilitada para la indización semántica.|1: TRUE<br /><br /> 0: FALSE|  
|**SystemDataAccess**|La columna se deriva de una función que tiene acceso a los datos de los catálogos del sistema o de las tablas virtuales del sistema de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esta propiedad solo se aplica a columnas calculadas y columnas de vistas.|1: TRUE (indica acceso de solo lectura)<br /><br /> 0: FALSE<br /><br /> NULL: entrada no válida|  
|**UserDataAccess**|La columna se deriva de una función que tiene acceso a los datos de las tablas de usuario, incluidas las vistas y tablas temporales, almacenadas en la instancia local de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esta propiedad solo se aplica a columnas calculadas y columnas de vistas.|1: TRUE (indica acceso de solo lectura)<br /><br /> 0: FALSE<br /><br /> NULL: entrada no válida|  
|**UsesAnsiTrim**|ANSI_PADDING se estableció em ON al crear la tabla. Esta propiedad solo se aplica a columnas o parámetros de tipo **char** o **varchar**.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: entrada no válida|  
  
## <a name="return-types"></a>Tipos de valores devueltos
 **int**  
  
## <a name="exceptions"></a>Excepciones  
Devuelve NULL si se produce un error o si el autor de la llamada no tiene permiso para ver el objeto.
  
Un usuario solo puede ver los metadatos de elementos protegibles que posea o para los que se le haya concedido permiso. Esto significa que las funciones integradas de emisión de metadatos, como `COLUMNPROPERTY`, es posible que devuelvan NULL si el usuario no tiene el permiso correcto para el objeto. Vea [Configuración de visibilidad de los metadatos](../../relational-databases/security/metadata-visibility-configuration.md) para obtener más información.
  
## <a name="remarks"></a>Observaciones  
Cuando se comprueba la propiedad determinista de una columna, primero se comprueba si se trata de una columna calculada. El argumento **IsDeterministic** devuelve NULL para las columnas no calculadas. Las columnas calculadas se pueden especificar como columnas de índice.
  
## <a name="examples"></a>Ejemplos  
En este ejemplo se devuelve la longitud de la columna `LastName`.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT COLUMNPROPERTY( OBJECT_ID('Person.Person'),'LastName','PRECISION')AS 'Column Length';  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Column Length
-------------
50
```  
  
## <a name="see-also"></a>Vea también
[Funciones de metadatos &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)  
[TYPEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/typeproperty-transact-sql.md)
  
  
