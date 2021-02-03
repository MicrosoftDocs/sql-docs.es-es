---
description: Usar las tablas insertadas y eliminadas
title: Usar las tablas insertadas y eliminadas | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- inserted tables
- UPDATE statement [SQL Server], DML triggers
- DELETE statement [SQL Server], DML triggers
- INSTEAD OF triggers
- deleted tables
- INSERT statement [SQL Server], DML triggers
- DML triggers, deleted or inserted tables
ms.assetid: ed84567f-7b91-4b44-b5b2-c400bda4590d
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6f78a502be14ffcf51937823d61bd61c144777f5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187259"
---
# <a name="use-the-inserted-and-deleted-tables"></a>Usar las tablas insertadas y eliminadas
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  En las instrucciones de desencadenadores DML se usan dos tablas especiales: la tabla inserted y la tabla deleted. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crea y administra automáticamente ambas tablas. Puede utilizar estas tablas temporales residentes en memoria para probar los efectos de determinadas modificaciones de datos y para establecer condiciones para las acciones de los desencadenadores DML. No puede modificar directamente los datos de estas tablas ni realizar en ellas operaciones de lenguaje de definición de datos (DDL), como CREATE INDEX.  
  
 En los desencadenadores DML, las tablas inserted y deleted se utilizan principalmente para realizar las siguientes tareas:  
  
-   Ampliar la integridad referencial entre tablas.  
  
-   Insertar o actualizar datos de tablas base subyacentes a una vista.  
  
-   Comprobar errores y realizar acciones en función del error.  
  
-   Conocer la diferencia entre el estado de una tabla antes y después de realizar una modificación en los datos, y actuar en función de dicha diferencia.  
  
 La tabla deleted almacena copias de las filas afectadas por las instrucciones DELETE y UPDATE. Durante la ejecución de una instrucción DELETE o UPDATE, las filas se eliminan de la tabla del desencadenador y se transfieren a la tabla deleted. La tabla deleted y la tabla del desencadenador no suelen tener filas en común.  
  
 La tabla inserted almacena copias de las filas afectadas durante las instrucciones INSERT y UPDATE. Durante una transacción de inserción o actualización, se agregan nuevas filas a la tabla inserted y a la tabla del desencadenador. Las filas de la tabla inserted son copias de las nuevas filas de la tabla del desencadenador.  
  
 Una transacción de actualización es similar a una operación de eliminación seguida de una operación de inserción; primero, se copian las filas antiguas en la tabla deleted y luego se copian las filas nuevas en la tabla del desencadenador y en la tabla inserted.  
  
 Cuando establezca condiciones para el desencadenador, utilice las tablas inserted y deleted correspondientes a la acción que lo activó. Aunque no se produce ningún error al hacer referencia a la tabla deleted cuando se prueba una instrucción INSERT, o bien al hacer referencia a la tabla inserted cuando se prueba una instrucción DELETE, estas tablas de prueba del desencadenador no contendrán filas en estos casos.  
  
