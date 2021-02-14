---
title: Comentarios en XQuery | Microsoft Docs
description: Obtenga información sobre la sintaxis y los delimitadores para agregar comentarios a una consulta XQuery.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- comments [XQuery]
- XQuery, comments
ms.assetid: 4d977268-de9d-4bf0-b310-b63f6a0fb0db
author: rothja
ms.author: jroth
ms.openlocfilehash: 9c6e7e6154ad3f91f0c45fb5a2bdfb090a810137
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345730"
---
# <a name="comments-in-xquery"></a>Comentarios en XQuery
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Se pueden agregar comentarios a XQuery. Las cadenas de comentarios se agregan utilizando los delimitadores "`(:`" y "`:)`". Por ejemplo:  
  
```  
declare @x xml  
set @x=''  
SELECT @x.query('  
(: simple query to construct an element :)  
<ProductModel ProductModelID="10" />  
')  
```  
  
 A continuación se indica otro ejemplo en el que se especifica una consulta en una columna de instrucciones del tipo **XML** :  
  
```  
SELECT Instructions.query('  
(: declare prefix and namespace binding in the prolog. :)  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
  (: Following expression retrieves the <Location> element children of the <root> element. :)  
  /AWMI:root/AWMI:Location  
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
  
