---
description: Escenario de persistencia del conjunto de registros XML
title: Escenario de persistencia de conjunto de registros XML | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- XML persistence [ADO], persistence scenario
ms.assetid: 353d569a-043a-4397-9ee6-564c4af8d5f6
author: rothja
ms.author: jroth
ms.openlocfilehash: bbad3ddcfd8c34be6ea911f4cef580832f1c55b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032227"
---
# <a name="xml-recordset-persistence-scenario"></a>Escenario de persistencia del conjunto de registros XML
En este escenario, creará una aplicación de Active Server páginas (ASP) que guarda el contenido de un objeto de conjunto de registros directamente en el objeto de respuesta de ASP.  
  
> [!NOTE]
>  Este escenario requiere que el servidor tenga instalado Internet Information Server 5,0 (IIS) o posterior.  
  
 El conjunto de registros devuelto se muestra en Internet Explorer mediante un [objeto DataControl (RDS)](../../reference/rds-api/datacontrol-object-rds.md).  
  
 Los pasos siguientes son necesarios para crear este escenario:  
  
-   Configuración de la aplicación  
  
-   Obtención de los datos  
  
-   Enviar los datos  
  
-   Recibir y mostrar los datos  
  
## <a name="step-1-set-up-the-application"></a>Paso 1: configuración de la aplicación  
 Cree un directorio virtual de IIS denominado "XMLPersist" con permisos de script. Cree dos nuevos archivos de texto en la carpeta a la que apunta el directorio virtual, uno denominado "XMLResponse. asp", el otro denominado "Default.htm".  
  
## <a name="step-2-get-the-data"></a>Paso 2: obtener los datos  
 En este paso, escribirá el código para abrir un conjunto de registros ADO y prepararlo para enviarlo al cliente. Abra el archivo XMLResponse. asp con un editor de texto, como el Bloc de notas, e inserte el código siguiente.  
  
```  
<%@ language="VBScript" %>  
  
<!-- #include file='adovbs.inc' -->  
  
<%  
  Dim strSQL, strCon  
  Dim adoRec   
  Dim adoCon   
  Dim xmlDoc   
  
  ' You will need to change "MySQLServer" below to the name of the SQL   
  ' server machine to which you want to connect.  
  strCon = "Provider=sqloledb;Data Source=MySQLServer;Initial Catalog=Pubs;Integrated Security=SSPI;"  
  Set adoCon = server.createObject("ADODB.Connection")  
  adoCon.Open strCon  
  
  strSQL = "SELECT Title, Price FROM Titles ORDER BY Price"  
  Set adoRec = Server.CreateObject("ADODB.Recordset")  
  adoRec.Open strSQL, adoCon, adOpenStatic, adLockOptimistic, adCmdText  
```  
  
 Asegúrese de cambiar el valor del `Data Source` parámetro en `strCon` al nombre del equipo Microsoft SQL Server.  
  
 Mantenga el archivo abierto y continúe con el paso siguiente.  
  
## <a name="step-3-send-the-data"></a>Paso 3: enviar los datos  
 Ahora que tiene un conjunto de registros, debe enviarlo al cliente guardándolo como XML en el objeto de respuesta de ASP. Agregue el código siguiente a la parte inferior de XMLResponse. asp.  
  
```  
  Response.ContentType = "text/xml"  
  Response.Expires = 0  
  Response.Buffer = False  
  
  Response.Write "<?xml version='1.0'?>" & vbNewLine  
  adoRec.save Response, adPersistXML  
  adoRec.Close  
  Set adoRec=Nothing  
%>  
```  
  
 Tenga en cuenta que el objeto de respuesta de ASP se especifica como destino para el [método Save](../../reference/ado-api/save-method.md)del conjunto de registros. El destino del método Save puede ser cualquier objeto que admita la interfaz IStream, como un [objeto de secuencia ADO (ADO)](../../reference/ado-api/stream-object-ado.md), o un nombre de archivo que incluya la ruta de acceso completa en la que se va a guardar el conjunto de registros.  
  
 Guarde y cierre XMLResponse. asp antes de ir al paso siguiente. Copie también el archivo adovbs. Inc de la carpeta de instalación de la biblioteca de ADO predeterminada en la misma carpeta donde guardó el archivo XMLResponse. asp.  
  
## <a name="step-4-receive-and-display-the-data"></a>Paso 4: recepción y visualización de los datos  
 En este paso, creará un archivo HTML con un objeto de [objeto de control de objetos (RDS)](../../reference/rds-api/datacontrol-object-rds.md) incrustado que apunta al archivo XMLResponse. asp para obtener el conjunto de registros. Abra default.htm con un editor de texto, como el Bloc de notas, y agregue el código siguiente. Reemplace "SQLServer" en la dirección URL por el nombre del servidor.  
  
```  
<HTML>  
<HEAD><TITLE>ADO Recordset Persistence Sample</TITLE></HEAD>  
<BODY>  
  
<TABLE DATASRC="#RDC1" border="1">  
  <TR>  
<TD><SPAN DATAFLD="title"></SPAN></TD>  
<TD><SPAN DATAFLD="price"></SPAN></TD>  
  </TR>  
</TABLE>  
<OBJECT CLASSID="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33" ID="RDC1">  
   <PARAM NAME="URL" VALUE="XMLResponse.asp">  
</OBJECT>  
  
</BODY>  
</HTML>  
```  
  
 Cierre el archivo de default.htm y guárdelo en la misma carpeta donde guardó XMLResponse. asp. Con Internet Explorer 4,0 o posterior, abra la dirección URL https://*SQLServer*/XMLPersist/default.htm y observe los resultados. Los datos se muestran en una tabla DHTML enlazada. Ahora, abra la dirección URL https:// *SQLServer* /XMLPersist/XMLResponse.asp y observe los resultados. Se muestra el XML.  
  
## <a name="see-also"></a>Consulte también  
 [Save (método)](../../reference/ado-api/save-method.md)   
 [Almacenar registros en formato XML](./persisting-records-in-xml-format.md)