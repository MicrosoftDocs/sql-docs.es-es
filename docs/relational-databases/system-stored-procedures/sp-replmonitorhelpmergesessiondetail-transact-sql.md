---
title: sp_replmonitorhelpmergesessiondetail (T-SQL)
description: Describe el sp_replmonitorhelpmergesessiondetail procedimiento almacenado que devuelve información detallada de nivel de artículo sobre una sesión de Agente de mezcla de replicación específica.
ms.custom: seo-lt-2019
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replmonitorhelpmergesessiondetail
- sp_replmonitorhelpmergesessiondetail_TSQL
helpviewer_keywords:
- sp_replmonitorhelpmergesessiondetail
ms.assetid: 805c92fc-3169-410c-984d-f37e063b791d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ba050d49579e217fa5be5a2dedcd7aa21cf0c99d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204382"
---
# <a name="sp_replmonitorhelpmergesessiondetail-transact-sql"></a>sp_replmonitorhelpmergesessiondetail (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Devuelve información detallada de nivel de artículo sobre una sesión determinada de Agente de mezcla de replicación, que se utiliza para supervisar la replicación de mezcla. El conjunto de resultados incluye una fila de detalles para cada artículo sincronizado durante la sesión. También incluye una fila que representa la inicialización de la sesión y filas que resumen las fases de carga y descarga de la sesión. Este procedimiento almacenado se ejecuta en el distribuidor de la base de datos de distribución o en el suscriptor de la base de datos de suscripciones.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_replmonitorhelpmergesessiondetail [ @session_id = ] session_id  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @session_id = ] session_id` Especifica una sesión del agente. *session_id* es de **tipo int** y no tiene ningún valor predeterminado.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**PhaseID**|**int**|Es la fase de la sesión de sincronización, que puede ser:<br /><br /> **0** = inicialización o fila de Resumen<br /><br /> **1** = carga<br /><br /> **2** = descargar|  
|**ArticleName**|**sysname**|Es el nombre del artículo que se va a sincronizar. **ArticleName** también contiene información de resumen para las filas del conjunto de resultados que no representan detalles del artículo.|  
|**PercentComplete**|**decimal**|Indica el porcentaje de los cambios totales aplicados en una fila determinada de detalles del artículo para las sesiones en ejecución o con errores.|  
|**RelativeCost**|**decimal**|Indica el tiempo dedicado a la sincronización del artículo como porcentaje del tiempo de sincronización total para la sesión.|  
|**Duración**|**int**|Duración de la sesión de agente.|  
|**Inserts**|**int**|Número de inserciones de cada sesión.|  
|**Actualizaciones**|**int**|Número de actualizaciones de cada sesión.|  
|**Eliminaciones**|**int**|Número de eliminaciones de cada sesión.|  
|**Conflictos**|**int**|Número de conflictos ocurridos en una sesión.|  
|**ErrorID**|**int**|Identificador de un error de la sesión.|  
|**SeqNo**|**int**|Orden de las sesiones en el conjunto de resultados.|  
|**RowType**|**int**|Indica el tipo de información que representa cada fila del conjunto de resultados.<br /><br /> **0** = inicialización<br /><br /> **1** = Resumen de la carga<br /><br /> **2** = detalle de carga del artículo<br /><br /> **3** = Resumen de la descarga<br /><br /> **4** = detalles de la descarga del artículo|  
|**SchemaChanges**|**int**|Número de cambios de esquema de cada sesión.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_replmonitorhelpmergesessiondetail** se utiliza para supervisar la replicación de mezcla.  
  
 Cuando se ejecuta en el suscriptor, **sp_replmonitorhelpmergesessiondetail** solo devuelve información detallada acerca de las últimas 5 sesiones agente de mezcla.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de base de datos **db_owner** o **replmonitor** en la base de datos de distribución en el distribuidor o en la base de datos de suscripciones del suscriptor pueden ejecutar **sp_replmonitorhelpmergesessiondetail**.  
  
## <a name="see-also"></a>Consulte también  
 [Supervisar la replicación mediante programación](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)  
  
  
