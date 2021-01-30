---
description: sp_fulltext_catalog (Transact-SQL)
title: sp_fulltext_catalog (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_fulltext_catalog_TSQL
- sp_fulltext_catalog
dev_langs:
- TSQL
helpviewer_keywords:
- sp_fulltext_catalog
ms.assetid: e49b98e4-d1f1-42b2-b16f-eb2fc7aa1cf5
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7f2d26fe07a6b5c224de6984dbf47d0ae6bc3b8f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185439"
---
# <a name="sp_fulltext_catalog-transact-sql"></a>sp_fulltext_catalog (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Crea y quita un catálogo de texto completo, e inicia y detiene la acción de indización de un catálogo. Se pueden crear varios catálogos de texto completo en cada base de datos.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Use [CREATE FULLTEXT CATALOG](../../t-sql/statements/create-fulltext-catalog-transact-sql.md), [ALTER fulltext](../../t-sql/statements/alter-fulltext-catalog-transact-sql.md)CATALOG y [Drop FULLTEXT CATALOG](../../t-sql/statements/drop-fulltext-catalog-transact-sql.md) en su lugar.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_fulltext_catalog [ @ftcat= ] 'fulltext_catalog_name' ,   
     [ @action= ] 'action'   
     [ , [ @path= ] 'root_directory' ]   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @ftcat = ] 'fulltext_catalog_name'` Es el nombre del catálogo de texto completo. Los nombres de catálogo deben ser únicos en cada base de datos. *fulltext_catalog_name* es de **tipo sysname**.  
  
`[ @action = ] 'action'` Es la acción que se va a realizar. *Action* es de tipo **VARCHAR (20)** y puede tener uno de estos valores.  
  
> [!NOTE]  
>  Los catálogos de texto completo se pueden crear, quitar o modificar como precise. No obstante, evite realizar cambios de esquema en varios catálogos al mismo tiempo. Estas acciones se pueden realizar mediante el **sp_fulltext_table** procedimiento almacenado, que es el método recomendado.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Creación**|Crea un nuevo catálogo de texto completo vacío en el sistema de archivos y agrega una fila asociada en **sysfulltextcatalogs** con los valores *fulltext_catalog_name* y *root_directory*, si existen,. *fulltext_catalog_name* debe ser único en la base de datos.|  
|**Omisiones**|Quita *fulltext_catalog_name* quitando del sistema de archivos y eliminando la fila asociada en **sysfulltextcatalogs**. Esta acción genera un error si el catálogo contiene índices de una o más tablas. **sp_fulltext_table** se debe ejecutar '*TABLE_NAME*', ' Drop ' para quitar las tablas del catálogo.<br /><br /> Se muestra un error si el catálogo no existe.|  
|**start_incremental**|Inicia un rellenado incremental para *fulltext_catalog_name*. Se muestra un error si el catálogo no existe. Si ya hay un rellenado de índices de texto completo activo, se muestra una advertencia y no se produce el rellenado. Con el rellenado incremental solo se recuperan las filas cambiadas para la indización de texto completo, siempre que haya una columna de **marca** de tiempo presente en la tabla que está indizada de texto completo.|  
|**start_full**|Inicia un rellenado completo de *fulltext_catalog_name*. Se recupera cada una de las filas de todas las tablas asociadas con este catálogo de texto para realizar la indización de texto, aunque ya se hayan indizado.|  
|**Detención**|Detiene un rellenado del índice para *fulltext_catalog_name*. Se muestra un error si el catálogo no existe. No se muestra ninguna advertencia si el rellenado ya se ha detenido.|  
|**Recompilación**|Vuelve a generar *fulltext_catalog_name*. Cuando vuelve a generarse un catálogo, el catálogo existente se elimina y se crea uno nuevo en su lugar. Todas las tablas que tienen referencias de índices de texto completo se asocian al catálogo nuevo. La regeneración restablece los metadatos de texto completo de las tablas del sistema de la base de datos.<br /><br /> Si el seguimiento de cambios está establecido en OFF, la regeneración no hace que se vuelva a rellenar el catálogo de texto completo recién creado. En este caso, para volver a rellenar, ejecute **sp_fulltext_catalog** con la acción **start_full** o **start_incremental** .|  
  
`[ @path = ] 'root_directory'` Es el directorio raíz (no la ruta de acceso física completa) de una acción de **creación** . *root_directory* es de tipo **nvarchar (100)** y su valor predeterminado es null, lo que indica el uso de la ubicación predeterminada especificada en el programa de instalación. Este es el subdirectorio FTDATA del directorio MSSQL; por ejemplo, C:\Archivos de Programa\microsoft SQL Server\MSSQL13. MSSQLSERVER\MSSQL\FTData. El directorio raíz especificado debe existir, residir en una unidad en el mismo equipo y constar de más datos que solo la letra de unidad, y no puede ser una ruta de acceso relativa. No se admiten las unidades de red, discos extraíbles, disquetes y rutas de acceso UNC. Los catálogos de texto completo deben crearse en una unidad de disco duro local asociada con una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **\@ path** solo es válido cuando *Action* es **Create**. En el caso de acciones distintas a **Create** (**Stop**, **rebuild**, etc.), **\@ path** debe ser null u omitirse.  
  
 Si la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es un servidor virtual en un clúster, el directorio del catálogo especificado debe estar en una unidad de disco compartida de la que depende el recurso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si @path no se especifica, la ubicación del directorio de catálogos predeterminado se encuentra en la unidad de disco compartida, en el directorio que se especificó cuando se instaló el servidor virtual.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 La acción **start_full** se usa para crear una instantánea completa de los datos de texto completo en *fulltext_catalog_name*. La acción **start_incremental** se utiliza para volver a indizar solo las filas modificadas en la base de datos. El rellenado incremental solo se puede aplicar si la tabla tiene una columna de tipo **timestamp**. Si una tabla del catálogo de texto completo no contiene una columna de tipo **timestamp**, se lleva a cabo un rellenado completo en la tabla.  
  
 Los datos del catálogo de texto completo y del índice se almacenan en archivos creados en un directorio de catálogos de texto completo. El directorio del catálogo de texto completo se crea como un subdirectorio del directorio especificado en **\@ ruta de acceso** o en el directorio del catálogo de texto completo predeterminado si no se especifica la **\@ ruta de acceso** . El nombre del directorio de catálogos de texto completo se genera de forma que se garantiza que será exclusivo en el servidor. Por lo tanto, todos los directorios de catálogos de texto completo de un servidor pueden compartir la misma ruta de acceso.  
  
## <a name="permissions"></a>Permisos  
 El llamador debe ser miembro del rol **db_owner** . Dependiendo de la acción solicitada, el autor de la llamada no debe denegar los permisos ALTER o CONTROL (que **db_owner** tiene) en el catálogo de texto completo de destino.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-create-a-full-text-catalog"></a>A. Crear un catálogo de texto completo  
 En este ejemplo se crea un catálogo de texto completo vacío, **Cat_Desc**, en la base de datos **AdventureWorks2012** .  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_catalog 'Cat_Desc', 'create';  
GO  
```  
  
