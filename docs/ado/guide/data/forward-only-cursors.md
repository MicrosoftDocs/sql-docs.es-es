---
description: Cursores de solo avance
title: Forward-Only cursores | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- cursors [ADO], forward-only
- forward-only cursors [ADO]
ms.assetid: 2b1e062f-3294-4a6f-8241-a17045c4df18
author: rothja
ms.author: jroth
ms.openlocfilehash: 0ac51891368a5551c29190de959f8a9559d03b4e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033145"
---
# <a name="forward-only-cursors"></a>Cursores de solo avance
El tipo de cursor predeterminado típico, denominado cursor de solo avance (o no desplazable), solo puede avanzar por el conjunto de resultados. Un cursor de solo avance no admite el desplazamiento (la capacidad de moverse hacia delante y hacia atrás en el conjunto de resultados); solo admite la captura de filas desde el principio hasta el final del conjunto de resultados. Con algunos cursores de solo avance (por ejemplo, con la biblioteca de cursores SQL Server), todas las instrucciones INSERT, Update y DELETE realizadas por el usuario actual (o confirmadas por otros usuarios) que afectan a las filas del conjunto de resultados son visibles a medida que se capturan las filas. Pero como el cursor no se puede desplazar hacia atrás, los cambios realizados en las filas de la base de datos tras capturar la fila no son visibles a través del cursor.  
  
 Una vez procesados los datos de la fila actual, el cursor de solo avance libera los recursos que se utilizaron para contener dichos datos. Los cursores de solo avance son dinámicos de forma predeterminada, lo que significa que todos los cambios se detectan cuando se procesa la fila actual. Esto proporciona una apertura del cursor más rápida y permite que el conjunto de resultados muestre las actualizaciones realizadas en las tablas subyacentes.  
  
 Aunque los cursores de solo avance no admiten el desplazamiento hacia atrás, la aplicación puede volver al principio del conjunto de resultados cerrando y volviendo a abrir el cursor. Se trata de una forma eficaz de trabajar con pequeñas cantidades de datos. Como alternativa, la aplicación puede leer el conjunto de resultados una vez, almacenar los datos en caché localmente y, a continuación, examinar la memoria caché de datos local.  
  
 Si la aplicación no requiere desplazarse por el conjunto de resultados, el cursor de solo avance es la mejor manera de recuperar los datos rápidamente con la menor cantidad de sobrecarga. Use el **AdOpenForwardOnly CursorTypeEnum** para indicar que desea usar un cursor de solo avance en ADO.  
  
## <a name="see-also"></a>Consulte también  
 [Cursores estáticos](./static-cursors.md)   
 [Cursores de conjunto de claves](./keyset-cursors.md)   
 [Cursores dinámicos](./dynamic-cursors.md)