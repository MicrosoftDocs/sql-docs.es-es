---
description: Objeto de secuencia (ADO)
title: Stream (objeto) (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Stream
helpviewer_keywords:
- Stream object [ADO]
ms.assetid: 0514531f-009d-4519-abc3-d727014a39f1
author: rothja
ms.author: jroth
ms.openlocfilehash: 6d18917bdcc0a08c5fbddcc1ff908c5aed8ca1f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170170"
---
# <a name="stream-object-ado"></a>Objeto de secuencia (ADO)
Representa un flujo de datos binarios o de texto.  
  
 En las jerarquías estructuradas por árbol, como un sistema de archivos o un sistema de correo electrónico, un [registro](./record-object-ado.md) puede tener una secuencia binaria predeterminada de bits asociada a él que incluye el contenido del archivo o el correo electrónico. Un objeto de **secuencia** se puede utilizar para manipular los campos o registros que contienen estos flujos de datos. Un objeto de **secuencia** se puede obtener de estas maneras:  
  
-   Desde una dirección URL que apunta a un objeto (normalmente un archivo) que contiene datos binarios o de texto. Este objeto puede ser un documento simple, un objeto de **registro** que representa un documento estructurado o una carpeta.  
  
-   Abriendo el objeto de **secuencia** predeterminado asociado a un objeto de **registro** . Puede obtener el flujo predeterminado asociado a un objeto de **registro** cuando se abre el **registro** , para eliminar un recorrido de ida y vuelta solo para abrir la secuencia.  
  
-   Creando una instancia de un objeto de **secuencia** . Estos objetos **Stream** se pueden usar para almacenar datos para los fines de la aplicación. A diferencia de un **flujo** asociado a una dirección URL, o la **secuencia** predeterminada de un **registro**, una **secuencia** con instancias no tiene ninguna asociación con un origen subyacente de forma predeterminada.  
  
 Con los métodos y las propiedades de un objeto de **secuencia** , puede hacer lo siguiente:  
  
-   Abra un objeto de **secuencia** desde un **registro** o una dirección URL con el método [Open](./open-method-ado-stream.md) .  
  
-   Cierre una **secuencia** con el método [Close](./close-method-ado.md) .  
  
-   Escriba bytes o texto en una **secuencia** con los métodos de [escritura](./write-method.md) y [WRITETEXT](./writetext-method.md) .  
  
-   Leer bytes de la **secuencia** con los métodos [Read](./read-method.md) y [READTEXT](./readtext-method.md) .  
  
-   Escriba los datos de **secuencia** que todavía estén en el búfer de ADO en el objeto subyacente con el método [Flush](./flush-method-ado.md) .  
  
-   Copie el contenido de una **secuencia** en otra **secuencia** con el método [CopyTo](./copyto-method-ado.md) .  
  
-   Controla cómo se leen las líneas del archivo de código fuente con el método [SkipLine](./skipline-method.md)y la propiedad [LineSeparator](./lineseparator-property-ado.md) .  
  
-   Determine el final de la posición de la secuencia con la propiedad [EOS](./eos-property.md)y el método [seteos](./seteos-method.md) .  
  
-   Guardar y restaurar datos en archivos con los métodos [SaveToFile](./savetofile-method.md)y [LoadFromFile](./loadfromfile-method-ado.md) .  
  
-   Especifique el juego de caracteres utilizado para almacenar la **secuencia** con la propiedad [CharSet](./charset-property-ado.md) .  
  
-   Detiene una operación de **secuencia** asincrónica con el método [Cancel](./cancel-method-ado.md) .  
  
-   Determine el número de bytes de una **secuencia** con la propiedad [size](./size-property-ado-stream.md) .  
  
-   Controlar la posición actual dentro de una **secuencia** con la propiedad [Position](./position-property-ado.md) .  
  
-   Determine el tipo de datos en una **secuencia** con la propiedad [Type](./type-property-ado-stream.md) .  
  
-   Determine el estado actual de la **secuencia** (cerrada, abierta o en ejecución) con la propiedad [State](./state-property-ado.md) .  
  
-   Especifique el modo de acceso para la **secuencia** con la propiedad [mode](./mode-property-ado.md) .  
  
> [!NOTE]
>  Las direcciones URL que usan el esquema http invocarán automáticamente el [proveedor de Microsoft OLE DB para la publicación en Internet](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Para obtener más información, consulte [direcciones URL absolutas y relativas](../../guide/data/absolute-and-relative-urls.md).  
  
 El objeto de **secuencia** es seguro para el scripting.  
  
 Esta sección contiene los temas siguientes.  
  
-   [Propiedades, métodos y eventos del objeto Stream](./stream-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Consulte también  
 [Registros y secuencias](../../guide/data/records-and-streams.md)