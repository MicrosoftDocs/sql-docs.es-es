---
description: sys.fulltext_languages (Transact-SQL)
title: sys.fulltext_languages (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fulltext_languages
- sys.fulltext_languages_TSQL
- fulltext_languages
- fulltext_languages_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- languages [full-text search]
- sys.fulltext_languages catalog view
ms.assetid: 2ed6b53d-1cf2-4763-9d58-36ea24a610ef
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 47032ad75fb96842f278533e184307b99017c798
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191500"
---
# <a name="sysfulltext_languages-transact-sql"></a>sys.fulltext_languages (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Esta vista de catálogo contiene una fila por cada idioma cuyos separadores de palabras estén registrados en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cada fila muestra el LCID y el nombre del idioma. Cuando se registran separadores de palabras para un idioma, los demás recursos lingüísticos (lematizadores, palabras irrelevantes (palabras irrelevantes) y archivos de Diccionario de sinónimos) están disponibles para las operaciones de indización y consulta de texto completo. El valor de **Name** o **LCID** se puede especificar en las instrucciones de texto completo y en las instrucciones de índice de texto completo [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
   
|Columna|Tipo de datos|Descripción|  
|------------|---------------|-----------------|  
|**lcid**|**int**|Identificador de configuración regional (LCID) de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows para el idioma.|  
|**name**|**sysname**|Es el valor del alias en [sys.sysidiomas](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md) correspondientes al valor de **LCID** o la representación de cadena del LCID numérico.|  
  
## <a name="values-returned-for-default-languages"></a>Valores devueltos para los idiomas predeterminados  
 En la tabla siguiente se muestran los valores para los idiomas cuyos separadores de palabras están registrados de forma predeterminada.  
  
|Lenguaje|LCID|  
|--------------|----------|  
|Árabe|1025|  
|Bengali (India)|1093|  
|British English|2057|  
|Búlgaro|1026|  
|Catalán|1027|  
|Chino (Hong Kong ZAE, RPC)|3076|  
|Chinese (Macao SAR)|5124|  
|Chinese (Singapore)|4100|  
|Croata|1050|  
|Checo|1029|  
|Danés|1030|  
|Neerlandés|1043|  
|Inglés|3082|  
|Francés|1036|  
|Alemán|1031|  
|**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> Griego|1032|  
|Gujarati|1095|  
|Hebreo|1037|  
|Hindi|1081|  
|Islandés|1039|  
|Indonesio|1057|  
|Italiano|1040|  
|Japonés|1041|  
|Canarés|1099|  
|Coreano|1042|  
|Letón|1062|  
|Lituano|1063|  
|Malay - Malaysia|1086|  
|Malayalam|1100|  
|Maratí|1102|  
|Neutra|0|  
|Noruego (Bokmål)|1044|  
|**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> Polaco|1045|  
|Portugués (Brasil)|1046|  
|Portugués (Portugal)|2070|  
|Punjabi|1094|  
|Rumano|1048|  
|Ruso|1049|  
|Serbio (cirílico)|3098|  
|Serbio (latino)|2074|  
|Chino simplificado|2052|  
|Eslovaco|1051|  
|Esloveno|1060|  
|Español|3082|  
|Sueco|1053|  
|Tamil|1097|  
|Telugu|1098|  
|Tailandés|1054|  
|Chino tradicional|1028|  
|**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> Turco|1055|  
|Ucraniano|1058|  
|Urdu|1056|  
|Vietnamita|1066|  
  
## <a name="remarks"></a>Observaciones  
 Para actualizar la lista de idiomas registrados en la búsqueda de texto completo, use [sp_fulltext_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md)"**update_languages**".  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]  
  
## <a name="see-also"></a>Consulte también  
 [sp_fulltext_load_thesaurus_file &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-fulltext-load-thesaurus-file-transact-sql.md)   
 [sp_fulltext_service &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md)   
 [Configurar y administrar separadores de palabras y lematizadores para la búsqueda](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md)   
 [Configurar y administrar archivos de sinónimos para búsquedas de Full-Text](../../relational-databases/search/configure-and-manage-thesaurus-files-for-full-text-search.md)   
 [Configurar y administrar palabras irrelevantes y listas de palabras irrelevantes para la búsqueda de texto completo](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)   
 [Actualizar la búsqueda de texto completo](../../relational-databases/search/upgrade-full-text-search.md)  
  
  
