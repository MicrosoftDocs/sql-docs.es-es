---
description: MSSQLSERVER_8710
title: MSSQLSERVER_8710 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 8710 (Database Engine error)
ms.assetid: 78b9f9da-5489-4be0-94df-f065d86ed18c
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: f515311384b33f508bbf125f80977a995fa7cb2b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194058"
---
# <a name="mssqlserver_8710"></a>MSSQLSERVER_8710
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|MSSQLSERVER|  
|Id. de evento|8710|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|QUERY2_CUBE_ILLEGAL_AGG_FUNC|  
|Texto del mensaje|Las funciones de agregado que se utilizan con consultas CUBE, ROLLUP o GROUPING SET se deben proporcionar para la combinación de subagregados. Para solucionar este problema, quite la función de agregado o escriba la consulta utilizando UNION ALL en cláusulas GROUP BY.|  
  
## <a name="explanation"></a>Explicación  
Se ha usado una función de agregado con CUBE, ROLLUP o GROUPING SETS que no proporciona un método para combinar subagregados.  
  
## <a name="user-action"></a>Acción del usuario  
Para solucionar este problema, quite la función de agregado o escriba la consulta utilizando UNION ALL en cláusulas GROUP BY.  
  
