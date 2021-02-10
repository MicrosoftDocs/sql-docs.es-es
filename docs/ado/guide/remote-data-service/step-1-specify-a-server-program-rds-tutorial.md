---
description: 'Paso 1: Especificación de un programa de servidor (Tutorial de RDS)'
title: 'Paso 1: especificar un programa de servidor (tutorial de RDS) | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS tutorial [ADO], specifying server program
ms.assetid: d8bb35b1-c02a-4231-8d55-016e56e53b95
author: rothja
ms.author: jroth
ms.openlocfilehash: d0e59c52ee311c5172b3cdc15f58c5e9448f7110
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036405"
---
# <a name="step-1-specify-a-server-program-rds-tutorial"></a>Paso 1: Especificación de un programa de servidor (Tutorial de RDS)
En el caso más general, use [RDS. DataSpace](../../reference/rds-api/dataspace-object-rds.md) Object, método [CreateObject](../../reference/rds-api/createobject-method-rds.md) para especificar el programa de servidor predeterminado, [RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md), o su propio programa de servidor personalizado (objeto comercial). Se crea una instancia de un programa de servidor en el servidor y se devuelve una referencia al programa de servidor, o *proxy*.  
  
 En este tutorial se usa el programa de servidor predeterminado:  
  
```vb
Sub RDSTutorial1()  
   Dim DS as New RDS.DataSpace  
   Dim DF as Object  
   Set DF = DS.("RDSServer.DataFactory", "https://yourServer")  
...  
```  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="see-also"></a>Consulte también  
 [Paso 2: invocar el programa de servidor (tutorial de RDS)](./step-2-invoke-the-server-program-rds-tutorial.md)   
 [Tutorial de RDS (VBScript)](./rds-tutorial-vbscript.md)