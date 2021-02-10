---
description: Resincronizar comando propiedad dinámicos (ADO)
title: Comando Resync Property-Dynamic (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Resync Command property [ADO]
ms.assetid: 4e2bb601-0fe8-4d61-b00e-38341d85a6bb
author: rothja
ms.author: jroth
ms.openlocfilehash: f39af5f61e010a60a10aeb47788e862dd7b16064
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051596"
---
# <a name="resync-command-property-dynamic-ado"></a>Resincronizar comando propiedad dinámicos (ADO)
Especifica una cadena de comandos proporcionada por el usuario que el método [Resync](./resync-method.md) emite para actualizar los datos de la tabla denominada en la propiedad dinámica de [tabla única](./unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md) .  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 Establece o devuelve un valor de **cadena** que es una cadena de comando.  
  
## <a name="remarks"></a>Observaciones  
 El objeto de [conjunto de registros](./recordset-object-ado.md) es el resultado de una operación de combinación ejecutada en varias tablas base. Las filas afectadas dependen del parámetro *AffectRecords* del método [Resync](./resync-method.md) . El método de **resincronización** estándar se ejecuta si no se establecen las propiedades de [tabla única](./unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md) y **comando de resincronización** .  
  
 La cadena de comando de la propiedad de **comando Resync** es un procedimiento almacenado o comando con parámetros que identifica de forma única la fila que se está actualizando y devuelve una sola fila que contiene el mismo número y orden de columnas que la fila que se va a actualizar. La cadena de comandos contiene un parámetro para cada columna de clave principal de la **tabla única**; de lo contrario, se devuelve un error en tiempo de ejecución. Los parámetros se rellenan automáticamente con los valores de clave principal de la fila que se va a actualizar.  
  
 A continuación se muestran dos ejemplos basados en SQL:  
  
 1 \) el **conjunto de registros** se define mediante un comando:  
  
```  
SELECT * FROM Customers JOIN Orders ON   
   Customers.CustomerID = Orders.CustomerID  
   WHERE city = 'Seattle'  
   ORDER BY CustomerID  
```  
  
 La propiedad de **comando Resync** está establecida en:  
  
```  
"SELECT * FROM   
   (SELECT * FROM Customers JOIN Orders   
   ON Customers.CustomerID = Orders.CustomerID  
   city = 'Seattle' ORDER BY CustomerID)  
WHERE Orders.OrderID = ?"  
```  
  
 La **tabla única** es *Orders* y su clave principal, *OrderID*, tiene parámetros. La subselección proporciona una manera sencilla de asegurarse mediante programación de que el mismo número y orden de las columnas se devuelven como en el comando original.  
  
 2 \) el **conjunto de registros** se define mediante un procedimiento almacenado:  
  
```  
CREATE PROC Custorders @CustomerID char(5) AS   
SELECT * FROM Customers JOIN Orders ON   
Customers.CustomerID = Orders.CustomerID   
WHERE Customers.CustomerID = @CustomerID  
```  
  
 El método **Resync** debe ejecutar el siguiente procedimiento almacenado:  
  
```  
CREATE PROC CustordersResync @ordid int AS   
SELECT * FROM Customers JOIN Orders ON   
Customers.CustomerID = Orders.CustomerID  
WHERE Orders.ordid  = @ordid  
```  
  
 La propiedad de **comando Resync** está establecida en:  
  
```  
"{call CustordersResync (?)}"  
```  
  
 Una vez más, la **tabla única** es *Orders* y su clave principal, *OrderID*, tiene parámetros.  
  
 El **comando Resync** es una propiedad dinámica anexada a la colección de [propiedades](./properties-collection-ado.md) del objeto de **conjunto de registros** cuando la propiedad [CursorLocation](./cursorlocation-property-ado.md) está establecida en **adUseClient**.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)