### <a name="b-to-rebuild-a-full-text-catalog"></a>B. Para regenerar un catálogo de texto completo  
 En este ejemplo se vuelve a generar un catálogo de texto completo existente, **Cat_Desc**, en la base de datos **AdventureWorks2012** .  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_catalog 'Cat_Desc', 'rebuild';  
GO  
```  
  
### <a name="c-start-the-population-of-a-full-text-catalog"></a>C. Iniciar el rellenado de un catálogo de texto completo  
 En este ejemplo se inicia un rellenado completo del catálogo de **Cat_Desc** .  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_catalog 'Cat_Desc', 'start_full';  
GO  
```  
  
### <a name="d-stop-the-population-of-a-full-text-catalog"></a>D. Detener el rellenado de un catálogo de texto completo  
 En este ejemplo se detiene el rellenado del catálogo de **Cat_Desc** .  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_catalog 'Cat_Desc', 'stop';  
GO  
```  
  
### <a name="e-to-remove-a-full-text-catalog"></a>E. Quitar un catálogo de texto completo  
 En este ejemplo se quita el catálogo de **Cat_Desc** .  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_catalog 'Cat_Desc', 'drop';  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [FULLTEXTCATALOGPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/fulltextcatalogproperty-transact-sql.md)   
 [sp_fulltext_database &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-fulltext-database-transact-sql.md)   
 [sp_help_fulltext_catalogs &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-transact-sql.md)   
 [sp_help_fulltext_catalogs_cursor &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-cursor-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Búsqueda de texto completo](../../relational-databases/search/full-text-search.md)  
  
  
