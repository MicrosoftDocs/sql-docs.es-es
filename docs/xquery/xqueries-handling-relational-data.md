---
title: Consultas XQuery controlar datos relacionales | Microsoft Docs
description: 'Obtenga información sobre cómo enlazar datos relacionales no XML a XML mediante las extensiones XQuery SQL: column () y SQL: variable ().'
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- relational data [XQuery]
- XQuery, relational data
ms.assetid: 9812b71a-52ec-48a0-92f3-016a93660229
author: rothja
ms.author: jroth
ms.openlocfilehash: cadad1b4db6a0f0a6284ecb4beb6eadba9e59adc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344953"
---
# <a name="xqueries-handling-relational-data"></a>Funciones de XQuery para controlar datos relacionales
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Se especifica XQuery en una columna o variable de tipo **XML** mediante uno de los [métodos de tipo de datos XML](../t-sql/xml/xml-data-type-methods.md). Estos incluyen **query ()**, **Value ()**, **exist ()** o **Modify ()**. La función de XQuery se ejecuta en la instancia XML identificada en la consulta que genera el XML.  
  
 EL XML generado mediante la ejecución de una XQuery puede incluir valores recuperados de otras columnas de conjunto de filas o variable de Transact-SQL. Para enlazar datos relacionales no XML con el XML resultante, SQL Server proporciona las siguientes pseudofunciones como extensiones de XQuery:  
  
-   **SQL: column () (función)**  
  
-   **SQL: variable () (función)**  
  
 Puede utilizar estas extensiones XQuery al especificar una expresión XQuery en el método **query ()** del tipo de datos **XML** . Como resultado, el método **query ()** puede generar XML que combina datos de tipos de datos XML y no **XML** .  
  
 También puede usar estas funciones cuando utilice los métodos de  tipo de datos XML **Modify ()**, **Value ()**, **query ()** y **exist ()** para exponer un valor relacional dentro de XML.  
  
 Para obtener más información, vea [SQL: column () (función de XQuery)](../xquery/xquery-extension-functions-sql-column.md) y [SQL: variable () (función de XQuery)](../xquery/xquery-extension-functions-sql-variable.md).  
  
## <a name="see-also"></a>Consulte también  
 [Datos XML &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)   
 [Referencia del lenguaje XQuery &#40;SQL Server&#41;](../xquery/xquery-language-reference-sql-server.md)   
 [Construcción XML &#40;XQuery&#41;](../xquery/xml-construction-xquery.md)  
  
  
