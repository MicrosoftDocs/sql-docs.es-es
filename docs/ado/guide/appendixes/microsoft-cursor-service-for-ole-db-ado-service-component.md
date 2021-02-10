---
description: Microsoft cursor Service para OLE DB (componente de servicio ADO)
title: Microsoft cursor Service para OLE DB (componente de servicio ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- providers [ADO], cursor service for OLE DB
- cursor service for OLE DB [ADO]
ms.assetid: 420d0989-7cfb-4c66-a7b5-f4199d13165d
author: rothja
ms.author: jroth
ms.openlocfilehash: 12224a64ffa8d67b212deacd3bd8794d6519cfb9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029389"
---
# <a name="microsoft-cursor-service-for-ole-db-overview"></a>Introducción al servicio de cursores de Microsoft para OLE DB
El servicio de cursores de Microsoft para OLE DB complementa las funciones de compatibilidad de cursores de los proveedores de datos. Como resultado, el usuario percibe una funcionalidad relativamente uniforme de todos los proveedores de datos.

 El servicio de cursor hace que las propiedades dinámicas estén disponibles y mejora el comportamiento de ciertos métodos. Por ejemplo, la propiedad dinámica [Optimize](../../reference/ado-api/optimize-property-dynamic-ado.md) permite la creación de índices temporales para facilitar ciertas operaciones, como el método [Find](../../reference/ado-api/find-method-ado.md) .

 El servicio de cursor habilita la compatibilidad con la actualización por lotes en todos los casos. También simula tipos de cursor más compatibles, como los cursores dinámicos, cuando un proveedor de datos solo puede proporcionar cursores menos compatibles, como cursores estáticos.

## <a name="keyword"></a>Palabra clave
 Para invocar este componente de servicio, establezca la propiedad [CursorLocation](../../reference/ado-api/cursorlocation-property-ado.md) del objeto de [conjunto de registros](../../reference/ado-api/recordset-object-ado.md) o del objeto de [conexión](../../reference/ado-api/connection-object-ado.md) en **adUseClient**.

```vb
connection.CursorLocation=adUseClient
recordset.CursorLocation=adUseClient
```

## <a name="dynamic-properties"></a>Propiedades dinámicas
 Cuando se invoca el servicio de cursor para OLE DB, se agregan las siguientes propiedades dinámicas a la colección de [propiedades](../../reference/ado-api/properties-collection-ado.md) del objeto de **conjunto de registros** . La lista completa de propiedades dinámicas de objetos de **conjunto de registros** y de **conexión** se muestra en el [Índice de propiedades dinámicas de ADO](../../reference/ado-api/ado-dynamic-property-index.md). Los nombres de propiedad OLE DB asociados, si procede, se incluyen entre paréntesis después del nombre de la propiedad ADO.

 Los cambios en algunas propiedades dinámicas no son visibles para el origen de datos subyacente una vez invocado el servicio de cursor. Por ejemplo, el establecimiento de la propiedad *tiempo de espera de comando* en un conjunto de **registros** no será visible para el proveedor de datos subyacente.

```vb

Recordset1.CursorLocation = adUseClient     'invokes cursor service
Recordset1.Open "authors", _
    "Provider=SQLOLEDB;Data Source=DBServer;User Id=MyUserID;" & _
    "Password=MyPassword;Initial Catalog=pubs;",,adCmdTable
Recordset1.Properties.Item("Command Time out") = 50
' 'Command Time out' property on DBServer is still default (30).

```

 Si su aplicación requiere el servicio de cursor, pero debe establecer propiedades dinámicas en el proveedor subyacente, establezca las propiedades antes de invocar el servicio de cursor. Los valores de la propiedad de objeto de comando siempre se pasan al proveedor de datos subyacente independientemente de la ubicación del cursor. Por lo tanto, también puede usar un objeto de comando para establecer las propiedades en cualquier momento.

> [!NOTE]
>  El servicio de cursor no admite la propiedad dinámica DBPROP_SERVERDATAONINSERT, incluso si es compatible con el proveedor de datos subyacente.

|Nombre de la propiedad|Descripción|
|-------------------|-----------------|
|Auto Calc (DBPROP_ADC_AUTORECALC)|En el caso de los conjuntos de registros creados con el servicio de forma de datos, este valor indica la frecuencia con la que se calculan las columnas calculadas y las agregadas. El valor predeterminado (valor = 1) es recalcular siempre que el servicio de forma de datos determine que los valores han cambiado. Si el valor es 0, las columnas calculadas o agregadas solo se calculan cuando se compila inicialmente la jerarquía.|
|Tamaño de lote (DBPROP_ADC_BATCHSIZE)|Indica el número de instrucciones Update que se pueden procesar por lotes antes de que se envíen al almacén de datos. Cuantas más instrucciones tenga un lote, menos viajes de ida y vuelta al almacén de datos.|
|Almacenar en caché filas secundarias (DBPROP_ADC_CACHECHILDROWS)|En el caso de los conjuntos de registros creados con el servicio de forma de datos, este valor indica si los conjuntos de registros secundarios se almacenan en una memoria caché para su uso posterior.|
|Versión del motor de cursores (DBPROP_ADC_CEVER)|Indica la versión del servicio de cursor que se está usando.|
|Mantener el estado de los cambios (DBPROP_ADC_MAINTAINCHANGESTATUS)|Indica el texto del comando que se usa para volver a sincronizar una o más filas en una combinación de varias tablas.|
|[Optimize](../../reference/ado-api/optimize-property-dynamic-ado.md) (Optimizar)|Indica si se debe crear un índice. Cuando se establece en **true**, autoriza la creación temporal de índices para mejorar la ejecución de determinadas operaciones.|
|[Nombre de la reformación](../../reference/ado-api/reshape-name-property-dynamic-ado.md)|Indica el nombre del **conjunto de registros**. Se puede hacer referencia a él en los comandos de forma de datos actuales o posteriores.|
|[Comando Resync](../../reference/ado-api/resync-command-property-dynamic-ado.md)|Indica una cadena de comandos personalizada que usa el método [Resync](../../reference/ado-api/resync-method.md) cuando la propiedad de [tabla única](../../reference/ado-api/unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md) está en vigor.|
|[Catálogo único](../../reference/ado-api/unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md)|Indica el nombre de la base de datos que contiene la tabla a la que se hace referencia en la propiedad de **tabla única** .|
|[Esquema único](../../reference/ado-api/unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md)|Indica el nombre del propietario de la tabla a la que se hace referencia en la propiedad de **tabla única** .|
|[Tabla única](../../reference/ado-api/unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md)|Indica el nombre de la tabla de un **conjunto de registros** creado a partir de varias tablas que pueden ser modificadas por inserciones, actualizaciones o eliminaciones.|
|Criterios de actualización (DBPROP_ADC_UPDATECRITERIA)|Indica qué campos de la cláusula **Where** se utilizan para controlar las colisiones que se producen durante una actualización.|
|[Actualización](../../reference/ado-api/update-resync-property-dynamic-ado.md) de la sincronización (DBPROP_ADC_UPDATERESYNC)|Indica si el método **Resync** se invoca implícitamente después del método [UpdateBatch](../../reference/ado-api/updatebatch-method.md) (y su comportamiento) cuando la propiedad de **tabla única** está en vigor.|

 También puede establecer o recuperar una propiedad dinámica especificando su nombre como índice de la colección de **propiedades** . Por ejemplo, obtenga e imprima el valor actual de la propiedad de [optimización](../../reference/ado-api/optimize-property-dynamic-ado.md) dinámica y, a continuación, establezca un nuevo valor, como se indica a continuación:

```vb
Debug.Print rs.Properties("Optimize")
rs.Properties("Optimize") = True
```

## <a name="built-in-property-behavior"></a>Comportamiento de la propiedad integrada
 El servicio de cursor para OLE DB también afecta al comportamiento de ciertas propiedades integradas.

|Nombre de la propiedad|Descripción|
|-------------------|-----------------|
|[CursorType](../../reference/ado-api/cursortype-property-ado.md)|Complementa los tipos de cursores que están disponibles para un **conjunto de registros**.|
|[LockType](../../reference/ado-api/locktype-property-ado.md)|Complementa los tipos de bloqueos disponibles para un **conjunto de registros**. Habilita las actualizaciones por lotes.|
|[Sort](../../reference/ado-api/sort-property.md)|Especifica uno o más nombres de campo en los que se ordena el **conjunto de registros** y si cada campo se ordena en orden ascendente o descendente.|

## <a name="method-behavior"></a>Comportamiento del método
 El servicio de cursor para OLE DB habilita o afecta al comportamiento del método [Append](../../reference/ado-api/append-method-ado.md) del objeto de [campo](../../reference/ado-api/field-object.md) ; y los métodos [Open](../../reference/ado-api/open-method-ado-recordset.md), [Resync](../../reference/ado-api/resync-method.md), [UpdateBatch](../../reference/ado-api/updatebatch-method.md)y [Save](../../reference/ado-api/save-method.md) del objeto de **conjunto de registros** .