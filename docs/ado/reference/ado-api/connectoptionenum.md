---
description: ConnectOptionEnum
title: ConnectOptionEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ConnectOptionEnum
helpviewer_keywords:
- ConnectOptionEnum enumeration [ADO]
ms.assetid: bff07eeb-dee3-4e4e-9b2d-d56061ea744d
author: rothja
ms.author: jroth
ms.openlocfilehash: 257dd82d5740c89fd8ca9f8f8d1969d27b9301ba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171426"
---
# <a name="connectoptionenum"></a>ConnectOptionEnum
Especifica si el método [Open](./open-method-ado-connection.md) de un objeto [Connection](./connection-object-ado.md) debe devolver una vez establecida la conexión (sincrónicamente) o antes de (de forma asincrónica).  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|**adAsyncConnect**|16|Abre la conexión de forma asincrónica. El evento [ConnectComplete](./connectcomplete-and-disconnect-events-ado.md) se puede usar para determinar si la conexión está disponible.|  
|**adConnectUnspecified**|-1|Predeterminada. Abre la conexión sincrónicamente.|  
  
## <a name="adowfc-equivalent"></a>Equivalente de ADO/WFC  
 Paquete: **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums. ConnectOption. ASYNCCONNECT|  
|AdoEnums. ConnectOption. CONNECTUNSPECIFIED|  
  
## <a name="applies-to"></a>Se aplica a  
 [Open (método) (conexión de ADO)](./open-method-ado-connection.md)