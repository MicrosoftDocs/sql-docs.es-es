---
description: Objeto Record (ADO)
title: Objeto Record (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Record
helpviewer_keywords:
- Record object [ADO]
ms.assetid: db83ed2c-a8e3-460c-8682-64667e4d5d01
author: rothja
ms.author: jroth
ms.openlocfilehash: 48fa1fcb993531f8644b931f216f9b38ed427282
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100020364"
---
# <a name="record-object-ado"></a>Objeto Record (ADO)
Representa una fila de un [conjunto de registros](./recordset-object-ado.md) o del proveedor de datos, o un objeto devuelto por un proveedor de datos semiestructurados, como un archivo o un directorio.  
  
## <a name="remarks"></a>Observaciones  
 Un objeto de **registro** representa una fila de datos y tiene algunas similitudes conceptuales con un conjunto de **registros** de una fila. En función de las capacidades del proveedor, los objetos de **registro** pueden devolverse directamente desde el proveedor en lugar de un **conjunto de registros** de una fila, por ejemplo, cuando se ejecuta una consulta SQL que selecciona solo una fila. O bien, un objeto de **registro** se puede obtener directamente de un objeto de **conjunto de registros** . O bien, se puede devolver un **registro** directamente de un proveedor a datos semiestructurados, como el proveedor de OLE DB de Microsoft Exchange.  
  
 Puede ver los campos asociados con el objeto de **registro** por medio de la colección [Fields](./fields-collection-ado.md) del objeto **Record** . ADO permite columnas con valores de objeto, incluidos **conjuntos de registros**, **SAFEARRAY** y valores escalares en la colección **Fields** de objetos **Record** .  
  
 Si el objeto **Record** representa una fila de un **conjunto de registros**, es posible volver a ese **conjunto de registros** original con la propiedad [source](./source-property-ado-record.md) .  
  
 Los proveedores de datos semiestructurados, como el [proveedor de Microsoft OLE DB para la publicación en Internet](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md), también pueden usar el objeto de **registro** para modelar los espacios de nombres estructurados en el árbol. Cada nodo del árbol es un objeto de **registro** con columnas asociadas. Las columnas pueden representar los atributos de ese nodo y otra información relevante. El objeto de **registro** puede representar tanto un nodo hoja como un nodo no hoja en la estructura de árbol. Los nodos no hoja tienen otros nodos como su contenido, pero los nodos hoja no tienen ese contenido. Normalmente, los nodos hoja contienen secuencias binarias de datos y los nodos no hoja también pueden tener una secuencia binaria predeterminada asociada. Las propiedades del objeto de **registro** identifican el tipo de nodo.  
  
 El objeto **Record** también representa una manera alternativa de navegar por los datos organizados jerárquicamente. Se puede crear un objeto de **registro** para representar la raíz de un subárbol específico en una estructura de árbol grande y se pueden abrir objetos de **registro** nuevos para representar los nodos secundarios.  
  
 Un recurso (por ejemplo, un archivo o un directorio) se puede identificar de forma única mediante una dirección URL absoluta. Un objeto de [conexión](./connection-object-ado.md) se crea implícitamente y se establece en el objeto de **registro** cuando el **registro** se abre mediante una dirección URL absoluta. Un objeto de **conexión** puede establecerse explícitamente en el objeto de **registro** a través de la propiedad [ActiveConnection](./activeconnection-property-ado.md) . Los archivos y directorios a los que se puede tener acceso mediante el objeto de **conexión** definen el *contexto* en el que se pueden producir las operaciones de **registro** .  
  
 La modificación de datos y los métodos de navegación del objeto **Record** también aceptan una dirección URL relativa, que busca un recurso mediante una dirección URL absoluta o el contexto del objeto de **conexión** como punto de partida.  
  
> [!NOTE]
>  Las direcciones URL que usan el esquema http invocarán automáticamente el [proveedor de Microsoft OLE DB para la publicación en Internet](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Para obtener más información, consulte [direcciones URL absolutas y relativas](../../guide/data/absolute-and-relative-urls.md).  
  
 Un objeto de **conexión** está asociado a cada objeto de **registro** . Por lo tanto, las operaciones de objeto de **registro** pueden formar parte de una transacción mediante la invocación de métodos de transacción de objeto de **conexión** .  
  
 El objeto de **registro** no admite eventos de ADO y, por lo tanto, no responderá a las notificaciones.  
  
 Con los métodos y las propiedades de un objeto de **registro** , puede hacer lo siguiente:  
  
-   Establece o devuelve el objeto de **conexión** asociado con la propiedad [ActiveConnection](./activeconnection-property-ado.md) .  
  
-   Indicar permisos de acceso con la propiedad [mode](./mode-property-ado.md) .  
  
-   Devuelve la dirección URL del directorio, si la hubiera, que contiene el recurso representado por el **registro** con la propiedad [ParentURL](./parenturl-property-ado.md) .  
  
-   Indique la dirección URL absoluta, la dirección URL relativa o el **conjunto de registros** del que se deriva el **registro** con la propiedad de [origen](./source-property-ado-record.md) .  
  
-   Indica el estado actual del **registro** con la propiedad [State](./state-property-ado.md) .  
  
-   Indicar el tipo de **registro**  -  *simple*, *colección* o *documento estructurado* : con la propiedad [RecordType](./recordtype-property-ado.md).  
  
-   Detiene la ejecución de una operación asincrónica con el método [Cancel](./cancel-method-ado.md) .  
  
-   Desasociar el **registro** de un origen de datos con el método [Close](./close-method-ado.md) .  
  
-   Copie el archivo o el directorio representado por un **registro** en otra ubicación con el método [CopyRecord](./copyrecord-method-ado.md) .  
  
-   Elimine el archivo, el directorio y los subdirectorios, representados por un **registro** con el método [DeleteRecord](./deleterecord-method-ado.md) .  
  
-   Abra un **conjunto de registros** que contenga filas que representen los subdirectorios y los archivos de la entidad representada por el **registro** con el método [GetChildren](./getchildren-method-ado.md) .  
  
-   Mueva (cambie el nombre) el archivo, el directorio y los subdirectorios, representados por un **registro** en otra ubicación con el método [MoveRecord](./moverecord-method-ado.md) .  
  
-   Asocie el **registro** a un origen de datos existente o cree un nuevo archivo o directorio con el método [Open](./open-method-ado-record.md) .  
  
 El objeto de **registro** es seguro para el scripting.  
  
 Esta sección contiene el siguiente tema.  
  
-   [Propiedades, métodos y eventos del objeto Record](./record-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Consulte también  
 [Fields (colección) (ADO)](./fields-collection-ado.md)   
 [Colección Properties (ADO)](./properties-collection-ado.md)   
 [Registros y secuencias](../../guide/data/records-and-streams.md)   
 [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)