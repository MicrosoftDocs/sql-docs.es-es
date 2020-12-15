---
title: Usar funciones de conversión en consultas XPath (SQLXML)
description: Obtenga información sobre cómo especificar las funciones de conversión explícita String () y Number () en consultas SQLXML 4,0 XPath.
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- explicit conversion functions [SQLXML]
- string function
- number function
- XPath queries [SQLXML], explicit conversion functions
ms.assetid: 1111cb5d-2bd9-4bdb-8de2-dc0e47452dd6
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 279863e3dee632201c9f931b9eb02ea85bfc287c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97430503"
---
# <a name="specifying-explicit-conversion-functions-in-xpath-queries-sqlxml-40"></a>Especificar funciones de conversión explícita en consultas XPath (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  En los siguientes ejemplos se muestra cómo especificar funciones de conversión explícita en consultas XPath. Las consultas XPath de estos ejemplos se especifican en el esquema de asignación que se incluye en SampleSchema1.xml. Para obtener información acerca de este esquema de ejemplo, vea [ejemplo de esquema XSD anotado para los ejemplos de XPath &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/sample-annotated-xsd-schema-for-xpath-examples-sqlxml-4-0.md).  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-use-the-number-explicit-conversion-function"></a>A. Usar la función de conversión explícita number()  
 La función **Number ()** convierte un argumento en un número.  
  
 Suponiendo que el valor de **ContactID** no es numérico, la siguiente consulta convierte **ContactID** en un número y lo compara con el valor 4. A continuación, la consulta devuelve todos los **\<Employee>** elementos secundarios del nodo de contexto con el atributo **ContactID** que tiene un valor numérico de 4:  
  
```  
/child::Contact[number(attribute::ContactID)= 4]  
```  
  
 Se puede especificar un acceso directo al eje de **atributo** (@) y, puesto que el eje **secundario** es el predeterminado, se puede omitir en la consulta:  
  
```  
/Contact[number(@ContactID) = 4]  
```  
  
 En términos relacionales, la consulta devuelve un empleado con un **ContactID** de 4.  
  
##### <a name="to-test-the-xpath-query-against-the-mapping-schema"></a>Para probar la consulta XPath en el esquema de asignación  
  
1.  Copie el [código de esquema de ejemplo](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/sample-annotated-xsd-schema-for-xpath-examples-sqlxml-4-0.md) y péguelo en un archivo de texto. Guarde el archivo como SampleSchema1.xml.  
  
2.  Cree la siguiente plantilla (ExplicitConversionA.xml) y guárdela en el mismo directorio donde esté guardado el archivo SampleSchema1.xml.  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
      <sql:xpath-query mapping-schema="SampleSchema1.xml">  
        /Contact[number(@ContactID)=4]  
      </sql:xpath-query>  
    </ROOT>  
    ```  
  
     La ruta de acceso al directorio especificada para el esquema de asignación (SampleSchema1.xml) es relativa al directorio donde se guarda la plantilla. También puede especificarse una ruta de acceso absoluta como, por ejemplo:  
  
    ```  
    mapping-schema="C:\MyDir\SampleSchema1.xml"  
    ```  
  
3.  Cree y use el script de prueba SQLXML 4.0 (Sqlxml4test.vbs) para ejecutar la plantilla.  
  
     Para obtener más información, vea [usar ado para ejecutar consultas SQLXML 4,0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Éste es el conjunto de resultados de ejecución de esta plantilla:  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <Contact ContactID="4" LastName="Acevedo" FirstName="Humberto" Title="Sr." />   
</ROOT>  
```  
  
### <a name="b-use-the-string-explicit-conversion-function"></a>B. Usar la función de conversión explícita string()  
 La función **String ()** convierte un argumento en una cadena.  
  
 La siguiente consulta convierte **ContactID** en una cadena y lo compara con el valor de cadena "4". La consulta devuelve todos los **\<Employee>** elementos secundarios del nodo de contexto con un **ContactID** con un valor de cadena de "4":  
  
```  
/child::Contact[string(attribute::ContactID)="4"]  
```  
  
 Se puede especificar un acceso directo al eje de **atributo** (@) y, puesto que el eje **secundario** es el predeterminado, se puede omitir en la consulta:  
  
```  
/Contact[string(@ContactID)="4"]  
```  
  
 Funcionalmente, esta consulta devuelve los mismos resultados que la consulta de ejemplo anterior, pero la evaluación se realiza en un valor de cadena y no en el valor numérico (es decir, el número 4).  
  
##### <a name="to-test-the-xpath-query-against-the-mapping-schema"></a>Para probar la consulta XPath en el esquema de asignación  
  
1.  Copie el [código de esquema de ejemplo](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/sample-annotated-xsd-schema-for-xpath-examples-sqlxml-4-0.md) y péguelo en un archivo de texto. Guarde el archivo como SampleSchema1.xml.  
  
2.  Cree la siguiente plantilla (ExplicitConversionB.xml) y guárdela en el mismo directorio donde esté guardado el archivo SampleSchema1.xml.  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
      <sql:xpath-query mapping-schema="SampleSchema1.xml">  
        Contact[string(@ContactID)="4"]  
      </sql:xpath-query>  
    </ROOT>  
    ```  
  
     La ruta de acceso al directorio especificada para el esquema de asignación (SampleSchema1.xml) es relativa al directorio donde se guarda la plantilla. También puede especificarse una ruta de acceso absoluta como, por ejemplo:  
  
    ```  
    mapping-schema="C:\MyDir\SampleSchema1.xml"  
    ```  
  
3.  Cree y use el script de prueba SQLXML 4.0 (Sqlxml4test.vbs) para ejecutar la plantilla.  
  
     Para obtener más información, vea [usar ado para ejecutar consultas SQLXML 4,0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Éste es el conjunto de resultados de ejecución de la plantilla:  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <Contact ContactID="4" LastName="Acevedo" FirstName="Humberto" Title="Sr." />   
</ROOT>  
```  
  
  
