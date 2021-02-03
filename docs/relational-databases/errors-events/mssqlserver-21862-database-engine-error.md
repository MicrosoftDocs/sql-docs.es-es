---
description: MSSQLSERVER_21862
title: MSSQLSERVER_21862 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 21862 (Database Engine error)
ms.assetid: a1d393dd-453b-4d45-9aa5-7d371213e32b
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 9e722121773db00eb1411b42a0206d83a22484c4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196402"
---
# <a name="mssqlserver_21862"></a>MSSQLSERVER_21862
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|MSSQLSERVER|  
|Id. de evento|21862|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|SQLErrorNum21862|  
|Texto del mensaje|Las columnas FILESTREAM no se pueden publicar en una publicación con el método de sincronización 'database snapshot' o 'database snapshot character'.|  
  
## <a name="explanation"></a>Explicación  
Dado que no se puede acceder a los datos FILESTREAM mediante una instantánea de base de datos, el Agente de instantáneas no podrá leer los datos FILESTREAM cuando se especifique el parámetro *database snapshot* o *database_snapshot_character* para el método de sincronización de la publicación.  
  
## <a name="user-action"></a>Acción del usuario  
Cambie el método de sincronización de la publicación para que no sea *database snapshot* ni *database_snapshot_character*, o simplemente excluya la columna FILESTREAM de la publicación.  
  
