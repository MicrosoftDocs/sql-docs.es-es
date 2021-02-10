---
description: Cursores KEYSET
title: Cursores de conjunto de claves | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Keyset cursors [ADO]
- cursors [ADO], Keyset
ms.assetid: 14b51b17-6fd9-4146-af45-ca4b0fe6d48a
author: rothja
ms.author: jroth
ms.openlocfilehash: 4071772212226b2b09886476c6577cac73cd3f79
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037275"
---
# <a name="keyset-cursors"></a>Cursores KEYSET
El cursor de conjunto de claves proporciona funcionalidad entre un cursor estático y dinámico en su capacidad para detectar cambios. Al igual que un cursor estático, no siempre detecta los cambios realizados en la pertenencia y el orden del conjunto de resultados. Como un cursor dinámico, detecta cambios en los valores de las filas del conjunto de resultados.  
  
 Los cursores controlados por conjunto de claves se supervisan mediante un conjunto de identificadores únicos (claves) denominado conjunto de claves. Las claves se generan a partir de un conjunto de columnas que identifican las filas del conjunto de resultados de forma unívoca. El conjunto de claves es el conjunto de valores de clave de todas las filas devueltas por la instrucción de consulta.  
  
 Con los cursores controlados por conjunto de claves, se crea y se guarda una clave para cada fila del cursor y se almacena en la estación de trabajo cliente o en el servidor. Al acceder a cada fila, se usa la clave almacenada para capturar los valores de datos actuales desde el origen de datos. En un cursor controlado por conjunto de claves, la pertenencia al conjunto de resultados se inmoviliza cuando se completa totalmente el conjunto de claves. Por tanto, las adiciones o actualizaciones que afectan a la pertenencia no forman parte del conjunto de resultados hasta que se vuelva a abrir.  
  
 Los cambios en los valores de datos (realizados por el propietario del conjunto de claves u otros procesos) están visibles cuando el usuario se desplaza por el conjunto de resultados. Las operaciones de inserción realizadas fuera del cursor (por otros procesos) son visibles solo si el cursor se cierra y se vuelve a abrir. Las operaciones de inserción realizadas desde dentro del cursor son visibles al final del conjunto de resultados.  
  
 Cuando un cursor controlado por conjunto de claves intenta recuperar una fila que se ha eliminado, la fila aparece como un "agujero" en el conjunto de resultados. La clave para la fila existe en el conjunto de claves, pero la fila ya no existe en el conjunto de resultados. Si se actualizan los valores de clave de una fila, se considera que la fila se ha eliminado y se ha insertado, por lo que dichas filas también aparecen como huecos en el conjunto de resultados. Aunque un cursor controlado por conjunto de claves siempre puede detectar filas eliminadas por otros procesos, puede quitar opcionalmente las claves de las filas que elimina. Los cursores controlados por conjunto de claves que lo hacen no pueden detectar sus propias eliminaciones porque se ha quitado la evidencia.  
  
 Una actualización de una columna de clave funciona como una eliminación de la clave antigua seguida de una inserción de la nueva clave. El nuevo valor de clave no es visible si la actualización no se realizó a través del cursor. Si la actualización se realizó a través del cursor, el nuevo valor de clave es visible al final del conjunto de resultados.  
  
 Hay una variación en los cursores controlados por conjunto de claves denominados cursores estándar controlados por conjunto de claves. En un cursor estándar controlado por conjunto de claves, la pertenencia de las filas del conjunto de resultados y el orden de las filas se fijan en el tiempo de apertura del cursor, pero se ven los cambios en los valores realizados por el propietario del cursor y los cambios confirmados realizados por otros procesos. Si un cambio descalifica una fila para la pertenencia o afecta al orden de una fila, la fila no desaparece ni se mueve a menos que el cursor se cierre y se vuelva a abrir. Los datos insertados no aparecen, pero los cambios realizados en los datos existentes aparecen a medida que se capturan las filas.  
  
 El cursor controlado por conjunto de claves es difícil de usar correctamente porque la sensibilidad a los cambios en los datos depende de muchas circunstancias diferentes, como se ha descrito anteriormente. Sin embargo, si la aplicación no está relacionada con las actualizaciones simultáneas, puede administrar mediante programación las claves no válidas y debe tener acceso directamente a determinadas filas con clave, el cursor controlado por conjunto de claves podría funcionar en su caso. Use el **AdOpenKeyset CursorTypeEnum** para indicar que desea utilizar un cursor de conjunto de claves en ADO.  
  
## <a name="see-also"></a>Consulte también  
 [Cursores de solo avance](./forward-only-cursors.md)   
 [Cursores estáticos](./static-cursors.md)   
 [Cursores dinámicos](./dynamic-cursors.md)