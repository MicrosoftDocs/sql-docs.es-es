---
title: Ejecutar consultas SQL (SQLXML)
description: Obtenga información sobre cómo ejecutar consultas SQL mediante clases administradas de SQLXML.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- queries [SQLXML], SQLXML Managed Classes
- SQLXML Managed Classes, executing SQL queries
- Managed Classes [SQLXML], executing SQL queries
- ExecuteToStream method
- SQL queries [SQLXML]
ms.assetid: a561ae83-a8b6-4b9b-a819-9b86839546b4
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9a20d41c1002f9e17ef10dbb86feaef100ce293f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97414366"
---
# <a name="executing-sql-queries-sqlxml-managed-classes"></a>Ejecutar consultas SQL (clases administradas de SQLXML)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  En este ejemplo se muestra:  
  
-   Crear parámetros (objetos SqlXmlParameter).  
  
-   Asignar valores a las propiedades (nombre y valor) de los objetos SqlXmlParameter.  
  
 En este ejemplo se ejecuta una consulta SQL simple para recuperar el nombre, apellido y fecha de nacimiento del empleado cuyo valor de apellido se pasa como parámetro. Al especificar el parámetro (*LastName*), solo se establece la propiedad Value. No se ha establecido la propiedad Name porque, en esta consulta, el parámetro es posicional y no se requiere ningún nombre.  
  
 De forma predeterminada, la propiedad CommandType del objeto SqlXmlCommand es **SQL**. Por tanto, la propiedad no se establece de forma explícita.  
  
> [!NOTE]  
>  En el código, debe proporcionar el nombre de la instancia de Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en la cadena de conexión.  
  
 Éste es el código de C#:  
  
```  
using System;  
  
using Microsoft.Data.SqlXml;  
using System.IO;  
class Test  
{  
      static string ConnString = "Provider=SQLOLEDB;Server=(local);database=AdventureWorks;Integrated Security=SSPI";  
      public static int testParams()  
      {  
         Stream strm;  
         SqlXmlParameter p;  
         SqlXmlCommand cmd = new SqlXmlCommand(ConnString);        
         cmd.CommandText = "SELECT FirstName, LastName FROM Person.Contact WHERE LastName=? For XML Auto";  
         p = cmd.CreateParameter();  
         p.Value = "Achong";  
         string strResult;  
         try   
         {  
            strm = cmd.ExecuteStream();  
            strm.Position = 0;  
            using(StreamReader sr = new StreamReader(strm))  
            {  
               Console.WriteLine(sr.ReadToEnd());  
            }  
         }  
         catch (SqlXmlException e)  
         {  
            //in case of an error, this prints error returned.  
            e.ErrorStream.Position=0;  
            strResult=new StreamReader(e.ErrorStream).ReadToEnd();  
            System.Console.WriteLine(strResult);  
         }  
  
         return 0;  
   }  
public static int Main(String[] args)  
{  
    testParams();  
    return 0;  
}  
}  
```  
  
### <a name="to-test-the-application"></a>Para probar la aplicación  
  
1.  Guarde en una carpeta el código de C# (DocSample.cs) que se proporciona en este tema.  
  
2.  Compile el código. Para compilar el código en el símbolo del sistema, use:  
  
    ```  
    csc /reference:Microsoft.Data.SqlXML.dll DocSample.cs  
    ```  
  
     Esto crea una aplicación ejecutable (DocSample.exe).  
  
3.  En el símbolo del sistema, ejecute DocSample.exe.  

 Para probar este ejemplo, debe tener instalado [!INCLUDE[msCoName](../../../includes/msconame-md.md)] .NET Framework en el equipo.  
  
 En lugar de especificar consultas SQL como texto de comando, puede especificar una plantilla (como se muestra en el siguiente fragmento de código) que ejecute un diagrama de actualización (que también es una plantilla) para insertar un registro del cliente. Puede especificar plantillas y diagramas de actualización en archivos y ejecutar los archivos. Para obtener más información, vea [ejecutar archivos de plantilla mediante la propiedad CommandText](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-template-files-by-using-the-commandtext-property.md).  
  
```  
SqlXmlCommand cmd = new SqlXmlCommand("Provider=SQLOLEDB;Data Source=SqlServerName;Initial Catalog=Database; Integrated Security=SSPI;");  
Stream stm;  
cmd.CommandType = SqlXmlCommandType.UpdateGram;  
cmd.CommandText = "<ROOT xmlns:sql='urn:schemas-microsoft-com:xml-sql' xmlns:updg='urn:schemas-microsoft-com:xml-updategram'>" +  
      "<updg:sync>" +  
       "<updg:before/>" +  
       "<updg:after>" +  
         "<Customer CustomerID='aaaaa' CustomerName='Some Name' CustomerTitle='SomeTitle' />" +  
       "</updg:after>" +  
       "</updg:sync>" +  
       "</ROOT>";  
  
stm = cmd.ExecuteStream();  
stm = null;  
cmd = null;  
```  
  
## <a name="using-executetostream"></a>Usar ExecuteToStream  
 Si tiene una secuencia existente, puede usar el método ExecuteToStream en lugar de crear un objeto de flujo y usar el método Execute. El código del ejemplo anterior se ha revisado aquí para usar el método ExecuteToStream:  
  
```  
using System;  
using Microsoft.Data.SqlXml;  
using System.IO;  
class Test  
{  
   static string ConnString = "Provider=SQLOLEDB;Server=SqlServerName;database=AdventureWorks;Integrated Security=SSPI;";  
   public static int testParams()  
   {  
      SqlXmlParameter p;  
      MemoryStream ms = new MemoryStream();  
      StreamReader sr = new StreamReader(ms);  
      ms.Position = 0;  
      SqlXmlCommand cmd = new SqlXmlCommand(ConnString);  
      cmd.CommandText = "select FirstName, LastName from Person.Contact where LastName = ? For XML Auto";  
      p = cmd.CreateParameter();  
      p.Value = "Achong";  
      cmd.ExecuteToStream(ms);  
      ms.Position = 0;  
      Console.WriteLine(sr.ReadToEnd());  
      return 0;        
   }  
   public static int Main(String[] args)  
   {  
      testParams();     
      return 0;  
   }  
}  
```  
  
> [!NOTE]  
>  También puede utilizar el ExecuteXMLReadermethod que devuelve un objeto XmlReader. Para obtener más información, vea [ejecutar consultas SQL mediante el método ExecuteXMLReader](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-sql-queries-by-using-the-executexmlreader-method.md).  
  
  
