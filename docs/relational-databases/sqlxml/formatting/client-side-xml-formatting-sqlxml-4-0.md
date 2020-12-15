---
title: Formato XML del lado cliente (SQLXML)
description: Obtenga información sobre el formato XML del lado cliente en SQLXML 4,0 mediante la cláusula FOR XML.
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- FOR XML clause, formatting
- middle tier XML formatting [SQLXML]
- client-side XML formatting
- client-side-xml attribute
ms.assetid: 9630a21d-a93b-4d3b-8a25-c4b32399f993
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9d13f5252fade9a05db1b9d31cc62f1ed9292aa4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97430173"
---
# <a name="client-side-xml-formatting-sqlxml-40"></a>Aplicación de formato XML en el cliente (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  En este tema se proporciona información acerca de la aplicación de formato XML del lado cliente. La aplicación de formato en el cliente se refiere a dar formato al XML en nivel intermedio.  
  
> [!NOTE]  
>  En este tema se proporciona información adicional acerca de la forma de usar la cláusula FOR XML en el cliente y se da por sentado que está familiarizado con la cláusula FOR XML. Para obtener más información acerca de FOR XML, vea [generar XML mediante for XML](../../../relational-databases/xml/for-xml-sql-server.md).  
  
 **Importante** Para usar la funcionalidad FOR XML del lado cliente con el nuevo tipo de datos **XML** , los clientes deben usar siempre el [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] proveedor de datos de Native Client (SQLNCLI11) en lugar del proveedor SQLOLEDB. SQLNCLI11 es la versión más reciente del proveedor de SQL Server y entiende a la perfección los tipos de datos incluidos en [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]. El comportamiento de FOR XML del lado cliente con el proveedor SQLOLEDB tratará los tipos de datos **XML** como cadenas.  
  
## <a name="formatting-xml-documents-on-the-client-side"></a>Aplicar formato a documentos XML en el cliente  
 Cuando una aplicación cliente ejecuta la siguiente consulta:  
  
```  
SELECT FirstName, LastName  
FROM   Person.Contact  
FOR XML RAW  
```  
  
 ...solo esta parte de la consulta se envía al servidor:  
  
```  
SELECT FirstName, LastName  
FROM   Person.Contact  
```  
  
 El servidor ejecuta la consulta y devuelve un conjunto de filas (que contiene FirstName y LastNamecolumns) al cliente. A continuación, el nivel intermedio aplica la transformación FOR XML al conjunto de filas y devuelve el formato XML al cliente.  
  
 Del mismo modo, al ejecutar una consulta XPath, el servidor devuelve el conjunto de filas al cliente y la transformación FOR XML EXPLICIT se aplica al conjunto de filas en el cliente, de forma que se genera el formato XML deseado.  
  
 En la tabla siguiente se muestran los modos que pueden especificarse con FOR XML del lado cliente.  
  
|Modo FOR XML del lado cliente|Comentario|  
|-------------------------------|-------------|  
|RAW|Genera resultados idénticos cuando se especifica en FOR XML del lado cliente o del lado servidor.|  
|NESTED|Es similar al modo FOR XML AUTO en el servidor.|  
|EXPLICIT|Es similar al modo FOR XML EXPLICIT en el servidor.|  
  
> [!NOTE]  
>  Si especifica el modo AUTO y solicita la aplicación de formato XML del lado cliente, se envía la consulta completa al servidor, es decir, el formato XML se aplica en el servidor. Esto se hace así por comodidad, pero observe que el modo NESTED devuelve los nombres de tabla base como nombres de elemento en el documento XML que se genera. Algunas de las aplicaciones que escriba podrían requerir nombres de tabla base. Por ejemplo, podría ejecutar un procedimiento almacenado y cargar los datos resultantes en un conjunto de datos (en [!INCLUDE[msCoName](../../../includes/msconame-md.md)] .NET Framework) y, más tarde, generar un DiffGram para actualizar los datos de las tablas. En ese caso, necesitaría la información de tabla base y tendría que usar el modo NESTED.  
  
## <a name="benefits-of-client-side-xml-formatting"></a>Ventajas de la aplicación de formato XML en el cliente  
 A continuación, se muestran algunas de las ventajas de la aplicación de formato XML en el cliente.  
  
