---
description: semanticsimilaritydetailstable (Transact-SQL)
title: semanticsimilaritydetailstable (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- semanticsimilaritydetailstable
- semanticsimilaritydetailstable_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- semanticsimilaritydetailstable function
ms.assetid: 038d751a-fca5-4b4c-9129-cba741a4e173
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: f26754f935338d586ebe958df9f49f99c189c414
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207328"
---
# <a name="semanticsimilaritydetailstable-transact-sql"></a>semanticsimilaritydetailstable (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una tabla de cero, una o más filas de frases clave comunes en dos documentos (un documento de origen y un documento coincidente) cuyo contenido sea similar semánticamente.  
  
 Se puede hacer referencia a esta función de conjunto de filas en la cláusula FROM de una instrucción SELECT 
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
SEMANTICSIMILARITYDETAILSTABLE  
    (  
    table,  
    source_column,  
    source_key,  
    matched_column,  
    matched_key  
    )  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Argumentos  
 **table**  
 Nombre de una tabla con indización de texto completo y semántica habilitada.  
  
 Este nombre puede ser un nombre de una a cuatro partes, pero no se permite un nombre de servidor remoto.  
  
 **source_column**  
 Nombre de la columna de la fila de origen que tiene el contenido cuya similitud se va a comparar.  
  
 **source_key**  
 Clave única que representa la fila del documento de origen.  
  
 Esta clave se convierte implícitamente al tipo de la clave única de texto completo en la tabla de origen siempre que sea posible. La clave se puede especificar como una constante o como una variable, pero no puede ser una expresión ni el resultado de una subconsulta escalar. Si se especifica una clave no válida, no se devuelve ninguna fila.  
  
 **matched_column**  
 Nombre de la columna de la fila coincidente que tiene el contenido cuya similitud se va a comparar.  
  
 **matched_key**  
 Clave única que representa la fila del documento coincidente.  
  
 Esta clave se convierte implícitamente al tipo de la clave única de texto completo en la tabla de origen siempre que sea posible. La clave se puede especificar como una constante o como una variable, pero no puede ser una expresión ni el resultado de una subconsulta escalar.  
  
## <a name="table-returned"></a>Tabla devuelta  
 En la tabla siguiente se describe la información sobre las frases clave que devuelve esta función de conjunto de filas.  
  
|Column_name|Tipo|Descripción|  
|------------------|----------|-----------------|  
|**frase clave**|**NVARCHAR**|Frase clave que contribuye a la similitud entre el documento de origen y el documento coincidente.|  
|**score**|**REAL**|Valor relativo de esta frase clave en su relación con todas las demás frases clave que son similares entre los dos documentos.<br /><br /> El valor es un valor fraccionario decimal en el intervalo de [0.0, 1.0] donde una puntuación superior representa una ponderación mayor y 1.0 es la puntuación perfecta.|  
  
## <a name="general-remarks"></a>Notas generales  
 Para obtener más información, vea [buscar documentos similares y relacionados con la búsqueda semántica](../../relational-databases/search/find-similar-and-related-documents-with-semantic-search.md).  
  
## <a name="metadata"></a>Metadatos  
 Para obtener información y estado sobre la extracción y el rellenado de similitud semánticos, consulte las siguientes vistas de administración dinámica:  
  
-   [sys.dm_db_fts_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-fts-index-physical-stats-transact-sql.md)  
  
-   [sys.dm_fts_semantic_similarity_population &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-semantic-similarity-population-transact-sql.md)  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 Necesita permisos SELECT en la tabla base en la que se crearon los índices semánticos y de texto completo.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se recuperan las 5 frases clave que tenían la máxima puntuación de similitud entre los candidatos especificados en la tabla **humanresources. JobCandidate** de la base de datos de ejemplo AdventureWorks2012. Las @CandidateId @MatchedID variables y representan los valores de la columna de clave del índice de texto completo.  
  
```sql  
SELECT TOP(5) KEY_TBL.keyphrase, KEY_TBL.score  
FROMSEMANTICSIMILARITYDETAILSTABLE  
    (  
    HumanResources.JobCandidate,  
    Resume, @CandidateID,  
    Resume, @MatchedID  
    ) AS KEY_TBL  
ORDER BY KEY_TBL.score DESC;  
  
```  
  
  
