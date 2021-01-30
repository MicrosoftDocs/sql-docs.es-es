---
description: sp_fulltext_semantic_register_language_statistics_db (Transact-SQL)
title: sp_fulltext_semantic_register_language_statistics_db (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_fulltext_semantic_register_language_statistics_db
- sp_fulltext_semantic_register_language_statistics_db_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_fulltext_semantic_register_language_statistics_db
ms.assetid: bef1b104-5a44-4327-9ae4-45eae3000f7e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: dba0092fdad469c7ce8d762cfd16efcf5f75d6ef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193552"
---
# <a name="sp_fulltext_semantic_register_language_statistics_db-transact-sql"></a>sp_fulltext_semantic_register_language_statistics_db (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Registra una base de datos previamente rellenada de estadísticas semánticas de lenguaje en la instancia actual de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Puede iniciar la extracción semántica solo después de haber adjuntado esta base de datos de estadísticas de lenguaje y haberla registrado utilizando este procedimiento almacenado. Solo necesita realizar esta tarea una vez para cada instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
EXEC sp_fulltext_semantic_register_language_statistics_db  
    [ @dbname = ] 'database_name';  
GO  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Argumentos  
 [ @dbname =] '*database_name*'  
 Es el nombre de la base de datos de estadísticas semánticas de lenguaje que se va a registrar para la instancia actual de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La base de datos debe estar ya adjunta. *database_name* es de **tipo sysname** y no puede ser null.  
  
## <a name="return-code-value"></a>Valor de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-set"></a>Tipo de cursor  
 Ninguno.  
  
## <a name="general-remarks"></a>Notas generales  
 La base de datos de estadísticas semánticas de lenguaje contiene estadísticas relacionadas con el lenguaje que se requieren para el procesamiento semántico de contenido de texto.  
  
 **sp_fulltext_semantic_register_language_statistics_db** realiza los pasos siguientes:  
  
1.  Comprueba que la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es una versión que admite el procesamiento semántico.  
  
2.  Comprueba que la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no tiene definida una base de datos de estadísticas semánticas de lenguaje.  
  
3.  Comprueba que la base de datos es una base de datos de estadísticas semánticas de lenguaje válida.  
  
4.  Establece permisos en la base de datos de estadísticas semánticas de lenguaje para restringir el acceso de los usuarios a la base de datos.  
  
5.  Inserta los metadatos que definen el nombre de la base de datos de estadísticas semánticas de lenguaje para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
6.  Inserta los metadatos que definen las asignaciones entre la base de datos de estadísticas semánticas de lenguaje instalada y las tablas internas de modelo de idioma.  
  
7.  Comprueba que la base de datos esté lista para usarse.  
  
 Para obtener más información, vea [Instalar y configurar la búsqueda semántica](../../relational-databases/search/install-and-configure-semantic-search.md).  
  
## <a name="metadata"></a>Metadatos  
 Para obtener información acerca de la base de datos de Estadísticas de lenguaje semántico instalada en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consulte la vista de catálogo [sys.fulltext_semantic_language_statistics_database &#40;&#41;de TRANSACT-SQL ](../../relational-databases/system-catalog-views/sys-fulltext-semantic-language-statistics-database-transact-sql.md).  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 Se requieren permisos CONTROL SERVER.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se muestra cómo registrar la base de datos de Estadísticas de lenguaje semántico llamando a **sp_fulltext_semantic_register_language_statistics_db**.  
  
```sql  
EXEC sp_fulltext_semantic_register_language_statistics_db @dbname = 'semanticsDb';  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Instalar y configurar la búsqueda semántica](../../relational-databases/search/install-and-configure-semantic-search.md)  
  
  
