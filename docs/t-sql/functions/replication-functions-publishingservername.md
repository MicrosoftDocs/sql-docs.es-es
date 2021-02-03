---
description: Funciones de replicación - PUBLISHINGSERVERNAME
title: PUBLISHINGSERVERNAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- PUBLISHINGSERVERNAME_TSQL
- PUBLISHINGSERVERNAME
dev_langs:
- TSQL
helpviewer_keywords:
- PUBLISHINGSERVERNAME function
- Publishers [SQL Server replication], names
ms.assetid: e7c278e5-ab23-419e-ab3e-3bb20b0636df
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 1d51901aef802b8c490b138781d9174fea1375a1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182277"
---
# <a name="replication-functions---publishingservername"></a>Funciones de replicación - PUBLISHINGSERVERNAME
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve el nombre del publicador que publica una base de datos que participa en una sesión de creación de reflejo de la base de datos. Esta función se ejecuta en una instancia del publicador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la base de datos de publicaciones. Utilícela para determinar el publicador original de la base de datos publicada.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
PUBLISHINGSERVERNAME()  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devuelto
 **nvarchar**  
  
## <a name="remarks"></a>Observaciones  
 PUBLISHINGSERVERNAME se utiliza en todos los tipos de replicación.  
  
 PUBLISHINGSERVERNAME se utiliza cuando existe una sesión de creación de reflejo de la base de datos en la base de datos de publicaciones entre el publicador y una instancia asociada reflejada.  
  
 Esta función debe ejecutarse dentro del contexto de una base de datos de publicaciones. Cuando PUBLISHINGSERVERNAME se ejecuta en una base de datos de publicaciones en la instancia del servidor reflejado de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se obtiene el nombre de la instancia del publicador desde el que se publica la base de datos. Cuando esta función se ejecuta en una base de datos en la instancia del servidor reflejado que no se ha publicado o que se ha publicado desde la misma tras una conmutación por error, se obtiene el nombre de esta instancia. Cuando esta función se ejecuta en la instancia del publicador original, se obtiene el nombre del publicador.  
  
## <a name="see-also"></a>Consulte también  
 [Replicación y creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md)   
 [Funciones de replicación &#40;Transact-SQL&#41;]()  
  
