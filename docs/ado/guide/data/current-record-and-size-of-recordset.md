---
description: Registro actual y el tamaño del conjunto de registros
title: Registro y tamaño actuales del conjunto de registros | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- record location [ADO]
- current record [ADO]
ms.assetid: e63ff331-8655-4be7-82c6-e6cd6cc9d16d
author: rothja
ms.author: jroth
ms.openlocfilehash: 791ca36a7f5a61a0ad0208753386fba826d6d06e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033345"
---
# <a name="current-record-and-size-of-recordset"></a>Registro actual y el tamaño del conjunto de registros
En esta sección se describe cómo buscar la posición actual del cursor en el **conjunto de registros** de ejemplo en el [ejemplo de código JScript para devolver un conjunto de registros](./jscript-code-example-to-return-a-recordset.md).  
  
## <a name="current-record"></a>Registro actual  
 El registro actual del conjunto de registros corresponde a que apunta a la posición del cursor del objeto de **conjunto de registros** . Cuando se devuelve un objeto de **conjunto de registros** desde el origen de datos como resultado de la llamada a **Recordset. Open**, **Command.Exe** a la variable, o **Connection.Exe** de la misma (incluidos **Connection. NamedCommand** y **Connection. StoredProcedure**), el cursor se establece para apuntar al primer registro. En el conjunto de registros de ejemplo, el registro actual inicial es el elemento "de la" tripas orgánicas orgánicos ".  
  
## <a name="size-of-recordset"></a>Tamaño del conjunto de registros  
 Para averiguar el tamaño de un objeto de **conjunto de registros** , obtenga el valor de la propiedad **Recordset. RecordCount** . Este valor es un entero largo que indica el número de registros del **conjunto** de registros. Si el proveedor OLEDB devuelve el DataSet para Microsoft SQL Server, este valor proporciona el número de filas devueltas. La lectura de la propiedad **RecordCount** en un **conjunto de registros** cerrado produce un error.  
  
 Si no se puede determinar el número de registros, el valor de la propiedad es-1.  
  
 El valor de la propiedad **RecordCount** también depende de las capacidades del proveedor y el tipo de cursor utilizado. Para un cursor de solo avance, el valor es-1. Para un cursor estático o de conjunto de claves, el valor es el número real de registros devueltos en el objeto de **conjunto de registros** . Para un cursor dinámico, el valor es-1 o el número real de registros, dependiendo del origen de datos.  
  
 Un cursor que admita **RecordCount** debe ser más difícil y, por tanto, requiere más potencia de computación, que un cursor no admite **RecordCount**. Si no necesita conocer el número de registros, el uso de diferentes tipos de cursor puede ayudar a mejorar el rendimiento de la aplicación, sobre todo si debe tratar con un conjunto de datos grande.  
  
 En algunos casos, un proveedor o cursor no puede determinar el valor de **RecordCount** sin obtener primero todos los registros del origen de datos. Para garantizar un recuento preciso, llame al **conjunto de registros**. Método **MoveLast** antes de llamar a **Recordset. RecordCount**.  
  
 El objeto de **conjunto de registros** de ejemplo obtenido mediante el ejemplo de [código JScript](./jscript-code-example-to-return-a-recordset.md) utiliza un cursor de solo avance, por lo que llamar a **RecordCount** en este objeto siempre da como resultado-1. Si cambia la línea de código que llama al **conjunto de registros**. **Open** Method tal y como se muestra en el ejemplo siguiente, la propiedad **RecordCount** devolverá el número real de registros capturados.  
  
```  
oRs.Open sSQL, sCnStr, adOpenStatic, adLockOptimistic, adCmdText   
```  
  
 Esto se debe a que los cursores estáticos con el [proveedor de OLE DB de Microsoft para SQL Server](../appendixes/microsoft-ole-db-provider-for-sql-server.md) admiten **RecordCount**. En este ejemplo, hay cinco registros y, por tanto, **RecordCount** debe proporcionar el valor de 5.  
  
 Esta sección contiene el siguiente tema.  
  
 [Límites de un conjunto de registros](./boundaries-of-a-recordset.md)