---
description: sp_helpmergedeleteconflictrows (Transact-SQL)
title: sp_helpmergedeleteconflictrows (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergedeleteconflictrows
- sp_helpmergedeleteconflictrows_TSQL
helpviewer_keywords:
- sp_helpmergedeleteconflictrows
ms.assetid: 222be651-5690-4341-9dfb-f9ec1d80c970
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 07ca90c1eb7792ea5d197fe37a3542f526f870b6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179252"
---
# <a name="sp_helpmergedeleteconflictrows-transact-sql"></a>sp_helpmergedeleteconflictrows (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información sobre las filas de datos que han perdido conflictos de eliminación. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicaciones o en el suscriptor de la base de datos de suscripciones cuando se utiliza el registro de conflictos no centralizado.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helpmergedeleteconflictrows [ [ @publication = ] 'publication']  
    [ , [ @source_object = ] 'source_object']  
    [ , [ @publisher = ] 'publisher'  
    [ , [ @publisher_db = ] 'publsher_db'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname y su** valor predeterminado es **%** . Si se especifica la publicación, se devuelven todos los conflictos calificados por la publicación.  
  
`[ @source_object = ] 'source_object'` Es el nombre del objeto de origen. *source_object* es de tipo **nvarchar (386)** y su valor predeterminado es NULL.  
  
`[ @publisher = ] 'publisher'` Es el nombre del publicador. *Publisher* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @publisher_db = ] 'publisher_db'` Es el nombre de la base de datos del publicador. *publisher_db* es de **tipo sysname y su** valor predeterminado es NULL.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**source_object**|**nvarchar (386)**|Objeto de origen del conflicto de eliminación.|  
|**rowguid**|**uniqueidentifier**|Identificador de la fila del conflicto de eliminación.|  
|**conflict_type**|**int**|Código que indica el tipo de conflicto:<br /><br /> **1** = UpdateConflict: se detecta un conflicto en el nivel de fila.<br /><br /> **2** = ColumnUpdateConflict: conflicto detectado en el nivel de columna.<br /><br /> **3** = UpdateDeleteWinsConflict: la eliminación gana el conflicto.<br /><br /> **4** = UpdateWinsDeleteConflict: el ROWGUID eliminado que pierde el conflicto se registra en esta tabla.<br /><br /> **5** = UploadInsertFailed: la inserción del suscriptor no se pudo aplicar en el publicador.<br /><br /> **6** = DownloadInsertFailed: no se pudo aplicar Insert from Publisher en el suscriptor.<br /><br /> **7** = UploadDeleteFailed: la eliminación en el suscriptor no se pudo cargar en el publicador.<br /><br /> **8** = DownloadDeleteFailed: la eliminación en el publicador no se pudo descargar en el suscriptor.<br /><br /> **9** = UploadUpdateFailed: la actualización en el suscriptor no se pudo aplicar en el publicador.<br /><br /> **10** = DownloadUpdateFailed: no se pudo aplicar la actualización en el publicador al suscriptor.|  
|**reason_code**|**Int**|Código del error, que puede depender del contexto.|  
|**reason_text**|**VARCHAR (720)**|Descripción del error, que puede depender del contexto.|  
|**origin_datasource**|**VARCHAR(255**|Origen del conflicto.|  
|**pubid**|**uniqueidentifier**|Identificador de la publicación.|  
|**MSrepl_create_time**|**datetime**|Hora en que Se ha agregado la información del conflicto.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_helpmergedeleteconflictrows** se utiliza en la replicación de mezcla.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** y del rol fijo de base de datos **db_owner** pueden ejecutar **sp_helpmergedeleteconflictrows**.  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
