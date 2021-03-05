---
description: DROP WORKLOAD CLASSIFIER (Transact-SQL)
title: DROP WORKLOAD CLASSIFIER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.prod_service: sql-data-warehouse
ms.reviewer: jrasnick
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- WORKLOAD CLASSIFIER
- WORKLOAD_CLASSIFIER_TSQL
- DROP_WORKLOAD_CLASSIFIER_TSQL
- DROP WORKLOAD GROUP
dev_langs:
- TSQL
helpviewer_keywords:
- DROP WORKLOAD CLASSIFIER statement
author: ronortloff
ms.author: rortloff
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: 2509ee07decbfcc9ca97b247b78191fead413c6b
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186466"
---
# <a name="drop-workload-classifier-transact-sql"></a>DROP WORKLOAD CLASSIFIER (Transact-SQL)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Coloca un clasificador de administración de cargas de trabajo definido por el usuario existente.  Si las solicitudes se ejecutan o están en la cola de solicitudes en estado suspendido, mantendrán su clasificación y el clasificador se podrá colocar inmediatamente. El hecho de colocar y volver a crear el clasificador con una importancia distinta no afectará a una solicitud ya clasificada.
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Sintaxis

```syntaxsql
DROP WORKLOAD CLASSIFIER classifier_name;
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Argumentos

*classifier_name*  
Especifica el nombre por el que se identifica el clasificador de carga de trabajo.
  
## <a name="permissions"></a>Permisos

Requiere el permiso CONTROL DATABASE.  
  
## <a name="examples"></a>Ejemplos

En el ejemplo siguiente se coloca el clasificador de cargas de trabajo denominado `wgcELTROLE`.  

```sql
DROP WORKLOAD CLASSIFIER wgcELTRole;
```

> [!NOTE]
> Una solicitud enviada sin un clasificador coincidente se clasificará en el grupo de cargas de trabajo predeterminado.  El grupo de cargas de trabajo predeterminado es la clase de recurso smallrc.
  
## <a name="see-also"></a>Consulte también

[CREATE WORKLOAD CLASSIFIER (Transact-SQL)](../../t-sql/statements/create-workload-classifier-transact-sql.md)</br>
[Clasificación de las cargas de trabajo de [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](/azure/sql-data-warehouse/sql-data-warehouse-workload-classification)
