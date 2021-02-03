---
description: ALTER XML SCHEMA COLLECTION (Transact-SQL)
title: ALTER XML SCHEMA COLLECTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_XML_SCHEMA_COLLECTION_TSQL
- ALTER XML SCHEMA COLLECTION
dev_langs:
- TSQL
helpviewer_keywords:
- schema collections [SQL Server], altering
- xml_schema_namespace function
- adding schema components
- ALTER XML SCHEMA COLLECTION statement
- XML schemas [SQL Server], adding
- XML schema collections [SQL Server], modifying
- schema collections [SQL Server], adding components
- XML schema collections [SQL Server], adding components
- importing schemas
- XML schema collections [SQL Server], altering
- schema collections [SQL Server], modifying
- multiple schema namespaces
ms.assetid: e311c425-742a-4b0d-b847-8b974bf66d53
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4aecc0d83a4161743e6d596f55b383b45da6bb27
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199418"
---
# <a name="alter-xml-schema-collection-transact-sql"></a>ALTER XML SCHEMA COLLECTION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Agrega componentes de esquema nuevos a una colección de esquemas XML ya existente.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
ALTER XML SCHEMA COLLECTION [ relational_schema. ]sql_identifier ADD 'Schema Component'  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *relational_schema*  
 Identifica el nombre del esquema relacional. Si no se especifica, se usará el esquema relacional predeterminado.  
  
 *sql_identifier*  
 Es el identificador de SQL para la colección de esquemas XML.  
  
 **'** *Schema Component* **'**  
 Es el componente de esquema que se va a insertar.  
  
## <a name="remarks"></a>Observaciones  
 Utilice la instrucción ALTER XML SCHEMA COLLECTION para agregar esquemas XML nuevos cuyos espacios de nombre no se encuentren todavía en la colección de esquemas XML o para agregar componentes nuevos a espacios de nombre ya existentes en la colección.  
  
 En el siguiente ejemplo se agrega un \<element> nuevo al espacio de nombres `https://MySchema/test_xml_schema` existente en la colección `MyColl`.  
  
```sql  
-- First create an XML schema collection.  
CREATE XML SCHEMA COLLECTION MyColl AS '  
   <schema   
    xmlns="http://www.w3.org/2001/XMLSchema"   
    targetNamespace="https://MySchema/test_xml_schema">  
      <element name="root" type="string"/>   
  </schema>'  
-- Modify the collection.   
ALTER XML SCHEMA COLLECTION MyColl ADD '  
  <schema xmlns="http://www.w3.org/2001/XMLSchema"   
         targetNamespace="https://MySchema/test_xml_schema">   
     <element name="anotherElement" type="byte"/>   
 </schema>';  
```  
  
 `ALTER XML SCHEMA` agrega el elemento `<anotherElement>` al espacio de nombres definido previamente `https://MySchema/test_xml_schema`.  
  
 Tenga en cuenta que si alguno de los componentes que desea agregar a los componentes de referencia de la colección ya existe en dicha colección, debe utilizar `<import namespace="referenced_component_namespace" />`. No obstante, no se puede utilizar el espacio de nombres del esquema actual en `<xsd:import>` ni, tampoco, componentes del mismo espacio de nombres de destino, ya que el espacio de nombres del esquema actual se importa automáticamente.  
  
 Para quitar colecciones, use [DROP XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-xml-schema-collection-transact-sql.md).  
  
 Si la colección de esquemas ya contiene un carácter comodín de validación lax o un elemento de tipo **xs:anyType**, al agregar un nuevo elemento, tipo o declaración de atributos global a la colección de esquemas, se producirá una nueva validación de todos los datos almacenados que están restringidos por la colección de esquemas.  
  
## <a name="permissions"></a>Permisos  
 Para modificar una colección de esquemas XML (XML SCHEMA COLLECTION) es necesario el permiso ALTER sobre la colección.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-creating-xml-schema-collection-in-the-database"></a>A. Crear una colección de esquemas XML en la base de datos  
 En el ejemplo siguiente se crea la colección de esquemas XML `ManuInstructionsSchemaCollection`. La colección solo tiene un espacio de nombres de esquema.  
  
