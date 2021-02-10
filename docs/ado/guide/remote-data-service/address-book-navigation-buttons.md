---
description: Botones de navegación de la libreta de direcciones
title: Botones de navegación de la libreta de direcciones | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS scenarios [ADO], navigation buttons
- address book application scenario [ADO], navigation buttons
ms.assetid: f0dd84c6-5c33-4ab9-82b4-4c42dfdd2277
author: rothja
ms.author: jroth
ms.openlocfilehash: e9ee0bb698c64de6fb04f776caa4e34c27b8d49a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032067"
---
# <a name="address-book-navigation-buttons"></a>Botones de navegación de la libreta de direcciones
La aplicación de libreta de direcciones muestra los botones de navegación en la parte inferior de la Página Web. Puede usar los botones de navegación para desplazarse por los datos de la pantalla de la cuadrícula HTML seleccionando la primera o la última fila de datos o las filas adyacentes a la selección actual.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="navigation-sub-procedures"></a>Procedimientos sub de navegación  
 La aplicación de libreta de direcciones contiene varios procedimientos que permiten a los usuarios hacer clic en los botones **primero**, **siguiente**, **anterior** y **último** para desplazarse por los datos.  
  
 Por ejemplo, al hacer clic en el **primer** botón, se activa el procedimiento Sub de VBScript First_OnClick. El procedimiento ejecuta un método [MoveFirst](../../reference/rds-api/movefirst-movelast-movenext-and-moveprevious-methods-rds.md) , que hace que la primera fila de datos sea la selección actual. Al hacer clic en el **último** botón, se activa el procedimiento Sub Last_OnClick, que invoca el método [MoveLast](../../reference/rds-api/movefirst-movelast-movenext-and-moveprevious-methods-rds.md) , convirtiendo la última fila de datos en la selección actual. Los botones de navegación restantes funcionan de manera similar.  
  
```vb
' Move to the first record in the bound Recordset.  
Sub First_OnClick  
   DC1.Recordset.MoveFirst  
End Sub  
  
' Move to the next record from the current position   
' in the bound Recordset.  
Sub Next_OnClick  
   If Not DC1.Recordset.EOF Then    ' Cannot move beyond bottom record.  
      DC1.Recordset.MoveNext  
      Exit Sub  
   End If     
End Sub  
  
' Move to the previous record from the current position in the bound   
' Recordset.  
Sub Prev_OnClick  
   If Not DC1.Recordset.BOF Then    ' Cannot move beyond top record.  
      DC1.Recordset.MovePrevious  
      Exit Sub  
   End If  
End Sub  
  
' Move to the last record in the bound Recordset.  
Sub Last_OnClick  
   DC1.Recordset.MoveLast  
End Sub  
```  
  
## <a name="see-also"></a>Consulte también  
 [Objeto DataControl (RDS)](../../reference/rds-api/datacontrol-object-rds.md)   
 [MoveFirst, MoveLast, MoveNext y MovePrevious métodos (RDS)](../../reference/rds-api/movefirst-movelast-movenext-and-moveprevious-methods-rds.md)