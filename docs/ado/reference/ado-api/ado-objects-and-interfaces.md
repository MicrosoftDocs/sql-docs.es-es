---
description: Interfaces y objetos ADO
title: Objetos e interfaces de ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- ADO, objects and interfaces
- objects [ADO]
ms.assetid: d0b7e254-c89f-4406-b846-a060ef038c30
author: rothja
ms.author: jroth
ms.openlocfilehash: c694a5051729a6ce9b925014ae93b5dbde11f5d9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164895"
---
# <a name="ado-objects-and-interfaces"></a>Interfaces y objetos ADO
Las relaciones entre estos objetos se representan en el [modelo de objetos ADO](./ado-object-model.md).  
  
 Cada objeto puede estar contenido en su colección correspondiente. Por ejemplo, un objeto de [error](./error-object.md) puede estar contenido en una colección de [errores](./errors-collection-ado.md) . Para obtener más información, vea [colecciones de ADO](./ado-collections.md) o un tema específico de la colección.  
  
|Objeto o interfaz|Descripción|  
|-|-|  
|[IADOCommandConstruction](/previous-versions/windows/desktop/aa965677(v=vs.85))|Se usa para recuperar el comando OLEDB subyacente de un objeto ADOCommand.|  
|[ADORecordConstruction](./adorecordconstruction-interface.md)|Construye un objeto **Record** de ADO a partir de un objeto de **fila** OLE DB en una aplicación de C/C++.|  
|[ADORecordsetConstruction](./adorecordsetconstruction-interface.md)|Construye un objeto de **conjunto de registros** ADO a partir de un objeto de **conjunto de filas** OLE DB en una aplicación de C/C++.|  
|[Interfaz ADOStreamConstruction](./adostreamconstruction-interface.md)|Construye un objeto de **secuencia** de ADO a partir de un OLE DB objeto **IStream** en una aplicación de C/C++.|  
|[Comando](./command-object-ado.md)|Define un comando específico que se va a ejecutar en un origen de datos.<br /><br /> El objeto **Command** no es seguro para el scripting.|  
|[Connection](./connection-object-ado.md)|Representa una conexión abierta a un origen de datos.<br /><br /> El objeto de **conexión** es seguro para el scripting.|  
|[Interfaz IDSOShapeExtensions](./idsoshapeextensions-interface.md)|Obtiene el objeto de origen de datos OLEDB subyacente para el proveedor de formas.|  
|[Error](./error-object.md)|Contiene detalles sobre los errores de acceso a datos que pertenecen a una única operación que implica al proveedor.<br /><br /> El objeto de **error** no es seguro para el scripting.|  
|[Campo](./field-object.md)|Representa una columna de datos con un tipo de datos común.|  
|[Parámetro](./parameter-object.md)|Representa un parámetro o un argumento asociado a un objeto de **comando** basado en una consulta con parámetros o un procedimiento almacenado.<br /><br /> El objeto de **parámetro** no es seguro para el scripting.|  
|[Propiedad](./property-object-ado.md)|Representa una característica dinámica de un objeto ADO definido por el proveedor.|  
|[Registro](./record-object-ado.md)|Representa una fila de un **conjunto de registros** o un directorio o archivo en un sistema de archivos. El objeto de **registro** es seguro para el scripting.|  
|[DataRecordsets](./recordset-object-ado.md)|Representa el conjunto de registros de una tabla base o los resultados de un comando ejecutado. En cualquier momento, el objeto de **conjunto de registros** hace referencia a un único registro del conjunto como el registro actual.<br /><br /> El objeto de **conjunto de registros** es seguro para el scripting.|  
|[Stream](./stream-object-ado.md)|Representa un flujo binario de datos.<br /><br /> El objeto de **secuencia** es seguro para el scripting.|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ADO](./ado-api-reference.md)   
 [Colecciones de ADO](./ado-collections.md)   
 [Propiedades dinámicas de ADO](./ado-dynamic-properties.md)   
 [Constantes enumeradas de ADO](./ado-enumerated-constants.md)   
 [Apéndice B: errores de ADO](../../guide/appendixes/appendix-b-ado-errors.md)   
 [Eventos de ADO](./ado-events.md)   
 [Métodos de ADO](./ado-methods.md)   
 [Modelo de objetos ADO](./ado-object-model.md)   
 [Propiedades de ADO](./ado-properties.md)