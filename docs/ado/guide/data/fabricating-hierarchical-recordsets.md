---
description: Fabricación de conjuntos de registros jerárquicos
title: Fabricación de conjuntos de registros jerárquicos | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Recordset fabrication [ADO]
- hierarchical Recordsets [ADO]
- fabricating hierarchical Recordsets [ADO]
- data shaping [ADO], hierarchical Recordsets
ms.assetid: a584e642-a4a3-418e-bc20-3aff81a5625a
author: rothja
ms.author: jroth
ms.openlocfilehash: 836c1e0091166485f68c04226c632e9999038469
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033134"
---
# <a name="fabricating-hierarchical-recordsets"></a>Fabricación de conjuntos de registros jerárquicos
En el ejemplo siguiente se muestra cómo fabricar un conjunto de registros jerárquico sin un origen de datos subyacente mediante la gramática de forma de datos para definir columnas para los **conjuntos de registros** primarios, secundarios y terciarios.  
  
 Para fabricar un **conjunto de registros** jerárquico, debe especificar el [servicio de forma de datos de Microsoft para OLE DB (proveedor de servicios ADO)](../appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md) (MSDataShape) y puede especificar un valor de proveedor de datos de None en el parámetro de cadena de conexión del método [Open](../../reference/ado-api/open-method-ado-connection.md) del objeto [Connection](../../reference/ado-api/connection-object-ado.md) . Para obtener más información, vea [proveedores necesarios para la forma de datos](./required-providers-for-data-shaping.md).  
  
```  
Dim cn As New ADODB.Connection  
Dim rsCustomers As New ADODB.Recordset  
  
cn.Open "Provider=MSDataShape;Data Provider=NONE;"  
  
strShape = _  
"SHAPE APPEND NEW adInteger AS CustID," & _  
            " NEW adChar(25) AS FirstName," & _  
            " NEW adChar(25) AS LastName," & _  
            " NEW adChar(12) AS SSN," & _  
            " NEW adChar(50) AS Address," & _  
         " ((SHAPE APPEND NEW adChar(80) AS VIN_NO," & _  
                        " NEW adInteger AS CustID," & _  
                        " NEW adChar(20) AS BodyColor, " & _  
                     " ((SHAPE APPEND NEW adChar(80) AS VIN_NO," & _  
                                    " NEW adChar(20) AS Make, " & _  
                                    " NEW adChar(20) AS Model," & _  
                                    " NEW adChar(4) AS Year) " & _  
                        " AS VINS RELATE VIN_NO TO VIN_NO))" & _  
            " AS Vehicles RELATE CustID TO CustID) "  
  
rsCustomers.Open strShape, cn, adOpenStatic, adLockOptimistic, -1  
```  
  
 En cuanto se ha fabricado el **conjunto de registros** , se puede rellenar, manipular o guardar en un archivo.  
  
## <a name="see-also"></a>Consulte también  
 [Obtener acceso a las filas de un conjunto de registros jerárquico](./accessing-rows-in-a-hierarchical-recordset.md)   
 [Gramática de forma formal](./formal-shape-grammar.md)   
 [Proveedores necesarios para el modelado de datos](./required-providers-for-data-shaping.md)   
 [Cláusula APPEND de forma](./shape-append-clause.md)   
 [Comandos Shape en General](./shape-commands-in-general.md)