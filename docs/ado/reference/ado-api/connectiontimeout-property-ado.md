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
ms.openlocfilehash: 266d57defde6abaae34e558496092ec2df7caebb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164584"
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