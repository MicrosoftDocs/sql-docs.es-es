---
description: Recuperar conjuntos de resultados en secuencias
title: Recuperar conjuntos de ResultSet en secuencias | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/20/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- streams [ADO], retrieving query results
- query results into stream [ADO]
- retrieving results into stream [ADO]
ms.assetid: 996c1321-c926-4f57-8297-85c8c20de974
author: rothja
ms.author: jroth
ms.openlocfilehash: f625a4f96aa5ec083057af4118315d9df494530e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032547"
---
# <a name="retrieving-resultsets-into-streams"></a>Recuperar conjuntos de resultados en secuencias
En lugar de recibir resultados en el objeto de **conjunto de registros** tradicional, ADO puede recuperar los resultados de la consulta en un flujo. El objeto de **secuencia** de ADO (u otros objetos que admiten la interfaz **IStream** com, como los objetos de **solicitud** y **respuesta** de ASP) se pueden usar para contener estos resultados. Un uso de esta característica consiste en recuperar los resultados en formato XML. Con SQL Server, por ejemplo, los resultados XML se pueden devolver de varias maneras, como usar la cláusula FOR XML con una consulta SELECT de SQL o mediante una consulta XPath.  
  
 Para recibir los resultados de la consulta en formato de secuencia en lugar de hacerlo en un **conjunto de registros**, debe especificar la constante **adExecuteStream** desde **ExecuteOptionEnum** como parámetro del método **Execute** de un objeto **Command** . Si el proveedor es compatible con esta característica, los resultados se devolverán en una secuencia en el momento de la ejecución. Es posible que sea necesario especificar propiedades adicionales específicas del proveedor antes de que se ejecute el código. Por ejemplo, con el proveedor de OLE DB de Microsoft para SQL Server, se deben especificar propiedades como el **flujo de salida** en la colección **Properties** del objeto **Command** . Para obtener más información acerca de las propiedades dinámicas específicas de SQL Server relacionadas con esta característica, vea XML-Related Properties en el Libros en pantalla de SQL Server.  
  
## <a name="for-xml-query-example"></a>Ejemplo de consulta FOR XML  
 El siguiente ejemplo está escrito en VBScript en la base de datos Northwind:  
  
