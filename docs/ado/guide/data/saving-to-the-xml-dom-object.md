---
description: Guardar datos en el objeto de DOM de XML
title: Guardando en el objeto DOM XML | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- XML DOM object [ADO], saving to
ms.assetid: 4d20fd28-aaf8-4232-83ce-f9d1e5f93dae
author: rothja
ms.author: jroth
ms.openlocfilehash: 790a53942722a8f425b48b6ef415d73e2f759374
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032517"
---
# <a name="saving-to-the-xml-dom-object"></a>Guardar datos en el objeto de DOM de XML
Puede guardar un conjunto de registros en formato XML en una instancia de un objeto DOM MSXML, tal como se muestra en el siguiente código de Visual Basic:  
  
```  
Dim xDOM As New MSXML.DOMDocument  
Dim rsXML As New ADODB.Recordset  
Dim sSQL As String, sConn As String  
  
sSQL = "SELECT customerid, companyname, contactname FROM customers"  
sConn="Provider=Microsoft.Jet.OLEDB.4.0;Data Source=D:\Program Files" & _  
        "\Common Files\System\msadc\samples\NWind.mdb"  
rsXML.Open sSQL, sConn  
rsXML.Save xDOM, adPersistADO   'Save Recordset directly into a DOM tree.  
...  
```  
  
## <a name="see-also"></a>Consulte también  
 [Almacenar registros en formato XML](../../../ado/guide/data/persisting-records-in-xml-format.md)
