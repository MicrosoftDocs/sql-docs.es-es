---
description: MSSQLSERVER_17130
title: MSSQLSERVER_17130 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 17130 (Database Engine error)
ms.assetid: 7ce6afca-221d-402f-89df-da7e74a339a8
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 05427e58b83e24844a0b9239d41e981b9e1b3272
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196778"
---
# <a name="mssqlserver_17130"></a>MSSQLSERVER_17130
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|17130|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|INIT_NOLOCKSPACE|  
|Texto del mensaje|Memoria insuficiente para el número de bloqueos configurado. Se está intentando iniciar con una tabla hash de bloqueos más pequeña, lo que puede afectar al rendimiento. Póngase en contacto con el administrador de la base de datos para configurar más memoria para esta instancia del motor de base de datos.|  
  
## <a name="explanation"></a>Explicación  
No hay suficiente memoria para el tamaño de tabla hash del administrador de bloqueos deseado.  Se intentará asignar una tabla hash menor.  
  
## <a name="user-action"></a>Acción del usuario  
Compruebe los parámetros de configuración de memoria del servidor (memoria de servidor máxima, memoria de servidor mínima) y si hay presiones de memoria. Proporcione más memoria para SQL Server.  
  
