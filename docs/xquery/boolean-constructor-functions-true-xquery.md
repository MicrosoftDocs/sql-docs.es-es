---
title: Función true (XQuery) | Microsoft Docs
description: Obtenga información sobre la función XQuery true () que devuelve el valor booleano true.
ms.custom: ''
ms.date: 08/10/2016
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- fn:true function
- true function
ms.assetid: 318e370d-0444-4812-afe4-307df7ef9f3b
author: rothja
ms.author: jroth
ms.openlocfilehash: 779e3fa093d851e99e2869053f155fb70570f67c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340504"
---
# <a name="boolean-constructor-functions---true-xquery"></a>Funciones de constructor booleano: true (XQuery)
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Devuelve el valor xs:boolean True. Es equivalente a `xs:boolean("1")`.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
fn:true() as xs:boolean  
```  
  
## <a name="examples"></a>Ejemplos  
 En este tema se proporcionan ejemplos de XQuery con instancias XML almacenadas en varias columnas de tipo **XML** de la base de datos AdventureWorks.  
  
### <a name="a-using-the-true-xquery-boolean-function"></a>A. Utilizar la función booleana true() de XQuery  
 En el ejemplo siguiente se consulta una variable **XML** sin tipo. La expresión del método **Value ()** devuelve el valor booleano **true ()** si "AAA" es el valor del atributo. El método **Value ()** del tipo de datos **XML** convierte el valor booleano en un bit y lo devuelve.  
  
```  
DECLARE @x XML  
SET @x= '<ROOT><elem attr="aaa">bbb</elem></ROOT>'  
select @x.value(' if ( (/ROOT/elem/@attr)[1] eq "aaa" ) then fn:true() else fn:false() ', 'bit')  
go  
-- result = 1  
```  
  
 En el ejemplo siguiente, la consulta se especifica en una columna **XML** con tipo. La `if` expresión comprueba el valor booleano con tipo del elemento <`ROOT`> y devuelve el XML construido, según corresponda. En el ejemplo, se realizan las tareas siguientes:  
  
-   Crea una colección de esquemas XML que define el `ROOT` elemento <> del tipo XS: Boolean.  
  
-   Crea una tabla con una columna **XML** con tipo utilizando la colección de esquemas XML.  
  
-   Se guarda una instancia XML en la columna y se consulta.  
  
```  
-- Drop table if exist  
--DROP TABLE T  
--go  
DROP XML SCHEMA COLLECTION SC  
go  
CREATE XML SCHEMA COLLECTION SC AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  
targetNamespace="QNameXSD" >  
      <element name="ROOT" type="boolean" nillable="true"/>  
</schema>'  
go  
CREATE TABLE T (xmlCol XML(SC))  
go  
-- following OK  
insert into T values ('<ROOT xmlns="QNameXSD">true</ROOT>')  
 go  
-- Retrieve the local name.   
SELECT xmlCol.query('declare namespace a="QNameXSD";   
   if (/a:ROOT[1] eq true()) then  
       <result>Found boolean true</result>  
   else  
       <result>Found boolean false</result>')  
  
FROM T  
-- result = <result>Found boolean true</result>  
-- Clean up  
DROP TABLE T  
go  
DROP XML SCHEMA COLLECTION SC  
go  
```  
  
## <a name="see-also"></a>Consulte también  
 [Funciones de constructor booleano &#40;XQuery&#41;](./xquery-functions-against-the-xml-data-type.md)  
  
