---
description: ¿Qué es un Cursor?
title: ¿Qué es un Cursor? | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- cursors [ADO], about cursors
ms.assetid: 596eb4b6-c22f-4cde-b23f-172dd66c3161
author: rothja
ms.author: jroth
ms.openlocfilehash: 915d41f78de6c789c82bca3d729aa2c28ff26f27
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032267"
---
# <a name="what-is-a-cursor"></a>¿Qué es un Cursor?
Las operaciones de una base de datos relacional actúan en un conjunto completo de filas. El conjunto de filas que devuelve una instrucción SELECT está compuesto por todas las filas que satisfacen las condiciones de la cláusula WHERE de la instrucción. Este conjunto completo de filas que devuelve la instrucción se conoce como conjunto de resultados. Las aplicaciones, especialmente las que son interactivas y en línea, no funcionan siempre de forma eficaz con el conjunto de resultados completo como una unidad. Estas aplicaciones necesitan un mecanismo que trabaje con una fila o un pequeño bloque de filas cada vez. Los cursores son una extensión de los conjuntos de resultados que proporcionan dicho mecanismo.  
  
 Una biblioteca de cursores implementa un cursor. Una biblioteca de cursores es software, que a menudo se implementa como parte de un sistema de base de datos o una API de acceso a datos, que se utiliza para administrar los atributos de los datos devueltos de un origen de datos (un conjunto de resultados). Estos atributos incluyen la administración de simultaneidad, la posición en el conjunto de resultados, el número de filas devueltas y si puede moverse hacia delante o hacia atrás (o ambos) a través del conjunto de resultados (desplazamiento).  
  
 Un cursor realiza un seguimiento de la posición en el conjunto de resultados y permite realizar varias operaciones fila a fila en un conjunto de resultados, con o sin volver a la tabla original. En otras palabras, los cursores devuelven conceptualmente un conjunto de resultados basado en las tablas de las bases de datos. El cursor se denomina así porque indica la posición actual en el conjunto de resultados, al igual que el cursor en la pantalla del equipo indica la posición actual.  
  
 Es importante familiarizarse con el concepto de cursores antes de continuar para obtener información sobre los detalles de su uso en ADO.  
  
 Mediante cursores, puede:  
  
-   Especifique la posición en filas específicas del conjunto de resultados.  
  
-   Recupera una fila o un bloque de filas basándose en la posición del conjunto de resultados actual.  
  
-   Modificar los datos de las filas en la posición actual del conjunto de resultados.  
  
-   Defina diferentes niveles de sensibilidad a los cambios de datos realizados por otros usuarios.  
  
 Por ejemplo, imagine una aplicación que muestra una lista de productos disponibles para un comprador potencial. El comprador se desplaza por la lista para ver los detalles y el costo del producto y, por último, selecciona un producto para su compra. El desplazamiento y la selección adicionales se producen para el resto de la lista. En lo que respecta al comprador, los productos aparecen de uno en uno, pero la aplicación usa un cursor desplazable para examinar hacia arriba y hacia abajo por el conjunto de resultados.  
  
 Puede utilizar cursores de varias maneras:  
  
-   Sin filas.  
  
-   Con algunas o todas las filas de una sola tabla.  
  
-   Con algunas o todas las filas de las tablas combinadas lógicamente.  
  
-   Como de solo lectura o actualizable en el nivel de campo o cursor.  
  
-   Como de solo avance o totalmente desplazable.  
  
-   Con el conjunto de claves del cursor ubicado en el servidor.  
  
-   En función de los cambios de tabla subyacente causados por otras aplicaciones (como la pertenencia, la ordenación, las inserciones, las actualizaciones y las eliminaciones).  
  
-   Ya existe en el servidor o en el cliente.  
  
 Los cursores de solo lectura ayudan a los usuarios a desplazarse por el conjunto de resultados, mientras que los cursores de lectura/escritura pueden implementar actualizaciones de filas individuales. Se pueden definir cursores complejos con conjuntos que apunten a las filas de la tabla base. Aunque algunos cursores son de solo lectura en una dirección hacia delante, otros pueden desplazarse y proporcionar una actualización dinámica del conjunto de resultados basándose en los cambios que realizan otras aplicaciones en la base de datos.  
  
 No todas las aplicaciones necesitan usar cursores para obtener acceso a los datos o actualizarlos. Algunas consultas simplemente no requieren la actualización directa de filas mediante un cursor. Los cursores deben ser una de las últimas técnicas que elija para recuperar datos y, a continuación, debe elegir el cursor de impacto más bajo posible. Cuando se crea un conjunto de resultados mediante un procedimiento almacenado, el conjunto de resultados no se puede actualizar mediante métodos de edición o actualización de cursores.  
  
## <a name="concurrency"></a>Simultaneidad  
 En algunas aplicaciones multiusuario es muy importante que los datos presentados al usuario final sean lo más actuales posible. Un ejemplo clásico de este tipo de sistema es un sistema de reserva de una línea aérea, en el que muchos usuarios pueden estar contratando el mismo puesto en un vuelo determinado (y, por lo tanto, un único registro). En un caso como este, el diseño de la aplicación debe controlar las operaciones simultáneas en un único registro.  
  
 En otras aplicaciones, la simultaneidad no es tan importante. En tales casos, no se puede justificar el gasto implicado en mantener los datos actualizados en todo momento.  
  
## <a name="position"></a>Posición  
 Un cursor también realiza un seguimiento de la posición actual en un conjunto de resultados. Considere la posición del cursor como un puntero al registro actual, de manera similar a la forma en que un índice de matriz señala el valor en esa ubicación concreta de la matriz.  
  
## <a name="scrollability"></a>Desplazamiento  
 El tipo de cursor empleado por la aplicación también afecta a la capacidad de moverse hacia delante y hacia atrás por las filas de un conjunto de resultados. a veces, esto se conoce como desplazamiento. La capacidad de avanzar *y* retroceder por un conjunto de resultados se agrega a la complejidad del cursor y, por tanto, es más costosa de implementar. Por esta razón, debe solicitar un cursor con esta funcionalidad solo cuando sea necesario.
