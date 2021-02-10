---
description: Transformaciones XSLT
title: Transformaciones XSLT | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- XSLT transformations in ADO
ms.assetid: 1a46196e-839f-4734-a59e-2c64609ffb9e
author: rothja
ms.author: jroth
ms.openlocfilehash: 75a29504d0d19300f8137959be150670f71e8b5b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032208"
---
# <a name="xslt-transformations"></a>Transformaciones XSLT
XSLT se puede aplicar al XML generado para transformarlo en otro formato. Entender el formato XML en ADO ayuda a desarrollar plantillas XSLT que pueden transformarlas en un formato más fácil de utilizar.  
  
 Por ejemplo, sabe que cada fila del conjunto de registros se guarda como el elemento z:Row dentro del elemento RS: Data. Del mismo modo, cada campo del conjunto de registros se guarda como un par atributo-valor para este elemento.  
  
## <a name="remarks"></a>Observaciones  
 El siguiente script XSLT se puede aplicar al XML que se muestra en la sección anterior para transformarlo en una tabla HTML que se mostrará en el explorador:  
  
```  
<?xml version="1.0" encoding="ISO-8859-1"?>  
<html xmlns:xsl="http://www.w3.org/TR/WD-xsl">  
<body STYLE="font-family:Arial, helvetica, sans-serif; font-size:12pt; background-color:white">  
<table border="1" style="table-layout:fixed" width="600">  
  <col width="200"></col>  
  <tr bgcolor="teal">  
    <th><font color="white">CustomerId</font></th>  
    <th><font color="white">CompanyName</font></th>  
    <th><font color="white">ContactName</font></th>  
  </tr>  
<xsl:for-each select="xml/rs:data/z:row">  
  <tr bgcolor="navy">  
    <td><font color="white"><xsl:value-of select="@CustomerID"/></font></td>  
    <td><font color="white"><xsl:value-of select="@CompanyName"/></font></td>  
    <td><font color="white"><xsl:value-of select="@ContactName"/></font></td>   
  </tr>  
</xsl:for-each>  
</table>  
</body>  
</html>  
```  
  
 El XSLT convierte la secuencia XML generada por el método Save de ADO en una tabla HTML que muestra cada campo del conjunto de registros junto con un encabezado de tabla. También se asignan fuentes y colores diferentes a los encabezados y las filas de la tabla.  
  
## <a name="see-also"></a>Consulte también  
 [Almacenar registros en formato XML](./persisting-records-in-xml-format.md)