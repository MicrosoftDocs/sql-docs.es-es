---
title: Instrucciones y limitaciones de la carga masiva XML (SQLXML)
description: Obtenga información sobre las instrucciones y limitaciones del uso de la carga masiva XML en SQLXML 4,0.
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- XML Bulk Load [SQLXML], about XML Bulk Load
- bulk load [SQLXML], about bulk load
ms.assetid: c5885d14-c7c1-47b3-a389-455e99a7ece1
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b17b1990f168326ae884b4db4f1ff22a63b24e21
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97439697"
---
# <a name="guidelines-and-limitations-of-xml-bulk-load-sqlxml-40"></a>Instrucciones y limitaciones de la carga masiva XML (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Antes de usar la carga masiva XML, debe familiarizarse con las siguientes limitaciones e instrucciones:  
  
-   No se admiten los esquemas insertados.  
  
     Si tiene un esquema insertado en el documento XML de origen, la carga masiva XML omite dicho esquema. Debe especificar el esquema de asignación para la carga masiva XML de forma externa con respecto a los datos XML. No se puede especificar el esquema de asignación en un nodo mediante el atributo **xmlns = "x:Schema"** .  
  
-   Se comprueba que el formato de un documento XML sea correcto, pero no se valida el documento.  
  
     La carga masiva XML comprueba el documento XML para determinar si está bien formada; es decir, para asegurarse de que el XML cumple los requisitos de sintaxis de la recomendación 1,0 de XML del World Wide Web Consortium. Si el documento no tiene el formato correcto, la carga masiva XML cancela el procesamiento y devuelve un error. La única excepción a esta regla es que el documento sea un fragmento (por ejemplo, el documento no tiene ningún elemento raíz único), en cuyo caso la carga masiva XML cargará el documento.  
  
     La carga masiva XML no valida el documento con respecto a cualquier esquema de datos XML o esquema DTD que se defina o al que se haga referencia dentro del archivo de datos XML. Además, la carga masiva XML no valida el archivo de datos XML en el esquema de asignación proporcionado.  
  
-   Se omite cualquier información de prólogo XML.  
  
     La carga masiva XML omite toda la información antes y después del \<root> elemento en el documento XML. Por ejemplo, la carga masiva XML omite todas las declaraciones XML, definiciones DTD internas, referencias DTD externas, todos los comentarios, etc.  
  
-   Si tiene un esquema de asignación que define una relación de clave principal y clave externa entre dos tablas (como Customer y CustOrder), la tabla con la clave principal debe describirse en primer lugar en el esquema. La tabla con la columna de clave externa debe aparecer posteriormente en el esquema. El motivo es que el orden en el que se identifican las tablas en el esquema es el que se usa para cargarlos en la base de datos. Por ejemplo, el esquema XDR siguiente generará un error si se utiliza en la carga masiva XML porque el **\<Order>** elemento se describe antes que el **\<Customer>** elemento. La columna CustomerID de CustOrder es una columna de clave externa que hace referencia a la columna de clave principal CustomerID de la tabla Cust.  
  
    ```  
    <?xml version="1.0" ?>  
    <Schema xmlns="urn:schemas-microsoft-com:xml-data"   
            xmlns:dt="urn:schemas-microsoft-com:xml:datatypes"    
            xmlns:sql="urn:schemas-microsoft-com:xml-sql" >  
  
        <ElementType name="Order" sql:relation="CustOrder" >  
          <AttributeType name="OrderID" />  
          <AttributeType name="CustomerID" />  
          <attribute type="OrderID" />  
          <attribute type="CustomerID" />  
        </ElementType>  
  
       <ElementType name="CustomerID" dt:type="int" />  
       <ElementType name="CompanyName" dt:type="string" />  
       <ElementType name="City" dt:type="string" />  
  
       <ElementType name="root" sql:is-constant="1">  
          <element type="Customers" />  
       </ElementType>  
       <ElementType name="Customers" sql:relation="Cust"   
                         sql:overflow-field="OverflowColumn"  >  
          <element type="CustomerID" sql:field="CustomerID" />  
          <element type="CompanyName" sql:field="CompanyName" />  
          <element type="City" sql:field="City" />  
          <element type="Order" >   
               <sql:relationship  
                   key-relation="Cust"  
                    key="CustomerID"  
                    foreign-key="CustomerID"  
                    foreign-relation="CustOrder" />  
          </element>  
       </ElementType>  
    </Schema>  
    ```  
  
-   Si el esquema no especifica las columnas de desbordamiento mediante la anotación **SQL: Overflow-Field** , la carga masiva XML omite cualquier dato presente en el documento XML, pero no se describe en el esquema de asignación.  
  
     La carga masiva XML aplica el esquema de asignación especificado cada vez que encuentra etiquetas conocidas en el flujo de datos XML. Omite los datos presentes en el documento XML que no se describen en el esquema. Por ejemplo, suponga que tiene un esquema de asignación que describe un **\<Customer>** elemento. El archivo de datos XML tiene una **\<AllCustomers>** etiqueta raíz (que no se describe en el esquema) que incluye todos los **\<Customer>** elementos:  
  
    ```  
    <AllCustomers>  
      <Customer>...</Customer>  
      <Customer>...</Customer>  
       ...  
    </AllCustomers>  
    ```  
  
     En este caso, la carga masiva XML omite el **\<AllCustomers>** elemento y comienza la asignación en el **\<Customer>** elemento. La carga masiva XML omite los elementos que no se describen en el esquema pero que están presentes en el documento XML.  
  
     Considere otro archivo de datos de origen XML que contenga **\<Order>** elementos. Estos elementos no se describen en el esquema de asignación:  
  
    ```  
    <AllCustomers>  
      <Customer>...</Customer>  
        <Order> ... </Order>  
        <Order> ... </Order>  
         ...  
      <Customer>...</Customer>  
        <Order> ... </Order>  
        <Order> ... </Order>  
         ...  
      ...  
    </AllCustomers>  
    ```  
  
     La carga masiva XML omite estos **\<Order>** elementos. Pero si utiliza la anotación **SQL: Overflow-Field** en el esquema para identificar una columna como una columna de desbordamiento, la carga masiva XML almacena todos los datos no consumidos en esta columna.  
  
