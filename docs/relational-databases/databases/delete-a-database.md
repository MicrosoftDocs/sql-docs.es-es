---
title: Eliminación de una base de datos | Microsoft Docs
description: Obtenga información sobre cómo eliminar una base de datos definida por un usuario en SQL Server Management Studio en SQL Server mediante SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- database removal [SQL Server], SQL Server Management Studio
- removing databases
- deleting databases
- dropping databases
- databases [SQL Server], dropping
- database removal [SQL Server]
ms.assetid: 1fd8c0f5-03e1-449a-af45-b8cacb479d9c
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8299034ab5ce3baf01c39391777ab551569887f8
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813049"
---
# <a name="delete-a-database"></a>Eliminar una base de datos
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  En este tema se describe cómo eliminar una base de datos definida por el usuario en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Requisitos previos](#Prerequisites)  
  
     [Recomendaciones](#Recommendations)  
  
     [Seguridad](#Security)  
  
-   **Para eliminar una base de datos, use:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Seguimiento:**  [tras eliminar una base de datos](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   Las bases de datos del sistema no se pueden eliminar.  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Requisitos previos  
  
-   Elimine las instantáneas de bases de datos que existan en la base de datos. Para obtener más información, vea [Eliminar una instantánea de base de datos &#40;Transact-SQL&#41;](../../relational-databases/databases/drop-a-database-snapshot-transact-sql.md).  
  
-   Si la base de datos interviene en el trasvase de registros, elimínelo.  
  
-   Si la base de datos se publica para la replicación transaccional, o se publica o suscribe para la replicación de mezcla, elimine la replicación de la base de datos.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
  
-   Tenga en cuenta la posibilidad de realizar una copia de seguridad completa de la base de datos. Una base de datos eliminada solo puede volver a crearse si se restaura una copia de seguridad.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Para ejecutar DROP DATABASE, el usuario debe tener, como mínimo, el permiso CONTROL en la base de datos.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-delete-a-database"></a>Para eliminar una base de datos  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]y, a continuación, expándala.  
  
2.  Expanda **Bases de datos**, haga clic con el botón derecho en la base de datos que quiera eliminar y, luego, haga clic en **Eliminar**.  
  
3.  Confirme que ha seleccionado la base de datos correcta y haga clic en **Aceptar**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-delete-a-database"></a>Para eliminar una base de datos  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. El ejemplo quita las bases de datos `Sales` y `NewSales` .  
  
```sql  
USE master ;  
GO  
DROP DATABASE Sales, NewSales ;  
GO  
```  
  
##  <a name="follow-up-after-deleting-a-database"></a><a name="FollowUp"></a> Seguimiento: tras eliminar una base de datos  
 Hacer una copia de seguridad de la base de datos **maestra** . Si es necesario restaurar la base de datos **maestra** , cualquier base de datos que se haya eliminado después de la última copia de seguridad **maestra** seguirá teniendo referencias en las vistas del catálogo del sistema y puede dar lugar a la aparición de mensajes de error.  
  
## <a name="see-also"></a>Consulte también  
 [CREATE DATABASE &#40;Transact-SQL de SQL Server&#41;](../../t-sql/statements/create-database-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)  
  
