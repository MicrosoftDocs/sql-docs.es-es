---
description: Montones (tablas sin índices clúster)
title: Montones (tablas sin índices agrupados) | Microsoft Docs
ms.custom: ''
ms.date: 11/01/2016
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- heaps
- forward record
- forwarded record
- forwarding pointer
- RID
ms.assetid: df5c4dfb-d372-4d0f-859a-a2d2533ee0d7
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: be2a84e21cc55340d675041cdd353dab887531c2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465296"
---
# <a name="heaps-tables-without-clustered-indexes"></a>Montones (tablas sin índices clúster)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Un montón es una tabla que no tiene un índice clúster. En las tablas almacenadas, se pueden crear uno o varios índices no clúster como un montón. Los datos se almacenan en el montón sin especificar un orden. Normalmente, en un principio los datos se almacenan en el orden en el que las filas se insertan en la tabla, pero [!INCLUDE[ssDE](../../includes/ssde-md.md)] puede mover los datos en el montón para almacenar las filas de forma eficaz, de modo que no se puede predecir el orden de los datos. Para garantizar el orden de las filas que se devuelven de un montón, debe usar la cláusula `ORDER BY`. Para especificar un orden lógico permanente con el fin de almacenar las filas, cree un índice agrupado en la tabla, de modo que la tabla no sea un montón.  
  
> [!NOTE]  
> A veces, hay buenos motivos para dejar una tabla como montón en lugar de crear un índice clúster, pero para usar los montones de forma eficaz se requieren conocimientos avanzados. La mayoría de las tablas deben tener un índice clúster cuidadosamente elegido a menos que exista un buen motivo para dejar la tabla como un montón.  
  
## <a name="when-to-use-a-heap"></a>Cuándo se usa un montón  
Cuando una tabla se almacena como montón, las filas individuales se identifican mediante una referencia a un identificador de fila (RID) de 8 bytes formado por el número de archivo, el número de páginas de datos y la ranura de la página (FileID:PageID:SlotID). El identificador de fila es una estructura pequeña y eficaz. 

Los montones se pueden usar como tablas de almacenamiento provisional para operaciones de inserción de gran tamaño y sin ordenar. Dado que los datos se insertan sin aplicar un orden estricto, la operación de inserción suele ser más rápida que la inserción equivalente en un índice agrupado. Si los datos del montón se leen y procesan en un destino final, puede ser útil crear un índice no agrupado estrecho que abarque el predicado de búsqueda que usa la consulta de lectura. 

> [!NOTE]  
> Los datos se recuperan de un montón en orden de páginas de datos, pero no necesariamente en el orden en el que se han insertado. 

A veces, los profesionales de datos también usan montones cuando el acceso a los datos se realiza siempre a través de índices no agrupados y el RID es menor que una clave de índice agrupado. 

Si una tabla es un montón y no tiene ningún índice agrupado, debe leerse la tabla completa (recorrido de tabla) cuando se busca una fila. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede buscar un RID directamente en el montón. Esto puede ser aceptable si la tabla es pequeña.  
  
## <a name="when-not-to-use-a-heap"></a>Cuándo no se usa un montón  
 No use un montón cuando los datos se devuelvan con frecuencia ordenados. Un índice clúster en la columna de ordenación podría impedir la operación de ordenación.  
  
 No use un montón cuando los datos se agrupan juntos a menudo. Los datos deben estar ordenados antes de que se agrupen y un índice clúster en la columna de ordenación podría impedir la operación de ordenación.  
  
 No use un montón cuando a menudo se consulten rangos de datos de la tabla. Un índice clúster en la columna de rango impedirá que se ordene el montón completo.  
  
 No use un montón cuando no haya índices no agrupados y la tabla sea grande, a menos que vaya a devolver el contenido completo de la tabla sin ningún orden especificado. En un montón, deben leerse todas las filas del montón para buscar una fila.  
 
 No use un montón si los datos se actualizan con frecuencia. Si actualiza un registro y la actualización usa más espacio en las páginas de datos del que se usa actualmente, el registro tiene que moverse a una página de datos que tenga suficiente espacio libre. Esto crea un **registro reenviado** que apunta a la nueva ubicación de los datos y hay que escribir el **puntero de reenvío** en la página que contenía los datos anteriormente para indicar la nueva ubicación física. Esto introduce una fragmentación en el montón. Al examinar un montón, se deben seguir estos punteros, lo que limita el rendimiento de lectura anticipada y puede generar operaciones adicionales de E/S que reduzcan el rendimiento del examen. 
  
