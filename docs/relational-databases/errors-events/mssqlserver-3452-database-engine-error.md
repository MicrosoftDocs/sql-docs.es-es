---
description: MSSQLSERVER_3452
title: MSSQLSERVER_3452 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3452 (Database Engine error)
ms.assetid: bf838f02-7186-4b33-b01e-361b0c02de1f
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 1fe941e7ffaa594daa11e11e72140b4ace07cd33
ms.sourcegitcommit: c52a6aeb6fa6d7c3a86b3e84449361f4a0949ad0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2021
ms.locfileid: "99623802"
---
# <a name="mssqlserver_3452"></a>MSSQLSERVER_3452
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|3452|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|REC_CHECKIDENTITY|  
|Texto del mensaje|En la recuperación de la base de datos '%.*ls' (%d) se detectó una posible incoherencia de valores de identidad en la tabla con id. %d. Ejecute DBCC CHECKIDENT ("%.\*ls").|  
  
## <a name="explanation"></a>Explicación  

Durante una actualización o al aplicar una actualización de servicio, el proceso para recuperar los valores de identidad de la base de datos encontró una incoherencia en los metadatos.  
  
## <a name="user-action"></a>Acción del usuario

Ejecute DBCC CHECKIDENT.  
  
