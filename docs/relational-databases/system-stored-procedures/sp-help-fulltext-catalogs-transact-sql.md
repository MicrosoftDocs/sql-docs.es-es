---
description: sp_help_fulltext_catalogs (Transact-SQL)
title: sp_help_fulltext_catalogs (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_fulltext_catalogs_TSQL
- sp_help_fulltext_catalogs
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_fulltext_catalogs
ms.assetid: 1b94f280-e095-423f-88bc-988c9349d44c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 12382d003a5f1207506164c35b171a9c1a5e5385
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198325"
---
# <a name="sp_help_fulltext_catalogs-transact-sql"></a>sp_help_fulltext_catalogs (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve el identificador, el nombre, el directorio raíz, el estado y el número de tablas indizadas de texto completo del catálogo de texto completo especificado.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] En su lugar, use la vista de catálogo [Sys.fulltext_catalogs](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md) .  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_help_fulltext_catalogs [ @fulltext_catalog_name = ] 'fulltext_catalog_name'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @fulltext_catalog_name = ] 'fulltext_catalog_name'` Es el nombre del catálogo de texto completo. *fulltext_catalog_name* es de **tipo sysname**. Si este parámetro se omite o tiene el valor NULL, se presenta información acerca de todos los catálogos de texto completo asociados con la base de datos actual.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 Esta tabla muestra el conjunto de resultados, ordenado por **ftcatid**.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**fulltext_catalog_id**|**smallint**|Identificador del catálogo de texto completo.|  
|**NOMBRE**|**sysname**|Nombre del catálogo de texto completo.|  
|**PATH**|**nvarchar(260)**|A partir de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], esta cláusula no tiene ningún efecto.|  
|**ESTADO**|**int**|Estado de rellenado del índice de texto completo del catálogo:<br /><br /> 0 = Inactivo<br /><br /> 1 = Rellenado completo en curso<br /><br /> 2 = En pausa<br /><br /> 3 = Acelerado<br /><br /> 4 = En recuperación<br /><br /> 5 = Apagado<br /><br /> 6 = Rellenado incremental en curso<br /><br /> 7 = Generación del índice<br /><br /> 8 = El disco está lleno. En pausa<br /><br /> 9 = Seguimiento de cambios<br /><br /> NULL = El usuario no tiene permiso VIEW en el catálogo de texto completo, la base de datos no está habilitada para texto completo o el componente de texto completo no está instalado.|  
|**NUMBER_FULLTEXT_TABLES**|**int**|Número de tablas con índice de texto completo asociadas al catálogo.|  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, los permisos de ejecución corresponden a los miembros del rol **public** .  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se devuelve información acerca del catálogo de texto completo `Cat_Desc`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_help_fulltext_catalogs 'Cat_Desc' ;  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [FULLTEXTCATALOGPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/fulltextcatalogproperty-transact-sql.md)   
 [sp_fulltext_catalog &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-fulltext-catalog-transact-sql.md)   
 [sp_help_fulltext_catalogs_cursor &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-cursor-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
