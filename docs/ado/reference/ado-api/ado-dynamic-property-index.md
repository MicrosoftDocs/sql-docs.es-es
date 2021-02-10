---
description: Índice de propiedades dinámicas de ADO
title: Índice de propiedades dinámicas de ADO | Microsoft Docs
ms.prod: sql
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- dynamic properties [ADO], index
ms.assetid: 80d389dd-46ef-459f-b0d4-6f712fc4f32d
author: rothja
ms.author: jroth
ms.openlocfilehash: 0a074ca1e5f50e9d9b634fd67cfceeae353c436e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100035757"
---
# <a name="ado-dynamic-property-index"></a>Índice de propiedades dinámicas de ADO
Los proveedores de datos, los proveedores de servicios y los componentes de servicio pueden agregar propiedades dinámicas a las colecciones de **propiedades** de los objetos [Connection](./connection-object-ado.md) y [Recordset](./recordset-object-ado.md) no abiertos. Un proveedor determinado también puede insertar propiedades adicionales cuando se abren estos objetos. Algunas de estas propiedades se muestran en la sección [propiedades dinámicas de ADO](./ado-dynamic-properties.md) . Encontrará más información en los proveedores específicos de la sección [Apéndice A: proveedores](../../guide/appendixes/appendix-a-providers.md) .  
  
 Las tablas siguientes son índices cruzados de los nombres de ADO y OLE DB para cada propiedad dinámica de proveedor de OLE DB estándar. Los proveedores pueden agregar más propiedades de las que se muestran aquí. Para obtener información específica sobre las propiedades dinámicas específicas del proveedor, consulte la documentación del proveedor.  
  
 La referencia del programador de OLE DB hace referencia a un nombre de propiedad ADO por el término "Descripción". Para obtener más información acerca de estas propiedades estándar, busque o examine el índice en la [documentación de OLE DB](/previous-versions/windows/desktop/ms722784(v=vs.85))para la propiedad OLE DB por su nombre.  
  
## <a name="connection-dynamic-properties"></a>Propiedades dinámicas de conexión  
  