> [!NOTE]  
>  Si las acciones del desencadenador dependen del número de filas afectadas por una modificación de datos, use pruebas (como, por ejemplo, un examen de @@ROWCOUNT) para las modificaciones de datos que afecten a varias filas (instrucciones INSERT, DELETE o UPDATE basadas en una instrucción SELECT) y tome las medidas oportunas. Para más información, consulte [Crear desencadenadores DML para administrar varias filas de datos](../../relational-databases/triggers/create-dml-triggers-to-handle-multiple-rows-of-data.md).
  
 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] no permite referencias a las columnas **text**, **ntext** o **image** en las tablas insertadas y eliminadas por los desencadenadores AFTER. Sin embargo, estos tipos de datos se incluyen únicamente por motivos de compatibilidad con versiones anteriores. El método preferido para almacenar datos grandes es usar los tipos de datos **varchar(max)** , **nvarchar(max)** y **varbinary(max)** . Tanto los desencadenadores AFTER como INSTEAD OF admiten los datos **varchar(max)** , **nvarchar(max)** y **varbinary(max)** en las tablas insertadas y eliminadas. Para obtener más información, vea [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md).  
  
 **Ejemplo del uso de la tabla inserted en un desencadenador para exigir reglas de negocios**  
  
 Debido a que las restricciones CHECK solo pueden hacer referencia a las columnas en las que se han definido las restricciones de columna o de tabla, cualquier restricción entre tablas, en este caso, reglas de negocios, debe definirse como desencadenadores.  
  
 En este ejemplo se crea un desencadenador DML. El desencadenador comprueba que la solvencia del proveedor es satisfactoria cuando se intenta insertar un nuevo pedido de compra en la tabla `PurchaseOrderHeader` . Para obtener la solvencia de crédito del proveedor correspondiente al pedido de compra recién insertado, la tabla `Vendor` debe hacer referencia a la tabla insertada y estar combinada con ella. Si la solvencia no es satisfactoria, se obtiene un mensaje y no se ejecuta la inserción.
  
 [!code-sql[TriggerDDL#CreateTrigger3](../../relational-databases/triggers/codesnippet/tsql/use-the-inserted-and-del_1.sql)]  
  
## <a name="using-the-inserted-and-deleted-tables-in-instead-of-triggers"></a>Utilizar las tablas inserted y deleted en desencadenadores INSTEAD OF  
 Las tablas inserted y deleted pasadas a desencadenadores INSTEAD OF definidos en tablas siguen las mismas reglas que las tablas inserted y deleted pasadas a desencadenadores AFTER. El formato de las tablas inserted y deleted es el mismo que el de una tabla que tiene definido un desencadenador INSTEAD OF. Cada columna de las tablas inserted y deleted se asigna directamente a una columna de la tabla base.  
  
 Las siguientes reglas aplicables cuando una instrucción INSERT o UPDATE que hace referencia a una tabla con un desencadenador INSTEAD OF debe suministrar valores para las columnas, son las mismas que se utilizan cuando la tabla no tiene un desencadenador INSTEAD OF:  
  
-   No se pueden especificar los valores para las columnas calculadas ni para las que contienen el tipo de datos **timestamp** .  
  
-   Solo se pueden especificar valores para columnas con la propiedad IDENTITY si la opción IDENTITY_INSERT se ha establecido en ON para la tabla. Cuando el valor de IDENTITY_INSERT es ON, las instrucciones INSERT deben suministrar un valor.  
  
-   Las instrucciones INSERT deben suministrar valores para todas las columnas NOT NULL que no tienen restricciones DEFAULT.  
  
-   Los valores son opcionales para cualquier columna que admita valores NULL o para cualquier columna NOT NULL que tenga una definición DEFAULT, excepto para las columnas calculadas, de identidad o **timestamp** .  
  
 Cuando una instrucción INSERT, UPDATE o DELETE hace referencia a una vista que tiene un desencadenador INSTEAD OF, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] llama al desencadenador en lugar de realizar una acción directa en cualquier tabla. El desencadenador utiliza la información de las tablas inserted y deleted a fin de generar las instrucciones necesarias para implementar la acción solicitada en las tablas base, incluso si el formato de la información de las tablas inserted y deleted generadas para la vista es distinto del formato de los datos de las tablas base.  
  
 El formato de las tablas inserted y deleted pasado a un desencadenador INSTEAD OF definido en una vista coincide con la lista de selección de la instrucción SELECT definida para la vista. Por ejemplo:  
  
```  
USE AdventureWorks2012;  
GO  
CREATE VIEW dbo.EmployeeNames (BusinessEntityID, LName, FName)  
AS  
SELECT e.BusinessEntityID, p.LastName, p.FirstName  
FROM HumanResources.Employee AS e   
JOIN Person.Person AS p  
ON e.BusinessEntityID = p.BusinessEntityID;  
```  
  
 El conjunto de resultados de esta vista tiene tres columnas: una columna de tipo **int** y dos columnas de tipo **nvarchar** . Las tablas insertadas y eliminadas a un desencadenador INSTEAD OF también tiene una columna **int** llamada `BusinessEntityID`, una columna **nvarchar** llamada `LName`y una columna **nvarchar** llamada `FName`.  
  
 La lista de selección de una vista también puede contener expresiones que no se asignen directamente a una sola columna de tabla base. Es posible que algunas expresiones de las vistas, como las llamadas a constantes o funciones, no hagan referencia a ninguna columna y puedan omitirse. Las expresiones complejas pueden hacer referencia a varias columnas, aunque las tablas inserted y deleted solo tienen un valor para cada fila insertada. Esto mismo se aplica a las expresiones sencillas de una vista si hacen referencia a columnas calculadas que contienen expresiones complejas. Los desencadenadores INSTEAD OF de las vistas pueden tratar estos tipos de expresiones.  
  
  
