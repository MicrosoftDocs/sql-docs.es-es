---
title: MSSQLSERVER_17132 | Microsoft Docs
description: El equipo de SQL Server no pudo procesar el paquete de inicio de sesión de cliente. Vea una explicación del error y las posibles resoluciones.
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 17132 (Database Engine error)
ms.assetid: d1d198bd-6730-4394-bd5f-28f320c01a38
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 887dde5194364affa8915ab1a30e13220ecb8e2e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196766"
---
# <a name="mssqlserver_17132"></a>MSSQLSERVER_17132
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|17132|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|INIT_NODESSPACE|  
|Texto del mensaje|Error al iniciar el servidor debido a una falta de memoria para el descriptor. Reduzca la carga de memoria no esencial o aumente la memoria del sistema.|  
  
## <a name="explanation"></a>Explicación  
No se pudo asignar suficiente memoria para almacenar el descriptor interno del servidor.  
  
## <a name="user-action"></a>Acción del usuario  
Agregue más memoria a la máquina. Puede ser útil seguir un procedimiento para solucionar problemas genéricos de memoria.  
  
