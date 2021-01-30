---
description: Sp_wait_for_database_copy_sync de Geo-Replication activo
title: sp_wait_for_database_copy_sync
titleSuffix: Azure SQL Database
ms.date: 03/03/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sp_wait_for_database_copy_sync_TSQL
- sp_wait_for_database_copy_sync
dev_langs:
- TSQL
helpviewer_keywords:
- sp_wait_for_database_copy_sync
ms.assetid: 7068da7f-cb74-47f2-b064-eb076a0d3885
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.custom: seo-dt-2019
ms.openlocfilehash: 3651d23116aa12e25e7f6f1441a432fdeaafe4fb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180250"
---
# <a name="active-geo-replication---sp_wait_for_database_copy_sync"></a>Sp_wait_for_database_copy_sync de Geo-Replication activo
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Este procedimiento tiene como ámbito una relación de [!INCLUDE[ssGeoDR](../../includes/ssgeodr-md.md)] entre una base de datos principal y una base de datos secundaria. La llamada a la **sp_wait_for_database_copy_sync** hace que la aplicación espere hasta que la base de datos secundaria activa replique y confirme todas las transacciones confirmadas. Ejecute **sp_wait_for_database_copy_sync** solo en la base de datos principal.  
  
||  
|-|  
|**Se aplica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|  
  
## <a name="syntax"></a>Sintaxis  
  
```  
sp_wait_for_database_copy_sync [ @target_server = ] 'server_name'   
     , [ @target_database = ] 'database_name'  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @target_server =] ' SERVER_NAME '  
 Nombre del servidor de SQL Database que hospeda la base de datos secundaria activa. server_name es sysname, sin valor predeterminado.  
  
 [ @target_database = ] 'database_name'  
 Nombre de la base de datos secundaria activa. database_name es sysname y no tiene ningún valor predeterminado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 Devuelve 0 para indicar que es correcto o un número de error si hay un error.  
  
 Las condiciones de error más probables son las siguientes:  
  
-   El nombre de servidor o el nombre de la base de datos falta.  
  
-   El vínculo no se puede encontrar en la base de datos o el nombre de servidor especificados.  
  
-   La conectividad interlink se pierde. **sp_wait_for_database_copy_sync** devolverá después del tiempo de espera de la conexión.  
  
## <a name="permissions"></a>Permisos  
 Cualquier usuario de la base de datos principal puede llamar a este procedimiento almacenado del sistema. El inicio de sesión debe ser un usuario tanto en la base de datos principal como en la secundaria activa.  
  
## <a name="remarks"></a>Observaciones  
 Todas las transacciones confirmadas antes de una llamada **sp_wait_for_database_copy_sync** se envían a la base de datos secundaria activa.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se invoca **sp_wait_for_database_copy_sync** para asegurarse de que todas las transacciones se confirman en la base de datos principal, DB0, y se envían a su base de datos secundaria activa en el servidor de destino ubfyu5ssyt.  
  
```  
USE db0;  
GO  
EXEC sys.sp_wait_for_database_copy_sync @target_server = N'ubfyu5ssyt1', @target_database = N'db0';  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sys.dm_continuous_copy_status &#40;Azure SQL Database&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-continuous-copy-status-azure-sql-database.md)   
 [Vistas de administración dinámica (DMV) y funciones de replicación geográfica &#40;Azure SQL Database&#41;](../../relational-databases/system-dynamic-management-views/geo-replication-dynamic-management-views-and-functions-azure-sql-database.md)   
 [sys.dm_geo_replication_link_status](../system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database.md)
  
  
