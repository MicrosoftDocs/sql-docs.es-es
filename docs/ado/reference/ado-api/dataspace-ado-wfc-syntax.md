---
description: DataSpace (ADO - sintaxis WFC)
title: DataSpace (ADO-sintaxis WFC) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- DataSpace collection [ADO], ADO/WFC syntax
ms.assetid: 950d45d8-07de-467b-b255-f9a7b997204c
author: rothja
ms.author: jroth
ms.openlocfilehash: 40207634f4111852bc280823826c108c49fde070
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034445"
---
# <a name="dataspace-ado---wfc-syntax"></a>DataSpace (ADO - sintaxis WFC)
El método **CreateObject** de la clase **DataSpace** especifica un objeto comercial para procesar las solicitudes de la aplicación cliente (*ProgID*) y el protocolo de comunicaciones y el servidor (*conexión*). **CreateObject** devuelve un objeto [ObjectProxy](../../../ado/reference/ado-api/objectproxy-ado-wfc-syntax.md) que representa el servidor.  
  
## <a name="package-commswfcdata"></a>paquete com. ms. wfc. Data  
  
### <a name="constructor"></a>Constructor  
  
```  
public DataSpace()  
```  
  
### <a name="methods"></a>Métodos  
  
```  
public static ObjectProxy DataSpace.createObject(String  
    progid, String connection)  
```  
  
### <a name="properties"></a>Propiedades  
  
```  
public static int getInternetTimeout()  
public static void setInternetTimeout(int plInetTimeout)  
```  
  
## <a name="see-also"></a>Consulte también  
 [Objeto DataSpace (RDS)](../../../ado/reference/rds-api/dataspace-object-rds.md)
