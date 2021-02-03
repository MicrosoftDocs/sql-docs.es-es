---
description: MSSQLSERVER_4064
title: MSSQLSERVER_4064 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 4064 (Database Engine error)
ms.assetid: 32112b90-0a2f-4834-a027-756811732be7
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: cdf7443042df535359320ccee9631a7612ecbdf1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185140"
---
# <a name="mssqlserver_4064"></a>MSSQLSERVER_4064
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|4064|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|DB_UFAIL_FATAL|  
|Texto del mensaje|No se puede abrir la base de datos predeterminada del usuario. Error de inicio de sesión.|  
  
## <a name="explanation"></a>Explicación  
El inicio de sesión de SQL Server no pudo conectarse debido a un problema con su base de datos predeterminada. Bien la propia base de datos no es válida o el inicio de sesión no tiene permiso CONNECT para la base de datos.  
  
## <a name="user-action"></a>Acción del usuario  
Use ALTER LOGIN para cambiar la base de datos predeterminada del inicio de sesión. Conceda el permiso CONNECT al inicio de sesión.  
  
