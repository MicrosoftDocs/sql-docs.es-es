---
description: Cursores estáticos
title: Cursores estáticos | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- cursors [ADO], static
- static cursors [ADO]
ms.assetid: cce93ace-c4ed-4c6c-940c-28a50ff2fd12
author: rothja
ms.author: jroth
ms.openlocfilehash: a6b6ef12f2da2416e5f2c3c6e2e6c0c94e2c5327
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036985"
---
# <a name="static-cursors"></a>Cursores estáticos
El cursor estático siempre muestra el conjunto de resultados tal como estaba al abrir el cursor por primera vez. En función de la implementación, los cursores estáticos son de solo lectura o de lectura/escritura y proporcionan un desplazamiento hacia delante y hacia atrás. Normalmente, el cursor estático no detecta los cambios realizados en la pertenencia, el orden o los valores del conjunto de resultados una vez abierto el cursor. Los cursores estáticos pueden detectar sus propias operaciones de inserción, actualización y eliminación, aunque no está obligados a hacerlo.  
  
 Los cursores estáticos nunca detectan otras actualizaciones, eliminaciones e inserciones. Por ejemplo, suponga que un cursor estático captura una fila y, después, otra aplicación la actualiza. Si la aplicación vuelve a capturar la fila del cursor estático, los valores que ve son iguales, a pesar de los cambios realizados por la otra aplicación. Se admiten todos los tipos de desplazamiento, pero los proveedores pueden admitir o no marcadores.  
  
 Si la aplicación no necesita detectar los cambios de datos y requiere desplazamiento, el cursor estático es la mejor opción. Use la variable **AdOpenStatic CursorTypeEnum** para indicar que desea utilizar un cursor estático en ADO.  
  
## <a name="see-also"></a>Consulte también  
 [Cursores de solo avance](../../../ado/guide/data/forward-only-cursors.md)   
 [Cursores de conjunto de claves](../../../ado/guide/data/keyset-cursors.md)   
 [Cursores dinámicos](../../../ado/guide/data/dynamic-cursors.md)
