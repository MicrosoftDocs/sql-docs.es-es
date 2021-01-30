---
description: Funciones de búsqueda de texto completo y de búsqueda semántica (Transact-SQL)
title: Full-Text funciones de búsqueda y de búsqueda semántica (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- semantic search [SQL Server], system functions
ms.assetid: a61a3694-7604-4583-962e-fc30f771c6fa
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 29b1abedf27c163cd21ab0113896c4a3ebaae459
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207463"
---
# <a name="full-text-search-and-semantic-search-functions-transact-sql"></a>Funciones de búsqueda de texto completo y de búsqueda semántica (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En esta sección se describen las funciones del sistema relacionadas con la búsqueda semántica y de texto completo.  
  
## <a name="full-text-search-functions"></a>Funciones de búsqueda de texto completo  
 [CONTAINSTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/containstable-transact-sql.md)  
 Devuelve una tabla con cero, una o más filas para las columnas que contienen coincidencias exactas o aproximadas (menos precisas) de palabras o frases únicas, palabras próximas a otra dada (dentro de una distancia determinada) o coincidencias ponderadas.  
  
 [FREETEXTTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/freetexttable-transact-sql.md)  
 Devuelve una tabla de cero, una o más filas para las columnas que contienen valores que coinciden con el significado, y no solo las palabras exactas, del texto del *freetext_string* especificado.  
  
## <a name="semantic-search-functions"></a>Funciones de búsqueda semántica  
 [semantickeyphrasetable &#40;Transact-SQL&#41;](../../relational-databases/system-functions/semantickeyphrasetable-transact-sql.md)  
 Devuelve una tabla con cero, una o más filas para las frases clave asociadas a columnas de la tabla especificada.  
  
 [semanticsimilaritydetailstable &#40;Transact-SQL&#41;](../../relational-databases/system-functions/semanticsimilaritydetailstable-transact-sql.md)  
 Devuelve una tabla de cero, una o más filas de frases clave comunes en dos documentos (un documento origen y un documento coincidente) cuyo contenido es similar semánticamente.  
  
 [semanticsimilaritytable &#40;Transact-SQL&#41;](../../relational-databases/system-functions/semanticsimilaritytable-transact-sql.md)  
 Devuelve una tabla de cero, una, o más filas para las columnas cuyo contenido es similar semánticamente a un documento especificado.  
  
  
