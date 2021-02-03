---
description: MSSQLSERVER_21898
title: MSSQLSERVER_21898 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 21898 (Database Engine error)
ms.assetid: 02405b21-3d4e-4c2d-b4b3-d7b1ec05edb4
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a899e50f0cfe5fb4d3e2c7b7aa63ebfa76afa6b8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208517"
---
# <a name="mssqlserver_21898"></a>MSSQLSERVER_21898
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|21898|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|SQLErrorNum21898|  
|Texto del mensaje|El publicador '%s' usa la base de datos de distribución '%s' y no '%s', que se requiere para hospedar la base de datos de publicación '%s'. Ejecute **sp_changedistpublisher** en el distribuidor '%s' para cambiar la base de datos de distribución que usa el publicador por '%s'.|  
  
## <a name="explanation"></a>Explicación  
**sp_validate_redirected_publisher** consulta a msdb.dbo.MSdistpublishers en el distribuidor local para comprobar que la base de datos de distribución usada por el nuevo publicador es la misma que la base de datos de distribución usada por el publicador original. Este error se devuelve cuando estas bases de datos son distintas, con lo que el editor no es un host adecuado para la base de datos del publicador.  
  
## <a name="user-action"></a>Acción del usuario  
Ejecute el procedimiento almacenado **sp_changedistpublisher** para cambiar la base de datos de distribución para el nuevo publicador por la usada por el publicador original.  
  
> [!NOTE]  
> La ejecución de **sp_changedistpublisher** corregirá el problema si se introdujo una base de datos de distribución errónea cuando se ejecutó **sp_adddistpublisher** en el distribuidor para el publicador. No obstante, si el publicador remoto tiene publicaciones existentes de otra base de datos de publicación que haga uso de la base de datos de distribución identificada, este cambio no resulta adecuado. La replicación con la base de datos de distribución con nombre se debe quitar sistemáticamente y, después, se debe volver a establecer con la base de datos de distribución del publicador original para que el publicador nuevo funcione como un host adecuado.  
  
