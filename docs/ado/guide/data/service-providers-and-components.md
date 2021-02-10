---
description: Proveedores de servicios y componentes
title: Proveedores de servicios y componentes | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- OLE DB providers [ADO]
- ADO, OLE DB providers
- service providers [ADO]
ms.assetid: 1fd7a374-587b-4ca9-9204-3a4019b67a71
author: rothja
ms.author: jroth
ms.openlocfilehash: 11e105becc0d62e315b4d44de9d26c093bb27326
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037025"
---
# <a name="service-providers-and-components"></a>Proveedores de servicios y componentes
Los proveedores de servicios son componentes que amplían la funcionalidad de los proveedores de datos mediante la implementación de interfaces extendidas que no son compatibles de forma nativa con el almacén de datos.  
  
 Universal Data Access proporciona una *arquitectura de componentes* que permite a los componentes individuales y especializados implementar conjuntos discretos de funcionalidad de base de datos, o "servicios", sobre almacenes menos compatibles. Por lo tanto, en lugar de obligar a cada almacén de datos a proporcionar su propia implementación de funcionalidad extendida o obligar a las aplicaciones genéricas a implementar la funcionalidad de la base de datos internamente, los componentes de servicio proporcionan una implementación común que cualquier aplicación puede usar al obtener acceso a cualquier almacén de datos. El hecho de que alguna funcionalidad esté implementada de forma nativa por el almacén de datos y otros a través de componentes genéricos sea transparente para la aplicación.  
  
 Por ejemplo, un motor de cursor, como [el servicio de cursor para OLE DB](/previous-versions/windows/desktop/ms714397(v=vs.85)), es un componente de servicio que puede consumir datos de un almacén de datos secuencial de solo avance para generar datos desplazables. Entre otros proveedores de servicios usados normalmente por ADO se incluye el [proveedor de persistencia de microsoft OLE DB (proveedor de servicios ADO)](../../../ado/guide/appendixes/microsoft-ole-db-persistence-provider-ado-service-provider.md) (para guardar datos en un archivo), el [servicio de forma de datos de Microsoft para OLE DB (proveedor de servicios ADO)](../../../ado/guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md) (para los **conjuntos de registros** jerárquicos) y el [proveedor de comunicación remota de Microsoft OLE DB (proveedor de servicios de ADO)](../../../ado/guide/appendixes/microsoft-ole-db-remoting-provider-ado-service-provider.md) .  
  
 Para obtener más información sobre el servicio y los proveedores de datos, vea [Apéndice A: proveedores](../../../ado/guide/appendixes/appendix-a-providers.md).