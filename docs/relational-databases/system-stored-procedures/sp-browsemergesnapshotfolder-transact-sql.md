---
description: sp_browsemergesnapshotfolder (Transact-SQL)
title: sp_browsemergesnapshotfolder (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_browsemergesnapshotfolder
- sp_browsemergesnapshotfolder_TSQL
helpviewer_keywords:
- sp_browsemergesnapshotfolder
ms.assetid: e248642f-5fea-4ed7-be1a-36ff75abcfde
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 294b9740da1005731f42ef0b3527d1417b15b9dc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199178"
---
# <a name="sp_browsemergesnapshotfolder-transact-sql"></a>sp_browsemergesnapshotfolder (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve la ruta de acceso completa de la última instantánea generada para una publicación de combinación. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_browsemergesnapshotfolder [@publication= ] 'publication'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**snapshot_folder**|**nvarchar (2000)**|Ruta de acceso completa del directorio de instantáneas.|  
  
## <a name="remarks"></a>Observaciones  
 **sp_browsemergesnapshotfolder** se utiliza en la replicación de mezcla.  
  
 Si la publicación está configurada para generar archivos de instantáneas en el directorio de trabajo y en la carpeta de instantáneas del publicador, el conjunto de resultados contendrá dos filas: la primera fila contendrá la carpeta de instantáneas de la publicación y la segunda fila contendrá el directorio de trabajo del publicador.  
  
 **sp_browsemergesnapshotfolder** es útil para determinar el directorio en el que se generan los archivos de instantáneas de combinación. Esta carpeta o ruta de acceso y su contenido se pueden copiar en un medio extraíble, así como la instantánea utilizada para sincronizar una suscripción desde una ubicación de instantáneas alternativa.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_browsemergesnapshotfolder**.  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