## <a name="managing-heaps"></a>Administrar montones  
 Para crear un montón, cree una tabla sin un índice clúster. Si la tabla ya tiene un índice clúster, quite el índice clúster para convertir de nuevo la tabla en un montón.  
  
 Para quitar un montón, cree un índice clúster en el montón.  
  
 Para volver a crear un montón a fin de recuperar el espacio desaprovechado:
 -  Cree un índice agrupado en el montón y, después, quítelo.  
 -  Use el comando `ALTER TABLE ... REBUILD` para volver a crear el montón.
  
> [!WARNING]  
> Para crear o quitar índices clúster, es necesario volver a escribir toda la tabla. Si la tabla tiene índices no clúster, todos los índices no clúster deberán volver a crearse cada vez que cambie el índice clúster. Por tanto, al convertir un montón en una estructura de índice agrupado o viceversa, puede necesitarse mucho tiempo y gran cantidad de espacio para reordenar los datos en tempdb.  

## <a name="heap-structures"></a>Estructuras de montón
Un montón es una tabla que no tiene un índice clúster. Los montones tienen una fila en [sys.partitions](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md), con `index_id = 0` para cada partición que usa el montón. De forma predeterminada, un montón contiene una sola partición. Cuando un montón tiene varias particiones, cada partición incluye una estructure de montón que contiene los datos de esa partición específica. Por ejemplo, si un montón tiene cuatro particiones, existirán cuatro estructuras de montón, una en cada partición.

Dependiendo de los tipos de datos incluidos en el montón, cada estructura de montón tendrá una o más unidades de asignación para almacenar y administrar los datos de una partición específica. Como mínimo, cada montón dispondrá de una unidad de asignación `IN_ROW_DATA` por partición. El montón también dispondrá de una unidad de asignación `LOB_DATA` por partición si contiene columnas de objeto grande (LOB). También tendrá una unidad de asignación `ROW_OVERFLOW_DATA` por partición si contiene columnas de longitud variable que superen el límite de tamaño de fila de 8060 bytes.

La columna `first_iam_page` en los puntos de vista del sistema `sys.system_internals_allocation_units` para la primera página IAM de la cadena de páginas IAM que administra el espacio asignado al montón en una partición específica. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa las páginas IAM para desplazarse por el montón. Las páginas de datos y las filas que se encuentran en ellas no están en ningún orden concreto y no están vinculadas. La única conexión lógica entre las páginas de datos es la información registrada en las páginas IAM.

> [!IMPORTANT]  
> La vista del sistema `sys.system_internals_allocation_units` solo se reserva para el uso interno de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La compatibilidad con versiones posteriores no está garantizada.
 
Los recorridos de tablas o las lecturas secuenciales de un montón se hacen recorriendo las páginas IAM para buscar las extensiones que almacenan las páginas de dicho montón. Como la IAM representa las extensiones en el mismo orden en el que se encuentran en los archivos de datos, ello significa que los recorridos secuenciales de un montón recorren secuencialmente cada archivo. Utilizar las páginas IAM para establecer la secuencia de recorrido también significa que las filas del montón no se devuelven normalmente en el orden en que se introdujeron.

La siguiente ilustración muestra cómo [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] utiliza las páginas IAM para recuperar las filas de datos en un solo montón con particiones. 

![iam_heap](../../relational-databases/indexes/media/iam-heap.gif)
  
## <a name="related-content"></a>Contenido relacionado  
[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)     
[DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md)     
[Índices agrupados y no agrupados descritos](../../relational-databases/indexes/clustered-and-nonclustered-indexes-described.md)     
  
  
