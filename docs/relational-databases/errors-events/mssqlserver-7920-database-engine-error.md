---
description: MSSQLSERVER_7920
title: MSSQLSERVER_7920 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 7920 (Database Engine error)
ms.assetid: d16290ea-3875-4148-8d53-057bfee00438
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 2c01d8149a0b2ff5747e4f1e5f05fdd9ec39b887
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198028"
---
# <a name="mssqlserver_7920"></a>MSSQLSERVER_7920
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|7920|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|DBCC2_SUMMARY_ENTRIES|  
|Texto del mensaje|Se han procesado ENTRY_COUNT entradas en el catálogo del sistema para el id. de base de datos D_ID.|  
  
## <a name="explanation"></a>Explicación  
Se trata de un mensaje informativo que devuelven todos los comandos DBCC CHECK excepto DBCC CHECKALLOC. El valor devuelto es el número total de conjuntos de filas comprobados.  
  
## <a name="user-action"></a>Acción del usuario  
None  
  
