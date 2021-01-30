---
description: Open (método) (ADO MD)
title: Método Open (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Open
- Cellset::Open
helpviewer_keywords:
- Open method [ADO MD]
ms.assetid: a87d8080-a238-45e5-bc80-9a8625b3810f
author: rothja
ms.author: jroth
ms.openlocfilehash: 75970571be282042338ab7a5eb870a725af5e201
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169745"
---
# <a name="open-method-ado-md"></a>Open (método) (ADO MD)
Recupera los resultados de una consulta multidimensional y devuelve los resultados a un [Cellset](./cellset-object-ado-md.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Cellset.Open Source, ActiveConnection  
```  
  
#### <a name="parameters"></a>Parámetros  
 *Origen*  
 Opcional. Una **variante** que se evalúa como una consulta multidimensional válida, como una consulta de expresión multidimensional (MDX). El argumento de *origen* corresponde a la propiedad de [origen](./source-property-ado-md.md) . Para obtener más información acerca de MDX, consulte la documentación del [OLE DB para el procesamiento analítico en línea (OLAP)](/previous-versions/windows/desktop/ms717005(v=vs.85)) en el SDK de Microsoft Data Access Components.  
  
 *ActiveConnection*  
 Opcional. Una **variante** que se evalúa como una cadena que especifica un nombre de variable de objeto de [conexión](../ado-api/connection-object-ado.md) ADO válido o una definición para una conexión. El argumento *ActiveConnection* especifica la conexión en la que se va a abrir el objeto [Cellset](./cellset-object-ado-md.md) . Si pasa una definición de conexión para este argumento, ADO abrirá una nueva conexión con los parámetros especificados. El argumento *ActiveConnection* corresponde a la propiedad [ActiveConnection](./activeconnection-property-ado-md.md) .  
  
## <a name="remarks"></a>Observaciones  
 El método **Open** genera un error si se omite cualquiera de sus parámetros y el valor de propiedad correspondiente no se ha establecido antes de intentar abrir el conjunto de **celdas**.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conjunto de celdas (ADO MD)](./cellset-object-ado-md.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de Cellset (VB)](./cellset-example-vb.md)   
 [Propiedad ActiveConnection (ADO MD)](./activeconnection-property-ado-md.md)   
 [Método Close (ADO MD)](./close-method-ado-md.md)   
 [Connection (objeto) (ADO)](../ado-api/connection-object-ado.md)   
 [Propiedad Source (ADO MD)](./source-property-ado-md.md)   
 [State (propiedad) (ADO MD)](./state-property-ado-md.md)