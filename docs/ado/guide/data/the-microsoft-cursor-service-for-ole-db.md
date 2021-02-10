---
description: El servicio de cursores de Microsoft para OLE DB
title: El servicio de cursores de Microsoft para OLE DB | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- cursor service for ole db [ADO]
- cursors [ADO], cursor service for OLE DB
ms.assetid: 1ac3bd9b-2d45-4cc8-88ec-bd8a218cfb49
author: rothja
ms.author: jroth
ms.openlocfilehash: c2a785ba338164392f1364166365df31b2bd6dfd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036955"
---
# <a name="the-microsoft-cursor-service-for-ole-db"></a>El servicio de cursores de Microsoft para OLE DB
Cuando se selecciona un cursor del lado cliente o se establece la propiedad **CursorLocation** en **adUseClient**, se invoca el servicio de cursores de Microsoft para OLE DB. También puede ver referencias al "motor de cursor de cliente", que básicamente es lo mismo en el contexto de ADO. Este servicio complementa las funciones de compatibilidad de cursores de los proveedores de datos. Como resultado, puede percibir funcionalidad relativamente uniforme de todos los proveedores de datos.  
  
 El servicio de cursor para OLE DB hace que las propiedades dinámicas estén disponibles y mejora el comportamiento de ciertos métodos. Por ejemplo, la propiedad dinámica **Optimize** permite la creación de índices temporales para facilitar ciertas operaciones, como el método **Find** .  
  
 El servicio de cursor habilita la compatibilidad con la actualización por lotes en todos los casos. También simula tipos de cursor más compatibles, como los cursores dinámicos, cuando un proveedor de datos solo puede proporcionar cursores menos compatibles, como cursores estáticos.  
  
## <a name="see-also"></a>Consulte también  
 [Microsoft cursor Service para OLE DB (componente de servicio ADO)](../../../ado/guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md)
