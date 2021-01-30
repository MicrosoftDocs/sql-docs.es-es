---
description: Resumen de funciones ODBC
title: Resumen de funciones ODBC | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- functions [ODBC], listed by task
ms.assetid: 7aa635da-e6b7-439f-8e9b-c3860e24de5e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d5d27a6093947ba4e29e43f4970848780cbf4e8e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174645"
---
# <a name="odbc-function-summary"></a>Resumen de funciones ODBC
En la tabla siguiente se enumeran las funciones ODBC, agrupadas por tipo de tarea, y se incluye la designación de conformidad y una breve descripción de la finalidad de cada función. Para obtener más información sobre las designaciones del cumplimiento, consulte [ODBC y la CLI estándar](../../../odbc/reference/odbc-and-the-standard-cli.md). Para obtener más información sobre la sintaxis y la semántica de cada función, vea referencia de la [API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md).  
  
 Una aplicación puede llamar a la función **SQLGetInfo** para obtener información de conformidad sobre un controlador. Para obtener información sobre la compatibilidad con una función específica en un controlador, una aplicación puede llamar a **SQLGetFunctions**.  
  
|Tarea|Nombre de la función|Conformidad|Propósito|  
|----------|-------------------|-----------------|-------------|  
|Conectarse a un origen de datos|[SQLAllocHandle](../../../odbc/reference/syntax/sqlallochandle-function.md)|ISO 92|Obtiene un entorno, una conexión, una instrucción o un identificador de descriptor.|  
||[SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md)|ISO 92|Se conecta a un controlador específico por nombre de origen de datos, identificador de usuario y contraseña.|  
||[SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)|ODBC|Se conecta a un controlador específico mediante una cadena de conexión o solicita que el administrador de controladores y los cuadros de diálogo de la conexión muestren el controlador del usuario.|  
||[SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md)|ODBC|Devuelve los niveles sucesivos de los atributos de conexión y los valores de atributo válidos. Cuando se ha especificado un valor para cada atributo de conexión, se conecta al origen de datos.|  
|Obtener información acerca de un controlador y un origen de datos|[SQLDataSources](../../../odbc/reference/syntax/sqldatasources-function.md)<br /><br /> [SQLDrivers](../../../odbc/reference/syntax/sqldrivers-function.md)|ISO 92<br /><br /> ODBC|Devuelve la lista de orígenes de datos disponibles.<br /><br /> Devuelve la lista de controladores instalados y sus atributos.|  
||[SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md)|ISO 92|Devuelve información acerca de un controlador y un origen de datos específicos.|  
||[SQLGetFunctions](../../../odbc/reference/syntax/sqlgetfunctions-function.md)|ISO 92|Devuelve funciones de controlador compatibles.|  
||[SQLGetTypeInfo](../../../odbc/reference/syntax/sqlgettypeinfo-function.md)|ISO 92|Devuelve información sobre los tipos de datos admitidos.|  
|Establecimiento y recuperación de atributos de controlador|[SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)<br /><br /> [SQLGetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|ISO 92<br /><br /> ISO 92|Establece un atributo de conexión.<br /><br /> Devuelve el valor de un atributo de conexión.|  
||[SQLSetEnvAttr](../../../odbc/reference/syntax/sqlsetenvattr-function.md)|ISO 92|Establece un atributo de entorno.|  
||[SQLGetEnvAttr](../../../odbc/reference/syntax/sqlgetenvattr-function.md)|ISO 92|Devuelve el valor de un atributo de entorno.|  
||[SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|ISO 92|Establece un atributo de instrucción.|  
||[SQLGetStmtAttr](../../../odbc/reference/syntax/sqlgetstmtattr-function.md)|ISO 92|Devuelve el valor de un atributo de instrucción.|  
|Establecimiento y recuperación de campos de descriptor|[SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md)<br /><br /> [SQLGetDescRec](../../../odbc/reference/syntax/sqlgetdescrec-function.md)|ISO 92<br /><br /> ISO 92|Devuelve el valor de un único campo descriptor.<br /><br /> Devuelve los valores de varios campos de descriptor.|  
||[SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|ISO 92|Establece un único campo descriptor.|  
||[SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md)|ISO 92|Establece varios campos de descriptor.|  
||[SQLCopyDesc](../../../odbc/reference/syntax/sqlcopydesc-function.md)|ISO 92|Copia la información de descriptor de un identificador de descriptor a otro.|  
|Preparación de las solicitudes SQL|[SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md)|ISO 92|Prepara una instrucción SQL para su ejecución posterior.|  
||[SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)|ODBC|Asigna almacenamiento para un parámetro en una instrucción SQL.|  
||[SQLGetCursorName](../../../odbc/reference/syntax/sqlgetcursorname-function.md)|ISO 92|Devuelve el nombre del cursor asociado a un identificador de instrucción.|  
||[SQLSetCursorName](../../../odbc/reference/syntax/sqlsetcursorname-function.md)|ISO 92|Especifica un nombre de cursor.|  
||[SQLSetScrollOptions](../../../odbc/reference/syntax/sqlsetscrolloptions-function.md)|ODBC|Establece las opciones que controlan el comportamiento del cursor.|  
|Enviar solicitudes|[SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)<br /><br /> [SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|ISO 92<br /><br /> ISO 92|Ejecuta una instrucción preparada.<br /><br /> Ejecuta una instrucción.|  
||[SQLNativeSql](../../../odbc/reference/syntax/sqlnativesql-function.md)|ODBC|Devuelve el texto de una instrucción SQL tal y como lo traduce el controlador.|  
||[SQLDescribeParam](../../../odbc/reference/syntax/sqldescribeparam-function.md)|ODBC|Devuelve la descripción de un parámetro concreto en una instrucción.|  
||[SQLNumParams](../../../odbc/reference/syntax/sqlnumparams-function.md)|ISO 92|Devuelve el número de parámetros de una instrucción.|  
||[SQLParamData](../../../odbc/reference/syntax/sqlparamdata-function.md)|ISO 92|Se usa junto con **SQLPutData** para proporcionar datos de parámetros en tiempo de ejecución. (Útil para los valores de datos largos).|  
||[SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)|ISO 92|Envía parte o todo un valor de datos para un parámetro. (Útil para los valores de datos largos).|  
|Recuperar resultados e información sobre los resultados|[SQLRowCount](../../../odbc/reference/syntax/sqlrowcount-function.md)<br /><br /> [SQLNumResultCols](../../../odbc/reference/syntax/sqlnumresultcols-function.md)|ISO 92<br /><br /> ISO 92|Devuelve el número de filas afectadas por una solicitud INSERT, Update o DELETE.<br /><br /> Devuelve el número de columnas del conjunto de resultados.|  
||[SQLDescribeCol](../../../odbc/reference/syntax/sqldescribecol-function.md)|ISO 92|Describe una columna en el conjunto de resultados.|  
||[SQLColAttribute](../../../odbc/reference/syntax/sqlcolattribute-function.md)|ISO 92|Describe los atributos de una columna en el conjunto de resultados.|  
||[SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|ISO 92|Asigna almacenamiento para una columna de resultados y especifica el tipo de datos.|  
||[SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|ISO 92|Devuelve varias filas de resultados.|  
||[SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|ISO 92|Devuelve las filas de resultados desplazables.|  
||[SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)|ISO 92|Devuelve parte o toda una columna de una fila de un conjunto de resultados. (Útil para los valores de datos largos).|  
||[SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md)|ODBC|Coloca un cursor dentro de un bloque de datos capturado y permite que una aplicación actualice los datos del conjunto de filas o actualice o elimine los datos del conjunto de resultados.|  
||[SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md)|ODBC|Realiza inserciones masivas y operaciones de marcador masivo, como Update, delete y fetch por marcador.|  
||[SQLMoreResults](../../../odbc/reference/syntax/sqlmoreresults-function.md)|ODBC|Determina si hay más conjuntos de resultados disponibles y, si es así, inicializa el procesamiento para el siguiente conjunto de resultados.|  
||[SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md)|ISO 92|Devuelve información de diagnóstico adicional (un campo único de la estructura de datos de diagnóstico).|  
||[SQLGetDiagRec](../../../odbc/reference/syntax/sqlgetdiagrec-function.md)|ISO 92|Devuelve información de diagnóstico adicional (varios campos de la estructura de datos de diagnóstico).|  
|Obtener información acerca de las tablas del sistema del origen de datos (funciones de catálogo)|[SQLColumnPrivileges](../../../odbc/reference/syntax/sqlcolumnprivileges-function.md)<br /><br /> [SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md)|ODBC<br /><br /> Abrir grupo|Devuelve una lista de columnas y privilegios asociados para una o más tablas.<br /><br /> Devuelve la lista de nombres de columna de las tablas especificadas.|  
||[SQLForeignKeys](../../../odbc/reference/syntax/sqlforeignkeys-function.md)|ODBC|Devuelve una lista de nombres de columna que constituyen claves externas, si existen para una tabla especificada.|  
||[SQLPrimaryKeys](../../../odbc/reference/syntax/sqlprimarykeys-function.md)|ODBC|Devuelve la lista de nombres de columna que constituyen la clave principal de una tabla.|  
||[SQLProcedureColumns](../../../odbc/reference/syntax/sqlprocedurecolumns-function.md)|ODBC|Devuelve la lista de parámetros de entrada y salida, así como las columnas que componen el conjunto de resultados para los procedimientos especificados.|  
||[SQLProcedures](../../../odbc/reference/syntax/sqlprocedures-function.md)|ODBC|Devuelve la lista de nombres de procedimientos almacenados en un origen de datos concreto.|  
||[SQLSpecialColumns](../../../odbc/reference/syntax/sqlspecialcolumns-function.md)|Abrir grupo|Devuelve información sobre el conjunto óptimo de columnas que identifica de forma única una fila de una tabla especificada, o las columnas que se actualizan automáticamente cuando una transacción actualiza cualquier valor de la fila.|  
||[SQLStatistics](../../../odbc/reference/syntax/sqlstatistics-function.md)|ISO 92|Devuelve estadísticas sobre una sola tabla y la lista de índices asociados a la tabla.|  
||[SQLTablePrivileges](../../../odbc/reference/syntax/sqltableprivileges-function.md)|ODBC|Devuelve una lista de tablas y los privilegios asociados a cada tabla.|  
||[SQLTables](../../../odbc/reference/syntax/sqltables-function.md)|Abrir grupo|Devuelve la lista de nombres de tabla almacenada en un origen de datos concreto.|  
|Finalizar una instrucción|[SQLFreeStmt](../../../odbc/reference/syntax/sqlfreestmt-function.md)|ISO 92|Finaliza el procesamiento de instrucciones, descarta los resultados pendientes y, opcionalmente, libera todos los recursos asociados al identificador de instrucción.|  
||[SQLCloseCursor](../../../odbc/reference/syntax/sqlclosecursor-function.md)|ISO 92|Cierra un cursor que se ha abierto en un identificador de instrucción.|  
||[SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|ISO 92|Cancela el procesamiento en una instrucción.|  
||[SQLCancelHandle](../../../odbc/reference/syntax/sqlcancelhandle-function.md)|ODBC|Cancela el procesamiento de una instrucción o una conexión.|  
||[SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md)|ISO 92|Confirma o revierte una transacción.|  
|Finalización de una conexión|[SQLDisconnect](../../../odbc/reference/syntax/sqldisconnect-function.md)<br /><br /> [SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md)|ISO 92<br /><br /> ISO 92|Cierra la conexión.<br /><br /> Libera un entorno, una conexión, una instrucción o un identificador de descriptor.|
