---
description: MSSQLSERVER_9692
title: MSSQLSERVER_9692 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 9692 (Database Engine error)
ms.assetid: 02399d6e-ab5e-4f30-8a3e-2bb1e8c135b5
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: f086df43d106810e471d8795dab212dcfc0c1dd9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187565"
---
# <a name="mssqlserver_9692"></a>MSSQLSERVER_9692
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|9692|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|SB2_CANT_LISTEN_PORT_IN_USE|  
|Texto del mensaje|El transporte de protocolo %S_MSG no puede escuchar en el puerto %d porque lo está utilizando otro proceso.|  
  
## <a name="explanation"></a>Explicación  
Otro programa del equipo está usando el puerto TCP indicado.  
  
## <a name="user-action"></a>Acción del usuario  
Ejecute **netstat -aon** para determinar qué programa está usando el puerto. Deshabilite esa aplicación o especifique otro puerto para Service Broker.  
  
