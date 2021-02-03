---
description: MSSQLSERVER_3937
title: MSSQLSERVER_3937 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3937 (Database Engine error)
ms.assetid: 312d5bbe-c8de-42db-af4b-4ccb448ce6ef
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d9c592a077e38da67073691e6d8eabb916fe791b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185189"
---
# <a name="mssqlserver_3937"></a>MSSQLSERVER_3937
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|MSSQLSERVER|  
|Id. de evento|3937|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|XACT_FILESTREAM_ROLLBACK_ERROR|  
|Texto del mensaje|Error al intentar notificar al controlador de filtro de secuencias de archivo que se revirtió una transacción. Código de error: 0x%0x.|  
  
## <a name="explanation"></a>Explicación  
El controlador RsFx ha devuelto un error al emitir una notificación de reversión de una transacción. Este error suele provocarlo la escasez de recursos. Esto puede producir una pequeña pérdida de memoria en el controlador de filtro RsFx, pero ésta se liberará cuando se cierre el proceso sqlservr.exe que creó la transacción.  
  
## <a name="user-action"></a>Acción del usuario  
