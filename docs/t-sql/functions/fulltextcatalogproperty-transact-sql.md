---
description: FULLTEXTCATALOGPROPERTY (Transact-SQL)
title: FULLTEXTCATALOGPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FULLTEXTCATALOGPROPERTY_TSQL
- FULLTEXTCATALOGPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- full-text catalogs [SQL Server], properties
- FULLTEXTCATALOGPROPERTY function
- status information [SQL Server], full-text catalogs
ms.assetid: f841dc79-2044-4863-aff0-56b8bb61f250
author: cawrites
ms.author: chadam
ms.openlocfilehash: d49757197a66a0af05a0115cf4b16d6d589acc13
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237494"
---
# <a name="fulltextcatalogproperty-transact-sql"></a>FULLTEXTCATALOGPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Devuelve información acerca de las propiedades de catálogo de texto completo de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
FULLTEXTCATALOGPROPERTY ('catalog_name' ,'property')  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
  
> [!NOTE]  
>  Se quitarán las propiedades siguientes de una versión futura de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **LogSize** y **PopulateStatus**. Evite el uso de estas propiedades en nuevos trabajos de desarrollo y piense en modificar las aplicaciones que las usan actualmente.  
  
_catalog\_name_  
Es una expresión que contiene el nombre del catálogo de texto completo.  
  
_property_  
Es una expresión que contiene el nombre de la propiedad del catálogo de texto completo. La tabla presenta las propiedades y proporciona descripciones de la información que se devuelve.  
  
|Propiedad|Descripción|  
|--------------|-----------------|  
|**AccentSensitivity**|Opción de distinción de acentos.<br /><br /> 0 = No distinguir acentos<br /><br /> 1 = Distinguir acentos|  
|**IndexSize**|Tamaño lógico del catálogo de texto completo en megabytes (MB). Incluye el tamaño de los índices semánticos de similitud de documentos y frases clave.<br /><br /> Para obtener más información, vea la sección "Comentarios" más adelante en este tema.|  
|**ItemCount**|Número de elementos indexados, lo que incluye todos los índices de similitud de documentos, frases clave y texto completo en un catálogo|  
|**LogSize**|Se admite únicamente por compatibilidad con versiones anteriores. Siempre devuelve 0.<br /><br /> Tamaño, en bytes, del conjunto combinado de los registros de errores asociados a un catálogo de texto completo de los servicios de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Search.|  
|**MergeStatus**|Indica si hay en curso una combinación maestra.<br /><br /> 0 = la combinación maestra no está en curso<br /><br /> 1 = la combinación maestra está en curso.|  
|**PopulateCompletionAge**|Diferencia, en segundos, entre la terminación del último rellenado del índice de texto completo y 01/01/1990 00:00:00.<br /><br /> Solo se actualiza para rastreos completos e incrementales. Devuelve 0 si no se ha producido ningún rellenado.|  
|**PopulateStatus**|0 = Inactivo<br /><br /> 1 = Rellenado completo en curso<br /><br /> 2 = En pausa<br /><br /> 3 = Acelerado<br /><br /> 4 = En recuperación<br /><br /> 5 = Apagado<br /><br /> 6 = Rellenado incremental en curso<br /><br /> 7 = Generación del índice<br /><br /> 8 = El disco está lleno. En pausa.<br /><br /> 9 = Seguimiento de cambios|  
|**UniqueKeyCount**|Número de claves únicas en el catálogo de texto completo.|  
|**ImportStatus**|Indica si se va a importar el catálogo de texto completo.<br /><br /> 0 = no se va a importar el catálogo de texto completo.<br /><br /> 1 = Si se va a importar el catálogo de texto completo.|  
  
## <a name="return-types"></a>Tipos de valor devuelto  
**int**  
  
## <a name="exceptions"></a>Excepciones  
Devuelve NULL si se produce un error o si el autor de la llamada no tiene permiso para ver el objeto.  
  
En [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], un usuario solo puede ver los metadatos de elementos protegibles. Estos elementos protegibles son los que posee el usuario o para los que se le ha concedido permiso. Como tales, las funciones integradas que emiten metadatos, como FULLTEXTCATALOGPROPERTY, pueden devolver NULL si el usuario no tiene permisos sobre el objeto. Para más información, vea [sp_help_fulltext_catalogs &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-transact-sql.md).  
  
## <a name="remarks"></a>Observaciones  
FULLTEXTCATALOGPROPERTY ('_catalog\_name_','**IndexSize**') solo examina fragmentos con el estado 4 o 6, como se muestra en [sys.fulltext_index_fragments](../../relational-databases/system-catalog-views/sys-fulltext-index-fragments-transact-sql.md). Estos fragmentos forman parte del índice lógico. Por lo tanto, la propiedad **IndexSize** solo devuelve el tamaño del índice lógico. 

Sin embargo, durante una mezcla de índice, el tamaño de índice real podría ser el doble de su tamaño lógico. Para encontrar el tamaño real que está usando un índice de texto completo durante una combinación, use el procedimiento almacenado del sistema [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md). Ese procedimiento mira en todos los fragmentos asociados a un índice de texto completo. 

El rellenado de texto completo puede producir un error si restringe el crecimiento del archivo de catálogo de texto completo y no permite suficiente espacio para el proceso de combinación. En este caso, FULLTEXTCATALOGPROPERTY ("_catalog\_name_','**IndexSize**") devuelve 0 y se escribe el siguiente error en el regidtro de texto completo:  
  
`Error: 30059, Severity: 16, State: 1. A fatal error occurred during a full-text population and caused the population to be cancelled. Population type is: FULL; database name is FTS_Test (id: 13); catalog name is t1_cat (id: 5); table name t1 (id: 2105058535). Fix the errors that are logged in the full-text crawl log. Then, resume the population. The basic Transact-SQL syntax for this is: ALTER FULLTEXT INDEX ON table_name RESUME POPULATION.`  
  
Es importante que las aplicaciones no experimenten bucles de espera; para ello, compruebe si la propiedad **PopulateStatus** indica inactividad. Esto significaría que el rellenado se ha completado. Esta comprobación excluye los ciclos de CPU de la base de datos y los procesos de búsqueda de texto completo, y provoca tiempos de espera. Normalmente, es mejor comprobar la propiedad **PopulateStatus** en el nivel de tabla, **TableFullTextPopulateStatus**, de la función del sistema OBJECTPROPERTYEX. Ésta y otras nuevas propiedades de texto completo en OBJECTPROPERTYEX proporcionan información más detallada sobre las tablas de indización de texto completo. Para obtener más información, vea [OBJECTPROPERTYEX &#40;Transact-SQL&#41;](../../t-sql/functions/objectpropertyex-transact-sql.md).  
  
## <a name="examples"></a>Ejemplos  
En el ejemplo siguiente se devuelve el número de elementos de texto completo indizados que se encuentran en el catálogo de texto completo `Cat_Desc`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT fulltextcatalogproperty('Cat_Desc', 'ItemCount');  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
[FULLTEXTSERVICEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/fulltextserviceproperty-transact-sql.md)   
[Funciones de metadatos &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)   
[sp_help_fulltext_catalogs &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-transact-sql.md)  
  
  
