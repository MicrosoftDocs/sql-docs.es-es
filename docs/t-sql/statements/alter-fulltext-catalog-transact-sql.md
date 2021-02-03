---
description: ALTER FULLTEXT CATALOG (Transact-SQL)
title: ALTER FULLTEXT CATALOG (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_FULLEXT_CATALOG_TSQL
- ALTER FULLEXT CATALOG
dev_langs:
- TSQL
helpviewer_keywords:
- modifying full-text catalogs
- full-text catalogs [SQL Server], rebuilding
- accent sensitivity
- ALTER FULLTEXT CATALOG statement
- full-text catalogs [SQL Server], modifying
- full-text catalogs [SQL Server], reorganizing
ms.assetid: 31a47aaf-6c7f-48a4-a86a-d57aec66c9cb
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: aedb9f32a845a7b25d856c8bfa5aff20facbf4a4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204239"
---
# <a name="alter-fulltext-catalog-transact-sql"></a>ALTER FULLTEXT CATALOG (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Cambia las propiedades de un catálogo de texto completo.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql 
ALTER FULLTEXT CATALOG catalog_name   
{ REBUILD [ WITH ACCENT_SENSITIVITY = { ON | OFF } ]  
| REORGANIZE  
| AS DEFAULT   
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *catalog_name*  
 Especifica el nombre del catálogo que se va a modificar. Si no existe un catálogo con el nombre especificado, [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve un error y no realiza la operación ALTER.  
  
 REBUILD  
 Indica a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que vuelva a generar todo el catálogo. Cuando vuelve a generarse un catálogo, el catálogo existente se elimina y se crea uno nuevo en su lugar. Todas las tablas que tienen referencias de índices de texto completo se asocian al catálogo nuevo. La regeneración restablece los metadatos de texto completo de las tablas del sistema de la base de datos.  
  
 WITH ACCENT_SENSITIVITY = {ON|OFF}  
 Especifica si el catálogo que se va a modificar distingue o no los acentos para la indización y las consultas de texto completo.  
  
 Para determinar la configuración de la propiedad de distinción de acentos actual de un catálogo de texto completo, use la función FULLTEXTCATALOGPROPERTY con el valor de propiedad **accentsensitivity** en *catalog_name*. Si la función devuelve '1', el catálogo de texto completo distingue acentos; si la función devuelve '0', el catálogo no distingue acentos.  
  
 La distinción de acentos predeterminada para el catálogo y la base de datos es la misma.  
  
 REORGANIZE  
 Indica a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que realice una *combinación maestra*, que combina en un índice mayor los índices más pequeños creados en el proceso de indización. La combinación de los fragmentos de índices de texto completo puede mejorar el rendimiento y liberar recursos de disco y memoria. Si se realizan cambios frecuentes en el catálogo de texto completo, utilice este comando de manera periódica para reorganizarlo.  
  
 REORGANIZE también optimiza la estructura interna de los índices y del catálogo.  
  
 Tenga en cuenta que, en función de la cantidad de datos indizados, una combinación maestra puede llevar cierto tiempo. La combinación maestra de una cantidad grande de datos puede crear una transacción de larga duración, con lo que se retrasa el truncamiento del registro de transacciones durante el punto de comprobación. En este caso, el registro de transacciones podría crecer significativamente bajo el modelo de recuperación completa. Como práctica recomendada, asegúrese de que su registro de transacciones contenga el espacio suficiente para una transacción de larga duración antes de reorganizar un índice de texto completo grande en una base de datos que use el modelo de recuperación completa. Para obtener más información, vea [Administrar el tamaño del archivo de registro de transacciones](../../relational-databases/logs/manage-the-size-of-the-transaction-log-file.md).  
  
 AS DEFAULT   
 Especifica que este catálogo es el predeterminado. Cuando se crean índices de texto completo sin especificar catálogos, se utiliza el catálogo predeterminado. Si hay un catálogo de texto completo predeterminado, al establecer este catálogo como AS DEFAULT, se invalidará el catálogo predeterminado existente.  
  
## <a name="permissions"></a>Permisos  
 El usuario debe tener el permiso ALTER en el catálogo de texto completo o ser un miembro de los roles fijos de base de datos **db_owner** o **db_ddladmin**, o del rol fijo de servidor sysadmin.  
  
> [!NOTE]  
>  Para utilizar ALTER FULLTEXT CATALOG AS DEFAULT, el usuario debe disponer del permiso ALTER en el catálogo de texto completo y del permiso CREATE FULLTEXT CATALOG en la base datos.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se cambia la propiedad `accentsensitivity` del catálogo de texto completo predeterminado `ftCatalog`, que hace distinción de acentos.  
  
```sql  
--Change to accent insensitive  
USE AdventureWorks2012;  
GO  
ALTER FULLTEXT CATALOG ftCatalog   
REBUILD WITH ACCENT_SENSITIVITY=OFF;  
GO  
-- Check Accentsensitivity  
SELECT FULLTEXTCATALOGPROPERTY('ftCatalog', 'accentsensitivity');  
GO  
--Returned 0, which means the catalog is not accent sensitive.  
```  
  
## <a name="see-also"></a>Vea también  
 [sys.fulltext_catalogs &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md)   
 [CREATE FULLTEXT CATALOG &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-catalog-transact-sql.md)   
 [DROP FULLTEXT CATALOG &#40;Transact-SQL&#41;](../../t-sql/statements/drop-fulltext-catalog-transact-sql.md)   
 [Búsqueda de texto completo](../../relational-databases/search/full-text-search.md)  
  
  
