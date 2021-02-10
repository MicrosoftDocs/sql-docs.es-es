---
description: Propiedad DataMember
title: Propiedad DataMember | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset20::DataMember
helpviewer_keywords:
- DataMember property
ms.assetid: 2c8fb09e-10ad-49b5-ab41-2603771780d9
author: rothja
ms.author: jroth
ms.openlocfilehash: 1026951122336e67760f74077044ff5fd9b9fd76
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034435"
---
# <a name="datamember-property"></a>Propiedad DataMember
Indica el nombre del miembro de datos que se recuperará del [conjunto de registros](../../../ado/reference/ado-api/recordset-object-ado.md) al que hace referencia la propiedad [DataSource](../../../ado/reference/ado-api/datasource-property-ado.md) .  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 Establece o devuelve un valor de **cadena** . El nombre no distingue entre mayúsculas y minúsculas.  
  
## <a name="remarks"></a>Observaciones  
 Esta propiedad se usa para crear controles enlazados a datos con el entorno de datos. El entorno de datos mantiene colecciones de datos (orígenes de datos) que contienen objetos con nombre (miembros de datos) que se representarán como un objeto de [conjunto de registros](../../../ado/reference/ado-api/recordset-object-ado.md) .  
  
 Las propiedades **DataMember** y **DataSource** deben usarse juntas.  
  
 La propiedad **DataMember** determina qué objeto especificado por la propiedad **DataSource** se representará como un objeto de **conjunto de registros** . El objeto de **conjunto de registros** debe cerrarse antes de que se establezca esta propiedad. Se genera un error si la propiedad **DataMember** no se establece antes que la propiedad **DataSource** , o si el objeto especificado en la propiedad **DataSource** no reconoce el nombre de **DataMember** .  
  
## <a name="usage"></a>Uso  
  
```  
Dim rs as New ADODB.Recordset  
rs.DataMember = "Command"     'Name of the rowset to bind to  
Set rs.DataSource = myDE      'Name of the object containing an IRowset  
```  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conjunto de registros (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Propiedad de origen de datos (ADO)](../../../ado/reference/ado-api/datasource-property-ado.md)
