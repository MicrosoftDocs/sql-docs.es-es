---
description: CHECKPOINT (Transact-SQL)
title: CHECKPOINT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/27/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CHECKPOINT_TSQL
- CHECKPOINT
dev_langs:
- TSQL
helpviewer_keywords:
- events [SQL Server], checkpoints
- automatic checkpoints
- writing dirty pages to disk
- pages [SQL Server], dirty
- checkpoints [SQL Server]
- CHECKPOINT statement
- log truncate mode [SQL Server]
- dirty pages
- logs [SQL Server], checkpoints
- manual checkpoints [SQL Server]
- pages [SQL Server], checkpoints
ms.assetid: ccdfc689-ad4e-44c0-83f7-0f2cfcfb6406
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 823bef4f26703b47404f28abb211cc87f70c0a04
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208808"
---
# <a name="checkpoint-transact-sql"></a>CHECKPOINT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Genera un punto de comprobación manual en la base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a la que está conectado actualmente.  
  
> [!NOTE]  
>  Para obtener información sobre los distintos tipos de puntos de comprobación de base de datos y del funcionamiento de los puntos de comprobación en general, vea [Puntos de comprobación de base de datos &#40;SQL Server&#41;](../../relational-databases/logs/database-checkpoints-sql-server.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CHECKPOINT [ checkpoint_duration ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *duración_del_punto_de_comprobación*  
 Especifica la cantidad de tiempo necesaria, en segundos, para que se complete el punto de comprobación manual. Cuando se especifica *duración_del_punto_de_comprobación*, [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] intenta realizar el punto de comprobación dentro de la duración solicitada. *duración_del_punto_de_comprobación* debe ser una expresión de tipo **int** y debe ser mayor que cero. Cuando se omite este parámetro, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] ajusta la duración del punto de comprobación para minimizar el impacto en el rendimiento de las aplicaciones de base de datos. *duración_del_punto_de_comprobación* es una opción avanzada.  
  
## <a name="factors-affecting-the-duration-of-checkpoint-operations"></a>Factores que afectan a la duración de las operaciones de puntos de comprobación  
 Generalmente, el tiempo necesario para una operación de punto de comprobación aumenta con el número de páginas desfasadas que la operación debe escribir. De forma predeterminada, para minimizar el impacto en el rendimiento de otras aplicaciones, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ajusta la frecuencia de escritura que una operación de punto de comprobación realiza. Reducir la frecuencia de escritura incrementa el tiempo que la operación de punto de comprobación necesita para completarse. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se usa esta estrategia para un punto de comprobación manual a menos que se especifique un valor *duración_del_punto_de_comprobación* en el comando CHECKPOINT.  
  
 El impacto que *duración_del_punto_de_comprobación* tiene en el rendimiento depende del número de páginas desfasadas, la actividad del sistema y la duración real especificada. Por ejemplo, si el punto de comprobación normalmente se completa en 120 segundos, especificar un valor de 45 segundos para *duración_del_punto_de_comprobación* hará que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] destine más recursos al punto de comprobación de los que asignaría de forma predeterminada. Por el contrario, si se especifica un valor de 180 segundos para *duración_del_punto_de_comprobación*, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] destinará un número inferior de recursos de los que asignaría de forma predeterminada. En general, un valor bajo de *duración_del_punto_de_comprobación* incrementará el número de recursos destinados al punto de comprobación, mientras que un valor elevado de *duración_del_punto_de_comprobación* lo reducirá. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] siempre completa los puntos de comprobación si es posible, y la instrucción CHECKPOINT devuelve un valor inmediatamente cuando se completa un punto de comprobación. Por tanto, en algunos casos, un punto de comprobación puede llevar más o menos tiempo que la duración especificada.  
  
##  <a name="security"></a><a name="Security"></a> Seguridad  
  
### <a name="permissions"></a>Permisos  
 Se conceden permisos CHECKPOINT de forma predeterminada a los miembros de la función fija de servidor **sysadmin** y a las funciones fijas de base de datos **db_owner** y **db_backupoperator** y no se pueden transferir.  
  
## <a name="see-also"></a>Consulte también  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Puntos de comprobación de base de datos &#40;SQL Server&#41;](../../relational-databases/logs/database-checkpoints-sql-server.md)   
 [Establecer la opción de configuración del servidor Intervalo de recuperación](../../database-engine/configure-windows/configure-the-recovery-interval-server-configuration-option.md)   
 [SHUTDOWN &#40;Transact-SQL&#41;](../../t-sql/language-elements/shutdown-transact-sql.md)  
  
  