```sql  
-- Create a sample database in which to load the XML schema collection.  
CREATE DATABASE SampleDB;  
GO  
USE SampleDB;  
GO  
CREATE XML SCHEMA COLLECTION ManuInstructionsSchemaCollection AS  
N'<?xml version="1.0" encoding="UTF-16"?>  
<xsd:schema targetNamespace="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"   
   xmlns          ="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"   
   elementFormDefault="qualified"   
   attributeFormDefault="unqualified"  
   xmlns:xsd="http://www.w3.org/2001/XMLSchema" >  
  
    <xsd:complexType name="StepType" mixed="true" >  
        <xsd:choice  minOccurs="0" maxOccurs="unbounded" >   
            <xsd:element name="tool" type="xsd:string" />  
            <xsd:element name="material" type="xsd:string" />  
            <xsd:element name="blueprint" type="xsd:string" />  
            <xsd:element name="specs" type="xsd:string" />  
            <xsd:element name="diag" type="xsd:string" />  
        </xsd:choice>   
    </xsd:complexType>  
  
    <xsd:element  name="root">  
        <xsd:complexType mixed="true">  
            <xsd:sequence>  
                <xsd:element name="Location" minOccurs="1" maxOccurs="unbounded">  
                    <xsd:complexType mixed="true">  
                        <xsd:sequence>  
                            <xsd:element name="step" type="StepType" minOccurs="1" maxOccurs="unbounded" />  
                        </xsd:sequence>  
                        <xsd:attribute name="LocationID" type="xsd:integer" use="required"/>  
                        <xsd:attribute name="SetupHours" type="xsd:decimal" use="optional"/>  
                        <xsd:attribute name="MachineHours" type="xsd:decimal" use="optional"/>  
                        <xsd:attribute name="LaborHours" type="xsd:decimal" use="optional"/>  
                        <xsd:attribute name="LotSize" type="xsd:decimal" use="optional"/>  
                    </xsd:complexType>  
                </xsd:element>  
            </xsd:sequence>  
        </xsd:complexType>  
    </xsd:element>  
</xsd:schema>' ;  
GO  
-- Verify - list of collections in the database.  
SELECT *  
FROM sys.xml_schema_collections;  
-- Verify - list of namespaces in the database.  
SELECT name  
FROM sys.xml_schema_namespaces;  
  
-- Use it. Create a typed xml variable. Note the collection name   
-- that is specified.  
DECLARE @x xml (ManuInstructionsSchemaCollection);  
GO  
--Or create a typed xml column.  
CREATE TABLE T (  
        i int primary key,   
        x xml (ManuInstructionsSchemaCollection));  
GO  
-- Clean up.  
DROP TABLE T;  
GO  
DROP XML SCHEMA COLLECTION ManuInstructionsSchemaCollection;  
Go  
USE master;  
GO  
DROP DATABASE SampleDB;  
```  
  
 También puede asignar la colección de esquemas a una variable y especificar la variable en la instrucción `CREATE XML SCHEMA COLLECTION` de la forma siguiente:  
  
```sql  
DECLARE @MySchemaCollection nvarchar(max);  
SET @MySchemaCollection  = N' copy the schema collection here';  
CREATE XML SCHEMA COLLECTION AS @MySchemaCollection;   
```  
  
 La variable del ejemplo es de tipo `nvarchar(max)`. La variable también puede ser del tipo de datos **xml**; si es así, se convierte de forma implícita en una cadena.  
  
 Para obtener más información, vea [Ver una colección de esquemas XML almacenada](../../relational-databases/xml/view-a-stored-xml-schema-collection.md).  
  
 Puede almacenar colecciones de esquemas en una columna de tipo **xml**. En este caso, para crear una colección de esquemas XML, realice los pasos siguientes:  
  
1.  Recupere la colección de esquemas de la columna mediante una instrucción SELECT y asígnela a una variable de tipo **xml** o **varchar**.  
  
2.  Especifique el nombre de la variable en la instrucción CREATE XML SCHEMA COLLECTION.  
  
 La instrucción CREATE XML SCHEMA COLLECTION solo almacena los componentes de esquema que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] entiende; no se almacena todo el contenido del esquema XML en la base de datos. Por lo tanto, si desea recuperar la colección de esquemas XML tal como fue suministrada, es recomendable que guarde los esquemas XML en una columna de base de datos o en alguna otra carpeta del equipo.  
  
