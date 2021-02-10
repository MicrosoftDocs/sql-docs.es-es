---
description: 'Paso 3: Obtención de un conjunto de registros por parte del servidor (Tutorial de RDS)'
title: 'Paso 3: el servidor obtiene un conjunto de registros (tutorial de RDS) | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS tutorial [ADO], server obtains Recordset
ms.assetid: 9c6779c9-1208-4696-ac51-c39f3a6d9240
author: rothja
ms.author: jroth
ms.openlocfilehash: d031c1af5719b711c5555af0db1bc1e420e2fc8d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036105"
---
# <a name="step-3-server-obtains-a-recordset-rds-tutorial"></a>Paso 3: Obtención de un conjunto de registros por parte del servidor (Tutorial de RDS)
El programa de servidor utiliza la cadena de conexión y el texto del comando para consultar el origen de datos para las filas deseadas. ADO suele usarse para recuperar este **conjunto de registros**, aunque podrían usarse otras interfaces de acceso a datos de Microsoft, como OLE DB.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
 Un programa de servidor personalizado podría tener este aspecto:  
  
```vb
Public Function ServerProgram(cn as String, qry as String) as Object  
Dim rs as New ADODB.Recordset  
   rs.CursorLocation = adUseClient  
   rs.Open qry, cn   
   rs.ActiveConnection = Nothing  
   Set ServerProgram = rs  
End Function  
```  
  
## <a name="see-also"></a>Consulte también  
 [Paso 4: el servidor devuelve el conjunto de registros (tutorial de RDS)](./step-4-server-returns-the-recordset-rds-tutorial.md)   
 [Tutorial de RDS (VBScript)](./rds-tutorial-vbscript.md)