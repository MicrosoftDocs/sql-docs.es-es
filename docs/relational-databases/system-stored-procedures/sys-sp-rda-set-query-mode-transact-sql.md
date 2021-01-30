---
title: sys.sp_rda_set_query_mode (Transact-SQL) | Microsoft Docs
description: Utilice sys.sp_rda_set_query_mode para especificar si las consultas realizadas en la base de datos habilitada para Stretch actual y sus tablas devuelven datos locales y remotos, o solo datos locales.
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sys.sp_rda_set_query_mode
- sys.sp_rda_set_query_mode_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_rda_set_query_mode stored procedure
ms.assetid: 65a0b390-cf87-4db7-972a-1fdf13456c88
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ed9e5afda379b094a61c9f6d3130b986e0b83ca3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211723"
---
# <a name="syssp_rda_set_query_mode-transact-sql"></a>sys.sp_rda_set_query_mode (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Especifica si las consultas realizadas en la base de datos habilitada para Stretch actual y en sus tablas devuelven datos locales y remotos (el valor predeterminado) o solo los datos locales.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_rda_set_query_mode [ @mode = ] @mode   
    [ , [ @force = ]  @force ]  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @mode =] *\@ modo*  
 Es uno de los valores siguientes.  
  
-   **Deshabilitado** Se produce un error en todas las consultas en tablas habilitadas para Stretch.  
  
-   **LOCAL_ONLY** Las consultas realizadas en tablas habilitadas para Stretch solo devuelven datos locales.  
  
-   **LOCAL_AND_REMOTE** Las consultas en tablas habilitadas para Stretch devuelven datos locales y remotos. Éste es el comportamiento predeterminado.  
  
 [ @force =] *\@ forzar*  
 Es un valor de bit opcional que se puede establecer en 1 si se desea cambiar el modo de consulta sin validación.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o >0 (error)  
  
## <a name="permissions"></a>Permisos  
 Requiere permisos de db_owner.  
  
## <a name="remarks"></a>Observaciones  
 Los siguientes procedimientos almacenados extendidos también establecen el modo de consulta para una base de datos habilitada para Stretch.  
  
-   **sp_rda_deauthorize_db**  
  
     Después de ejecutar **sp_rda_deauthorize_db** , se producirá un error en todas las consultas de bases de datos y tablas habilitadas para Stretch. Es decir, el modo de consulta se establece en Disabled. Para salir de este modo, realice una de las siguientes acciones.  
  
    -   Ejecute [sys.sp_rda_reauthorize_db &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-rda-reauthorize-db-transact-sql.md) para volver a conectar con la base de datos de Azure remota. Esta operación restablece automáticamente el modo de consulta en LOCAL_AND_REMOTE, que es el comportamiento predeterminado de Stretch Database. Es decir, las consultas devuelven los resultados de los datos locales y remotos.  
  
    -   Ejecute [Sys.sp_rda_set_query_mode](../../relational-databases/system-stored-procedures/sys-sp-rda-set-query-mode-transact-sql.md) con el argumento LOCAL_ONLY para permitir que las consultas sigan ejecutándose solo con datos locales.  
  
-   **sp_rda_reauthorize_db**  
  
     Al ejecutar [sys.sp_rda_reauthorize_db &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-rda-reauthorize-db-transact-sql.md) para volver a conectar con la base de datos remota de Azure, esta operación restablece automáticamente el modo de consulta en LOCAL_AND_REMOTE, que es el comportamiento predeterminado de stretch Database. Es decir, las consultas devuelven los resultados de los datos locales y remotos.  
  
## <a name="see-also"></a>Consulte también  
 [sys.sp_rda_deauthorize_db &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-rda-deauthorize-db-transact-sql.md)   
 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)  
  
  
