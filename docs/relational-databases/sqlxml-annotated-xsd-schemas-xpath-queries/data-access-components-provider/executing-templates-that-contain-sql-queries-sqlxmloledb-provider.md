---
title: Ejecutar plantillas con consultas SQL (SQLXMLOLEDB)
description: Vea un ejemplo de una aplicación ADO del lado cliente con el proveedor SQLXMLOLEDB para ejecutar una plantilla XML de servidor que contenga una consulta SQL.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- templates [SQLXML]
- SQLXMLOLEDB Provider, executing template files
- templates [SQLXML], SQL queries
- XML templates [SQLXML]
- SQL queries [SQLXML]
ms.assetid: ff2bc36f-e3fb-4d8f-8e3a-2680a39eda11
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9298024d8010b8ab4684eb54b85f4b8afc636b69
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467156"
---
# <a name="executing-templates-that-contain-sql-queries-sqlxmloledb-provider"></a>Ejecutar plantillas que contienen consultas SQL (proveedor SQLXMLOLEDB)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  En este ejemplo se muestra el uso de la propiedad específica del proveedor SQLXMLOLEDB Clientsidexml,. En esta aplicación de ejemplo ADO del lado cliente, se ejecuta en el servidor una plantilla XML que consta de una consulta SQL.  
  
 Dado que la propiedad Clientsidexml, está establecida en true, la instrucción SELECT sin la cláusula FOR XML se envía al servidor. El servidor ejecuta la consulta y devuelve un conjunto de filas al cliente. A continuación, el cliente aplica la transformación FOR XML al conjunto de filas y genera un documento XML.  
  
 La plantilla XML proporciona un único elemento raíz de nivel superior ( \<ROOT> ) para el documento XML que se genera; por lo tanto, no se proporciona la propiedad raíz de XML.  
  
 Para ejecutar plantillas XML, se debe especificar el dialecto {5d531cb2-e6ed-11d2-b252-00c04f681b71}.  
  
> [!NOTE]  
>  En el código, debe suministrarse el nombre de la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en la cadena de conexión. Además, este ejemplo especifica el uso de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client (SQLNCLI11) para el proveedor de datos que requiere la instalación de software cliente de red adicional. Para obtener más información, vea [requisitos del sistema para SQL Server Native Client](../../../relational-databases/native-client/system-requirements-for-sql-server-native-client.md).  
  
```  
Option Explicit  
  
Sub Main()  
  Dim oTestStream As New ADODB.Stream  
  Dim oTestConnection As New ADODB.Connection  
  Dim oTestCommand As New ADODB.Command  
  oTestConnection.Open "Provider=SQLXMLOLEDB.4.0;Data Provider=SQLNCLI11;Data Source=SqlServerName;Initial Catalog=AdventureWorks;Integrated Security=SSPI;"  
  
  Set oTestCommand.ActiveConnection = oTestConnection  
  oTestCommand.Properties("ClientSideXML") = True  
  oTestCommand.CommandText = "<ROOT xmlns:sql='urn:schemas-microsoft-com:xml-sql'> " & _  
        " <sql:query> " & _  
        "   SELECT TOP 10 FirstName, LastName FROM Person.Contact FOR XML AUTO " & _  
        "   </sql:query> " & _  
        " </ROOT> "  
  oTestStream.Open  
  ' You need the dialect if you are executing   
  ' XML templates (not for SQL queries).  
  oTestCommand.Dialect = "{5d531cb2-e6ed-11d2-b252-00c04f681b71}"  
  oTestCommand.Properties("Output Stream").Value = oTestStream  
  oTestCommand.Execute , , adExecuteStream  
  
  oTestStream.Position = 0  
  oTestStream.Charset = "utf-8"  
  Debug.Print oTestStream.ReadText(adReadAll)  
End Sub  
  
Sub Form_Load()  
  Main  
End Sub  
```  
  
  
