---
description: sp_browsesnapshotfolder (Transact-SQL)
title: sp_browsesnapshotfolder (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_browsesnapshotfolder
- sp_browsesnapshotfolder_TSQL
helpviewer_keywords:
- sp_browsesnapshotfolder
ms.assetid: 0872edf2-4038-4bc1-a68d-05ebfad434d2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a74c1325b2af76a96c0fc544dfbbf136b0561225
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199185"
---
# <a name="sp_browsesnapshotfolder-transact-sql"></a>sp_browsesnapshotfolder (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Devuelve la ruta de acceso completa de la última instantánea generada para una publicación. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_browsesnapshotfolder [@publication= ] 'publication'  
    { [ , [ @subscriber = ] 'subscriber' ]  
 [ , [ @subscriber_db = ] 'subscriber_db' ] }  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación que contiene el artículo. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @subscriber = ] 'subscriber'` Es el nombre del suscriptor. *Subscriber* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @subscriber_db = ] 'subscriber_db'` Es el nombre de la base de datos de suscripciones. *subscriber_db* es de **tipo sysname y su** valor predeterminado es NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**snapshot_folder**|**nvarchar(512)**|Ruta de acceso completa del directorio de instantáneas.|  
  
## <a name="remarks"></a>Observaciones  
 **sp_browsesnapshotfolder** se utiliza en la replicación de instantáneas y en la replicación transaccional.  
  
 Si el *suscriptor* y los campos de *subscriber_db* se dejan como null, el procedimiento almacenado devuelve la carpeta de instantáneas de la instantánea más reciente que puede encontrar para la publicación. Si se especifican los campos de *suscriptor* y *subscriber_db* , el procedimiento almacenado devuelve la carpeta de instantáneas para la suscripción especificada. Si no se ha generado una instantánea para la publicación, se devuelve un conjunto de resultados vacío.  
  
 Si la publicación está configurada para generar archivos de instantáneas en el directorio de trabajo y en la carpeta de instantáneas del publicador, el conjunto de resultados contiene dos filas. La primera fila contiene la carpeta de instantáneas de la publicación y la segunda fila contiene el directorio de trabajo del publicador. **sp_browsesnapshotfolder** es útil para determinar el directorio en el que se generan los archivos de instantáneas.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_browsesnapshotfolder**.  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