-   Las secciones CDATA y las referencias a entidades se traducen a sus cadenas equivalentes antes de almacenarse en la base de datos.  
  
     En este ejemplo, una sección CDATA ajusta el valor del **\<City>** elemento. La carga masiva XML extrae el valor de cadena ("NY") antes de insertar el **\<City>** elemento en la base de datos.  
  
    ```  
    <City><![CDATA[NY]]> </City>  
    ```  
  
     La carga masiva XML no conserva las referencias a entidades.  
  
-   Si el esquema de asignación especifica el valor predeterminado de un atributo y los datos de origen XML no contienen dicho atributo, la carga masiva XML usa el valor predeterminado.  
  
     El siguiente esquema XDR de ejemplo asigna un valor predeterminado al atributo **HireDate** :  
  
    ```  
    <?xml version="1.0" ?>  
    <Schema xmlns="urn:schemas-microsoft-com:xml-data"   
            xmlns:dt="urn:schemas-microsoft-com:xml:datatypes"    
            xmlns:sql="urn:schemas-microsoft-com:xml-sql" >  
       <ElementType name="root" sql:is-constant="1">  
          <element type="Customers" />  
       </ElementType>  
  
       <ElementType name="Customers" sql:relation="Cust3" >  
          <AttributeType name="CustomerID" dt:type="int"  />  
          <AttributeType name="HireDate"  default="2000-01-01" />  
          <AttributeType name="Salary"   />  
  
          <attribute type="CustomerID" sql:field="CustomerID" />  
          <attribute type="HireDate"   sql:field="HireDate"  />  
          <attribute type="Salary"     sql:field="Salary"    />  
       </ElementType>  
    </Schema>  
    ```  
  
     En estos datos XML, falta el atributo **HireDate** en el segundo **\<Customers>** elemento. Cuando la carga masiva XML inserta el segundo **\<Customers>** elemento en la base de datos, utiliza el valor predeterminado que se especifica en el esquema.  
  
    ```  
    <ROOT>  
      <Customers CustomerID="1" HireDate="1999-01-01" Salary="10000" />  
      <Customers CustomerID="2" Salary="10000" />  
    </ROOT>  
    ```  
  
-   No se admite la anotación **SQL: URL-encode** :  
  
     No puede especificar una dirección URL en la entrada de datos XML y esperar que la carga masiva lea los datos de dicha ubicación.  
  
     Se crean las tablas que se identifican en el esquema de asignación (la base de datos debe existir). Si una o más tablas ya existen en la base de datos, la propiedad SGDropTables determina si estas tablas preexistentes se van a quitar y volver a crear.  
  
-   Si especifica la propiedad SchemaGen (por ejemplo, SchemaGen = true), se crean las tablas que se identifican en el esquema de asignación. Pero SchemaGen no crea ninguna restricción (como las restricciones de clave principal y clave externa) en estas tablas con una excepción: si los nodos XML que constituyen la clave principal de una relación se definen como si tuvieran un tipo XML de identificador (es decir, **type = "xsd: ID"** para XSD) y la propiedad SGUseID se establece en true para SchemaGen, no solo se crean las claves principales a partir de los nodos con tipo ID, pero las relaciones de clave principal y clave externa se crean a partir de las relaciones  
  
-   SchemaGen no utiliza las extensiones y las caras del esquema XSD para generar el [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] esquema relacional.  
  
-   Si especifica la propiedad SchemaGen (por ejemplo, SchemaGen = true) en la carga masiva, solo se actualizarán las tablas (y no las vistas de nombre compartido) que se especifiquen.  
  
-   SchemaGen solo proporciona la funcionalidad básica para generar el esquema relacional a partir de XSD anotado. El usuario debe modificar las tablas generadas manualmente si es preciso.  
  
-   Cuando existe más de una relación entre las tablas, SchemaGen intenta crear una relación única que incluye todas las claves implicadas entre las dos tablas. Esta limitación podría dar lugar a un error [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
-   Al realizar cargas masivas de datos XML en una base de datos, debe haber al menos un atributo o elemento secundario en el esquema de asignación que esté asignado a una columna de base de datos.  
  
-   Si está insertando valores de fecha mediante la carga masiva XML, estos valores deben especificarse con el formato (-)CCYY-MM-DD((+-)TZ). Se trata del formato XSD estándar para la fecha.  
  
-   Algunas marcas de propiedad no son compatibles con otras. Por ejemplo, la carga masiva no admite **Ignoreduplicatekeys = true** junto con **KEEPIDENTITY = false**. Cuando **KEEPIDENTITY = false**, la carga masiva espera que el servidor genere los valores de clave. Las tablas deben tener una restricción de **identidad** en la clave. El servidor no generará claves duplicadas, lo que significa que no es necesario establecer **Ignoreduplicatekeys** en **true**. **Ignoreduplicatekeys** debe establecerse en **true** solo cuando se cargan valores de clave principal de los datos entrantes en una tabla que tiene filas y existe la posibilidad de conflictos de valores de clave principal.  
  
  
