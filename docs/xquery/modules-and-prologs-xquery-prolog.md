---
title: Prólogo de XQuery | Microsoft Docs
description: Obtenga información acerca del prólogo de XQuery que contiene una serie de declaraciones y definiciones que crean el entorno necesario para el procesamiento de consultas.
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- XQuery, prolog
- prolog
- namespaces [XQuery]
- default namespace declarations
ms.assetid: 03924684-c5fd-44dc-8d73-c6ab90f5e069
author: rothja
ms.author: jroth
ms.openlocfilehash: 2b460f6da448e45d688ebcc08f418a9acbae85b8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341844"
---
# <a name="modules-and-prologs---xquery-prolog"></a>Módulos y prólogos: prólogo de XQuery
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Una consulta XQuery se compone de un prólogo y un cuerpo. El prólogo de una consulta XQuery consiste en una serie de declaraciones y definiciones que, juntas, crean el entorno necesario para el procesamiento de la consulta. En SQL Server, el prólogo de una consulta XQuery puede incluir declaraciones de espacio de nombres. El cuerpo de una consulta XQuery está formado por una secuencia de expresiones que especifican el resultado previsto de la consulta.  
  
 Por ejemplo, la siguiente expresión XQuery se especifica en la columna instructions de tipo **XML** que almacena las instrucciones de fabricación como XML. La consulta recupera las instrucciones de fabricación para la ubicación del centro de trabajo `10`. El `query()` método del tipo de datos **XML** se utiliza para especificar la consulta XQuery.  
  
```  
SELECT Instructions.query('declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";           
    /AWMI:root/AWMI:Location[@LocationID=10]  
') AS Result   
FROM  Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Observe lo siguiente en la consulta anterior:  
  
-   El prólogo de XQuery incluye una declaración de prefijo de espacio de nombres (AWMI), `(namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";` .  
  
-   La palabra clave `declare namespace` define un prefijo de espacio de nombres que se utiliza posteriormente en el cuerpo de la consulta.  
  
-   `/AWMI:root/AWMI:Location[@LocationID="10"]` es el cuerpo de la consulta.  
  
## <a name="namespace-declarations"></a>Declaraciones de espacios de nombres  
 Una declaración de espacio de nombres define un prefijo y lo asocia a un URI de espacio de nombres, como se muestra en la consulta siguiente. En la consulta, `CatalogDescription` es una columna de tipo **XML** .  
  
 Al especificar una consulta XQuery en esta columna, el prólogo de la consulta incluye la declaración `declare namespace` para asociar el prefijo `PD` (descripción del producto) al URI de espacio de nombres. Después, este prefijo se utiliza en el cuerpo de la consulta en lugar del URI de espacio de nombres. Los nodos del XML resultante están en el espacio de nombres asociado al URI de espacio de nombres.  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
         /PD:ProductDescription/PD:Summary   
    ') as Result  
FROM Production.ProductModel  
where ProductModelID=19  
```  
  
 Para mejorar la legibilidad de la consulta, los espacios de nombres se pueden declarar con WITH XMLNAMESPACES en lugar de declarar enlaces de prefijos y espacios de nombres con `declare namespace` en el prólogo.  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS PD)  
  
SELECT CatalogDescription.query('  
         /PD:ProductDescription/PD:Summary   
    ') as Result  
FROM Production.ProductModel  
where ProductModelID=19  
```  
  
 Para obtener más información, vea [Agregar espacios de nombres a consultas con with XMLNAMESPACES](../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md).  
  
### <a name="default-namespace-declaration"></a>Declaración del espacio de nombres predeterminado  
 En lugar de declarar un prefijo de espacio de nombres mediante la declaración `declare namespace`, se puede utilizar la declaración `declare default element namespace` para enlazar un espacio de nombres predeterminado para nombres de elementos. En este caso, no se especifica ningún prefijo.  
  
 En el ejemplo siguiente, la expresión de ruta de acceso del cuerpo de la consulta no especifica ningún prefijo de espacio de nombres. De manera predeterminada, todos los nombres de elementos pertenecen al espacio de nombres predeterminado especificado en el prólogo.  
  
```  
SELECT CatalogDescription.query('  
     declare default element namespace  "https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
        /ProductDescription/Summary   
    ') as Result  
FROM  Production.ProductModel  
WHERE ProductModelID=19   
```  
  
 Para declarar un espacio de nombres predeterminado, se puede utilizar WITH XMLNAMESPACES:  
  
```  
WITH XMLNAMESPACES (DEFAULT 'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription')  
SELECT CatalogDescription.query('  
        /ProductDescription/Summary   
    ') as Result  
FROM  Production.ProductModel  
WHERE ProductModelID=19   
```  
  
## <a name="see-also"></a>Consulte también  
 [Agregar espacios de nombres a consultas con WITH XMLNAMESPACES](../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)  
  
  
