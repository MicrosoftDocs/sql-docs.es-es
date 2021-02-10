---
description: Tutorial de RDS (VBScript)
title: Tutorial de RDS (VBScript) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- VB
helpviewer_keywords:
- RDS tutorial [ADO], VBScript
ms.assetid: e2a48c4d-88b1-43ff-a202-9cdec54997d2
author: rothja
ms.author: jroth
ms.openlocfilehash: 693f5858a80d5bc986dc16f993aa5c5a78c54d7c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031877"
---
# <a name="rds-tutorial-vbscript"></a>Tutorial de RDS (VBScript)
Este es el tutorial de RDS, escrito en Microsoft Visual Basic Scripting Edition. Para obtener una descripción del propósito de este tutorial, vea el [tutorial de RDS](./rds-tutorial.md).  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
 En este tutorial, [RDS. DataControl](../../reference/rds-api/datacontrol-object-rds.md) y [RDS. El espacio](../../reference/rds-api/dataspace-object-rds.md) de bits se crea en tiempo de diseño; es decir, se definen con etiquetas de objeto, como se indica a continuación: `<OBJECT>...</OBJECT>` . Como alternativa, se podrían crear en tiempo de ejecución con el método [CreateObject (RDS)](../../reference/rds-api/createobject-method-rds.md) . Por ejemplo, **RDS.** El objeto DataControl podría crearse de la siguiente manera:  
  
```vb
Set DC = Server.CreateObject("RDS.DataControl")  
   <!-- RDS.DataControl -->  
   <OBJECT   
      ID="DC1" CLASSID="CLSID:BD96C556-65A3-11D0-983A-00C04FC29E33">  
   </OBJECT>  
  
   <!-- RDS.DataSpace -->  
   <OBJECT   
      ID="DS1" WIDTH=1 HEIGHT=1  
      CLASSID="CLSID:BD96C556-65A3-11D0-983A-00C04FC29E36">  
   </OBJECT>  
  
   <SCRIPT LANGUAGE="VBScript">  
  
   Sub RDSTutorial()  
   Dim DF1   
```  
  
## <a name="step-1---specify-a-server-program"></a>Paso 1: especificar un programa de servidor  
 VBScript puede detectar el nombre del servidor Web de IIS en el que se está ejecutando mediante el acceso al método **request. ServerVariables** de VBScript disponible para Active Server páginas:  
  
```vb
"https://<%=Request.ServerVariables("SERVER_NAME")%>"  
```  
  
 Sin embargo, para este tutorial, use el servidor imaginario, "yourServer".  
  
> [!NOTE]
>  Preste atención al tipo de datos de los argumentos **ByRef** . VBScript no permite especificar el tipo de variable, por lo que siempre debe pasar una **variante**. Cuando se usa HTTP, RDS permitirá pasar una variante a un método que espera un valor no variante si se invoca con el **objeto RDS.** El método [CreateObject](../../reference/rds-api/createobject-method-rds.md) del objeto DataSpace. Al utilizar DCOM o un servidor en proceso, debe coincidir con los tipos de parámetro en los lados cliente y servidor, o bien se producirá un error de coincidencia de tipos.  
  
```vb
Set DF1 = DS1.CreateObject("RDSServer.DataFactory", "https://yourServer")  
```  
  
## <a name="step-2a---invoke-the-server-program-with-rdsdatacontrol"></a>Paso 2A: invocación del programa de servidor con RDS. DataControl  
 Este ejemplo es simplemente un comentario que muestra el comportamiento predeterminado de **RDS. DataControl** es para realizar la consulta especificada.  
  
```vb
<OBJECT CLASSID="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33" ID="DC1">  
   <PARAM NAME="SQL" VALUE="SELECT * FROM Authors">  
   <PARAM NAME="Connect" VALUE="DSN=Pubs;">  
   <PARAM NAME="Server" VALUE="https://yourServer/">  
</OBJECT>  
...  
<SCRIPT LANGUAGE="VBScript">  
  
Sub RDSTutorial2A()  
   Dim RS  
   DC1.Refresh  
   Set RS = DC1.Recordset  
...  
```  
  
## <a name="step-2b---invoke-the-server-program-with-rdsserverdatafactory"></a>Paso 2B: invocación del programa de servidor con RDSServer. DataFactory  
  
## <a name="step-3---server-obtains-a-recordset"></a>Paso 3: el servidor obtiene un conjunto de registros  
  
## <a name="step-4---server-returns-the-recordset"></a>Paso 4: el servidor devuelve el conjunto de registros  
  
```vb
Set RS = DF1.Query("DSN=Pubs;", "SELECT * FROM Authors")  
```  
  
## <a name="step-5---datacontrol-is-made-usable-by-visual-controls"></a>Paso 5: el control de objetos se puede usar con controles visuales  
  
```vb
' Assign the returned recordset to the DataControl.  
  
DC1.SourceRecordset = RS  
```  
  
## <a name="step-6a---changes-are-sent-to-the-server-with-rdsdatacontrol"></a>Paso 6A: los cambios se envían al servidor de con RDS. DataControl  
 Este ejemplo es simplemente un comentario que muestra cómo **RDS. DataControl** realiza actualizaciones.  
  
```vb
<OBJECT CLASSID="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33" ID="DC1">  
   <PARAM NAME="SQL" VALUE="SELECT * FROM Authors">  
   <PARAM NAME="Connect" VALUE="DSN=Pubs;">  
   <PARAM NAME="Server" VALUE="https://yourServer/">  
</OBJECT>  
...  
<SCRIPT LANGUAGE="VBScript">  
  
Sub RDSTutorial6A()  
Dim RS  
DC1.Refresh  
...  
Set RS = DC1.Recordset  
' Edit the Recordset object...  
' The SERVER and CONNECT properties are already set from Step 2A.  
Set DC1.SourceRecordset = RS  
...  
DC1.SubmitChanges  
```  
  
## <a name="step-6b---changes-are-sent-to-the-server-with-rdsserverdatafactory"></a>Paso 6B: los cambios se envían al servidor con RDSServer. DataFactory  
  
```vb
DF.SubmitChanges "DSN=Pubs", RS  
  
End Sub  
</SCRIPT>  
</BODY>  
</HTML>  
```  
  
 **Éste es el final del tutorial.**  
  
## <a name="see-also"></a>Consulte también  
 [Tutorial de RDS](./rds-tutorial.md)