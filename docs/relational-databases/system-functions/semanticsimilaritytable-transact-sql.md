---
description: semanticsimilaritytable (Transact-SQL)
title: semanticsimilaritytable (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- semanticsimilaritytable
- semanticsimilaritytable_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- semanticsimilaritytable function
ms.assetid: b49d40ab-7552-438b-ad67-6237dcccb75b
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 532ec509de094db343406b6cb0f9f0cca924a724
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194560"
---
# <a name="semanticsimilaritytable-transact-sql"></a>semanticsimilaritytable (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una tabla de cero, una o más filas para los documentos cuyo contenido de las columnas especificadas sea similar semánticamente a un documento indicado.  
  
 Se puede hacer referencia a esta función de conjunto de filas en la cláusula FROM de una instrucción SELECT como un nombre de tabla normal.  

 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
SEMANTICSIMILARITYTABLE  
    (  
    table,  
    { column | (column_list) | * },  
    source_key  
    )  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Argumentos  
 **table**  
 Nombre de una tabla con indización de texto completo y semántica habilitada.  
  
 Este nombre puede ser un nombre de una a cuatro partes, pero no se permite un nombre de servidor remoto.  
  
 **column**  
 Nombre de la columna indizada para la que deben devolverse resultados. La columna debe tener habilitada la indización semántica.  
  
 **lista_de_columnas**  
 Indica varias columnas, separadas por comas y escritas entre paréntesis. Todas las columnas deben tener habilitada la indización semántica.  
  
 **\** _  
 Indica que se incluyen todas las columnas que tienen la indización semántica habilitada.  
  
 _ *source_key**  
 Clave única de la fila, para solicitar los resultados de una fila concreta.  
  
 La clave se convierte implícitamente al tipo de la clave única de texto completo en la tabla de origen siempre que sea posible. La clave se puede especificar como una constante o como una variable, pero no puede ser una expresión ni el resultado de una subconsulta escalar.  
  
## <a name="table-returned"></a>Tabla devuelta  
 En la tabla siguiente se describe la información acerca de documentos similares o relacionados que esta función de conjunto de filas devuelve.  
  
 Los documentos coincidentes se devuelven por columna si se solicitan resultados de más de una columna.  
  
|Column_name|Tipo|Descripción|  
|------------------|----------|-----------------|  
|**source_column_id**|**int**|Identificador de la columna de la que se usó un documento de origen para buscar documentos similares.<br /><br /> Vea las funciones COL_NAME y COLUMNPROPERTY para obtener información detallada sobre cómo recuperar el nombre de columna desde column_id y viceversa.|  
|**matched_column_id**|**int**|Identificador de la columna de la que se encontró un documento similar.<br /><br /> Vea las funciones COL_NAME y COLUMNPROPERTY para obtener información detallada sobre cómo recuperar el nombre de columna desde column_id y viceversa.|  
|**matched_document_key**|**\** _<br /><br /> Esta clave coincide con el tipo de la clave única de la tabla de origen.|Valor de clave única de extracción semántica y de texto completo del documento o la fila que resultaron ser similares al documento especificado en la consulta.|  
|_ *puntuación**|**REAL**|Valor relativo de similitud para este documento en su relación con todos los demás documentos similares.<br /><br /> El valor es un valor fraccionario decimal en el intervalo de [0.0, 1.0] donde una puntuación superior representa una coincidencia más próxima y 1.0 es una puntuación perfecta.|  
  
## <a name="general-remarks"></a>Notas generales  
 Para obtener más información, vea [buscar documentos similares y relacionados con la búsqueda semántica](../../relational-databases/search/find-similar-and-related-documents-with-semantic-search.md).  
  
## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones  
 No se puede consultar en varias columnas para documentos similares. La función **SEMANTICSIMILARITYTABLE** solo recupera documentos similares de la misma columna que la columna de origen, que se identifica mediante el argumento **source_key** .  
  
## <a name="metadata"></a>Metadatos  
 Para obtener información y estado sobre la extracción y el rellenado de similitud semánticos, consulte las siguientes vistas de administración dinámica:  
  
-   [sys.dm_db_fts_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-fts-index-physical-stats-transact-sql.md)  
  
-   [sys.dm_fts_semantic_similarity_population &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-semantic-similarity-population-transact-sql.md)  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 Necesita permisos SELECT en la tabla base en la que se crearon los índices semánticos y de texto completo.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se recuperan los 10 candidatos principales que son similares a un candidato especificado de la tabla HumanResources.JobCandidate de la base de datos de ejemplo AdventureWorks2012.  
  
```scr  
SELECT TOP(10) KEY_TBL.matched_document_key AS Candidate_ID  
FROMSEMANTICSIMILARITYTABLE  
    (  
    HumanResources.JobCandidate,  
    Resume,  
    @CandidateID  
    ) AS KEY_TBL  
ORDER BY KEY_TBL.score DESC;  
  
```  
  
  
