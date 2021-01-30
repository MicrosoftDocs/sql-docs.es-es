---
description: sp_xml_preparedocument (Transact-SQL)
title: sp_xml_preparedocument (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_xml_preparedocument_TSQL
- sp_xml_preparedocument
dev_langs:
- TSQL
helpviewer_keywords:
- sp_xml_preparedocument
ms.assetid: 95f41cff-c52a-4182-8ac6-bf49369d214c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 8fc47f280e0c6f32e41cd7a0f7db34ff22ac98f1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201789"
---
# <a name="sp_xml_preparedocument-transact-sql"></a>sp_xml_preparedocument (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Lee el texto de XML proporcionado como entrada, analiza el texto con el analizador MSXML (Msxmlsql.dll) y proporciona el documento analizado en un estado apto para su uso. Este documento analizado es una representación en árbol de varios nodos en el documento XML: elementos, atributos, texto, comentarios, etc.  
  
 **sp_xml_preparedocument** devuelve un identificador que se puede utilizar para tener acceso a la representación interna que se acaba de crear del documento XML. Este identificador es válido mientras dure la sesión o hasta que el identificador se invalide ejecutando **sp_xml_removedocument**.  
  
> [!NOTE]  
>  Un documento analizado se guarda en la caché interna de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El analizador MSXML usa un octavo de la memoria total disponible para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para evitar quedarse sin memoria, ejecute **sp_xml_removedocument** para liberar memoria.  
  
> [!NOTE]  
>  Por compatibilidad con versiones anteriores, **sp_xml_preparedocument** contrae los caracteres CR (Char (13)) y LF (Char (10)) en los atributos, incluso si estos caracteres son entidades.  
  
> [!NOTE]  
>  El analizador XML invocado por **sp_xml_preparedocument** puede analizar las DTD internas y las declaraciones de entidad. Dado que las DTD y las declaraciones de entidad construidas de forma malintencionada se pueden usar para realizar un ataque de denegación de servicio, se recomienda encarecidamente que los usuarios no pasen directamente documentos XML de orígenes que no sean de confianza a **sp_xml_preparedocument**.  
>   
>  Para mitigar los ataques de expansión de entidades recursivas, **sp_xml_preparedocument** limita a 10.000 el número de entidades que se pueden expandir bajo una sola entidad en el nivel superior de un documento. Este límite no se aplica a los caracteres o entidades numéricas. Este límite permite almacenar documentos con muchas entidades, pero evita que cualquier entidad se expanda de forma recursiva en una cadena con más de 10.000 expansiones.  
  
> [!NOTE]  
>  **sp_xml_preparedocument** limita el número de elementos que pueden estar abiertos al mismo tiempo a 256.  

 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_xml_preparedocument  
hdoc   
OUTPUT  
[ , xmltext ]  
[ , xpath_namespaces ]   
```  
  
## <a name="arguments"></a>Argumentos  
 *hdoc*  
 Es el identificador del documento recién creado. *hdoc* es un entero.  
  
 [ *XmlText* ]  
 Es el documento XML original. El analizador MSXML analiza este documento XML. *XmlText* es un parámetro de texto **: char**, **nchar**, **VARCHAR**, **nvarchar**, **Text**, **ntext** o **XML**. El valor predeterminado es NULL, en cuyo caso se crea una representación interna de un documento XML vacío.  
  
> [!NOTE]  
>  **sp_xml_preparedocument** solo puede procesar texto o XML sin tipo. Si el valor de instancia que se va a utilizar como entrada ya es XML con tipo, primero conviértalo a una nueva instancia XML sin tipo o a una cadena y, a continuación, pase ese valor como entrada. Para obtener más información, vea [Comparar XML con tipo y XML sin tipo](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md).  
  
 [ *xpath_namespaces* ]  
 Especifica las declaraciones de espacio de nombres que se utilizan en las expresiones XPath de fila y columna de OPENXML. *xpath_namespaces* es un parámetro de texto **: char**, **nchar**, **VARCHAR**, **nvarchar**, **Text**, **ntext** o **XML**.  
  
 El valor predeterminado es **\<root xmlns:mp="urn:schemas-microsoft-com:xml-metaprop">** . *xpath_namespaces* proporciona los URI de espacio de nombres para los prefijos utilizados en las expresiones XPath de OPENXML mediante un documento XML con formato correcto. *xpath_namespaces* declara el prefijo que se debe usar para hacer referencia al espacio de nombres **urn: schemas-microsoft-com: XML-metaprop**; Esto proporciona metadatos sobre los elementos XML analizados. Aunque puede redefinir el prefijo del espacio de nombres para el espacio de nombres de metapropiedad mediante esta técnica, este espacio de nombres no se pierde. El **módulo de administración** de prefijo sigue siendo válido para **urn: schemas-microsoft-com: XML-metaprop** , incluso si *xpath_namespaces* no contiene tal declaración.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o >0 (error)  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** .  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-preparing-an-internal-representation-for-a-well-formed-xml-document"></a>A. Preparar una representación interna para un documento XML bien construido  
 En el siguiente ejemplo se devuelve un identificador para la representación interna que se acaba de crear del documento XML proporcionado como entrada. En la llamada a `sp_xml_preparedocument` se utiliza la asignación predeterminada del prefijo de espacio de nombres.  
  
```  
DECLARE @hdoc int;  
DECLARE @doc varchar(1000);  
SET @doc ='  
<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order CustomerID="VINET" EmployeeID="5" OrderDate="1996-07-04T00:00:00">  
      <OrderDetail OrderID="10248" ProductID="11" Quantity="12"/>  
      <OrderDetail OrderID="10248" ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order CustomerID="LILAS" EmployeeID="3" OrderDate="1996-08-16T00:00:00">  
      <OrderDetail OrderID="10283" ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>';  
--Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @hdoc OUTPUT, @doc;  
-- Remove the internal representation.  
exec sp_xml_removedocument @hdoc;  
```  
  
### <a name="b-preparing-an-internal-representation-for-a-well-formed-xml-document-with-a-dtd"></a>B. Preparar una representación interna de un documento XML bien construido con un DTD  
 En el siguiente ejemplo se devuelve un identificador para la representación interna que se acaba de crear del documento XML proporcionado como entrada. El procedimiento almacenado valida el documento cargado contra el DTD incluido en el documento. En la llamada a `sp_xml_preparedocument` se utiliza la asignación predeterminada del prefijo de espacio de nombres.  
  
```  
DECLARE @hdoc int;  
DECLARE @doc varchar(2000);  
SET @doc = '  
<?xml version="1.0" encoding="UTF-8" ?>   
<!DOCTYPE root   
[<!ELEMENT root (Customers)*>  
<!ELEMENT Customers EMPTY>  
<!ATTLIST Customers CustomerID CDATA #IMPLIED ContactName CDATA #IMPLIED>]>  
<root>  
<Customers CustomerID="ALFKI" ContactName="Maria Anders"/>  
</root>';  
  
EXEC sp_xml_preparedocument @hdoc OUTPUT, @doc;  
```  
  
### <a name="c-specifying-a-namespace-uri"></a>C. Especificar un URI de espacio de nombres  
 En el siguiente ejemplo se devuelve un identificador para la representación interna que se acaba de crear del documento XML proporcionado como entrada. La llamada a `sp_xml_preparedocument` conserva el `mp` prefijo en la asignación del espacio de nombres de la metapropiedad y agrega el `xyz` Prefijo de asignación al espacio de nombres `urn:MyNamespace` .  
  
```  
DECLARE @hdoc int;  
DECLARE @doc varchar(1000);  
SET @doc ='  
<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order CustomerID="VINET" EmployeeID="5"   
           OrderDate="1996-07-04T00:00:00">  
      <OrderDetail OrderID="10248" ProductID="11" Quantity="12"/>  
      <OrderDetail OrderID="10248" ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order CustomerID="LILAS" EmployeeID="3"   
           OrderDate="1996-08-16T00:00:00">  
      <OrderDetail OrderID="10283" ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
--Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @hdoc OUTPUT, @doc, '<ROOT xmlns:xyz="urn:MyNamespace"/>';  
```  
  
## <a name="see-also"></a>Consulte también  
 <br>[Procedimientos almacenados XML (Transact-SQL)](../../relational-databases/system-stored-procedures/xml-stored-procedures-transact-sql.md)
 <br>[Procedimientos almacenados del sistema (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)
 <br>[OPENXML (Transact-SQL)](../../t-sql/functions/openxml-transact-sql.md)
 <br>[sys.dm_exec_xml_handles (Transact-SQL)](../system-dynamic-management-views/sys-dm-exec-xml-handles-transact-sql.md)
 <br>[sp_xml_removedocument (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-xml-removedocument-transact-sql.md)
  
  
