---
description: Ejecutar SQL Server Native Client comandos que contienen parámetros de Table-Valued
title: Comandos con parámetros de Table-Valued
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- table-valued parameters, executing commands containing
ms.assetid: 7ecba6f6-fe7a-462a-9aa3-d5115b6d4529
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0d1c2e6d2ecf739f63f00702f55f653d14ededc1
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97419160"
---
# <a name="executing-sql-server-native-client-commands-containing-table-valued-parameters"></a>Ejecutar SQL Server Native Client comandos que contienen parámetros de Table-Valued
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  La ejecución de un comando que contiene parámetros con valores de tabla consta de dos fases:  
  
1.  Especificar los tipos de parámetros.  
  
2.  Enlazar los datos de los parámetros.  

## <a name="table-valued-parameter-specification"></a>Especificación de parámetros con valores de tabla  
 El consumidor puede especificar el tipo del parámetro con valores de tabla. Esta información incluye el nombre del tipo del parámetro con valores de tabla. También incluye el nombre de esquema si el tipo de tabla definido por el usuario para el parámetro con valores de tabla no está en el esquema predeterminado actual para la conexión. Dependiendo de la compatibilidad del servidor, el consumidor también puede especificar información opcional sobre los metadatos, como el orden de las columnas, y puede especificar que todas las filas de ciertas columnas tengan valores predeterminados.  
  
 Para especificar un parámetro con valores de tabla, el consumidor llama a ISSCommandWithParameter::SetParameterInfo y, opcionalmente, llama a ISSCommandWithParameters::SetParameterProperties. En el caso de un parámetro con valores de tabla, el campo *pwszDataSourceType* de la estructura DBPARAMBINDINFO tiene un valor de DBTYPE_TABLE. El campo *ulParamSize* se establece en ~0 para indicar que no se conoce la longitud. Algunas propiedades de los parámetros con valores de tabla, como nombre de esquema, nombre de tipo, orden de columnas y columnas predeterminadas, se pueden establecer mediante ISSCommandWithParameters::SetParameterProperties.  
  
## <a name="table-valued-parameter-binding"></a>Enlace de parámetros con valores de tabla  
 Un parámetro con valores de tabla puede ser cualquier objeto de conjunto de filas. Durante la ejecución, el proveedor lee en este objeto mientras envía los parámetros con valores de tabla al servidor.  
  
 Para enlazar el parámetro con valores de tabla, el consumidor llama a IAccessor::CreateAccessor. El campo *wType* de la estructura DBBINDING del parámetro con valores de tabla se establece en DBTYPE_TABLE. El miembro *pObject* de la estructura DBBINDING es distinto de NULL y el miembro *iid* de *pObject* se establece en IID_IRowset o cualquier otra interfaz de objeto de conjunto de filas de parámetro con valores de tabla. Los campos restantes de la estructura DBBINDING se deben establecer de la misma manera que los BLOB transmitidos.  
  
 En los enlaces para el parámetro con valores de tabla y el objeto de conjunto de filas asociado a un parámetro con valores de tabla, se aplican las restricciones siguientes:  
  
-   Los únicos valores de estado permitidos para los datos de columnas de conjunto de filas de parámetro con valores de tabla son DBSTATUS_S_ISNULL y DBSTATUS_S_OK. DBSTATUS_S_DEFAULT producirá un error y el valor de estado enlazado se establecerá en DBSTATUS_E_BADSTATUS.  
  
-   Un parámetro con valores de tabla se puede marcar con el estado DBSTATUS_S_DEFAULT. Los únicos valores válidos son DBSTATUS_S_DEFAULT y DBSTATUS_S_OK. Cuando el estado se establece en DBSTATUS_S_DEFAULT, el valor del parámetro con valores de tabla corresponde a una tabla vacía.  
  
-   Las columnas de solo lectura en parámetros con valores de tabla (columnas de identidad o calculadas) se deben marcar como predeterminadas utilizando la propiedad SSPROP_PARAM_TABLE_DEFAULT_COLUMNS. Las columnas que tienen un valor predeterminado también se deben marcar como predeterminadas mediante la propiedad SSPROP_PARAM_TABLE_DEFAULT_COLUMNS para que se pueda utilizar el valor predeterminado para los valores de datos de la columna para un parámetro con valores de tabla determinado. El proveedor no tendrá en cuenta los valores de datos enlazados para las columnas marcadas como predeterminadas.  
  
-   Los datos se enviarán al servidor para las columnas con DBPROP_COL_AUTOINCREMENT o SSPROP_COL_COMPUTED, a menos que también se establezca SSPROP_PARAM_TABLE_DEFAULT.  
  
## <a name="see-also"></a>Consulte también  
 [Parámetros con valores de tabla &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-table-valued-parameters/table-valued-parameters-ole-db.md)   
 [Usar parámetros con valores de tabla &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-how-to/use-table-valued-parameters-ole-db.md)  
  
  
