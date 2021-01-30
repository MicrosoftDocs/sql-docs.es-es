---
description: Propiedad de proveedor (ADO)
title: Provider (propiedad, ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::get_Provider
- Connection15::PutProvider
- Connection15::put_Provider
- Connection15::GetProvider
- Connection15::Provider
helpviewer_keywords:
- Provider property [ADO]
ms.assetid: 0ff70e72-0061-4ffc-90fb-e3ea23129bb2
author: rothja
ms.author: jroth
ms.openlocfilehash: f30bd886941909dd072e0eb5d5fafc9cff3b26dc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170497"
---
# <a name="provider-property-ado"></a>Propiedad de proveedor (ADO)
Indica el nombre del proveedor de un objeto de [conexión](./connection-object-ado.md) .  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 Establece o devuelve un valor de **cadena** que indica el nombre del proveedor.  
  
## <a name="remarks"></a>Observaciones  
 Utilice la propiedad **Provider** para establecer o devolver el nombre del proveedor de una conexión. Esta propiedad también se puede establecer mediante el contenido de la propiedad [ConnectionString](./connectionstring-property-ado.md) o el argumento *ConnectionString* del método [Open](./open-method-ado-connection.md) ; sin embargo, la especificación de un proveedor en más de un lugar mientras se llama al método **Open** puede tener resultados imprevisibles. Si no se especifica ningún proveedor, la propiedad se establecerá de forma predeterminada en MSDASQL ([proveedor de Microsoft OLE DB para ODBC](../../guide/appendixes/microsoft-ole-db-provider-for-odbc.md)).  
  
 La propiedad del **proveedor** es de lectura y escritura cuando la conexión está cerrada y es de solo lectura cuando está abierta. La configuración no surte efecto hasta que se abra el objeto de **conexión** o se tenga acceso a la colección de [propiedades](./properties-collection-ado.md) del objeto de **conexión** . Si el valor no es válido, se produce un error.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conexión (ADO)](./connection-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades Provider y DefaultDatabase (VB)](./provider-and-defaultdatabase-properties-example-vb.md)   
 [Ejemplo de las propiedades Provider y DefaultDatabase (VB)](./provider-and-defaultdatabase-properties-example-vb.md)   
 [Proveedor de OLE DB de Microsoft para ODBC](../../guide/appendixes/microsoft-ole-db-provider-for-odbc.md)   
 [Apéndice A: Proveedores](../../guide/appendixes/appendix-a-providers.md)