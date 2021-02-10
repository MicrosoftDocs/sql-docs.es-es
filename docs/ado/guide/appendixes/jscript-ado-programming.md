---
description: Programación ADO con JScript
title: Programación ADO en JScript | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- JScript
helpviewer_keywords:
- JScript programming in ADO
- ADO, JScript programming
ms.assetid: 62273658-0fe7-4aac-b4d8-f725e6baf043
author: rothja
ms.author: jroth
ms.openlocfilehash: 15a58e69810e3df5221d4c869e14b20112a181c9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029419"
---
# <a name="jscript-ado-programming"></a>Programación ADO con JScript
## <a name="creating-an-ado-project"></a>Crear un proyecto de ADO  
 Microsoft JScript no es compatible con las bibliotecas de tipos, por lo que no es necesario hacer referencia a ADO en el proyecto. Por consiguiente, no se admiten características asociadas como la finalización de la línea de comandos. Además, de forma predeterminada, las constantes enumeradas de ADO no se definen en JScript.  
  
 Sin embargo, ADO proporciona dos archivos de inclusión que contienen las siguientes definiciones que se van a usar con JScript:  
  
-   Para el scripting del lado servidor, use Adojavas. Inc, que se instala en las carpetas de la biblioteca de ADO.  
  
-   En el caso del scripting de cliente, use Adcjavas. Inc, que se instala en las carpetas de la biblioteca de ADO.  
  
 Puede copiar y pegar definiciones de constantes de estos archivos en las páginas ASP, o bien, si va a realizar scripts en el servidor, copie el archivo Adojavas. Inc en una carpeta de su sitio web y haga referencia a él desde la página ASP del modo siguiente:  
  
```javascript
<!--#include File="adojavas.inc"-->  
```  
  
## <a name="creating-ado-objects-in-jscript"></a>Crear objetos ADO en JScript  
 En su lugar, debe usar la llamada a la función **CreateObject** :  
  
```javascript
var Rs1;  
Rs1 = Server.CreateObject("ADODB.Recordset");  
```  
  
## <a name="jscript-example"></a>Ejemplo de JScript  
 El código siguiente es un ejemplo genérico de programación del lado servidor de JScript en un archivo de Active Server página (ASP) que abre un objeto de **conjunto de registros** :  
  
```javascript
<%  @LANGUAGE="JScript" %>  
<!--#include File="adojavas.inc"-->  
<HTML>  
<BODY BGCOLOR="White" topmargin="10" leftmargin="10">  
<%  
var Source = "SELECT * FROM Authors";  
var Connect =  "Provider=sqloledb;Data Source=srv;" +  
    "Initial Catalog=Pubs;Integrated Security=SSPI;"  
var Rs1 = Server.CreateObject( "ADODB.Recordset.2.5" );  
Rs1.Open(Source,Connect,adOpenForwardOnly);  
Response.Write("Success!");  
%>  
</BODY>  
</HTML>  
```
