---
description: Introducción al proveedor de Microsoft OLE DB para el servicio de indización de Microsoft
title: Proveedor de Microsoft OLE DB para el servicio de indización de Microsoft | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Indexing Service provider [ADO]
- providers [ADO], OLE DB provider for Microsoft Indexing service
- OLE DB provider for Microsoft Indexing service [ADO]
ms.assetid: f86a0598-5097-471b-8318-d2c859d085f2
author: rothja
ms.author: jroth
ms.openlocfilehash: f7d2204a2d91e862616713dbfee6eec3d6d3e8ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029309"
---
# <a name="microsoft-ole-db-provider-for-microsoft-indexing-service-overview"></a>Introducción al proveedor de Microsoft OLE DB para el servicio de indización de Microsoft
El proveedor de Microsoft OLE DB para el servicio de indización de Microsoft proporciona acceso de solo lectura mediante programación al sistema de archivos y a los datos Web indexados por el servicio de indización de Microsoft. Las aplicaciones ADO pueden emitir consultas SQL para recuperar contenido e información de propiedades de archivo.

 El proveedor es de subprocesamiento libre y está habilitado para Unicode.

## <a name="connection-string-parameters"></a>Parámetros de la cadena de conexión
 Para conectarse a este proveedor, establezca el argumento **Provider =** en la propiedad [ConnectionString](../../reference/ado-api/connectionstring-property-ado.md) en:

```vb
MSIDXS
```

 La lectura de la propiedad [Provider](../../reference/ado-api/provider-property-ado.md) también devolverá esta cadena.

## <a name="typical-connection-string"></a>Cadena de conexión típica
 Una cadena de conexión típica para este proveedor es:

```vb
"Provider=MSIDXS;Data Source=myCatalog;Locale Identifier=nnnn;"
```

 La cadena consta de estas palabras clave:

|Palabra clave|Descripción|
|-------------|-----------------|
|**Proveedor**|Especifica el proveedor de OLE DB para el servicio de indización de Microsoft. Normalmente, esta es la única palabra clave especificada en la cadena de conexión.|
|**Data Source** (Origen de datos)|Especifica el nombre del catálogo de servicios de Index Server. Si no se especifica esta palabra clave, se utiliza el catálogo predeterminado del sistema.|
|**Identificador de configuración regional**|Especifica un número único de 32 bits (por ejemplo, 1033) que especifica las preferencias relacionadas con el idioma del usuario. Si no se especifica esta palabra clave, se utiliza el identificador de configuración regional predeterminado del sistema.|

## <a name="command-text"></a>Texto de comando
 La sintaxis de consulta SQL de servicio de indización se compone de las extensiones de la instrucción **Select** de sql-92 y sus cláusulas **from** y **Where** . Los resultados de la consulta se devuelven a través de OLE DB conjuntos de filas, que pueden ser consumidos por ADO y manipulados como objetos de [conjunto de registros](../../reference/ado-api/recordset-object-ado.md) .

 Puede buscar palabras o frases exactas o usar caracteres comodín para buscar patrones o raíces de palabras. La lógica de búsqueda se puede basar en decisiones booleanas, términos ponderados o proximidad con otras palabras. También puede buscar por "texto libre", que busca coincidencias según el significado, en lugar de palabras exactas.

 El dialecto del comando específico se documenta totalmente en la documentación de los lenguajes de consulta para el servicio de Index Server.

 El proveedor no acepta llamadas a procedimientos almacenados ni nombres de tabla simples (por ejemplo, la propiedad [CommandType](../../reference/ado-api/commandtype-property-ado.md) siempre será **adCmdText**).

## <a name="recordset-behavior"></a>Comportamiento del conjunto de registros
 En las tablas siguientes se enumeran las características disponibles con un objeto de **conjunto de registros** abierto con este proveedor. Solo está disponible el tipo de cursor estático (**adOpenStatic**).

 Para obtener información más detallada sobre el comportamiento del **conjunto de registros** para la configuración del proveedor, ejecute el método [Supports](../../reference/ado-api/supports-method.md) y enumere la colección [Properties](../../reference/ado-api/properties-collection-ado.md) del **conjunto de registros** para determinar si las propiedades dinámicas específicas del proveedor están presentes.

 **Disponibilidad de propiedades de conjunto de registros de ADO estándar:**

