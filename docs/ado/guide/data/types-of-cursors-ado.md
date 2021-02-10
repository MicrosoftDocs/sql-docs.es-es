---
description: Tipos de cursores (ADO)
title: Tipos de cursores (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- cursors [ADO], types
ms.assetid: 7cc01544-e814-403b-bbfe-a2750bf921bd
author: rothja
ms.author: jroth
ms.openlocfilehash: c264cda3a42dd49848d4ad047f24d54102c60a3c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037825"
---
# <a name="types-of-cursors-ado"></a>Tipos de cursores (ADO)
Como norma general, la aplicación debe utilizar el cursor más sencillo que proporciona el acceso a datos requerido. Cada característica de cursor adicional más allá de los aspectos básicos (solo avance, solo lectura, estático, desplazable, sin almacenamiento en búfer) tiene un precio en la memoria de cliente, carga de red o rendimiento. En muchos casos, las opciones de cursor predeterminadas generan un cursor más complejo de lo que realmente necesita la aplicación.  
  
 La elección del tipo de cursor depende del modo en que la aplicación utiliza el conjunto de resultados y también en varias consideraciones de diseño, incluido el tamaño del conjunto de resultados, el porcentaje de los datos que se pueden usar, la sensibilidad a los cambios de datos y los requisitos de rendimiento de la aplicación.  
  
 En su nivel más básico, la elección del cursor depende de si necesita cambiar o simplemente ver los datos:  
  
-   Si solo necesita desplazarse por un conjunto de resultados, pero no los datos modificados, utilice un cursor [de solo avance](../../../ado/guide/data/forward-only-cursors.md) o [estático](../../../ado/guide/data/static-cursors.md) .  
  
-   Si tiene un conjunto de resultados grande y necesita seleccionar solo unas pocas filas, use un cursor de [conjunto de claves](../../../ado/guide/data/keyset-cursors.md) .  
  
-   Si desea sincronizar un conjunto de resultados con adiciones, cambios y eliminaciones recientes de todos los usuarios simultáneos, utilice un cursor [dinámico](../../../ado/guide/data/dynamic-cursors.md) .  
  
 Aunque parece que cada tipo de cursor es distinto, tenga en cuenta que estos tipos de cursor no son tantas clases, como el resultado de la superposición de características y opciones.  
  
 Esta sección contiene los temas siguientes.  
  
-   [Cursores de solo avance](../../../ado/guide/data/forward-only-cursors.md)  
  
-   [Cursores estáticos](../../../ado/guide/data/static-cursors.md)  
  
-   [Cursores KEYSET](../../../ado/guide/data/keyset-cursors.md)  
  
-   [Cursores dinámicos](../../../ado/guide/data/dynamic-cursors.md)  
  
## <a name="see-also"></a>Consulte también  
 [Cursores de solo avance](../../../ado/guide/data/forward-only-cursors.md)   
 [Cursores estáticos](../../../ado/guide/data/static-cursors.md)   
 [Cursores de conjunto de claves](../../../ado/guide/data/keyset-cursors.md)   
 [Cursores dinámicos](../../../ado/guide/data/dynamic-cursors.md)
