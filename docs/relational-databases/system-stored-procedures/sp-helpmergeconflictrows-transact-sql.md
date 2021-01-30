---
description: sp_helpmergeconflictrows (Transact-SQL)
title: sp_helpmergeconflictrows (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergeconflictrows_TSQL
- sp_helpmergeconflictrows
helpviewer_keywords:
- sp_helpmergeconflictrows
ms.assetid: 131395a5-cb18-4795-a7ae-fa09d8ff347f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4aeb1205f3d929eed7b2717ff388d8e03891d9e3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179258"
---
# <a name="sp_helpmergeconflictrows-transact-sql"></a>sp_helpmergeconflictrows (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve las filas de la tabla de conflictos especificada. Este procedimiento almacenado se ejecuta en el equipo donde se almacena la tabla de conflictos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helpmergeconflictrows [ [ @publication = ] 'publication' ]  
        , [ @conflict_table = ] 'conflict_table'  
    [ , [ @publisher = ] 'publisher' ]   
    [ , [ @publisher_db = ] 'publsher_db' ]   
    [ , [ @logical_record_conflicts = ] logical_record_conflicts ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname y su** valor predeterminado es **%** . Si se especifica la publicación, se devuelven todos los conflictos calificados por la publicación. Por ejemplo, si la tabla **MSmerge_conflict_Customers** tiene filas de conflictos para las publicaciones **wa** y **CA** , pasar un nombre de publicación **CA** recupera los conflictos que pertenecen a la publicación de la **entidad de certificación** .  
  
`[ @conflict_table = ] 'conflict_table'` Es el nombre de la tabla de conflictos. *conflict_table* es de **tipo sysname** y no tiene ningún valor predeterminado. En [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] y versiones posteriores, las tablas de conflictos se denominan con el **\_ _\_ artículo_** nombres de formato con MSmerge_conflict publicación, con una tabla para cada artículo publicado.  
  
`[ @publisher = ] 'publisher'` Es el nombre del publicador. *Publisher* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @publisher_db = ] 'publisher_db'` Es el nombre de la base de datos del publicador. *publisher_db* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @logical_record_conflicts = ] logical_record_conflicts` Indica si el conjunto de resultados contiene información sobre conflictos de registros lógicos. *logical_record_conflicts* es de **tipo int** y su valor predeterminado es 0. **1** indica que se devuelve información de conflictos de registros lógicos.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 **sp_helpmergeconflictrows** devuelve un conjunto de resultados que consta de la estructura de la tabla base y estas columnas adicionales.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**origin_datasource**|**VARCHAR(255**|Origen del conflicto.|  
|**conflict_type**|**int**|Código que indica el tipo de conflicto:<br /><br /> **1** = conflicto de actualización: el conflicto se detecta en el nivel de fila.<br /><br /> **2** = conflicto de actualización de columna: el conflicto detectado en el nivel de columna.<br /><br /> **3** = conflicto de actualización de eliminación de WINS: la eliminación gana el conflicto.<br /><br /> **4** = conflicto de actualización WINS delete: el ROWGUID eliminado que pierde el conflicto se registra en esta tabla.<br /><br /> **5** = error en la inserción de carga: no se pudo aplicar la inserción del suscriptor en el publicador.<br /><br /> **6** = error en la inserción de descarga: no se pudo aplicar la inserción del publicador en el suscriptor.<br /><br /> **7** = error al eliminar la carga: no se pudo cargar la eliminación en el suscriptor en el publicador.<br /><br /> **8** = error al eliminar la descarga: no se pudo descargar la eliminación en el publicador en el suscriptor.<br /><br /> **9** = error al cargar la actualización: no se pudo aplicar la actualización en el suscriptor en el publicador.<br /><br /> **10** = error de actualización de descarga: no se pudo aplicar la actualización en el publicador al suscriptor.<br /><br /> **12** = actualización de registro lógico WINS eliminar: el registro lógico eliminado que pierde el conflicto se registra en esta tabla.<br /><br /> **13** = conflicto del registro lógico insertar actualización: la inserción en un registro lógico entra en conflicto con una actualización.<br /><br /> **14** = conflicto de actualización de WINS de registro lógico: el registro lógico actualizado que pierde el conflicto se registra en esta tabla.|  
|**reason_code**|**int**|Código del error, que puede depender del contexto.|  
|**reason_text**|**VARCHAR (720)**|Descripción del error, que puede depender del contexto.|  
|**pubid**|**uniqueidentifier**|Identificador de la publicación.|  
|**MSrepl_create_time**|**datetime**|Hora en que Se ha agregado la información del conflicto.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_helpmergeconflictrows** se utiliza en la replicación de mezcla.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** , el rol fijo de base de datos **db_owner** y el rol **replmonitor** en la base de datos de distribución pueden ejecutar **sp_helpmergeconflictrows**.  
  
## <a name="see-also"></a>Consulte también  
 [Ver información de conflictos para publicaciones de combinación &#40;la programación de la replicación con Transact-SQL&#41;](../replication/view-and-resolve-data-conflicts-for-merge-publications.md)   
 [Procedimientos almacenados de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
