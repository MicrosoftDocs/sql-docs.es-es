---
description: sp_helparticledts (Transact-SQL)
title: sp_helparticledts (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helparticledts
- sp_helparticledts_TSQL
helpviewer_keywords:
- sp_helparticledts
ms.assetid: cd1aed60-e056-4ff3-86ee-62b19433d890
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e819ecd2e452f4a8a1be8e8199df6f9a6cf753d6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176464"
---
# <a name="sp_helparticledts-transact-sql"></a>sp_helparticledts (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Se utiliza para obtener información sobre los nombres de tarea personalizada correctos que se deben utilizar cuando se cree una suscripción de transformación con [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helparticledts [ @publication = ] 'publication', [ @article = ] 'article'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @article = ] 'article'` Es el nombre de un artículo de la publicación. *article* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**pre_script_ignore_error_task_name**|**sysname**|Nombre de la tarea de programación que tiene lugar antes de que se copien los datos de la instantánea. La ejecución del programa debe continuar si se encuentra un error de script.|  
|**pre_script_task_name**|**sysname**|Nombre de la tarea de programación que tiene lugar antes de que se copien los datos de la instantánea. La ejecución del programa se detiene si encuentra un error.|  
|**transformation_task_name**|**sysname**|Nombre de la tarea de programación cuando se utiliza una tarea de consulta controlada por datos.|  
|**post_script_ignore_error_task_name**|**sysname**|Nombre de la tarea de programación que tiene lugar después de copiarse los datos de la instantánea. La ejecución del programa debe continuar si se encuentra un error de script.|  
|**post_script_task_name**|**sysname**|Nombre de la tarea de programación que tiene lugar después de copiarse los datos de la instantánea. La ejecución del programa se detiene si encuentra un error.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_helparticledts** se utiliza en la replicación de instantáneas y en la replicación transaccional.  
  
 Existen convenciones de nomenclatura, necesarias para los agentes de replicación, que se deben seguir cuando se da nombre a las tareas de un programa de Servicios de transformación de datos (DTS) de replicación. En las tareas personalizadas, como una tarea Ejecutar SQL, el nombre es una cadena concatenada que consta del nombre del artículo, un prefijo y una parte opcional. Al escribir el código, si no está seguro de cómo deben ser los nombres de tarea, el conjunto de resultados proporciona los nombres de tarea que se deben utilizar.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** y del rol fijo de base de datos **db_owner** pueden ejecutar **sp_helparticledts**.  
  
  