|Nombre de la propiedad ADO|OLE DB nombre de propiedad|  
|-----------------------|--------------------------|  
|Sesiones activas|DBPROP_ACTIVESESSIONS|  
|Anulación de asíncrona|DBPROP_ASYNCTXNABORT|  
|Confirmación de asíncrona|DBPROP_ASYNCTNXCOMMIT|  
|Niveles de aislamiento de confirmación automática|DBPROP_SESS_AUTOCOMMITISOLEVELS|  
|Ubicación del catálogo|DBPROP_CATALOGLOCATION|  
|Término de catálogo|DBPROP_CATALOGTERM|  
|Definición de columna|DBPROP_COLUMNDEFINITION|  
|Tiempo de espera de la conexión|DBPROP_INIT_TIMEOUT|  
|Catálogo actual|DBPROP_CURRENTCATALOG|  
|Origen de datos|DBPROP_INIT_DATASOURCE|  
|Data Source Name|DBPROP_DATASOURCENAME|  
|Modelo de subprocesos de objetos de origen de datos|DBPROP_DSOTHREADMODEL|  
|Nombre DBMS|DBPROP_DBMSNAME|  
|Versión DBMS|DBPROP_DBMSVER|  
|Propiedades extendidas|DBPROP_INIT_PROVIDERSTRING|  
|AGRUPAr por compatibilidad|DBPROP_GROUPBY|  
|Compatibilidad con tablas heterogéneas|DBPROP_HETEROGENEOUSTABLES|  
|Distinción de mayúsculas y minúsculas de identificadores|DBPROP_IDENTIFIERCASE|  
|Catálogo original|DBPROP_INIT_CATALOG|  
|Niveles de aislamiento|DBPROP_SUPPORTEDTXNISOLEVELS|  
|Retención de aislamiento|DBPROP_SUPPORTEDTXNISORETAIN|  
|Identificador de configuración regional|DBPROP_INIT_LCID|  
|Location|DBPROP_INIT_LOCATION|  
|Tamaño máximo del índice|DBPROP_MAXINDEXSIZE|  
|Tamaño máximo de fila|DBPROP_MAXROWSIZE|  
|El tamaño máximo de fila incluye BLOB|DBPROP_MAXROWSIZEINCLUDESBLOB|  
|Número máximo de tablas en SELECT|DBPROP_MAXTABLESINSELECT|  
|Modo|DBPROP_INIT_MODE|  
|Varios conjuntos de parámetros|DBPROP_MULTIPLEPARAMSETS|  
|Varios resultados|DBPROP_MULTIPLERESULTS|  
|Varios objetos de almacenamiento|DBPROP_MULTIPLESTORAGEOBJECTS|  
|Actualización de varias tablas|DBPROP_MULTITABLEUPDATE|  
|Orden de intercalación NULL|DBPROP_NULLCOLLATION|  
|Comportamiento de concatenación NULL|DBPROP_CONCATNULLBEHAVIOR|  
|Servicios OLE DB|DBPROP_INIT_OLEDBSERVICES|  
|Versión de OLE DB|DBPROP_PROVIDEROLEDBVER|  
|Compatibilidad con objetos OLE|DBPROP_OLEOBJECTS|  
|Compatibilidad con abrir conjuntos de filas|DBPROP_OPENROWSETSUPPORT|  
|ORDENAR por columnas en la lista de selección|DBPROP_ORDERBYCOLUMNSINSELECT|  
|Disponibilidad del parámetro de salida|DBPROP_OUTPUTPARAMETERAVAILABILITY|  
|Descriptores de acceso pass by Ref|DBPROP_BYREFACCESSORS|  
|Contraseña|DBPROP_AUTH_PASSWORD|  
|Persist Security Info|DBPROP_AUTH_PERSIST_SENSITIVE_AUTHINFO|  
|Tipo de ID. persistente|DBPROP_PERSISTENTIDTYPE|  
|Preparar el comportamiento de anulación|DBPROP_PREPAREABORTBEHAVIOR|  
|Preparar el comportamiento de confirmación|DBPROP_PREPARECOMMITBEHAVIOR|  
|Término del procedimiento|DBPROP_PROCEDURETERM|  
|Solicitud|DBPROP_INIT_PROMPT|  
|Nombre descriptivo del proveedor|DBPROP_PROVIDERFRIENDLYNAME|  
|Nombre del proveedor|DBPROP_PROVIDERFILENAME|  
|Versión del proveedor|DBPROP_PROVIDERVER|  
|Read-Only origen de datos|DBPROP_DATASOURCEREADONLY|  
|Conversiones del conjunto de filas en el comando|DBPROP_ROWSETCONVERSIONSONCOMMAND|  
|Término de esquema|DBPROP_SCHEMATERM|  
|Uso de esquemas|DBPROP_SCHEMAUSAGE|  
|Soporte técnico de SQL|DBPROP_SQLSUPPORT|  
|Almacenamiento estructurado|DBPROP_STRUCTUREDSTORAGE|  
|Compatibilidad con subconsultas|DBPROP_SUBQUERIES|  
|Término de tabla|DBPROP_TABLETERM|  
|DDL de transacción|DBPROP_SUPPORTEDTXNDDL|  
|Id. de usuario|DBPROP_AUTH_USERID|  
|Nombre de usuario|DBPROP_USERNAME|  
|Identificador de ventana|DBPROP_INIT_HWND|  
  
## <a name="recordset-dynamic-properties"></a>Propiedades dinámicas del conjunto de registros  
 Tenga en cuenta que las **propiedades dinámicas** del objeto de **conjunto de registros** salen del ámbito (no están disponibles) cuando se cierra el **conjunto de registros** .  
  