### <a name="if-you-have-stored-procedures-on-the-server-that-return-a-single-rowset-you-can-request-client-side-for-xml-transformation-to-generate-an-xml"></a>Si tiene procedimientos almacenados en el servidor que devuelven un único conjunto de filas, puede solicitar la transformación FOR XML del lado cliente para generar XML.  
 Por ejemplo, fíjese en el siguiente procedimiento almacenado. Este procedimiento devuelve el nombre y los apellidos de los empleados de la tabla Person.Contact de la base de datos AdventureWorks:  
  
```  
IF EXISTS (SELECT name FROM sysobjects  
   WHERE name = 'GetContacts' AND type = 'P')  
   DROP PROCEDURE GetContacts  
GO  
CREATE PROCEDURE GetContacts  
AS  
    SELECT   FirstName, LastName  
    FROM     Person.Contact  
```  
  
 La siguiente plantilla XML de ejemplo ejecuta el procedimiento almacenado. La cláusula FOR XML se especifica después del nombre del procedimiento almacenado.  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <sql:query client-side-xml="1">  
    EXEC GetContacts FOR XML NESTED  
  </sql:query>  
</ROOT>  
```  
  
 Dado que el atributo **Client-Side-XML** se establece en 1 (true) en la plantilla, el procedimiento almacenado se ejecuta en el servidor y el conjunto de filas de dos columnas que devuelve el servidor se transforma en XML en el nivel intermedio y se devuelve al cliente. (Aquí solamente se muestra un resultado parcial.)  
  
```  
 <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <Person.Contact FirstName="Gustavo" LastName="Achong" />   
  <Person.Contact FirstName="Catherine" LastName="Abel" />  
</ROOT>  
```  
  
> [!NOTE]  
>  Cuando se usa el proveedor SQLXMLOLEDB o las clases administradas de SQLXML, se puede usar la propiedad **clientsidexml,** para solicitar el formato XML del lado cliente.  
  
### <a name="the-workload-is-more-balanced"></a>La carga de trabajo está más equilibrada.  
 Dado que el cliente aplica el formato XML, la carga de trabajo se equilibra entre el servidor y cliente, dejando libre al servidor para que realice otras tareas.  
  
## <a name="supporting-client-side-xml-formatting"></a>Compatibilidad con el formato XML en el cliente  
 Para admitir la funcionalidad de formato XML del lado cliente, SQLXML proporciona:  
  
-   proveedor SQLXMLOLEDB  
  
-   clases administradas de SQLXML  
  
-   Compatibilidad mejorada con plantillas XML  
  
-   Propiedad SqlXmlCommand. Clientsidexml,  
  
     Puede especificar el formato del lado cliente estableciendo esta propiedad de las clases administradas SQLXML en True.  
  
## <a name="enhanced-xml-template-support"></a>Compatibilidad mejorada con plantillas XML  
 A partir de [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] , la plantilla XML de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se ha mejorado con la adición del atributo **Client-Side-XML** . Si este atributo está establecido en True, se aplica formato al XML en el cliente. Tenga en cuenta que este atributo de plantilla es idéntico en la funcionalidad de la propiedad Clientsidexml, específica del proveedor de SQLXMLOLEDB.  
  
> [!NOTE]  
>  Si ejecuta una plantilla XML en una aplicación ADO que usa el proveedor SQLXMLOLEDB y especifica el atributo **Client-Side-XML** en la plantilla y la propiedad Provider clientsidexml,, el valor especificado en la plantilla tiene prioridad.  
  
## <a name="see-also"></a>Consulte también  
 [Arquitectura del formato XML del lado cliente y del lado servidor &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml/formatting/architecture-of-client-side-and-server-side-xml-formatting-sqlxml-4-0.md)   
 [SQL Server &#40;FOR XML&#41;](../../../relational-databases/xml/for-xml-sql-server.md)   
 [Consideraciones de seguridad de FOR XML &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/for-xml-security-considerations-sqlxml-4-0.md)   
 [Compatibilidad con tipos de datos XML en SQLXML 4,0](../../../relational-databases/sqlxml/xml-data-type-support-in-sqlxml-4-0.md)   
 [Clases administradas de SQLXML](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/sqlxml-4-0-net-framework-support-managed-classes.md)   
 [Formato XML del lado cliente y de servidor &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml/formatting/client-side-vs-server-side-xml-formatting-sqlxml-4-0.md)   
 [Objeto SqlXmlCommand &#40;clases administradas de SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/sqlxml-managed-classes-sqlxmlcommand-object.md)   
 [Datos XML &#40;SQL Server&#41;](../../../relational-databases/xml/xml-data-sql-server.md)  
  
  
