---
description: Propiedades de ADO
title: Propiedades de ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- properties [ADO]
- ADO properties
ms.assetid: 0ac0d1a7-6c7a-4f4c-b115-428935e0f98b
author: rothja
ms.author: jroth
ms.openlocfilehash: bdacd558161c09d1d43f590f9f355e14da262b14
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164885"
---
# <a name="ado-properties"></a>Propiedades de ADO

|Propiedad|Descripción|  
|-|-|  
|[AbsolutePage](./absolutepage-property-ado.md)|Indica en qué página reside el registro actual.|  
|[AbsolutePosition](./absoluteposition-property-ado.md)|Indica la posición ordinal del registro actual de un objeto de **conjunto de registros** .|  
|[ActiveCommand](./activecommand-property-ado.md)|Indica el objeto de **comando** que creó el objeto de **conjunto de registros** asociado.|  
|[ActiveConnection](./activeconnection-property-ado.md)|Indica a qué objeto de **conexión** pertenece actualmente el objeto de **comando**, **conjunto de registros** o **registro** especificado.|  
|[ActualSize](./actualsize-property-ado.md)|Indica la longitud real del valor de un campo.|  
|[Atributos](./attributes-property-ado.md)|Indica una o más características de un objeto.|  
|[BOF y EOF](./bof-eof-properties-ado.md)|**BOF** indica que la posición del registro actual es anterior al primer registro de un objeto de conjunto de registros.<br /><br /> **EOF** indica que la posición del registro actual es posterior al último registro de un objeto de conjunto de registros.|  
|[Marcador](./bookmark-property-ado.md)|Indica un marcador que identifica de forma única el registro actual en un objeto de **conjunto de registros** o establece el registro actual de un objeto de **conjunto de registros** en el registro identificado por un marcador válido.|  
|[CacheSize](./cachesize-property-ado.md)|Indica el número de registros de un objeto de **conjunto de registros** que se almacenan en caché localmente en la memoria.|  
|[Capítulo](./chapter-property-ado.md)|Obtiene o establece un objeto **Chapter** de OLE DB de/en un objeto **ADORecordsetConstruction** .|  
|[CharSet](./charset-property-ado.md)|Indica el juego de caracteres en el que se debe traducir el contenido de una **secuencia** de texto.|  
|[CommandStream](./commandstream-property-ado.md)|Indica la secuencia que se usa como entrada para un objeto de **comando** .|  
|[CommandText](./commandtext-property-ado.md)|Indica el texto de un comando que se va a emitir en un proveedor.|  
|[CommandTimeout](./commandtimeout-property-ado.md)|Indica cuánto tiempo se debe esperar mientras se ejecuta un comando antes de terminar el intento y generar un error.|  
|[CommandType](./commandtype-property-ado.md)|Indica el tipo de un objeto de **comando** .|  
|[Propiedad ConnectionString](./connectionstring-property-ado.md)|Indica la información utilizada para establecer una conexión con un origen de datos.|  
|[ConnectionTimeout](./connectiontimeout-property-ado.md)|Indica cuánto tiempo se debe esperar mientras se establece una conexión antes de terminar el intento y generar un error.|  
|[Recuento](./count-property-ado.md)|Indica el número de objetos de una colección.|  
|[CursorLocation](./cursorlocation-property-ado.md)|Indica la ubicación del servicio de cursor.|  
|[CursorType](./cursortype-property-ado.md)|Indica el tipo de cursor utilizado en un objeto de **conjunto de registros** .|  
|[DataMember](./datamember-property.md)|Indica el nombre del miembro de datos que se recuperará del objeto al que hace referencia la propiedad **DataSource** .|  
|[DataSource](./datasource-property-ado.md)|Indica un objeto que contiene los datos que se van a representar como un objeto de **conjunto de registros** .|  
|[DefaultDatabase](./defaultdatabase-property.md)|Indica la base de datos predeterminada para un objeto de **conexión** .|  
|[DefinedSize](./definedsize-property.md)|Indica la capacidad de los datos de un objeto de **campo** .|  
|[Descripción](./description-property.md)|Describe un objeto de **error** .|  
|[Dialecto](./dialect-property.md)|Indica la sintaxis y las reglas generales que utilizará el proveedor para analizar las propiedades **CommandText** o **CommandStream** .|  
|[Dirección](./direction-property.md)|Indica si el **parámetro** representa un parámetro de entrada, un parámetro de salida o ambos, o si el parámetro es el valor devuelto de un procedimiento almacenado.|  
|[EditMode](./editmode-property.md)|Indica el estado de edición del registro actual.|  
|[OCASIONA](./eos-property.md)|Indica si la posición actual está al final de la secuencia.|  
|[Filter](./filter-property.md)|Indica un filtro para los datos de un **conjunto de registros**.|  
|[HelpContext y HelpFile](./helpcontext-helpfile-properties.md)|Indica el archivo de ayuda y el tema asociados a un objeto de **error** .<br /><br /> **HelpContextID** devuelve un identificador de contexto, como un valor **largo** , para un tema de un archivo de ayuda.<br /><br /> **HelpFile** devuelve un valor de **cadena** que se evalúa como una ruta de acceso totalmente resuelta de un archivo de ayuda.|  
|[Index](./index-property.md)|Indica el nombre del índice actualmente activo para un objeto de **conjunto de registros** .|  
|[IsolationLevel](./isolationlevel-property.md)|Indica el nivel de aislamiento para un objeto de **conexión** .|  
|[Elemento](./item-property-ado.md)|Indica un miembro específico de una colección, por nombre o número ordinal.|  
|[LineSeparator](./lineseparator-property-ado.md)|Indica el carácter binario que se va a utilizar como separador de línea en los objetos de **flujo** de texto.|  
|[LockType](./locktype-property-ado.md)|Indica el tipo de bloqueos colocados en los registros durante la edición.|  
|[MarshalOptions](./marshaloptions-property-ado.md)|Indica qué registros deben calcularse de nuevo en el servidor.|  
|[MaxRecords](./maxrecords-property-ado.md)|Indica el número máximo de registros que se van a devolver a un **conjunto** de registros desde una consulta.|  
|[Modo](./mode-property-ado.md)|Indica los permisos disponibles para modificar los datos de un objeto de **conexión**, **registro** o **secuencia** .|  
|[Nombre](./name-property-ado.md)|Indica el nombre de un objeto.|  
|[NativeError](./nativeerror-property-ado.md)|Indica el código de error específico del proveedor para un objeto de **error** determinado.|  
|[Number](./number-property-ado.md)|Indica el número que identifica de forma única un objeto de **error** .|  
|[NumericScale](./numericscale-property-ado.md)|Indica la escala de valores numéricos en un **parámetro** o un objeto de **campo** .|  
|[OriginalValue](./originalvalue-property-ado.md)|Indica el valor de un **campo** que existía en el registro antes de que se realizaran cambios.|  
|[NúmeroDePáginas](./pagecount-property-ado.md)|Indica el número de páginas de datos que contiene el objeto de **conjunto de registros** .|  
|[PageSize](./pagesize-property-ado.md)|Indica el número de registros que representan una página en el **conjunto de registros**.|  
|[ParentRow](./parentrow-property-ado.md)|Establece el contenedor de un objeto de **fila** OLE DB en un objeto **ADORecordConstruction** , de modo que el elemento primario de la fila se convierta en un objeto **Record** de ADO.|  
|[ParentURL](./parenturl-property-ado.md)|Indica una cadena de dirección URL absoluta que apunta al **registro** primario del objeto de **registro** actual.|  
|[Position](./position-property-ado.md)|Indica la posición actual dentro de un objeto de **flujo** .|  
|[Precisión](./precision-property-ado.md)|Indica el grado de precisión de los valores numéricos de un objeto de **parámetro** o de los objetos de **campo** numérico.|  
|[Prepared](./prepared-property-ado.md)|Indica si se debe guardar una versión compilada de un comando antes de la ejecución.|  
|[Proveedor](./provider-property-ado.md)|Indica el nombre del proveedor de un objeto de **conexión** .|  
|[RecordCount](./recordcount-property-ado.md)|Indica el número de registros de un objeto de **conjunto de registros** .|  
|[RecordType](./recordtype-property-ado.md)|Indica el tipo de objeto de **registro** .|  
|[Row](./row-property-ado.md)|Obtiene o establece un objeto de **fila** OLE DB de/en un objeto **ADORecordConstruction** .|  
|[RowPosition](./rowposition-property-ado.md)|Obtiene o establece un objeto **RowPosition** OLE DB de/en un objeto **ADORecordsetConstruction** .|  
|[Conjunto de filas](./rowset-property-ado.md)|Obtiene o establece un objeto de **conjunto de filas** OLE DB de/en un objeto **ADORecordsetConstruction** .|  
|[Origen (error de ADO)](./source-property-ado-error.md)|Indica el nombre del objeto o la aplicación que originalmente generó un error.|  
|[Source (registro de ADO)](./source-property-ado-record.md)|Indica la entidad representada por el objeto de **registro** .|  
|[Source (conjunto de registros ADO)](./source-property-ado-recordset.md)|Indica el origen de los datos de un objeto de **conjunto de registros**|  
|[SQLState](./sqlstate-property.md)|Indica el estado de SQL para un objeto de **error** específico.|  
|[Estado](./state-property-ado.md)|Indica para todos los objetos aplicables si el estado del objeto es abierto o cerrado. Indica para todos los objetos aplicables que ejecutan un método asincrónico, si el estado actual del objeto es conexión, ejecución o recuperación.|  
|[Status (campo de ADO)](./status-property-ado-field.md)|Indica el estado de un objeto de **campo** .|  
|[Status (conjunto de registros ADO)](./status-property-ado-recordset.md)|Indica el estado del registro actual en relación con las actualizaciones por lotes u otras operaciones masivas.|  
|[StayInSync](./stayinsync-property.md)|Indica, en un objeto de **conjunto de registros** jerárquico, si la referencia a los registros secundarios subyacentes (es decir, el *capítulo*) cambia cuando cambia la posición de la fila primaria.|  
|[Propiedad de la secuencia](./stream-property.md)|Obtiene o establece un objeto de **flujo** de OLE DB de/en un objeto **ADOStreamConstruction** .|  
|[Type](./type-property-ado.md)|Indica el tipo operativo o el tipo de datos de un **parámetro**, **campo** o objeto de **propiedad** .|  
|[Type (secuencia de ADO)](./type-property-ado-stream.md)|Indica el tipo de datos contenidos en la **secuencia** (binario o de texto).|  
|[UnderlyingValue](./underlyingvalue-property.md)|Indica el valor actual de la base de datos para un objeto de **campo** .|  
|[Valor](./value-property-ado.md)|Indica el valor asignado a un **campo**, un **parámetro** o un objeto de **propiedad** .|  
|[Versión](./version-property-ado.md)|Indica el número de versión de ADO.|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ADO](./ado-api-reference.md)   
 [Colecciones de ADO](./ado-collections.md)   
 [Propiedades dinámicas de ADO](./ado-dynamic-properties.md)   
 [Constantes enumeradas de ADO](./ado-enumerated-constants.md)   
 [Apéndice B: errores de ADO](../../guide/appendixes/appendix-b-ado-errors.md)   
 [Eventos de ADO](./ado-events.md)   
 [Métodos de ADO](./ado-methods.md)   
 [Modelo de objetos ADO](./ado-object-model.md)   
 [Interfaces y objetos ADO](./ado-objects-and-interfaces.md)