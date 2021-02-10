---
description: Colecciones y los objetos ADO
title: Objetos y colecciones de ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- ADO, objects and collections
ms.assetid: 7a745aae-9372-49b6-8dae-b9c93e5f3216
author: rothja
ms.author: jroth
ms.openlocfilehash: c03e1b792e23552252eb84fae9a71e693b3781c8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028070"
---
# <a name="ado-objects-and-collections"></a>Colecciones y los objetos ADO
ADO consta de los siguientes nueve objetos y cuatro colecciones.  
  
|Objeto o colección|Descripción|  
|--------------------------|-----------------|  
|Objeto de **conexión**|Representa una sesión única con un origen de datos. En el caso de un sistema de base de datos cliente/servidor, puede ser equivalente a una conexión de red real al servidor. Dependiendo de la funcionalidad admitida por el proveedor, es posible que algunas colecciones, métodos o propiedades de un objeto de **conexión** no estén disponibles.|  
|Objeto **Command**|Se utiliza para definir un comando específico, como una consulta SQL, que se va a ejecutar en un origen de datos.|  
|Objeto de **conjunto de registros**|Representa el conjunto completo de registros de una tabla base o los resultados de un comando ejecutado. Todos los objetos de **conjunto de registros** se componen de registros (filas) y campos (columnas).|  
|Objeto **Record**|Representa una sola fila de datos, ya sea de un **conjunto de registros** o del proveedor. Este registro podría representar un registro de base de datos o algún otro tipo de objeto, como un archivo o un directorio, dependiendo del proveedor.|  
|**Stream** (objeto)|Representa una secuencia de datos binarios o de texto. Por ejemplo, un documento XML se puede cargar en una secuencia para la entrada de comando o devolverse desde ciertos proveedores como los resultados de una consulta. Un objeto de **secuencia** se puede utilizar para manipular los campos o registros que contienen estos flujos de datos.|  
|**Parameter** (objeto)|Representa un parámetro o un argumento asociado a un objeto de **comando** , basado en una consulta con parámetros o un procedimiento almacenado.|  
|**Field** (objeto)|Representa una columna de datos con un tipo de datos común. Cada objeto de **campo** corresponde a una columna del **conjunto de registros**.|  
|**Property (objeto)**|Representa una característica de un objeto ADO definido por el proveedor. Los objetos ADO tienen dos tipos de propiedades: integrado y dinámico. Las propiedades integradas son aquellas que se implementan en ADO y que están inmediatamente disponibles para cualquier objeto nuevo. El objeto **Property** es un contenedor para las propiedades dinámicas, definido por el proveedor subyacente.|  
|Objeto **Error**|Contiene detalles sobre los errores de acceso a datos que pertenecen a una única operación que implica al proveedor.|  
|Colección **Fields**|Contiene todos los objetos de **campo** de un **conjunto de registros** o un objeto de **registro** .|  
|Colección de **propiedades**|Contiene todos los objetos de **propiedad** para una instancia específica de un objeto.|  
|Colección **Parameters**|Contiene todos los objetos de **parámetro** de un objeto **Command** .|  
|Colección de **errores**|Contiene todos los objetos de **error** creados en respuesta a un error único relacionado con el proveedor.|  
  
## <a name="see-also"></a>Consulte también  
 [Modelo de objetos ADO](../../reference/ado-api/ado-object-model.md)