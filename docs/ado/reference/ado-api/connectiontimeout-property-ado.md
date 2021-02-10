---
description: Propiedad ConnectionTimeout (ADO)
title: Propiedad ConnectionTimeout (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::ConnectionTimeout
helpviewer_keywords:
- ConnectionTimeout property [ADO]
ms.assetid: 8904a403-1383-4b4b-b53d-5c01d6f5deac
author: rothja
ms.author: jroth
ms.openlocfilehash: 6cd14e4cdaba28b0591b4fa324e01efbe9a2ae5e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100026411"
---
# <a name="connectiontimeout-property-ado"></a>Propiedad ConnectionTimeout (ADO)
Indica cuánto tiempo se debe esperar mientras se establece una conexión antes de terminar el intento y generar un error.  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 Establece o devuelve un valor **Long** que indica, en segundos, cuánto tiempo se debe esperar para que se abra la conexión. El valor predeterminado es 15.  
  
## <a name="remarks"></a>Observaciones  
 Utilice la propiedad **ConnectionTimeout** en un objeto de [conexión](./connection-object-ado.md) si los retrasos del tráfico de red o de un uso intensivo del servidor hacen que sea necesario abandonar un intento de conexión. Si la hora del valor de la propiedad **ConnectionTimeout** transcurre antes de la apertura de la conexión, se produce un error y ADO cancela el intento. Si establece la propiedad en cero, ADO esperará indefinidamente hasta que se abra la conexión. Asegúrese de que el proveedor en el que está escribiendo código admite la funcionalidad **ConnectionTimeout** .  
  
 La propiedad **ConnectionTimeout** es de lectura y escritura cuando se cierra la conexión y de solo lectura cuando está abierta.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conexión (ADO)](./connection-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades ConnectionString, ConnectionTimeout y State (VB)](./connectionstring-connectiontimeout-and-state-properties-example-vb.md)   
 [Ejemplo de las propiedades ConnectionString, ConnectionTimeout y State (VC + +)](./connectionstring-connectiontimeout-and-state-properties-example-vc.md)   
 [CommandTimeout (propiedad, ADO)](./commandtimeout-property-ado.md)