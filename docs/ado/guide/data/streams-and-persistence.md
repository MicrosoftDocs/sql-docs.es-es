---
description: Secuencias y persistencia
title: Flujos y persistencia | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- persisted streams [ADO]
- streams [ADO], persistence
ms.assetid: ad5bf52c-fd10-4cfa-bf7d-fcedcaa41eea
author: rothja
ms.author: jroth
ms.openlocfilehash: 1b554afa949acea4f816ce66e92e897477c2fe00
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032427"
---
# <a name="streams-and-persistence"></a>Secuencias y persistencia
El método [Save](../../../ado/reference/ado-api/save-method.md) del objeto de [conjunto de registros](../../../ado/reference/ado-api/recordset-object-ado.md) almacena o *conserva* un **conjunto de registros** en un archivo y el método [Open](../../../ado/reference/ado-api/open-method-ado-recordset.md) restaura el **conjunto de registros** a partir de ese archivo.  
  
 Con ADO 2,7 o posterior, los métodos **Save** y **Open** pueden conservar también un **conjunto de registros** en un objeto [Stream](../../../ado/reference/ado-api/stream-object-ado.md) . Esta característica es especialmente útil cuando se trabaja con el servicio de datos remotos (RDS) y páginas de Active Server (ASP).  
  
 Para obtener más información sobre cómo se puede usar la persistencia en páginas ASP, vea la documentación de ASP actual.  
  
 A continuación se muestran algunos escenarios en los que se muestra cómo se pueden usar los objetos de **secuencia** y la persistencia.  
  
## <a name="scenario-1"></a>Escenario 1  
 Este escenario simplemente guarda un **conjunto de registros** en un archivo y, a continuación, en una **secuencia**. A continuación, abre la secuencia persistente en otro **conjunto de registros**.  
  
```  
Dim rs1 As ADODB.Recordset  
Dim rs2 As ADODB.Recordset  
Dim stm As ADODB.Stream  
  
Set rs1 = New ADODB.Recordset  
Set rs2 = New ADODB.Recordset  
Set stm = New ADODB.Stream  
  
rs1.Open   "SELECT * FROM Customers", "Provider=sqloledb;" & _  
        "Data Source=MyServer;Initial Catalog=Northwind;" & _  
        "Integrated Security=SSPI;""", adopenStatic, adLockReadOnly, adCmdText  
rs1.Save "c:\myfolder\mysubfolder\myrs.xml", adPersistXML  
rs1.Save stm, adPersistXML  
rs2.Open stm  
```  
  
## <a name="scenario-2"></a>Escenario 2  
 En este escenario se conserva un **conjunto de registros** en un **flujo** en formato XML. A continuación, lee el **flujo** en una cadena que se puede examinar, manipular o mostrar.  
  
```  
Dim rs As ADODB.Recordset  
Dim stm As ADODB.Stream  
Dim strRst As String  
  
Set rs = New ADODB.Recordset  
Set stm = New ADODB.Stream  
  
' Open, save, and close the recordset.   
rs.Open "SELECT * FROM Customers", "Provider=sqloledb;" & _  
        "Data Source=MyServer;Initial Catalog=Northwind;" & _  
        "Integrated Security=SSPI;"""  
rs.Save stm, adPersistXML  
rs.Close  
Set rs = nothing  
  
' Put saved Recordset into a string variable.  
strRst = stm.ReadText(adReadAll)  
  
' Examine, manipulate, or display the XML data.  
...  
```  
  
## <a name="scenario-3"></a>Escenario 3  
 En este código de ejemplo se muestra el código ASP que conserva un **conjunto de registros** como XML directamente en el objeto de **respuesta** :  
  
```  
...  
<%  
response.ContentType = "text/xml"  
  
' Create and open a Recordset.  
Set rs = Server.CreateObject("ADODB.Recordset")  
rs.Open "select * from Customers", "Provider=sqloledb;" & _  
        "Data Source=MyServer;Initial Catalog=Northwind;" & _  
        "Integrated Security=SSPI;"""  
  
' Save Recordset directly into output stream.  
rs.Save Response, adPersistXML   
  
' Close Recordset.  
rs.Close  
Set rs = nothing  
%>  
...  
```  
  
## <a name="scenario-4"></a>Escenario 4  
 En este escenario, el código ASP escribe el contenido del **conjunto de registros** en formato ADTG en el cliente. El [servicio de cursores de Microsoft para OLE DB](../../../ado/guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md) puede usar estos datos para crear un **conjunto de registros** desconectado.  
  
 Una nueva propiedad en el [control](../../../ado/reference/rds-api/datacontrol-object-rds.md)de objetos de RDS, [URL](../../../ado/reference/rds-api/url-property-rds.md), apunta a la página. asp que genera el **conjunto de registros**. Esto significa que se puede obtener un objeto de **conjunto de registros** sin RDS mediante el objeto [DataFactory](../../../ado/reference/rds-api/datafactory-object-rdsserver.md) del servidor o el usuario que escribe un objeto comercial. Esto simplifica significativamente el modelo de programación de RDS.  
  
 Código del lado servidor, denominado https://server/directory/recordset.asp:  
  
```  
<%  
Dim rs   
Set rs = Server.CreateObject("ADODB.Recordset")  
rs.Open "select au_fname, au_lname, phone from Authors", ""& _  
        "Provider=sqloledb;Data Source=MyServer;" & _  
        "Initial Catalog=Pubs;Integrated Security=SSPI;"  
response.ContentType = "multipart/mixed"  
rs.Save response, adPersistADTG  
%>  
```  
  
 Código de cliente:  
  
```  
<HTML>  
<HEAD>  
<TITLE>RDS Query Page</TITLE>  
</HEAD>  
<body>  
<CENTER>  
<H1>Remote Data Service 2.5</H1>  
<TABLE DATASRC="#DC1">  
   <TR>   
      <TD><SPAN DATAFLD="au_fname"></SPAN></TD>  
      <TD><SPAN DATAFLD="au_lname"></SPAN></TD>  
      <TD><SPAN DATAFLD="phone"></SPAN></TD>  
   </TR>  
</TABLE>  
<BR>  
  
<OBJECT classid="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33"  
    ID=DC1 HEIGHT=1 WIDTH = 1>  
    <PARAM NAME="URL" VALUE="https://server/directory/recordset.asp">  
</OBJECT>  
  
</SCRIPT>  
</BODY>  
</HTML>  
```  
  
 Los desarrolladores también tienen la opción de usar un objeto de **conjunto de registros** en el cliente:  
  
```  
...  
function GetRs()   
    {  
    rs = CreateObject("ADODB.Recordset");  
    rs.Open "https://server/directory/recordset.asp"  
    DC1.SourceRecordset = rs;  
    }  
...  
```  
  
## <a name="see-also"></a>Consulte también  
 [Open (método) (conjunto de registros ADO)](../../../ado/reference/ado-api/open-method-ado-recordset.md)   
 [Objeto Record (ADO)](../../../ado/reference/ado-api/record-object-ado.md)   
 [Save (método)](../../../ado/reference/ado-api/save-method.md)
