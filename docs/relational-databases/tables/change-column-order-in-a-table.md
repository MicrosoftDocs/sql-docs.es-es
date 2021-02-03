---
description: Cambiar el orden de las columnas de una tabla
title: Cambio del orden de las columnas en una tabla | Microsoft Docs
ms.custom: ''
ms.date: 06/15/2018
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- columns [SQL Server], change order in a table
- column order, change
ms.assetid: cd99ef56-9085-431a-a0fc-58e7add5399f
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d8064366c25925bf5ba6781e0afa0168a8df04fe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181515"
---
# <a name="change-column-order-in-a-table"></a>Cambiar el orden de las columnas de una tabla

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  Puede cambiar el orden de las columnas en el Diseñador de tablas en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
> [!CAUTION]  
>  Si cambia el orden de las columnas de una tabla, el código y las aplicaciones que dependen de un orden de columnas determinado pueden verse afectados. Los elementos afectados pueden ser consultas, vistas, procedimientos almacenados, funciones definidas por el usuario y aplicaciones cliente. Tenga en cuenta las consecuencias antes de realizar cualquier cambio en el orden de las columnas. El procedimiento recomendado es especificar el orden en que las columnas se devuelven en el nivel de aplicación y de consulta. No debe confiar en el uso de SELECT * para devolver todas las columnas en un orden esperado según el orden en que están definidos en la tabla. Especifique siempre las columnas por nombre en las consultas y aplicaciones en el orden en que desea que aparezcan.  
  
 **En este tema**  
  
-   **Para cambiar el orden de las columnas con:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-change-the-column-order"></a>Para cambiar el orden de las columnas  
  
1.  En el **Explorador de objetos**, haga clic con el botón derecho en la tabla cuyas columnas quiera reordenar y seleccione **Diseñar**.  
  
2.  Seleccione la casilla que hay a la izquierda del nombre de la columna cuyo orden desea cambiar.  
  
3.  Arrastre la columna a otra ubicación en la tabla.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 **Para cambiar el orden de las columnas**  
  
 Esta tarea no es compatible con las instrucciones Transact-SQL.  
  
###  <a name="TsqlExample"></a>  
