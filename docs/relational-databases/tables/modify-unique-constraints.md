---
description: Modificar restricciones UNIQUE
title: Modificación de restricciones UNIQUE | Microsoft Docs
ms.custom: ''
ms.date: 10/12/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- modifying constraints
- UNIQUE constraints [SQL Server], modifying
- constraints [SQL Server], modifying
- constraints [SQL Server], unique
ms.assetid: fddbdc9e-958b-4614-8e88-6ca205d64a4e
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: dba88ef2d1aa6239d0e3bbac92b392ee96a7270c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184287"
---
# <a name="modify-unique-constraints"></a>Modificar restricciones UNIQUE

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Puede modificar una restricción UNIQUE en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Seguridad](#Security)  
  
-   **Para modificar una restricción UNIQUE con:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Requiere el permiso ALTER en la tabla.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-modify-a-unique-constraint"></a>Para modificar una restricción UNIQUE  
  
1.  En el **Explorador de objetos**, haga clic con el botón derecho en la tabla que contiene la restricción UNIQUE y seleccione **Diseño**.  
  
2.  En el menú **Diseñador de tablas**, haga clic en **Índices o claves...** .  
  
3.  En el cuadro de diálogo **Índices o claves** , en **Clave principal o única, o índice seleccionado**, seleccione la restricción que quiera modificar.  
  
4.  Complete una de las acciones descritas en la tabla siguiente:  
  
    |En|Siga estos pasos|  
    |--------|------------------------|  
    |Cambiar las columnas a las que está asociada la restricción|1) En la cuadrícula situada debajo de **(General)**, haga clic en **Columnas** y después en los puntos suspensivos **(...)** situados a la derecha de la propiedad.<br /><br /> 2) En el cuadro de diálogo **Columnas de índice** , especifique la nueva columna o criterio de ordenación (o ambos) del índice.|  
    |Cambiar el nombre de la restricción|En la cuadrícula situada debajo de **Identidad**, escriba un nuevo nombre en el cuadro **Nombre** . Asegúrese de que el nuevo nombre no esté duplicado en la lista **Clave principal o única, o índice seleccionado** .|  
    |Establecer la opción de índice clúster|En la cuadrícula situada debajo de **Diseñador de tablas**, seleccione **Crear como CLUSTERED** y, en el menú desplegable, elija Sí para crear un índice agrupado o No para crear un índice no agrupado. Solo puede existir un índice clúster por tabla. Si ya existe un índice clúster en esta tabla, deberá desactivar esta configuración en el índice original.|  
    |Definir un factor de relleno|En la cuadrícula situada debajo de **Diseñador de tablas**, expanda la categoría **Especificación de relleno** y escriba un entero de 0 a 100 en el cuadro **Factor de relleno** .|  
  
5.  En el menú **Archivo**, haga clic en ***Guardar**_nombre de tabla_.  
  
##  <a name="to-modify-a-unique-constraint"></a><a name="TsqlProcedure"></a> **Para modificar una restricción UNIQUE**  
  
 Para modificar una restricción UNIQUE mediante Transact-SQL, deberá eliminar la restricción UNIQUE existente y, a continuación, volver a crearla con la nueva definición. Para obtener más información, consulte [Delete Unique Constraints](../../relational-databases/tables/delete-unique-constraints.md) y [Create Unique Constraints](../../relational-databases/tables/create-unique-constraints.md).  
  
###  <a name="TsqlExample"></a>  
