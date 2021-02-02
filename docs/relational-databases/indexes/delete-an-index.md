---
description: Eliminar un índice
title: Eliminación de un índice | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- removing indexes
- deleting indexes
- dropping indexes
- indexes [SQL Server], dropping
- index deletions [SQL Server]
ms.assetid: fd38a0ed-26c4-4c76-9ef7-e0a16147329d
author: MikeRayMSFT
ms.author: mikeray
monikerRange: = azuresqldb-current || >= sql-server-2016
ms.openlocfilehash: 5275fe35478ddfcccb7cd7f70da0d2d8c8998c7f
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048875"
---
# <a name="delete-an-index"></a>Eliminar un índice
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx_md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  En este tema se describe cómo eliminar (quitar) un índice en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Seguridad](#Security)  
  
-   **Para eliminar un índice, use:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
 Los índices creados como resultado de una restricción PRIMARY KEY o UNIQUE no se pueden eliminar con este método. En su lugar, se debe eliminar la restricción. Para quitar la restricción y el índice correspondiente, use [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) con la cláusula DROP CONSTRAINT en [!INCLUDE[tsql](../../includes/tsql-md.md)]. Para obtener más información, consulte [Delete Primary Keys](../../relational-databases/tables/delete-primary-keys.md).  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Requiere el permiso ALTER en la tabla o la vista. Este permiso se concede de forma predeterminada al rol fijo de servidor **sysadmin** y a los roles fijos de base de datos **db_ddladmin** y **db_owner** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-delete-an-index-by-using-object-explorer"></a>Para eliminar un índice mediante el Explorador de objetos  
  
1.  En el Explorador de objetos, expanda la base de datos que contiene la tabla en la que desea eliminar un índice.  
  
2.  Expanda la carpeta **Tablas** .  
  
3.  Expanda la tabla que contiene el índice que desee eliminar.  
  
4.  Expanda la carpeta **Índices** .  
  
5.  Haga clic con el botón derecho en el índice que quiere eliminar y seleccione **Eliminar**.  
  
6.  En el cuadro de diálogo **Eliminar objeto** , compruebe que el índice correcto se encuentra en la cuadrícula **Objeto que se va a eliminar** y haga clic en **Aceptar**.  
  
#### <a name="to-delete-an-index-using-table-designer"></a>Para eliminar un índice mediante el Diseñador de tablas  
  
1.  En el Explorador de objetos, expanda la base de datos que contiene la tabla en la que desea eliminar un índice.  
  
2.  Expanda la carpeta **Tablas** .  
  
3.  Haga clic con el botón secundario en la tabla que contiene el índice que desee eliminar y haga clic en Diseño.  
  
4.  En el menú **Diseñador de tablas** , haga clic en **Índices o claves**.  
  
5.  En el cuadro de diálogo **Índices o claves** , seleccione el índice que quiera eliminar.  
  
6.  Haga clic en **Eliminar**.  
  
7.  Haga clic en **Cerrar**.  
  
8.  En el menú **Archivo** , seleccione **Guardar**_nombre_tabla_.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-delete-an-index"></a>Para eliminar un índice  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- delete the IX_ProductVendor_BusinessEntityID index  
    -- from the Purchasing.ProductVendor table  
    DROP INDEX IX_ProductVendor_BusinessEntityID   
        ON Purchasing.ProductVendor;  
    GO  
    ```  
  
 Para obtener más información, vea [DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md).  
  
  