|Propiedad|Disponibilidad|
|--------------|------------------|
|[AbsolutePage](../../reference/ado-api/absolutepage-property-ado.md)|lectura/escritura|
|[AbsolutePosition](../../reference/ado-api/absoluteposition-property-ado.md)|lectura/escritura|
|[ActiveConnection](../../reference/ado-api/activeconnection-property-ado.md)|solo lectura|
|[BOF](../../reference/ado-api/bof-eof-properties-ado.md)|solo lectura|
|[Marcador](../../reference/ado-api/bookmark-property-ado.md)*|lectura/escritura|
|[CacheSize](../../reference/ado-api/cachesize-property-ado.md)|lectura/escritura|
|[CursorLocation](../../reference/ado-api/cursorlocation-property-ado.md)|siempre **adUseServer**|
|[CursorType](../../reference/ado-api/cursortype-property-ado.md)|siempre **adOpenStatic**|
|[EditMode](../../reference/ado-api/editmode-property.md)|siempre **adEditNone**|
|[ALCANZAR](../../reference/ado-api/bof-eof-properties-ado.md)|solo lectura|
|[Filter](../../reference/ado-api/filter-property.md)|lectura/escritura|
|[LockType](../../reference/ado-api/locktype-property-ado.md)|lectura/escritura|
|[MarshalOptions](../../reference/ado-api/marshaloptions-property-ado.md)|no disponible|
|[MaxRecords](../../reference/ado-api/maxrecords-property-ado.md)|lectura/escritura|
|[NúmeroDePáginas](../../reference/ado-api/pagecount-property-ado.md)|solo lectura|
|[PageSize](../../reference/ado-api/pagesize-property-ado.md)|lectura/escritura|
|[RecordCount](../../reference/ado-api/recordcount-property-ado.md)|solo lectura|
|[Origen](../../reference/ado-api/source-property-ado-recordset.md)|lectura/escritura|
|[Estado](../../reference/ado-api/state-property-ado.md)|solo lectura|
|[Estado](../../reference/ado-api/status-property-ado-recordset.md)|solo lectura|

 \*Los marcadores deben estar habilitados en el proveedor para que esta característica exista en el **conjunto de registros**.

 **Disponibilidad de métodos de conjunto de registros de ADO estándar:**

|Método|¿Disponible?|
|------------|----------------|
|[AgregarNuevo](../../reference/ado-api/addnew-method-ado.md)|No|
|[Cancelar](../../reference/ado-api/cancel-method-ado.md)|Sí|
|[CancelBatch](../../reference/ado-api/cancelbatch-method-ado.md)|No|
|[CancelUpdate](../../reference/ado-api/cancelupdate-method-ado.md)|No|
|[Clonar](../../reference/ado-api/clone-method-ado.md)|Sí|
|[Cerrar](../../reference/ado-api/close-method-ado.md)|Sí|
|[Eliminar](../../reference/ado-api/delete-method-ado-recordset.md)|No|
|[GetRows](../../reference/ado-api/getrows-method-ado.md)|Sí|
|[Mover](../../reference/ado-api/move-method-ado.md)|Sí|
|[MoveFirst](../../reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)|Sí|
|[NextRecordset](../../reference/ado-api/nextrecordset-method-ado.md)|Sí|
|[Abrir](../../reference/ado-api/open-method-ado-recordset.md)|Sí|
|[Requery](../../reference/ado-api/requery-method.md)|Sí|
|[Resincronizar](../../reference/ado-api/resync-method.md)|Sí|
|[Es compatible con](../../reference/ado-api/supports-method.md)|Sí|
|[Actualizar](../../reference/ado-api/update-method.md)|No|
|[UpdateBatch](../../reference/ado-api/updatebatch-method.md)|No|

 Para obtener detalles de implementación específicos e información funcional acerca del proveedor de Microsoft OLE DB para el servicio de indización de Microsoft, consulte la [Guía del programador de OLE DB](/previous-versions/windows/desktop/ms713643(v=vs.85))o visite la página servicios web del sitio web de Windows NT Server.

## <a name="see-also"></a>Consulte también
 [Propiedad CommandType (ADO](../../reference/ado-api/commandtype-property-ado.md) ) propiedad [ConnectionString (](../../reference/ado-api/connectionstring-property-ado.md) ADO) [colección de propiedades (](../../reference/ado-api/properties-collection-ado.md) ADO) [propiedad del proveedor (](../../reference/ado-api/provider-property-ado.md) ADO) [objeto Recordset (ADO)](../../reference/ado-api/recordset-object-ado.md) [admite el método](../../reference/ado-api/supports-method.md)