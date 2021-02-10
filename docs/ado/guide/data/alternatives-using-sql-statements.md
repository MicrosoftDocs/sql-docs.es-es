---
description: 'Alternativas: Uso de instrucciones SQL'
title: 'Alternativas: usar instrucciones SQL | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- SQL statements [ADO]
- editing data [ADO], sql statements
- ADO, SQL statements
ms.assetid: 8b528b23-063d-45ea-8dea-6a90d4060b20
author: rothja
ms.author: jroth
ms.openlocfilehash: 91111602587b7857559a633a816d7fbb6b125dcb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028060"
---
# <a name="alternatives-using-sql-statements"></a>Alternativas: Uso de instrucciones SQL
ADO también permite el uso de comandos como alternativas a sus propiedades y métodos integrados para editar datos. En función del proveedor, todas las operaciones mencionadas en esta sección también se pueden llevar a cabo pasando comandos al origen de datos. Por ejemplo, las instrucciones UPDATE de SQL se pueden usar para modificar los datos sin utilizar la propiedad **Value** de un **campo**. Las instrucciones INSERT de SQL se pueden usar para agregar nuevos registros a un origen de datos, en lugar del método **AddNew** de ADO. Para obtener más información sobre SQL o el lenguaje de manipulación de datos de su proveedor, consulte la documentación del origen de datos.  
  
 Por ejemplo, puede pasar una cadena de SQL que contenga una instrucción DELETE a una base de datos, como se muestra en el código siguiente:  
  
```  
'BeginSQLDelete  
strSQL = "DELETE FROM Shippers WHERE ShipperID = " & intId  
objConn1.Execute strSQL, , adCmdText + adExecuteNoRecords  
'EndSQLDelete  
```
