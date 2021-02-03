---
description: MSSQLSERVER_1461
title: MSSQLSERVER_1461 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1461 (Database Engine error)
ms.assetid: fce10907-4753-441b-b624-f28e00ed7520
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 2dea7c8af8be207fd164563596d2084b87747acd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196945"
---
# <a name="mssqlserver_1461"></a>MSSQLSERVER_1461
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|1461|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|DBM_SAFETY_MISMATCH|  
|Texto del mensaje|Se detectaron distintos niveles de seguridad en los servidores para la creación de reflejo de la base de datos "%.*ls". Se utilizará el nivel de seguridad FULL.|  
  
## <a name="explanation"></a>Explicación  
Las conexiones de creación de reflejo se interrumpieron cuando el nivel de seguridad de la transacción se modificó porque la configuración de seguridad de la transacción era incoherente en las bases de datos principal y reflejada. Se usará la configuración de seguridad predeterminada de seguridad de transacciones completa. La sesión se ejecutará en modo de alta seguridad.  
  
## <a name="user-action"></a>Acción del usuario  
Para desactivar la seguridad de transacciones, vuelva a ejecutar la instrucción ALTER DATABASE *nombreDeBaseDeDatos* SET PARTNER SAFETY OFF en la base de datos principal.  
  
