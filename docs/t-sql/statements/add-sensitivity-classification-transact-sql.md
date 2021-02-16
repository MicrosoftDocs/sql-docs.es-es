---
description: ADD SENSITIVITY CLASSIFICATION (Transact-SQL)
title: ADD SENSITIVITY CLASSIFICATION (Transact-SQL)
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
author: DavidTrigano
ms.author: datrigan
ms.reviewer: vanto
f1_keywords:
- ADD SENSITIVITY CLASSIFICATION
- ADD_SENSITIVITY_CLASSIFICATION
helpviewer_keywords:
- ADD SENSITIVITY CLASSIFICATION statement
- add labels
- adding labels
- adding labels
- classification [SQL]
- labels [SQL]
- information types
- data classification
- rank
ms.custom: ''
ms.date: 06/10/2020
monikerRange: " >= sql-server-linux-ver15 || >= sql-server-ver15 || = azuresqldb-current"
ms.openlocfilehash: 09bf980c0bfa6e07a9d2de35519439c8b18da039
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348586"
---
# <a name="add-sensitivity-classification-transact-sql"></a>ADD SENSITIVITY CLASSIFICATION (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Agrega metadatos sobre la clasificación de confidencialidad a una o varias columnas de base de datos. La clasificación puede incluir una etiqueta de confidencialidad y un tipo de información.

Para SQL Server, se incluyó por primera vez en SQL Server 2019.

La clasificación de datos confidenciales en su entorno de base de datos le ayuda a lograr mayor visibilidad y una mejor protección. Puede encontrar información adicional en [Clasificación y detección de datos de Azure SQL Database](/azure/azure-sql/database/data-discovery-and-classification-overview).

## <a name="syntax"></a>Sintaxis

```syntaxsql
    ADD SENSITIVITY CLASSIFICATION TO
    <object_name> [, ...n ]
    WITH ( <sensitivity_option> [, ...n ] )

<object_name> ::=
{
    [schema_name.]table_name.column_name
}

<sensitivity_option> ::=  
{
    LABEL = string |
    LABEL_ID = guidOrString |
    INFORMATION_TYPE = string |
    INFORMATION_TYPE_ID = guidOrString |
    RANK = NONE | LOW | MEDIUM | HIGH | CRITICAL
}
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos  

*object_name* ([schema_name.]table_name.column_name)

Es el nombre de la columna de base de datos que se va a clasificar. Actualmente solo se admite la clasificación de columnas.
    - *schema_name* (opcional): es el nombre del esquema al que pertenece la columna clasificada.
    - *table_name*: es el nombre de la tabla a la que pertenece la columna clasificada.
    - *column_name*: es el nombre de la columna que se va a clasificar.

*LABEL*

Es el nombre legible de la etiqueta de confidencialidad. Las etiquetas de confidencialidad representan la confidencialidad de los datos almacenados en la columna de base de datos.

*LABEL_ID*

Es un identificador asociado a la etiqueta de confidencialidad. A menudo se usa con plataformas de protección de información centralizadas para identificar etiquetas de forma única en el sistema.

*INFORMATION_TYPE*

Es el nombre legible del tipo de información. Los tipos de información se utilizan para describir el tipo de datos que se almacenan en la columna de base de datos.

*INFORMATION_TYPE_ID*

Es un identificador asociado con el tipo de información. A menudo se usa con plataformas de protección de información centralizadas para identificar tipos de información de forma única en el sistema.

*RANK*

Es un identificador basado en un conjunto de valores predefinidos que definen el rango de sensibilidad. Lo usan otros servicios como Advanced Threat Protection para detectar anomalías en función de su rango.

## <a name="remarks"></a>Observaciones  

- Solo se puede agregar una clasificación a un único objeto. La adición de una clasificación a un objeto que ya está clasificado sobrescribirá la clasificación existente.
- Se pueden clasificar varios objetos con una sola instrucción `ADD SENSITIVITY CLASSIFICATION`.
- La vista del sistema [sys.sensitivity_classifications](../../relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql.md) puede usarse para recuperar la información de clasificación de confidencialidad para una base de datos.

## <a name="permissions"></a>Permisos

Requiere el permiso ALTER ANY SENSITIVITY CLASSIFICATION. ALTER ANY SENSITIVITY CLASSIFICATION está implícito en el permiso de base de datos ALTER, o en el permiso de servidor CONTROL SERVER.

## <a name="examples"></a>Ejemplos  

### <a name="a-classifying-two-columns"></a>A. Clasificación de dos columnas

En el ejemplo siguiente se clasifican las columnas **dbo.sales.price** y **dbo.sales.discount** con la etiqueta de confidencialidad **Highly Confidential**, la clasificación **Critical** y el tipo de información **Financial**.

```sql
ADD SENSITIVITY CLASSIFICATION TO
    dbo.sales.price, dbo.sales.discount
    WITH ( LABEL='Highly Confidential', INFORMATION_TYPE='Financial', RANK=CRITICAL )
```  

### <a name="b-classifying-only-a-label"></a>B. Clasificación de solo una etiqueta

En el ejemplo siguiente se clasifica la columna **dbo.customer.comments** con la etiqueta **Confidential** y el identificador de etiqueta **643f7acd-776a-438d-890c-79c3f2a520d6**. El tipo de información no está clasificado en esta columna.

```sql
ADD SENSITIVITY CLASSIFICATION TO
    dbo.customer.comments
    WITH ( LABEL='Confidential', LABEL_ID='643f7acd-776a-438d-890c-79c3f2a520d6' )
```  

## <a name="see-also"></a>Consulte también

- [DROP SENSITIVITY CLASSIFICATION (Transact-SQL)](../../t-sql/statements/drop-sensitivity-classification-transact-sql.md)
- [sys.sensitivity_classifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql.md)
- [Permisos (motor de base de datos)](../../relational-databases/security/permissions-database-engine.md)
- [Clasificación y detección de datos de Azure SQL Database](/azure/azure-sql/database/data-discovery-and-classification-overview)