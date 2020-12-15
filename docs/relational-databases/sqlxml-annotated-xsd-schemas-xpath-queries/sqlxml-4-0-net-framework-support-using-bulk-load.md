---
title: Usar la carga masiva SQLXML en el entorno .NET
description: Aprenda a usar el objeto COM de carga masiva SQLXML 4,0 en el entorno de .NET para cargar datos XML de forma masiva en una base de datos.
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- SQLXML, XML Bulk Load
- XML Bulk Load [SQLXML], .NET environment
- .NET Framework [SQLXML], XML Bulk Load
- bulk load [SQLXML], .NET environment
ms.assetid: b85df83b-ba56-43bf-bcdf-b2a6fca43276
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5bc731438d8691664b4db502ab05e3489e5faaa1
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461726"
---
# <a name="sqlxml-40-net-framework-support---using-bulk-load"></a>Compatibilidad de SQLXML 4.0 con .NET Framework: utilizar la carga masiva
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  En este tema se explica cómo se puede usar la funcionalidad de carga masiva XML en el entorno .NET. Para obtener información detallada acerca de la carga masiva XML, vea [realizar la carga masiva de datos xml &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/performing-bulk-load-of-xml-data-sqlxml-4-0.md).  
  
 Para utilizar el objeto COM de carga masiva SQLXML desde un entorno administrado, tiene que agregar una referencia de proyecto a este objeto. Esto genera una interfaz de contenedor administrado para el objeto COM de carga masiva.  
  
> [!NOTE]  
>  La carga masiva XML administrada no funciona con flujos administrados y requiere un contenedor para los flujo nativos. El componente Carga masiva SQLXML no se ejecutará en un entorno multiproceso (atributo ' [MTAThread]'). Si intenta ejecutar el componente de carga masiva en un entorno de varios subprocesos, obtendrá una excepción InvalidCastException con la siguiente información adicional: "error de QueryInterface para la interfaz SQLXMLBULKLOADLib. ISQLXMLBulkLoad". La solución consiste en hacer que el objeto que contiene el objeto de carga masiva sea accesible desde un solo subproceso (por ejemplo, mediante el atributo **[STAThread]** como se muestra en el ejemplo).  
  
 En este tema se proporciona una aplicación de ejemplo funcional en C# para realizar la carga masiva de los datos XML de la base de datos. Para crear un ejemplo funcional, siga estos pasos:  
  
1.  Cree las tablas siguientes:  
  
    ```  
    CREATE TABLE Ord (  
             OrderID     int identity(1,1)  PRIMARY KEY,  
             CustomerID  varchar(5))  
    GO  
    CREATE TABLE Product (  
             ProductID   int identity(1,1) PRIMARY KEY,  
             ProductName varchar(20))  
    GO  
    CREATE TABLE OrderDetail (  
           OrderID     int FOREIGN KEY REFERENCES Ord(OrderID),  
           ProductID   int FOREIGN KEY REFERENCES Product(ProductID),  
                       CONSTRAINT OD_key PRIMARY KEY (OrderID, ProductID))  
    GO  
    ```  
  
2.  Guarde el esquema siguiente en un archivo (schema.xml):  
  
    ```  
    <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
                xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
    <xsd:annotation>  
      <xsd:appinfo>  
        <sql:relationship name="OrderOD"  
              parent="Ord"  
              parent-key="OrderID"  
              child="OrderDetail"  
              child-key="OrderID" />  
  
        <sql:relationship name="ODProduct"  
              parent="OrderDetail"  
              parent-key="ProductID"  
              child="Product"  
              child-key="ProductID"   
              inverse="true"/>  
      </xsd:appinfo>  
    </xsd:annotation>  
  
      <xsd:element name="Order" sql:relation="Ord"   
                                sql:key-fields="OrderID" >  
       <xsd:complexType>  
         <xsd:sequence>  
            <xsd:element name="Product" sql:relation="Product"   
                         sql:key-fields="ProductID"  
                         sql:relationship="OrderOD ODProduct">  
              <xsd:complexType>  
                 <xsd:attribute name="ProductID" type="xsd:int" />  
                 <xsd:attribute name="ProductName" type="xsd:string" />  
              </xsd:complexType>  
            </xsd:element>  
         </xsd:sequence>  
            <xsd:attribute name="OrderID"   type="xsd:integer" />   
            <xsd:attribute name="CustomerID"   type="xsd:string" />  
        </xsd:complexType>  
      </xsd:element>  
    </xsd:schema>  
    ```  
  
3.  Guarde el documento XML de ejemplo siguiente en un archivo (data.xml):  
  
    ```  
    <ROOT>    
      <Order OrderID="11" CustomerID="ALFKI">  
        <Product ProductID="11" ProductName="Chai" />  
        <Product ProductID="22" ProductName="Chang" />  
      </Order>  
      <Order OrderID="22" CustomerID="ANATR">  
         <Product ProductID="33" ProductName="Aniseed Syrup" />  
        <Product ProductID="44" ProductName="Gumbo Mix" />  
      </Order>  
    </ROOT>  
    ```  
  
4.  Inicie Visual Studio.  
  
5.  Cree una aplicación de consola de C#.  
  
6.  En el menú **proyecto** , seleccione **Agregar referencia**.  
  
7.  En la pestaña **com** , seleccione **biblioteca de tipos de carga masiva de Microsoft SQLXML 4,0** (xblkld4.dll) y haga clic en **Aceptar**. Verá el ensamblado **Interop. SQLXMLBULKLOADLib** creado en el proyecto.  
  
8.  Reemplace el método Main() por el código siguiente. Actualice la propiedad **ConnectionString** y la ruta de acceso del archivo para el esquema y los archivos de datos.  
  
    ```  
    [STAThread]  
       static void Main(string[] args)  
       {     
             try  
             {  
                SQLXMLBULKLOADLib.SQLXMLBulkLoad4Class objBL = new SQLXMLBULKLOADLib.SQLXMLBulkLoad4Class();  
                objBL.ConnectionString = "Provider=sqloledb;server=server;database=databaseName;integrated security=SSPI";  
                objBL.ErrorLogFile = "error.xml";  
                objBL.KeepIdentity = false;  
                objBL.Execute ("schema.xml","data.xml");  
             }  
             catch(Exception e)  
             {  
             Console.WriteLine(e.ToString());  
             }  
       }  
    ```  
  
9. Para cargar el XML en la tabla creada, genere y ejecute el proyecto.  
  
    > [!NOTE]  
    >  La referencia al componente Carga masiva (xblkld4.dll) también se puede agregar utilizando la herramienta tlbimp.exe, que está disponible como parte de .NET Framework. Esta herramienta crea un contenedor administrado para la DLL nativa (xblkld4.dll), que se puede utilizar después en cualquier proyecto de .NET. Por ejemplo:  
  
    ```  
    c:\>tlbimp xblkld4.dll  
    ```  
  
     Esto crea la DLL del contenedor administrado (SQLXMLBULKLOADLib.dll) que puede utilizar en el proyecto de .NET Framework. En .NET Framework, puede agregar la referencia del proyecto a la DLL recién creada.  
  
## <a name="see-also"></a>Consulte también  
 [Carga masiva de datos XML &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/performing-bulk-load-of-xml-data-sqlxml-4-0.md)  
  
  
