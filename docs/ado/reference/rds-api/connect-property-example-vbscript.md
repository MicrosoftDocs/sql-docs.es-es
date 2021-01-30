---
description: Ejemplo de la propiedad Connect (VBScript)
title: Ejemplo de la propiedad Connect (VBScript) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- Connect property [ADO], VBScript example
ms.assetid: 06297993-fe72-4446-aa76-3b8bc25444f6
author: rothja
ms.author: jroth
ms.openlocfilehash: 02ca24ae541a3de32eff253cd5602ac0072d6665
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163887"
---
# <a name="connect-property-example-vbscript"></a>Ejemplo de la propiedad Connect (VBScript)
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
 Este código muestra cómo establecer la propiedad [Connect](./connect-property-rds.md) en tiempo de diseño:  
  
```  
<OBJECT CLASSID="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33" ID="ADC1">  
.  
   <PARAM NAME="SQL" VALUE="Select * from Sales">  
   <PARAM NAME="CONNECT" VALUE="Provider=SQLOLEDB;Integrated Security=SSPI;Initial Catalog=Pubs">  
   <PARAM NAME="Server" VALUE="https://MyWebServer">  
.  
</OBJECT>  
```  
  
 En el ejemplo siguiente se muestra cómo establecer la propiedad de **conexión** en tiempo de ejecución en el código de VBScript.  
  
 Para probar este ejemplo, corte y pegue el código entre las \<Body> \</Body> etiquetas y de un documento HTML normal y asígnele el nombre **ConnectVBS. asp**. El script ASP identificará el servidor.  
  
```  
<!-- BeginConnectVBS -->  
<%@ Language=VBScript %>  
<HTML>  
<HEAD>  
<title>ADO Connect Property</title>  
  
    <%' local style sheet used for display%>  
<STYLE>  
<!--  
BODY {  
   font-family: 'Verdana','Arial','Helvetica',sans-serif;  
   BACKGROUND-COLOR:white;  
   COLOR:black;  
    }  
  
.tbody {   
   text-align: center;  
   background-color: #f7efde;  
   font-family: 'Verdana','Arial','Helvetica',sans-serif;   
   font-size: x-small;  
    }  
-->  
</STYLE>  
</HEAD>  
  
<BODY>  
<h1>ADO Connect Property (RDS)</h1>  
  
<HR>  
<H3>Set Connect Property at Run Time</H3>  
  
<% ' RDS.DataControl with no parameters set at design time %>  
<OBJECT classid="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33" ID=RDS HEIGHT=1 WIDTH=1></OBJECT>  
<% ' Bind table to control for data display  %>  
<TABLE DATASRC=#RDS>  
<TBODY>  
  <TR class="tbody">  
    <TD><SPAN DATAFLD="FirstName"></SPAN></TD>  
    <TD><SPAN DATAFLD="LastName"></SPAN></TD>  
  </TR>  
</TBODY>  
</TABLE>  
<FORM name="frmInput">  
    SERVER: <INPUT Name="txtServer" Size="103" Value="https://<%=Request.ServerVariables("SERVER_NAME")%>"><BR>  
    DATA SOURCE: <INPUT Name="txtDataSource" Size="93" Value="<%=Request.ServerVariables("SERVER_NAME")%>"><BR>  
    CONNECT: <INPUT Name="txtConnect" Size="100"><BR>  
    SQL: <INPUT Name="txtSQL" Size="110" Value="Select FirstName, LastName from Employees">  
    <BR>  
    <INPUT TYPE=BUTTON NAME="Run" VALUE="Run">  
    <h4>  
    To make data grid appear, click 'Run' to see the connect string in text box above.  
    </h4>   
</FORM>  
<Script Language="VBScript">  
  
    ' Set parameters of RDS.DataControl at Run Time  
    Sub Run_OnClick  
  
        Dim Cnxn  
            ' build connection string  
        Cnxn = "Provider='sqloledb';"  
        Cnxn = Cnxn & "Data Source="  
        Cnxn = Cnxn & document.frmInput.txtDataSource.value & ";"  
        Cnxn = Cnxn & "Initial Catalog='Northwind';"  
        Cnxn = Cnxn & "Integrated Security='SSPI';"  
            ' assign the value  
        document.frmInput.txtConnect.value = Cnxn  
        MsgBox "Here we go!"  
            ' set RDS properties  
        RDS.Server = document.frmInput.txtServer.value  
        RDS.SQL = document.frmInput.txtSQL.value  
        RDS.Connect = document.frmInput.txtConnect.value  
        RDS.Refresh  
  
    End Sub  
  
</Script>  
  
</BODY>  
</HTML>  
<!-- EndConnectVBS -->  
```  
  
## <a name="see-also"></a>Consulte también  
 [Propiedad Connect (RDS)](./connect-property-rds.md)