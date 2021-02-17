---
title: expandido-QName (XQuery) | Microsoft Docs
description: Obtenga información sobre cómo usar la función Expanded-QName () para devolver el URI de espacio de nombres y la parte del nombre local de un QName.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- expanded-QName function
- fn:expanded-QName function
ms.assetid: b8377042-95cc-467b-9ada-fe43cebf4bc3
author: rothja
ms.author: jroth
ms.openlocfilehash: 704cac94b48fe172eaa6a972cdd6c49bad1601fe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344928"
---
# <a name="functions-related-to-qnames---expanded-qname"></a>Funciones relacionadas con QNames: expanded-QName
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Devuelve un valor del tipo XS: QName con el URI de espacio de nombres especificado en el *$paramURI* y el nombre local especificado en el *$paramLocal*. Si *$paramURI* es una cadena vacía o una secuencia vacía, no representa ningún espacio de nombres.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
fn:expanded-QName($paramURI as xs:string?, $paramLocal as xs:string?) as xs:QName?  
```  
  
## <a name="arguments"></a>Argumentos  
 *$paramURI*  
 Es el URI del espacio de nombres de QName.  
  
 *$paramLocal*  
 Es la parte local del nombre de QName.  
  
## <a name="remarks"></a>Observaciones  
 Lo siguiente se aplica a la función **Expanded-QName ()** :  
  
-   Si el valor de *$paramLocal* especificado no tiene el formato léxico correcto para el tipo XS: NCName, se devuelve la secuencia vacía y representa un error dinámico.  
  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] no admite la conversión del tipo xs:QName a ningún otro tipo. Por este motivo, no se puede usar la función **Expanded-QName ()** en la construcción de XML. Por ejemplo, cuando se construye un nodo, como `<e> expanded-QName(...) </e>`, el valor no puede tener un tipo. Esto requeriría la conversión del valor de tipo xs:QName devuelto por `expanded-QName()` en xdt:untypedAtomic. Sin embargo, esta opción no se admite. Más adelante en este tema se ofrece un ejemplo.  
  
-   Es posible modificar o comparar los valores de tipo QName existentes. Por ejemplo, `/root[1]/e[1] eq expanded-QName("http://nsURI" "myNS")` compara el valor del elemento, <`e`>, con el QName devuelto por la función **Expanded-QName ()** .  
  
## <a name="examples"></a>Ejemplos  
 En este tema se proporcionan ejemplos de XQuery con instancias XML almacenadas en varias columnas de tipo **XML** de la [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] base de datos.  
  
### <a name="a-replacing-a-qname-type-node-value"></a>A. Reemplazar un valor de nodo del tipo QName  
 En este ejemplo se ilustra cómo se puede modificar el valor de un nodo de elemento del tipo QName. En el ejemplo, se realizan las tareas siguientes:  
  
-   Se crea una colección de esquemas XML que define un elemento del tipo QName.  
  
-   Crea una tabla con una columna de tipo **XML** utilizando la colección de esquemas XML.  
  
-   Se guarda una instancia XML en la tabla.  
  
-   Utiliza el método **Modify ()** del tipo de datos XML para modificar el valor del elemento de tipo QName en la instancia. La función **Expanded-QName ()** se utiliza para generar el nuevo valor de tipo QName.  
  
```  
-- If XML schema collection (if exists)  
-- drop xml schema collection SC  
-- go  
-- Create XML schema collection  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  
    xmlns:xs="http://www.w3.org/2001/XMLSchema"   
    targetNamespace="QNameXSD"   
      xmlns:xqo="QNameXSD" elementFormDefault="qualified">  
      <element name="Root" type="xqo:rootType" />  
      <complexType name="rootType">  
            <sequence minOccurs="1" maxOccurs="1">  
                        <element name="ElemQN" type="xs:QName" />  
            </sequence>  
      </complexType>  
