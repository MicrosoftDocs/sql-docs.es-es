---
description: Programación ADO en VBScript
title: Programación ADO en VBScript | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- VB
helpviewer_keywords:
- ADO, VBScript
- VBScript [ADO]
ms.assetid: 6aaaf6d0-1376-4473-bea6-b81f2645a9ac
author: rothja
ms.author: jroth
ms.openlocfilehash: 31e6c98d12c6c6f472053634d8b3aea314ccbab2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037875"
---
# <a name="vbscript-ado-programming"></a>Programación ADO en VBScript
## <a name="creating-an-ado-project"></a>Crear un proyecto de ADO  
 Microsoft Visual Basic, Scripting Edition no es compatible con las bibliotecas de tipos, por lo que no es necesario hacer referencia a ADO en el proyecto. Por consiguiente, no se admiten características asociadas como la finalización de la línea de comandos. Además, de forma predeterminada, las constantes enumeradas de ADO no se definen en VBScript.  
  
 Sin embargo, ADO proporciona dos archivos de inclusión que contienen las siguientes definiciones que se van a usar con VBScript:  
  
-   Para el scripting del lado servidor, use adovbs. Inc, que se instala en la carpeta C:\Archivos de Programa\archivos Files\System\ado\ de forma predeterminada.  
  
-   Para el scripting del lado cliente, use Adcvbs. Inc, que se instala en la carpeta C:\Archivos de Programa\archivos Files\System\msdac\ de forma predeterminada.  
  
 Puede copiar y pegar definiciones de constantes de estos archivos en las páginas ASP, o bien, si está realizando scripting de servidor, copie el archivo adovbs. Inc en una carpeta de su sitio web y haga referencia a él desde la página ASP de la siguiente manera:  
  
```vb
<!--#include File="adovbs.inc"-->  
```  
  
## <a name="creating-ado-objects-in-vbscript"></a>Crear objetos ADO en VBScript  
 No se puede usar la instrucción **Dim** para asignar objetos a un tipo específico en VBScript. Además, VBScript no admite la **nueva** sintaxis que se usa con la instrucción **Dim** en Visual Basic para aplicaciones. En su lugar, debe usar la llamada a la función **CreateObject** :  
  
```vb
Dim Rs1  
Set Rs1 = Server.CreateObject( "ADODB.Recordset" )  
```  
  
## <a name="vbscript-examples"></a>Ejemplos de VBScript  
 El código siguiente es un ejemplo genérico de programación del lado servidor de VBScript en un archivo de Active Server página (ASP):  
  
```vb
<%  @LANGUAGE="VBSCRIPT" %>  
<%  Option Explicit %>  
<!--#include File="adovbs.inc"-->  
<HTML>  
    <BODY BGCOLOR="White" topmargin="10" leftmargin="10">  
  
    <!-- Your ASP Code goes here -->  
<%  
Dim Source  
Dim Connect  
Dim Rs1  
  
Source = "SELECT * FROM Authors"  
Connect = "Provider=sqloledb;Data Source=srv;" & _  
    "Initial Catalog=Pubs;Integrated Security=SSPI;"  
  
Set Rs1 = Server.CreateObject( "ADODB.Recordset" )  
Rs1.Open Source, Connect, adOpenForwardOnly  
Response.Write("Success!")  
%>  
    </BODY>  
</HTML>  
```  
  
 En la documentación de ADO se incluyen ejemplos de VBScript más específicos. Para obtener más información, vea [ejemplos de código ADO en Microsoft Visual Basic Scripting Edition](../../reference/ado-api/ado-code-examples-vbscript.md).  
  
## <a name="differences-between-vbscript-and-visual-basic"></a>Diferencias entre VBScript y Visual Basic  
 El uso de ADO con VBScript es similar al uso de ADO con Visual Basic de muchas maneras, incluida la forma en que se usa la sintaxis. Sin embargo, existen algunas diferencias importantes:  
  
-   VBScript solo admite el tipo de datos Variant, que puede contener diferentes tipos de datos. Puede almacenar los datos que necesita en un tipo de datos Variant y los datos funcionarán correctamente debido a la conversión realizada por VBScript. Reconoce el tipo requerido por ADO y convierte el valor de la variante en consecuencia.  
  
-   No se puede usar **on error \<label> goto** en VBScript.  
  
-   VBScript admite algunas de las funciones de Visual Basic integradas como **MsgBox**, **Date** y **IsNumeric**. Sin embargo, dado que VBScript es un subconjunto de Visual Basic, no se admiten todas las funciones integradas. Por ejemplo, VBScript no admite la función de **formato** y las funciones de e/s de archivo.