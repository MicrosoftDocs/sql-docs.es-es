---
title: Migración de desencadenadores | Microsoft Docs
description: Obtenga información sobre las tablas optimizadas para memoria y los desencadenadores DDL, que se activan para las instrucciones CREATE, ALTER, DROP, GRANT, DENY, REVOKE, o UPDATE STATISTICS en una instancia de SQL Server.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: ad5385c5-5a50-40ca-a319-97d5606b8511
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4147c69de6eb360820ed7bd47c7ee7c8c867d52b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465366"
---
# <a name="migrating-triggers"></a>Migrar desencadenadores
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  En este tema se describen los desencadenadores DDL y las tablas optimizadas para memoria.  
  
 Se admiten los desencadenadores DML en tablas optimizadas en memoria, pero solo con el evento de desencadenador FOR | AFTER. Para obtener un ejemplo, vea [Implementación de UPDATE con FROM o subconsultas](../../relational-databases/in-memory-oltp/implementing-update-with-from-or-subqueries.md). 
  
 Los desencadenadores LOGON son desencadenadores definidos para activarse en eventos LOGON. Los desencadenadores LOGON no afectan a las tablas optimizadas para memoria.  
  
## <a name="ddl-triggers"></a>Desencadenadores DDL  
 Los desencadenadores DDL son desencadenadores definidos para activarse cuando se ejecuta una instrucción CREATE, ALTER, DROP, GRANT, DENY, REVOKE o UPDATE STATISTICS en la base de datos o el servidor en el que se define.  
  
 No se pueden crear tablas optimizadas para memoria si la base de datos o el servidor tiene uno o varios desencadenadores DDL definidos en CREATE_TABLE o en cualquier grupo de eventos que lo incluyan. No se puede quitar una tabla optimizada para memoria si la base de datos o el servidor tiene uno o varios desencadenadores DDL definidos en DROP_TABLE o en cualquier grupo de eventos que lo incluyan.  
  
 No se pueden crear procedimientos almacenados compilados de forma nativa si hay uno o más desencadenadores DDL en CREATE_PROCEDURE, DROP_PROCEDURE o cualquier grupo de eventos que incluya los eventos.  
  
## <a name="see-also"></a>Consulte también  
 [Migrar a OLTP en memoria](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)  
  