```html
<!-- BeginRecordAndStreamVBS -->  
<%@ LANGUAGE = VBScript %>  
<%  Option Explicit      %>  
  
<HTML>  
<HEAD>  
<META NAME="GENERATOR" Content="Microsoft Developer Studio"/>  
<META HTTP-EQUIV="Content-Type" content="text/html"; charset="iso-8859-1">  
<TITLE>FOR XML Query Example</TITLE>  
  
<STYLE>  
   BODY  
   {  
      FONT-FAMILY: Tahoma;  
      FONT-SIZE: 8pt;  
      OVERFLOW: auto  
   }  
  
   H3  
   {  
      FONT-FAMILY: Tahoma;  
      FONT-SIZE: 8pt;  
      OVERFLOW: auto  
   }  
</STYLE>  
  
<!-- #include file="adovbs.inc" -->  
<%  
   Response.Write "<H3>Server-side processing</H3>"  
  
   Response.Write "Page Generated @ " & Now() & "<BR/>"  
  
   Dim adoConn  
   Set adoConn = Server.CreateObject("ADODB.Connection")  
  
   Dim sConn  
   sConn = "Provider=SQLOLEDB;Data Source=" & _  
      Request.ServerVariables("SERVER_NAME") & ";" & _  
      Initial Catalog=Northwind;Integrated Security=SSPI;"  
  
   Response.write "Connect String = " & sConn & "<BR/>"  
  
   adoConn.ConnectionString = sConn  
   adoConn.CursorLocation = adUseClient  
  
   adoConn.Open  
  
   Response.write "ADO Version = " & adoConn.Version & "<BR/>"  
   Response.write "adoConn.State = " & adoConn.State & "<BR/>"  
  
   Dim adoCmd  
   Set adoCmd = Server.CreateObject("ADODB.Command")  
   Set adoCmd.ActiveConnection = adoConn  
  
   Dim sQuery  
   sQuery = "<ROOT xmlns:sql='urn:schemas-microsoft-com:xml-sql'><sql:query>SELECT * FROM PRODUCTS WHERE ProductName='Gumbr Gummibrchen' FOR XML AUTO</sql:query></ROOT>"  
  
   Response.write "Query String = " & sQuery & "<BR/>"  
  
   Dim adoStreamQuery  
   Set adoStreamQuery = Server.CreateObject("ADODB.Stream")  
   adoStreamQuery.Open  
   adoStreamQuery.WriteText sQuery, adWriteChar  
   adoStreamQuery.Position = 0  
  
   adoCmd.CommandStream = adoStreamQuery  
   adoCmd.Dialect = "{5D531CB2-E6Ed-11D2-B252-00C04F681B71}"  
  
   Response.write "Pushing XML to client for processing "  & "<BR/>"  
  
   adoCmd.Properties("Output Stream") = Response  
   Response.write "<XML ID='MyDataIsle'>"  
   adoCmd.Execute , , 1024  
   Response.write "</XML>"  
  
%>  
  
<SCRIPT language="VBScript" For="window" Event="onload">  
   Dim xmlDoc  
   Set xmlDoc = MyDataIsle.XMLDocument  
   xmlDoc.resolveExternals=false  
   xmlDoc.async=false  
  
   If xmlDoc.parseError.Reason <> "" then  
      Msgbox "parseError.Reason = " & xmlDoc.parseError.Reason  
   End If  
  
   Dim root, child  
   Set root = xmlDoc.documentElement  
   For each child in root.childNodes  
      dim OutputXML  
      OutputXML = document.all("log").innerHTML  
      document.all("log").innerHTML = OutputXML & "<LI>" & child.getAttribute("ProductName") & "</LI>"  
   Next  
</SCRIPT>  
  
</HEAD>  
  
<BODY>  
  
   <H3>Client-side processing of XML Document MyDataIsle</H3>  
   <UL id=log>  
   </UL>  
  
</BODY>  
</HTML>  
<!-- EndRecordAndStreamVBS -->  
  
```  
  
 La cláusula FOR XML indica a SQL Server que devuelva los datos en forma de un documento XML.  
  
### <a name="for-xml-syntax"></a>Sintaxis FOR XML  
  
```syntax
FOR XML [RAW|AUTO|EXPLICIT]  
```  
  
 FOR XML RAW genera elementos de fila genéricos que tienen valores de columna como atributos. FOR XML AUTO usa la heurística para generar un árbol jerárquico con nombres de elemento basados en nombres de tabla. FOR XML EXPLICIT genera una tabla universal con relaciones totalmente descritas por los metadatos.  
  
 A continuación se muestra un ejemplo de la instrucción SELECT FOR XML de SQL:  
  
```sql
SELECT * FROM PRODUCTS ORDER BY PRODUCTNAME FOR XML AUTO  
```  
  
 El comando se puede especificar en una cadena como se mostró anteriormente, se asigna a **CommandText** o en forma de una consulta de plantilla XML asignada a **CommandStream**. Para obtener más información sobre las consultas de plantilla XML, vea [secuencias de comandos](../../../ado/guide/data/command-streams.md) en ADO o usar secuencias para la entrada de comandos en el libros en pantalla de SQL Server.  
  
 Como consulta de plantilla XML, la consulta FOR XML aparece de la siguiente manera:  
  
```xml
<sql:query> SELECT * FROM PRODUCTS ORDER BY PRODUCTNAME FOR XML AUTO </sql:query>  
```  
  
 Este ejemplo especifica el objeto de **respuesta** de ASP para la propiedad de **flujo de salida** :  
  
```vb
adoCmd.Properties("Output Stream") = Response  
```  
  
 A continuación, especifique el parámetro **adExecuteStream** de **Execute**. En este ejemplo se ajusta la secuencia en etiquetas XML para crear una isla de datos XML:  
  
```vb
Response.write "<XML ID=MyDataIsle>"  
adoCmd.Execute , , adExecuteStream  
Response.write "</XML>"  
```  
  
### <a name="remarks"></a>Observaciones  
 En este momento, XML se ha transmitido al explorador del cliente y está listo para mostrarse. Esto se hace mediante el uso de VBScript del lado cliente para enlazar el documento XML a una instancia del DOM y recorrer en bucle cada nodo secundario para generar una lista de productos en HTML.
