---
description: Agregar columnas a una tabla (motor de base de datos)
title: Agregar columnas a una tabla (motor de base de datos) | Microsoft Docs
ms.custom: ''
ms.date: 10/27/2016
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- inserting columns
- columns [SQL Server], adding
- adding columns
ms.assetid: abeb8d52-d562-4e29-9e1e-2923ae874859
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4265056a6fe8485840e8026d7f1d0456d5dbeec0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181527"
---
# <a name="add-columns-to-a-table-database-engine"></a>Agregar columnas a una tabla (motor de base de datos)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

En este artículo se describe cómo agregar columnas nuevas a una tabla en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar

### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones

 Al usar la instrucción ALTER TABLE para agregar columnas a una tabla, se agregan automáticamente las columnas al final de la tabla. Si desea que las columnas aparezcan en un orden concreto en la tabla, use [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Sin embargo, tenga en cuenta que esto no es un procedimiento recomendado del diseño de base de datos. El procedimiento recomendado es especificar el orden en que las columnas se devuelven en el nivel de aplicación y de consulta. No debe confiar en el uso de SELECT * para devolver todas las columnas en un orden esperado según el orden en que están definidos en la tabla. Especifique siempre las columnas por nombre en las consultas y aplicaciones en el orden en que desea que aparezcan.

### <a name="security"></a><a name="Security"></a> Seguridad

#### <a name="permissions"></a><a name="Permissions"></a> Permisos

Requiere el permiso ALTER en la tabla.

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio

### <a name="to-insert-columns-into-a-table-with-table-designer"></a>Para insertar columnas en una tabla con el Diseñador de tablas

1. En el **Explorador de objetos**, haga clic con el botón derecho en la tabla a la que quiera agregar columnas y elija **Diseño**.
2. Haga clic en la primera celda vacía de la columna **Nombre de columna** .
3. Escriba el nombre de columna en la celda. El nombre de la columna es un valor obligatorio.
4. Presione la tecla TAB para desplazarse a la celda **Tipo de datos** y seleccione un tipo de datos en el menú desplegable.

   Este valor es obligatorio, por lo que, si no elige ninguno, se le asignará un valor predeterminado.

   > [!NOTE]
   >  Puede cambiar el valor predeterminado en el cuadro de diálogo **Opciones** situado bajo **Herramientas para bases de datos**.

5. Continúe definiendo las propiedades de la columna en la pestaña **Propiedades de columna** .

    > [!NOTE]
    >  Los valores predeterminados de las propiedades de la columna se agregan cuando crea una columna nueva, pero se pueden cambiar en la pestaña **Propiedades de columna** .

6. Cuando haya terminado de agregar columnas, en el menú **Archivo**, seleccione **Guardar** _nombre de tabla_.
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL
  
### <a name="to-insert-columns-into-a-table"></a>Para insertar columnas en una tabla  
  
El ejemplo siguiente agrega dos columnas a la tabla `dbo.doc_exa`.

```sql
ALTER TABLE dbo.doc_exa ADD column_b VARCHAR(20) NULL, column_c INT NULL ;
```

#### <a name="for-more-information-see-alter-table-40transact-sql41"></a><a name="FollowUp"></a> Para obtener más información, consulte [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)