|Nombre de la propiedad ADO|OLE DB nombre de propiedad|  
|-----------------------|--------------------------|  
|IAccessor|DBPROP_IACCESSOR|  
|IChapteredRowset||  
|IColumnsInfo|DBPROP_ICOLUMNSINFO|  
|IColumnsRowset|DBPROP_ICOLUMNSROWSET|  
|IConnectionPointContainer|DBPROP_ICONNECTIONPOINTCONTAINER|  
|IConvertType||  
|ILockBytes|DBPROP_ILOCKBYTES|  
|IRowset|DBPROP_IROWSET|  
|IDBAsynchStatus|DBPROP_IDBASYNCHSTATUS|  
|IParentRowset||  
|IRowsetChange|DBPROP_IROWSETCHANGE|  
|IRowsetExactScroll||  
|IRowsetFind|DBPROP_IROWSETFIND|  
|IRowsetIdentity|DBPROP_IROWSETIDENTITY|  
|IRowsetInfo|DBPROP_IROWSETINFO|  
|IRowsetLocate|DBPROP_IROWSETLOCATE|  
|IRowsetRefresh|DBPROP_IROWSETREFRESH|  
|IRowsetResynch||  
|IRowsetScroll|DBPROP_IROWSETSCROLL|  
|IRowsetUpdate|DBPROP_IROWSETUPDATE|  
|IRowsetView|DBPROP_IROWSETVIEW|  
|IRowsetIndex|DBPROP_IROWSETINDEX|  
|ISequentialStream|DBPROP_ISEQUENTIALSTREAM|  
|IStorage|DBPROP_ISTORAGE|  
|IStream|DBPROP_ISTREAM|  
|ISupportErrorInfo|DBPROP_ISUPPORTERRORINFO|  
|Orden de acceso|DBPROP_ACCESSORDER|  
|Conjunto de filas Append-Only|DBPROP_APPENDONLY|  
|Procesamiento de conjuntos de filas asíncronos|DBPROP_ROWSET_ASYNCH|  
|Recalc automático|DBPROP_ADC_AUTORECALC|  
|Tamaño de captura en segundo plano|DBPROP_ASYNCHFETCHSIZE|  
|Prioridad de los subprocesos en segundo plano|DBPROP_ASYNCHTHREADPRIORITY|  
|Tamaño de lote|DBPROP_ADC_BATCHSIZE|  
|Bloquear objetos de almacenamiento|DBPROP_BLOCKINGSTORAGEOBJECTS|  
|Tipo de marcador|DBPROP_BOOKMARKTYPE|  
|Marcador|DBPROP_IROWSETLOCATE|  
|Marcadores ordenados|DBPROP_ORDEREDBOOKMARKS|  
|Almacenar en caché filas secundarias|DBPROP_ADC_CACHECHILDROWS|  
|Columnas diferidas de caché|DBPROP_CACHEDEFERRED|  
|Cambiar filas insertadas|DBPROP_CHANGEINSERTEDROWS|  
|Privilegios de columna|DBPROP_COLUMNRESTRICT|  
|Notificación de conjunto de columnas|DBPROP_NOTIFYCOLUMNSET|  
|Columna grabable|DBPROP_MAYWRITECOLUMN|  
|Tiempo de espera del comando|DBPROP_COMMANDTIMEOUT|  
|Versión del motor de cursores|DBPROP_ADC_CEVER|  
|Diferir columna|DBPROP_DEFERRED|  
|Retraso de las actualizaciones de objetos de almacenamiento|DBPROP_DELAYSTORAGEOBJECTS|  
|Capturar hacia atrás|DBPROP_CANFETCHBACKWARDS|  
|Operaciones de filtro|DBPROP_FILTERCOMPAREOPS|  
|Operaciones de búsqueda|DBPROP_FINDCOMPAREOPS|  
|Columnas ocultas (recuento)|DBPROP_HIDDENCOLUMNS|  
|Almacenar filas|DBPROP_CANHOLDROWS|  
|Filas immóviles|DBPROP_IMMOBILEROWS|  
|Tamaño de captura inicial|DBPROP_ASYNCHPREFETCHSIZE|  
|Marcadores literales|DBPROP_LITERALBOOKMARKS|  
|Identidad de fila literal|DBPROP_LITERALIDENTITY|  
|Mantener el estado de los cambios|DBPROP_ADC_MAINTAINCHANGESTATUS|  
|Número máximo de filas abiertas|DBPROP_MAXOPENROWS|  
|Número máximo de filas pendientes|DBPROP_MAXPENDINGROWS|  
|Número máximo de filas|DBPROP_MAXROWS|  
|Uso de la memoria|DBPROP_MEMORYUSAGE|  
|Granularidad de notificaciones|DBPROP_NOTIFICATIONGRANULARITY|  
|Fases de notificación|DBPROP_NOTIFICATIONPHASES|  
|Objetos con transacciones|DBPROP_TRANSACTEDOBJECT|  
|Cambios visibles en otros usuarios|DBPROP_OTHERUPDATEDELETE|  
|Inserciones visibles de otros usuarios|DBPROP_OTHERINSERT|  
|Propios cambios visibles|DBPROP_OWNUPDATEDELETE|  
|Propias inserciones visibles|DBPROP_OWNINSERT|  
|Conservar al anular|DBPROP_ABORTPRESERVE|  
|Conservar al confirmar|DBPROP_COMMITPRESERVE|  
|Private1||  
|Reinicio rápido|DBPROP_QUICKRESTART|  
|Eventos reentrantes|DBPROP_REENTRANTEVENTS|  
|Quitar filas eliminadas|DBPROP_REMOVEDELETED|  
|Informar de varios cambios|DBPROP_REPORTMULTIPLECHANGES|  
|Nombre de la reformación|DBPROP_ADC_RESHAPENAME|  
|Comando Resync|DBPROP_ADC_CUSTOMRESYNCH|  
|Devolver inserciones pendientes|DBPROP_RETURNPENDINGINSERTS|  
|Notificación de eliminación de fila|DBPROP_NOTIFYROWDELETE|  
|Notificación de cambio de primera fila|DBPROP_NOTIFYROWFIRSTCHANGE|  
|Notificación de inserción de fila|DBPROP_NOTIFYROWINSERT|  
|Privilegios de fila|DBPROP_ROWRESTRICT|  
|Notificación de resincronización de filas|DBPROP_NOTIFYROWRESYNCH|  
|Modelo de subprocesos de filas|DBPROP_ROWTHREADMODEL|  
|Notificación de cambio de deshacer filas|DBPROP_NOTIFYROWUNDOCHANGE|  
|Notificación de eliminación de fila deshacer|DBPROP_NOTIFYROWUNDODELETE|  
|Anular la notificación de inserción de fila|DBPROP_NOTIFYROWUNDOINSERT|  
|Notificación de actualización de fila|DBPROP_NOTIFYROWUPDATE|  
|Notificación de cambio de posición de captura de conjunto de filas|DBPROP_NOTIFYROWSETFETCHPOSITIONCHANGE|  
|Notificación de versión del conjunto de filas|DBPROP_NOTIFYROWSETRELEASE|  
|Desplazarse hacia atrás|DBPROP_CANSCROLLBACKWARDS|  
|Cursor de servidor|DBPROP_SERVERCURSOR|  
|Omitir marcadores eliminados|DBPROP_BOOKMARKSKIPPED|  
|Identidad de fila segura|DBPROP_STRONGIDENTITY|  
|Catálogo único|DBPROP_ADC_UNIQUECATALOG|  
|Filas únicas|DBPROP_UNIQUEROWS|  
|Esquema único|DBPROP_ADC_UNIQUESCHEMA|  
|Tabla única|DBPROP_ADC_UNIQUETABLE|  
|Actualización|DBPROP_UPDATABILITY|  
|Criterios de actualización|DBPROP_ADC_UPDATECRITERIA|  
|Actualizar resincronización|DBPROP_ADC_UPDATERESYNC|  
|Usar marcadores|DBPROP_BOOKMARKS|