</schema>'  
go  
-- Create table.  
CREATE TABLE T( XmlCol xml(SC) )  
-- Insert sample XML instnace  
INSERT INTO T VALUES ('  
<Root xmlns="QNameXSD" xmlns:ns="https://myURI">  
      <ElemQN>ns:someName</ElemQN>  
</Root>')  
go  
-- Verify the insertion  
SELECT * from T  
go  
-- Result  
<Root xmlns="QNameXSD" xmlns:ns="https://myURI">  
  <ElemQN>ns:someName</ElemQN>  
</Root>   
```  
  
 En la consulta siguiente, el `ElemQN` valor del elemento> de <se reemplaza con el método **Modify ()** del tipo de datos XML y el valor Replace de XML DML, tal como se muestra.  
  
```  
-- the value.  
UPDATE T   
SET XmlCol.modify('  
  declare default element namespace "QNameXSD";   
  replace value of /Root[1]/ElemQN   
  with expanded-QName("https://myURI", "myLocalName") ')  
go  
-- Verify the result  
SELECT * from T  
go  
```  
  
 Éste es el resultado. Tenga en cuenta que el elemento <`ElemQN`> de tipo QName ahora tiene un valor nuevo:  
  
```  
<Root xmlns="QNameXSD" xmlns:ns="urn">  
  <ElemQN xmlns:p1="https://myURI">p1:myLocalName</ElemQN>  
</Root>  
```  
  
 Las instrucciones siguientes eliminan los objetos utilizados en el ejemplo.  
  
```  
-- Cleanup  
DROP TABLE T  
go  
drop xml schema collection SC  
go  
```  
  
### <a name="b-dealing-with-the-limitations-when-using-the-expanded-qname-function"></a>B. Superar las limitaciones cuando se utiliza la función expanded-QName()  
 No se puede usar la función **Expanded-QName** en la construcción de XML. Esto se ilustra en el siguiente ejemplo: Para evitar esta limitación, el ejemplo inserta primero un nodo y, a continuación, modifica el nodo.  
  
```  
-- if exists drop the table T  
--drop table T  
-- go  
-- Create XML schema collection  
-- DROP XML SCHEMA COLLECTION SC  
-- go  
CREATE XML SCHEMA COLLECTION SC AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema">  
      <element name="root" type="QName" nillable="true"/>  
</schema>'  
go  
 -- Create table T with a typed xml column (using the XML schema collection)  
CREATE TABLE T (xmlCol XML(SC))  
go  
-- Insert an XML instance.  
insert into T values ('<root xmlns:a="https://someURI">a:b</root>')  
 go  
-- Verify  
SELECT *   
FROM T  
```  
  
 El siguiente intento agrega otro <`root` elemento de>, pero produce un error, porque la función Expanded-QName () no se admite en la construcción de XML.  
  
```  
update T SET xmlCol.modify('  
insert <root>{expanded-QName("http://ns","someLocalName")}</root> as last into / ')  
go  
```  
  
 Una solución a esto consiste en Insertar primero una instancia de con un valor para el `root` elemento <> y, a continuación, modificarlo. En este ejemplo, se utiliza un valor inicial Nil cuando `root` se inserta el elemento> de <. La colección de esquemas XML de este ejemplo permite un valor Nil para el `root` elemento> <.  
  
```  
update T SET xmlCol.modify('  
insert <root xsi:nil="true"/> as last into / ')  
go  
-- now replace the nil value with another QName.  
update T SET xmlCol.modify('  
replace value of /root[last()] with expanded-QName("http://ns","someLocalName") ')  
go  
 -- verify   
SELECT * FROM T  
go  
-- result  
<root>b</root>  
```  
  
 `<root xmlns:a="https://someURI">a:b</root>`  
  
 `<root xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p1="http://ns">p1:someLocalName</root>`  
  
 Se puede comparar el valor QName, tal y como se muestra en la consulta siguiente. La consulta devuelve solo los `root` elementos <> cuyos valores coinciden con el valor de tipo QName devuelto por la función **Expanded-QName ()** .  
  
```  
SELECT xmlCol.query('  
    for $i in /root  
    return  
       if ($i eq expanded-QName("http://ns","someLocalName") ) then  
          $i  
       else  
          ()')  
FROM T  
```  
  
### <a name="implementation-limitations"></a>Limitaciones de la implementación  
 Existe una limitación: la función **Expanded-QName ()** acepta la secuencia vacía como segundo argumento y devolverá un valor vacío en lugar de generar un error en tiempo de ejecución cuando el segundo argumento sea incorrecto.  
  
## <a name="see-also"></a>Consulte también  
 [Funciones relacionadas con QNames &#40;XQuery&#41;]()  
  