### <a name="b-specifying-multiple-schema-namespaces-in-a-schema-collection"></a>B. Especificar varios espacios de nombres de esquema en una colección de esquemas  
 Al crear una colección de esquemas XML, puede especificar varios esquemas XML. Por ejemplo:  
  
```sql  
CREATE XML SCHEMA COLLECTION N'  
<xsd:schema>....</xsd:schema>  
<xsd:schema>...</xsd:schema>';  
```  
  
 En el ejemplo siguiente se crea la colección de esquemas XML `ProductDescriptionSchemaCollection`, que incluye dos espacios de nombres de esquema XML.  
  
```sql  
CREATE XML SCHEMA COLLECTION ProductDescriptionSchemaCollection AS   
'<xsd:schema targetNamespace="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain"  
    xmlns="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain"   
    elementFormDefault="qualified"   
    xmlns:xsd="http://www.w3.org/2001/XMLSchema" >  
    <xsd:element name="Warranty"  >  
        <xsd:complexType>  
            <xsd:sequence>  
                <xsd:element name="WarrantyPeriod" type="xsd:string"  />  
                <xsd:element name="Description" type="xsd:string"  />  
            </xsd:sequence>  
        </xsd:complexType>  
    </xsd:element>  
</xsd:schema>  
 <xs:schema targetNamespace="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription"   
    xmlns="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription"   
    elementFormDefault="qualified"   
    xmlns:mstns="https://tempuri.org/XMLSchema.xsd"   
    xmlns:xs="http://www.w3.org/2001/XMLSchema"  
    xmlns:wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain" >  
    <xs:import   
namespace="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain" />  
    <xs:element name="ProductDescription" type="ProductDescription" />  
        <xs:complexType name="ProductDescription">  
            <xs:sequence>  
                <xs:element name="Summary" type="Summary" minOccurs="0" />  
            </xs:sequence>  
            <xs:attribute name="ProductModelID" type="xs:string" />  
            <xs:attribute name="ProductModelName" type="xs:string" />  
        </xs:complexType>  
        <xs:complexType name="Summary" mixed="true" >  
            <xs:sequence>  
                <xs:any processContents="skip" namespace="http://www.w3.org/1999/xhtml" minOccurs="0" maxOccurs="unbounded" />  
            </xs:sequence>  
        </xs:complexType>  
</xs:schema>'  
;  
GO   
-- Clean up  
DROP XML SCHEMA COLLECTION ProductDescriptionSchemaCollection;  
GO  
```  
  
### <a name="c-importing-a-schema-that-does-not-specify-a-target-namespace"></a>C. Importar un esquema que no especifique un espacio de nombres de destino  
 Si un esquema que no contiene un atributo **targetNamespace** se importa en una colección, sus componentes se asocian al espacio de nombres de destino de cadena vacío, como se muestra en el ejemplo siguiente. Tenga en cuenta que si no asocia uno o varios esquemas importados en la colección, varios componentes del esquema (potencialmente no relacionados) se asociarán al espacio de nombres de cadena vacía predeterminado.  
  
```sql  
-- Create a collection that contains a schema with no target namespace.  
CREATE XML SCHEMA COLLECTION MySampleCollection AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  xmlns:ns="http://ns">  
<element name="e" type="dateTime"/>  
</schema>';  
GO  
-- query will return the names of all the collections that   
--contain a schema with no target namespace  
SELECT sys.xml_schema_collections.name   
FROM   sys.xml_schema_collections   
JOIN   sys.xml_schema_namespaces   
ON     sys.xml_schema_collections.xml_collection_id =   
       sys.xml_schema_namespaces.xml_collection_id   
WHERE  sys.xml_schema_namespaces.name='';  
```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-xml-schema-collection-transact-sql.md)   
 [DROP XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-xml-schema-collection-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [Comparar XML con tipo y XML sin tipo](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Requisitos y limitaciones de las colecciones de esquemas XML en el servidor](../../relational-databases/xml/requirements-and-limitations-for-xml-schema-collections-on-the-server.md)  
  
  
