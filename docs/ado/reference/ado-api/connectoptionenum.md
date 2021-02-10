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
ms.openlocfilehash: e159309a0fb1875851ff6ae0a7bf56ade908bafd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100026080"
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