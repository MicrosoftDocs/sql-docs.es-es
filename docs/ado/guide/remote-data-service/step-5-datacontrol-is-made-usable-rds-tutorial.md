---
description: 'Paso 5: Habilitación del uso del control de datos (Tutorial de RDS)'
title: 'Paso 5: DataControl se puede usar (tutorial de RDS) | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS tutorial [ADO], datacontrol made usable
ms.assetid: ed5c4a24-9804-4c85-817e-317652acb9b4
author: rothja
ms.author: jroth
ms.openlocfilehash: 6453043514657b03e72eeea032451f647f6c2eec
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036095"
---
# <a name="step-5-datacontrol-is-made-usable-rds-tutorial"></a>Paso 5: Habilitación del uso del control de datos (Tutorial de RDS)
El objeto de **conjunto de registros** devuelto está disponible para su uso. Puede examinarlo, desplazarse o editarlo como lo haría con cualquier otro **conjunto de registros**. Lo que puede hacer con el **conjunto de registros** depende de su entorno. Visual Basic y Visual C++ tienen controles visuales que pueden usar un **conjunto de registros** directa o indirectamente con la ayuda de un control de datos habilitado.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
 Por ejemplo, si va a mostrar una página web en Microsoft Internet Explorer, es posible que desee mostrar los datos del objeto de **conjunto de registros** en un control visual. Los controles visuales de una página web no pueden tener acceso directamente a un objeto de **conjunto de registros** . Sin embargo, pueden tener acceso al objeto de **conjunto de registros** a través de [RDS. DataControl](../../reference/rds-api/datacontrol-object-rds.md). **Objeto RDS. DataControl** se puede usar con un control visual cuando su propiedad [SourceRecordset](../../reference/rds-api/recordset-sourcerecordset-properties-rds.md) está establecida en el objeto de **conjunto de registros** .  
  
 El objeto de control visual debe tener su parámetro **DATASRC** establecido en **RDS. DataControl** y su propiedad **DATAFLD** establecida en un campo de objeto de **conjunto de registros** (columna).  
  
 En este tutorial, establezca la propiedad **SourceRecordset** :  
  
```vb
Sub RDSTutorial5()  
   Dim DS as New RDS.DataSpace  
   Dim RS as ADODB.Recordset  
   Dim DC as New RDS.DataControl  
   Dim DF as Object  
   Set DF = DS.CreateObject("RDSServer.DataFactory", "https://yourServer")  
   Set RS = DF.Query ("DSN=Pubs", "SELECT * FROM Authors")  
   DC.SourceRecordset = RS         ' Visual controls can now bind to DC.  
...  
```  
  
## <a name="see-also"></a>Consulte también  
 [Paso 6: los cambios se envían al servidor (tutorial de RDS)](./step-6-changes-are-sent-to-the-server-rds-tutorial.md)   
 [Tutorial de RDS (VBScript)](./rds-tutorial-vbscript.md)