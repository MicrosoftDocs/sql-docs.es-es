---
description: Miembros SQLServerDatabaseMetaData
title: Miembros SQLServerDatabaseMetaData | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 327ba0bc-438a-494c-b119-1cd4a096bb58
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4fe11f6a3377d0a26941138b8a581f642a6b0755
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172787"
---
# <a name="sqlserverdatabasemetadata-members"></a>Miembros SQLServerDatabaseMetaData
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  En las siguientes tablas se enumeran los miembros que expone la clase [SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md).  
  
## <a name="constructors"></a>Constructores  
 Ninguno.  
  
## <a name="fields"></a>Fields  
 Ninguno.  
  
## <a name="inherited-fields"></a>Campos heredados  
  
|NOMBRE|Descripción|  
|----------|-----------------|  
|java.sql.DatabaseMetaData|attributeNoNulls, attributeNullable, attributeNullableUnknown, bestRowNotPseudo, bestRowPseudo, bestRowSession, bestRowTemporary, bestRowTransaction, bestRowUnknown, columnNoNulls, columnNullable, columnNullableUnknown, importedKeyCascade, importedKeyInitiallyDeferred, importedKeyInitiallyImmediate, importedKeyNoAction, importedKeyNotDeferrable, importedKeyRestrict, importedKeySetDefault, importedKeySetNull, procedureColumnIn, procedureColumnInOut, procedureColumnOut, procedureColumnResult, procedureColumnReturn, procedureColumnUnknown, procedureNoNulls, procedureNoResult, procedureNullable, procedureNullableUnknown, procedureResultUnknown, procedureReturnsResult, sqlStateSQL, sqlStateSQL99, sqlStateXOpen, tableIndexClustered, tableIndexHashed, tableIndexOther, tableIndexStatistic, typeNoNulls, typeNullable, typeNullableUnknown, typePredBasic, typePredChar, typePredNone, typeSearchable, versionColumnNotPseudo, versionColumnPseudo, versionColumnUnknown|  
  
## <a name="methods"></a>Métodos  
  
|Nombre|Descripción|  
|----------|-----------------|  
|[allProceduresAreCallable](../../../connect/jdbc/reference/allproceduresarecallable-method-sqlserverdatabasemetadata.md)|Recupera si el usuario actual tiene los permisos para llamar a todos los procedimientos que devuelve el método [getProcedures](../../../connect/jdbc/reference/getprocedures-method-sqlserverdatabasemetadata.md).|  
|[allTablesAreSelectable](../../../connect/jdbc/reference/alltablesareselectable-method-sqlserverdatabasemetadata.md)|Recupera si el usuario actual tiene los permisos para utilizar todas las tablas que devuelve el método [getTables](../../../connect/jdbc/reference/gettables-method-sqlserverdatabasemetadata.md) en una instrucción SELECT.|  
|[autoCommitFailureClosesAllResultSets](../../../connect/jdbc/reference/autocommitfailureclosesallresultsets-method-sqlserverdatabasemetadata.md)|Indica si el controlador JDBC cierra todos los conjuntos de resultados abiertos, incluso los que se pueden retener, cuando se habilita la confirmación automática y se produce una excepción.|  
|[dataDefinitionCausesTransactionCommit](../../../connect/jdbc/reference/datadefinitioncausestransactioncommit-method-sqlserverdatabasemetadata.md)|Recupera si una instrucción de definición de datos dentro de una transacción obliga a la transacción a confirmarse.|  
|[dataDefinitionIgnoredInTransactions](../../../connect/jdbc/reference/datadefinitionignoredintransactions-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos omite una instrucción de definición de datos dentro de una transacción.|  
|[deletesAreDetected](../../../connect/jdbc/reference/deletesaredetected-method-sqlserverdatabasemetadata.md)|Recupera si se puede detectar una eliminación de filas visible mediante una llamada al método [rowDeleted](../../../connect/jdbc/reference/rowdeleted-method-sqlserverresultset.md) de la clase [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[doesMaxRowSizeIncludeBlobs](../../../connect/jdbc/reference/doesmaxrowsizeincludeblobs-method-sqlserverdatabasemetadata.md)|Recupera si el valor devuelto para el método [getMaxRowSize](../../../connect/jdbc/reference/getmaxrowsize-method-sqlserverdatabasemetadata.md) incluye los tipos de datos LONGVARCHAR y LONGVARBINARY de SQL.|  
|[getAttributes](../../../connect/jdbc/reference/getattributes-method-sqlserverdatabasemetadata.md)|Recupera una descripción del atributo determinado del tipo determinado para un tipo definido por el usuario que está disponible en el esquema y catálogos determinados.|  
|[getBestRowIdentifier](../../../connect/jdbc/reference/getbestrowidentifier-method-sqlserverdatabasemetadata.md)|Recupera una descripción del conjunto óptimo de columnas de una tabla que identifique una fila de forma única.|  
|[getCatalogs](../../../connect/jdbc/reference/getcatalogs-method-sqlserverdatabasemetadata.md)|Recupera los nombres del catálogo que están disponibles en el servidor conectado.|  
|[getCatalogSeparator](../../../connect/jdbc/reference/getcatalogseparator-method-sqlserverdatabasemetadata.md)|Recupera el objeto **String** que esta base de datos emplea como separador entre un nombre de catálogo y de tabla.|  
|[getCatalogTerm](../../../connect/jdbc/reference/getcatalogterm-method-sqlserverdatabasemetadata.md)|Recupera el término preferido del proveedor de la base de datos para "catálogo".|  
|[getClientInfoProperties](../../../connect/jdbc/reference/getclientinfoproperties-method-sqlserverdatabasemetadata.md)|Recupera una lista de las propiedades de la información de cliente que admite el controlador.|  
|[getColumnPrivileges](../../../connect/jdbc/reference/getcolumnprivileges-method-sqlserverdatabasemetadata.md)|Recupera una descripción de los derechos de acceso para las columnas en una tabla.|  
|[getColumns](../../../connect/jdbc/reference/getcolumns-method-sqlserverdatabasemetadata.md)|Recupera una descripción de las columnas de la tabla que están disponibles en el catálogo especificado.|  
|[getConnection](../../../connect/jdbc/reference/getconnection-method-sqlserverdatabasemetadata.md)|Recupera la conexión que generó este objeto de metadatos.|  
|[getCrossReference](../../../connect/jdbc/reference/getcrossreference-method-sqlserverdatabasemetadata.md)|Recupera una descripción de las columnas de clave externa en la tabla de clave externa determinada que hace referencia a las columnas de clave principal de la tabla de claves principales determinada.|  
|[getDatabaseMajorVersion](../../../connect/jdbc/reference/getdatabasemajorversion-method-sqlserverdatabasemetadata.md)|Recupera el número de versión principal de la base de datos subyacente.|  
|[getDatabaseMinorVersion](../../../connect/jdbc/reference/getdatabaseminorversion-method-sqlserverdatabasemetadata.md)|Recupera el número de versión secundaria de la base de datos subyacente.|  
|[getDatabaseProductName](../../../connect/jdbc/reference/getdatabaseproductname-method-sqlserverdatabasemetadata.md)|Recupera el nombre de este producto de base de datos.|  
|[getDatabaseProductVersion](../../../connect/jdbc/reference/getdatabaseproductversion-method-sqlserverdatabasemetadata.md)|Recupera el número de versión de este producto de base de datos.|  
|[getDefaultTransactionIsolation](../../../connect/jdbc/reference/getdefaulttransactionisolation-method-sqlserverdatabasemetadata.md)|Recupera el nivel de aislamiento de transacción predeterminado de esta base de datos.|  
|[getDriverMajorVersion](../../../connect/jdbc/reference/getdrivermajorversion-method-sqlserverdatabasemetadata.md)|Recupera el número de versión principal de este controlador JDBC.|  
|[getDriverMinorVersion](../../../connect/jdbc/reference/getdriverminorversion-method-sqlserverdatabasemetadata.md)|Recupera el número de versión secundaria de este controlador JDBC.|  
|[getDriverName](../../../connect/jdbc/reference/getdrivername-method-sqlserverdatabasemetadata.md)|Recupera el nombre de este controlador JDBC.|  
|[getDriverVersion](../../../connect/jdbc/reference/getdriverversion-method-sqlserverdatabasemetadata.md)|Recupera el número de versión de este controlador JDBC.|  
|[getExportedKeys](../../../connect/jdbc/reference/getexportedkeys-method-sqlserverdatabasemetadata.md)|Recupera una descripción de las columnas de clave externa que hacen referencia a las columnas de clave principal de la tabla determinada.|  
|[getExtraNameCharacters](../../../connect/jdbc/reference/getextranamecharacters-method-sqlserverdatabasemetadata.md)|Recupera todos los caracteres adicionales que se pueden utilizar en nombres de identificador sin comillas, por ejemplo, aquellos que no sean a-z, A-Z, 0-9 y _.|  
|[getFunctions](../../../connect/jdbc/reference/getfunctions-method-sqlserverdatabasemetadata.md)|Recupera una descripción de las funciones de usuario y del sistema.|  
|[getFunctionColumns](../../../connect/jdbc/reference/getfunctioncolumns-method-sqlserverdatabasemetadata.md)|Recupera una descripción de los parámetros de las funciones del sistema o de usuario del catálogo y del tipo de devolución.|  
|[getIdentifierQuoteString](../../../connect/jdbc/reference/getidentifierquotestring-method-sqlserverdatabasemetadata.md)|Recupera el objeto **String** que se utiliza para entrecomillar los identificadores de SQL.|  
|[getImportedKeys](../../../connect/jdbc/reference/getimportedkeys-method-sqlserverdatabasemetadata.md)|Recupera una descripción de las columnas de clave principal a las que hacen referencia las columnas de clave externa de la tabla.|  
|[getIndexInfo](../../../connect/jdbc/reference/getindexinfo-method-sqlserverdatabasemetadata.md)|Recupera una descripción de los índices y estadísticas de la tabla determinada.|  
|[getJDBCMajorVersion](../../../connect/jdbc/reference/getjdbcmajorversion-method-sqlserverdatabasemetadata.md)|Recupera el número de versión principal de JDBC para este controlador.|  
|[getJDBCMinorVersion](../../../connect/jdbc/reference/getjdbcminorversion-method-sqlserverdatabasemetadata.md)|Recupera el número de versión secundaria de JDBC para este controlador.|  
|[getMaxBinaryLiteralLength](../../../connect/jdbc/reference/getmaxbinaryliterallength-method-sqlserverdatabasemetadata.md)|Recupera el número máximo de caracteres hexadecimales que esta base de datos permite en un literal binario insertado.|  
|[getMaxCatalogNameLength](../../../connect/jdbc/reference/getmaxcatalognamelength-method-sqlserverdatabasemetadata.md)|Recupera el número máximo de caracteres que esta base de datos permite en un nombre de catálogo.|  
|[getMaxCharLiteralLength](../../../connect/jdbc/reference/getmaxcharliterallength-method-sqlserverdatabasemetadata.md)|Recupera el número máximo de caracteres que esta base de datos permite para un literal de caracteres.|  
|[getMaxColumnNameLength](../../../connect/jdbc/reference/getmaxcolumnnamelength-method-sqlserverdatabasemetadata.md)|Recupera el número máximo de caracteres que esta base de datos permite para un nombre de columna.|  
|[getMaxColumnsInGroupBy](../../../connect/jdbc/reference/getmaxcolumnsingroupby-method-sqlserverdatabasemetadata.md)|Recupera el número máximo de columnas que esta base de datos permite en una cláusula GROUP BY.|  
|[getMaxColumnsInIndex](../../../connect/jdbc/reference/getmaxcolumnsinindex-method-sqlserverdatabasemetadata.md)|Recupera el número máximo de columnas que esta base de datos permite en un índice.|  
|[getMaxColumnsInOrderBy](../../../connect/jdbc/reference/getmaxcolumnsinorderby-method-sqlserverdatabasemetadata.md)|Recupera el número máximo de columnas que esta base de datos permite en una cláusula ORDER BY.|  
|[getMaxColumnsInSelect](../../../connect/jdbc/reference/getmaxcolumnsinselect-method-sqlserverdatabasemetadata.md)|Recupera el número máximo de columnas que esta base de datos permite en una lista SELECT.|  
|[getMaxColumnsInTable](../../../connect/jdbc/reference/getmaxcolumnsintable-method-sqlserverdatabasemetadata.md)|Recupera el número máximo de columnas que esta base de datos permite en una tabla.|  
|[getMaxConnections](../../../connect/jdbc/reference/getmaxconnections-method-sqlserverdatabasemetadata.md)|Recupera el número máximo de conexiones simultáneas posibles para esta base de datos.|  
|[getMaxCursorNameLength](../../../connect/jdbc/reference/getmaxcursornamelength-method-sqlserverdatabasemetadata.md)|Recupera el número máximo de caracteres que esta base de datos permite en un nombre de cursor.|  
|[getMaxIndexLength](../../../connect/jdbc/reference/getmaxindexlength-method-sqlserverdatabasemetadata.md)|Recupera el número máximo de bytes que esta base de datos permite para un índice, lo cual incluye todas las partes del índice.|  
|[getMaxProcedureNameLength](../../../connect/jdbc/reference/getmaxprocedurenamelength-method-sqlserverdatabasemetadata.md)|Recupera el número máximo de caracteres que esta base de datos permite en un nombre de procedimiento.|  
|[getMaxRowSize](../../../connect/jdbc/reference/getmaxrowsize-method-sqlserverdatabasemetadata.md)|Recupera el número máximo de bytes que esta base de datos permite en una única fila.|  
|[getMaxSchemaNameLength](../../../connect/jdbc/reference/getmaxschemanamelength-method-sqlserverdatabasemetadata.md)|Recupera el número máximo de caracteres que esta base de datos permite en un nombre de esquema.|  
|[getMaxStatementLength](../../../connect/jdbc/reference/getmaxstatementlength-method-sqlserverdatabasemetadata.md)|Recupera el número máximo de caracteres que esta base de datos permite en una instrucción SQL.|  
|[getMaxStatements](../../../connect/jdbc/reference/getmaxstatements-method-sqlserverdatabasemetadata.md)|Recupera el número máximo de instrucciones activas que se pueden abrir para esta base de datos al mismo tiempo.|  
|[getMaxTableNameLength](../../../connect/jdbc/reference/getmaxtablenamelength-method-sqlserverdatabasemetadata.md)|Recupera el número máximo de caracteres que esta base de datos permite en un nombre de tabla.|  
|[getMaxTablesInSelect](../../../connect/jdbc/reference/getmaxtablesinselect-method-sqlserverdatabasemetadata.md)|Recupera el número máximo de tablas que esta base de datos permite en una instrucción SELECT.|  
|[getMaxUserNameLength](../../../connect/jdbc/reference/getmaxusernamelength-method-sqlserverdatabasemetadata.md)|Recupera el número máximo de caracteres que esta base de datos permite en un nombre de usuario.|  
|[getNumericFunctions](../../../connect/jdbc/reference/getnumericfunctions-method-sqlserverdatabasemetadata.md)|Recupera una lista separada por comas de funciones matemáticas que están disponibles con esta base de datos.|  
|[getPrimaryKeys](../../../connect/jdbc/reference/getprimarykeys-method-sqlserverdatabasemetadata.md)|Recupera una descripción de las columnas de clave principal de la tabla determinada.|  
|[getProcedureColumns](../../../connect/jdbc/reference/getprocedurecolumns-method-sqlserverdatabasemetadata.md)|Recupera una descripción de los parámetros de procedimiento almacenado y de las columnas de resultados.|  
|[getProcedures](../../../connect/jdbc/reference/getprocedures-method-sqlserverdatabasemetadata.md)|Recupera una descripción de los procedimientos almacenados que están disponibles en un modelo de nombre determinado de catálogo, esquema o procedimiento.|  
|[getProcedureTerm](../../../connect/jdbc/reference/getprocedureterm-method-sqlserverdatabasemetadata.md)|Recupera el término preferido para "procedimiento" en esta base de datos.|  
|[getResultSetHoldability](../../../connect/jdbc/reference/getresultsetholdability-method-sqlserverdatabasemetadata.md)|Recupera la capacidad de alojamiento predeterminada de los conjuntos de resultados para esta base de datos.|  
|[getRowIdLifetime](../../../connect/jdbc/reference/getrowidlifetime-method-sqlserverdatabasemetadata.md)|Devuelve un estado que indica si se admite el tipo de datos RowId de SQL. Si así fuera, devuelve la duración de un objeto RowId.|  
|[getSchemas](../../../connect/jdbc/reference/getschemas-method.md)|Recupera los nombres de esquema que están disponibles en la base de datos actual.|  
|[getSchemaTerm](../../../connect/jdbc/reference/getschematerm-method-sqlserverdatabasemetadata.md)|Recupera el término preferido para "esquema" en esta base de datos.|  
|[getSearchStringEscape](../../../connect/jdbc/reference/getsearchstringescape-method-sqlserverdatabasemetadata.md)|Recupera el objeto **String** que se puede utilizar para establecer como carácter de escape a los caracteres comodín.|  
|[getSQLKeywords](../../../connect/jdbc/reference/getsqlkeywords-method-sqlserverdatabasemetadata.md)|Recupera una lista separada por comas de las palabras clave de SQL de toda esta base de datos que sean también palabras clave de SQL92.|  
|[getSQLStateType](../../../connect/jdbc/reference/getsqlstatetype-method-sqlserverdatabasemetadata.md)|Indica si SQLSTATE, que devolvió el método SQLException.getSQLState, es X/Open (ahora se denomina Open Group), SQL CLI, SQL99 (JDBC 3.0) o SQL:2003 (JDBC 4.0).|  
|[getStringFunctions](../../../connect/jdbc/reference/getstringfunctions-method-sqlserverdatabasemetadata.md)|Recupera una lista separada por comas de funciones **String** del sistema que están disponibles con esta base de datos.|  
|[getSuperTables](../../../connect/jdbc/reference/getsupertables-method-sqlserverdatabasemetadata.md)|Recupera una descripción de las jerarquías de la tabla que se definen en un esquema determinado en esta base de datos.|  
|[getSuperTypes](../../../connect/jdbc/reference/getsupertypes-method-sqlserverdatabasemetadata.md)|Recupera una descripción de las jerarquías del tipo definido por el usuario que se definen en un esquema determinado en esta base de datos.|  
|[getSystemFunctions](../../../connect/jdbc/reference/getsystemfunctions-method-sqlserverdatabasemetadata.md)|Recupera una lista separada por comas de funciones del sistema que están disponibles con esta base de datos.|  
|[getTablePrivileges](../../../connect/jdbc/reference/gettableprivileges-method-sqlserverdatabasemetadata.md)|Recupera una descripción de los derechos de acceso para cada tabla que está disponible en el modelo del nombre determinado de catálogo, esquema o tabla.|  
|[getTables](../../../connect/jdbc/reference/gettables-method-sqlserverdatabasemetadata.md)|Recupera una descripción de las tablas que están disponibles en el patrón de nombre determinado de catálogo, esquema o tabla.|  
|[getTableTypes](../../../connect/jdbc/reference/gettabletypes-method-sqlserverdatabasemetadata.md)|Recupera los tipos de tabla que están disponibles en la base de datos actual.|  
|[getTimeDateFunctions](../../../connect/jdbc/reference/gettimedatefunctions-method-sqlserverdatabasemetadata.md)|Recupera una lista separada por comas de las funciones de fecha y hora que están disponibles con esta base de datos.|  
|[getTypeInfo](../../../connect/jdbc/reference/gettypeinfo-method-sqlserverdatabasemetadata.md)|Recupera una descripción de todos los tipos SQL estándar que se admiten en la base de datos actual.|  
|[getUDTs](../../../connect/jdbc/reference/getudts-method-sqlserverdatabasemetadata.md)|Recupera una descripción de los tipos definidos por el usuario que se describen en un esquema determinado.|  
|[getURL](../../../connect/jdbc/reference/geturl-method-sqlserverdatabasemetadata.md)|Recupera la dirección URL para esta base de datos.|  
|[getUserName](../../../connect/jdbc/reference/getusername-method-sqlserverdatabasemetadata.md)|Recupera el nombre de usuario según se conoce en esta base de datos.|  
|[getVersionColumns](../../../connect/jdbc/reference/getversioncolumns-method-sqlserverdatabasemetadata.md)|Recupera una descripción de las columnas de una tabla que se actualiza automáticamente cuando cualquier valor de una fila se actualiza.|  
|[insertsAreDetected](../../../connect/jdbc/reference/insertsaredetected-method-sqlserverdatabasemetadata.md)|Recupera si se puede detectar una inserción de filas visible mediante una llamada al método [rowInserted](../../../connect/jdbc/reference/rowinserted-method-sqlserverresultset.md) de la clase [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[isCatalogAtStart](../../../connect/jdbc/reference/iscatalogatstart-method-sqlserverdatabasemetadata.md)|Recupera si un catálogo aparece en el inicio de un nombre de tabla completo.|  
|[isReadOnly](../../../connect/jdbc/reference/isreadonly-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos está en modo de solo lectura.|  
|[locatorsUpdateCopy](../../../connect/jdbc/reference/locatorsupdatecopy-method-sqlserverdatabasemetadata.md)|Indica si las actualizaciones realizadas a un LOB se efectúan en una copia o directamente en el LOB.|  
|[nullPlusNonNullIsNull](../../../connect/jdbc/reference/nullplusnonnullisnull-method-sqlserverdatabasemetadata.md)|Indica si esta base de datos admite que se establezcan en NULL las concatenaciones entre valores NULL y que no sean NULL.|  
|[nullsAreSortedAtEnd](../../../connect/jdbc/reference/nullsaresortedatend-method-sqlserverdatabasemetadata.md)|Recupera si los valores NULL están ordenados al final independientemente del criterio de ordenación.|  
|[nullsAreSortedAtStart](../../../connect/jdbc/reference/nullsaresortedatstart-method-sqlserverdatabasemetadata.md)|Recupera si los valores NULL están ordenados al inicio independientemente del criterio de ordenación.|  
|[nullsAreSortedHigh](../../../connect/jdbc/reference/nullsaresortedhigh-method-sqlserverdatabasemetadata.md)|Recupera si los valores NULL están ordenados en orden ascendente.|  
|[nullsAreSortedLow](../../../connect/jdbc/reference/nullsaresortedlow-method-sqlserverdatabasemetadata.md)|Recupera si los valores NULL están ordenados en orden descendente.|  
|[othersDeletesAreVisible](../../../connect/jdbc/reference/othersdeletesarevisible-method-sqlserverdatabasemetadata.md)|Recupera si están visibles las eliminaciones que han realizado otros.|  
|[othersInsertsAreVisible](../../../connect/jdbc/reference/othersinsertsarevisible-method-sqlserverdatabasemetadata.md)|Recupera si están visibles las inserciones que han realizado otros.|  
|[othersUpdatesAreVisible](../../../connect/jdbc/reference/othersupdatesarevisible-method-sqlserverdatabasemetadata.md)|Recupera si están visibles las actualizaciones que han realizado otros.|  
|[ownDeletesAreVisible](../../../connect/jdbc/reference/owndeletesarevisible-method-sqlserverdatabasemetadata.md)|Recupera si están visibles las eliminaciones propias de un conjunto de resultados.|  
|[ownInsertsAreVisible](../../../connect/jdbc/reference/owninsertsarevisible-method-sqlserverdatabasemetadata.md)|Recupera si están visibles las inserciones propias de un conjunto de resultados.|  
|[ownUpdatesAreVisible](../../../connect/jdbc/reference/ownupdatesarevisible-method-sqlserverdatabasemetadata.md)|Recupera si están visibles las actualizaciones propias de un conjunto de resultados.|  
|[storesLowerCaseIdentifiers](../../../connect/jdbc/reference/storeslowercaseidentifiers-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos trata a los identificadores de SQL con combinaciones de mayúsculas y minúsculas que no se entrecomillan como elementos que distinguen entre mayúsculas y minúsculas y los almacena en minúscula.|  
|[storesLowerCaseQuotedIdentifiers](../../../connect/jdbc/reference/storeslowercasequotedidentifiers-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos trata a los identificadores de SQL con combinaciones de mayúsculas y minúsculas que se entrecomillan como elementos que distinguen entre mayúsculas y minúsculas y los almacena en minúscula.|  
|[storesMixedCaseIdentifiers](../../../connect/jdbc/reference/storesmixedcaseidentifiers-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos trata a los identificadores de SQL con combinaciones de mayúsculas y minúsculas que no se entrecomillan como elementos que distinguen entre mayúsculas y minúsculas y los almacena combinando ambos formatos.|  
|[storesMixedCaseQuotedIdentifiers](../../../connect/jdbc/reference/storesmixedcasequotedidentifiers-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos trata a los identificadores de SQL con combinaciones de mayúsculas y minúsculas que se entrecomillan como elementos que distinguen entre mayúsculas y minúsculas y los almacena combinando ambos formatos.|  
|[storesUpperCaseIdentifiers](../../../connect/jdbc/reference/storesuppercaseidentifiers-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos trata a los identificadores de SQL con combinaciones de mayúsculas y minúsculas que no se entrecomillan como elementos que distinguen entre mayúsculas y minúsculas y los almacena en mayúsculas.|  
|[storesUpperCaseQuotedIdentifiers](../../../connect/jdbc/reference/storesuppercasequotedidentifiers-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos trata a los identificadores de SQL con combinaciones de mayúsculas y minúsculas que se entrecomillan como elementos que distinguen entre mayúsculas y minúsculas y los almacena en mayúsculas.|  
|[supportsAlterTableWithAddColumn](../../../connect/jdbc/reference/supportsaltertablewithaddcolumn-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite ALTER TABLE con la incorporación de columnas.|  
|[supportsAlterTableWithDropColumn](../../../connect/jdbc/reference/supportsaltertablewithdropcolumn-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite ALTER TABLE con la eliminación de columnas.|  
|[supportsANSI92EntryLevelSQL](../../../connect/jdbc/reference/supportsansi92entrylevelsql-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite la gramática de SQL de nivel de entrada ANSI92.|  
|[supportsANSI92FullSQL](../../../connect/jdbc/reference/supportsansi92fullsql-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite la gramática de SQL completa de ANSI92.|  
|[supportsANSI92IntermediateSQL](../../../connect/jdbc/reference/supportsansi92intermediatesql-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite la gramática de SQL intermedia de ANSI92.|  
|[supportsBatchUpdates](../../../connect/jdbc/reference/supportsbatchupdates-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite las actualizaciones por lotes.|  
|[supportsCatalogsInDataManipulation](../../../connect/jdbc/reference/supportscatalogsindatamanipulation-method-sqlserverdatabasemetadata.md)|Recupera si un nombre de catálogo se puede utilizar en una instrucción de manipulación de datos.|  
|[supportsCatalogsInIndexDefinitions](../../../connect/jdbc/reference/supportscatalogsinindexdefinitions-method-sqlserverdatabasemetadata.md)|Recupera si un nombre de catálogo se puede utilizar en una instrucción de definición de índice.|  
|[supportsCatalogsInPrivilegeDefinitions](../../../connect/jdbc/reference/supportscatalogsinprivilegedefinitions-method-sqlserverdatabasemetadata.md)|Recupera si un nombre de catálogo se puede utilizar en una instrucción de definición de privilegios.|  
|[supportsCatalogsInProcedureCalls](../../../connect/jdbc/reference/supportscatalogsinprocedurecalls-method-sqlserverdatabasemetadata.md)|Recupera si un nombre de catálogo se puede utilizar en una instrucción de llamada a procedimientos.|  
|[supportsCatalogsInTableDefinitions](../../../connect/jdbc/reference/supportscatalogsintabledefinitions-method-sqlserverdatabasemetadata.md)|Recupera si un nombre de catálogo se puede utilizar en una instrucción de definición de tablas.|  
|[supportsColumnAliasing](../../../connect/jdbc/reference/supportscolumnaliasing-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite suavizado para contorno de columnas.|  
|[supportsConvert](../../../connect/jdbc/reference/supportsconvert-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite la función CONVERT entre tipos SQL.|  
|[supportsCoreSQLGrammar](../../../connect/jdbc/reference/supportscoresqlgrammar-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite la gramática básica de SQL de ODBC.|  
|[supportsCorrelatedSubqueries](../../../connect/jdbc/reference/supportscorrelatedsubqueries-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite subconsultas correlacionadas.|  
|[supportsDataDefinitionAndDataManipulationTransactions](../../../connect/jdbc/reference/supportsdatadefinitionanddatamanipulationtransactions-method.md)|Recupera si esta base de datos admite instrucciones de definición y manipulación de datos en una transacción.|  
|[supportsDataManipulationTransactionsOnly](../../../connect/jdbc/reference/supportsdatamanipulationtransactionsonly-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos solo admite instrucciones de manipulación de datos en una transacción.|  
|[supportsDifferentTableCorrelationNames](../../../connect/jdbc/reference/supportsdifferenttablecorrelationnames-method-sqlserverdatabasemetadata.md)|Recupera si, cuando se admiten nombres de correlación de tabla, estos deben ser diferentes de los nombres de las tablas.|  
|[supportsExpressionsInOrderBy](../../../connect/jdbc/reference/supportsexpressionsinorderby-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite expresiones en listas ORDER BY.|  
|[supportsExtendedSQLGrammar](../../../connect/jdbc/reference/supportsextendedsqlgrammar-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite la gramática extendida de SQL de ODBC.|  
|[supportsFullOuterJoins](../../../connect/jdbc/reference/supportsfullouterjoins-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite las combinaciones externas anidadas completas.|  
|[supportsGetGeneratedKeys](../../../connect/jdbc/reference/supportsgetgeneratedkeys-method-sqlserverdatabasemetadata.md)|Recupera si las claves generadas automáticamente se pueden recuperar después de que se haya ejecutado una instrucción.|  
|[supportsGroupBy](../../../connect/jdbc/reference/supportsgroupby-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite algún formato de la cláusula GROUP BY.|  
|[supportsGroupByBeyondSelect](../../../connect/jdbc/reference/supportsgroupbybeyondselect-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos permite la utilización de columnas que no se incluyan en la instrucción SELECT en una cláusula GROUP BY siempre que todas las columnas en la instrucción SELECT se incluyan en la cláusula GROUP BY.|  
|[supportsGroupByUnrelated](../../../connect/jdbc/reference/supportsgroupbyunrelated-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite la utilización de una columna que no esté en la instrucción SELECT en una cláusula GROUP BY.|  
|[supportsIntegrityEnhancementFacility](../../../connect/jdbc/reference/supportsintegrityenhancementfacility-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite SQL Integrity Enhancement Facility.|  
|[supportsLikeEscapeClause](../../../connect/jdbc/reference/supportslikeescapeclause-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite la especificación de la cláusula de escape LIKE.|  
|[supportsLimitedOuterJoins](../../../connect/jdbc/reference/supportslimitedouterjoins-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos ofrece compatibilidad limitada para las combinaciones externas.|  
|[supportsMinimumSQLGrammar](../../../connect/jdbc/reference/supportsminimumsqlgrammar-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite la gramática mínima de SQL de ODBC.|  
|[supportsMixedCaseIdentifiers](../../../connect/jdbc/reference/supportsmixedcaseidentifiers-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos trata a los identificadores de SQL con combinaciones de mayúsculas y minúsculas que no se entrecomillan como elementos que distinguen entre mayúsculas y minúsculas y los almacena combinando ambos formatos.|  
|[supportsMixedCaseQuotedIdentifiers](../../../connect/jdbc/reference/supportsmixedcasequotedidentifiers-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos trata a los identificadores de SQL con combinaciones de mayúsculas y minúsculas que se entrecomillan como elementos que distinguen entre mayúsculas y minúsculas y los almacena combinando ambos formatos.|  
|[supportsMultipleOpenResults](../../../connect/jdbc/reference/supportsmultipleopenresults-method-sqlserverdatabasemetadata.md)|Recupera si es posible que varios objetos [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) se devuelvan desde un objeto [SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md) de forma simultánea.|  
|[supportsMultipleResultSets](../../../connect/jdbc/reference/supportsmultipleresultsets-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite recibir varios objetos [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) a partir de una llamada única al método [execute](../../../connect/jdbc/reference/execute-method.md) de la clase [SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md).|  
|[supportsMultipleTransactions](../../../connect/jdbc/reference/supportsmultipletransactions-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos permite tener varias transacciones abiertas a la vez en conexiones diferentes.|  
|[supportsNamedParameters](../../../connect/jdbc/reference/supportsnamedparameters-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite los parámetros con nombre en instrucciones invocables.|  
|[supportsNonNullableColumns](../../../connect/jdbc/reference/supportsnonnullablecolumns-method-sqlserverdatabasemetadata.md)|Recupera si las columnas en esta base de datos se pueden definir para que no admitan valores NULL.|  
|[supportsOpenCursorsAcrossCommit](../../../connect/jdbc/reference/supportsopencursorsacrosscommit-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos permite mantener cursores abiertos en las confirmaciones.|  
|[supportsOpenCursorsAcrossRollback](../../../connect/jdbc/reference/supportsopencursorsacrossrollback-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos permite mantener cursores abiertos en las reversiones.|  
|[supportsOpenStatementsAcrossCommit](../../../connect/jdbc/reference/supportsopenstatementsacrosscommit-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos permite mantener instrucciones abiertas en las confirmaciones.|  
|[supportsOpenStatementsAcrossRollback](../../../connect/jdbc/reference/supportsopenstatementsacrossrollback-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos permite mantener instrucciones abiertas en las reversiones.|  
|[supportsOrderByUnrelated](../../../connect/jdbc/reference/supportsorderbyunrelated-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite la utilización de una columna que no esté en la instrucción SELECT en una cláusula ORDER BY.|  
|[supportsOuterJoins](../../../connect/jdbc/reference/supportsouterjoins-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite alguna forma de combinación externa.|  
|[supportsPositionedDelete](../../../connect/jdbc/reference/supportspositioneddelete-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite instrucciones DELETE posicionadas.|  
|[supportsPositionedUpdate](../../../connect/jdbc/reference/supportspositionedupdate-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite instrucciones UPDATE posicionadas.|  
|[supportsResultSetConcurrency](../../../connect/jdbc/reference/supportsresultsetconcurrency-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite el tipo de simultaneidad determinado en combinación con el tipo de conjunto de resultados determinado.|  
|[supportsResultSetHoldability](../../../connect/jdbc/reference/supportsresultsetholdability-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite la capacidad de alojamiento del conjunto de resultados determinado.|  
|[supportsResultSetType](../../../connect/jdbc/reference/supportsresultsettype-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite el tipo del conjunto de resultados determinado.|  
|[supportsSavepoints](../../../connect/jdbc/reference/supportssavepoints-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite puntos de retorno.|  
|[supportsSchemasInDataManipulation](../../../connect/jdbc/reference/supportsschemasindatamanipulation-method-sqlserverdatabasemetadata.md)|Recupera si un nombre de esquema se puede utilizar en una instrucción de manipulación de datos.|  
|[supportsSchemasInIndexDefinitions](../../../connect/jdbc/reference/supportsschemasinindexdefinitions-method-sqlserverdatabasemetadata.md)|Recupera si un nombre de esquema se puede utilizar en una instrucción de definición de índice.|  
|[supportsSchemasInPrivilegeDefinitions](../../../connect/jdbc/reference/supportsschemasinprivilegedefinitions-method-sqlserverdatabasemetadata.md)|Recupera si un nombre de esquema se puede utilizar en una instrucción de definición de privilegios.|  
|[supportsSchemasInProcedureCalls](../../../connect/jdbc/reference/supportsschemasinprocedurecalls-method-sqlserverdatabasemetadata.md)|Recupera si un nombre de esquema se puede utilizar en una instrucción de llamada a procedimientos.|  
|[supportsSchemasInTableDefinitions](../../../connect/jdbc/reference/supportsschemasintabledefinitions-method-sqlserverdatabasemetadata.md)|Recupera si un nombre de esquema se puede utilizar en una instrucción de definición de tablas.|  
|[supportsSelectForUpdate](../../../connect/jdbc/reference/supportsselectforupdate-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite instrucciones SELECT FOR UPDATE.|  
|[supportsStatementPooling](../../../connect/jdbc/reference/supportsstatementpooling-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite la agrupación de instrucciones.|  
|[supportsStoredFunctionsUsingCallSyntax](../../../connect/jdbc/reference/supportsstoredfunctionsusingcallsyntax-method-sqlserverdatabasemetadata.md)|Indica si la base de datos actual permite invocar las funciones definidas por el usuario o proveedor mediante el uso de la sintaxis de escape para procedimientos almacenados.|  
|[supportsStoredProcedures](../../../connect/jdbc/reference/supportsstoredprocedures-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite llamadas a procedimientos almacenados que utilicen sintaxis de escape para procedimientos almacenados.|  
|[supportsSubqueriesInComparisons](../../../connect/jdbc/reference/supportssubqueriesincomparisons-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite subconsultas en expresiones de comparación.|  
|[supportsSubqueriesInExists](../../../connect/jdbc/reference/supportssubqueriesinexists-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite subconsultas en expresiones EXISTS.|  
|[supportsSubqueriesInIns](../../../connect/jdbc/reference/supportssubqueriesinins-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite subconsultas en instrucciones IN.|  
|[supportsSubqueriesInQuantifieds](../../../connect/jdbc/reference/supportssubqueriesinquantifieds-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite subconsultas en expresiones cuantificadas.|  
|[supportsTableCorrelationNames](../../../connect/jdbc/reference/supportstablecorrelationnames-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite nombres de correlación de tabla.|  
|[supportsTransactionIsolationLevel](../../../connect/jdbc/reference/supportstransactionisolationlevel-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite el nivel de aislamiento de transacción determinado.|  
|[supportsTransactions](../../../connect/jdbc/reference/supportstransactions-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite transacciones.|  
|[supportsUnion](../../../connect/jdbc/reference/supportsunion-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite UNION de SQL.|  
|[supportsUnionAll](../../../connect/jdbc/reference/supportsunionall-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos admite UNION ALL de SQL.|  
|[updatesAreDetected](../../../connect/jdbc/reference/updatesaredetected-method-sqlserverdatabasemetadata.md)|Recupera si se puede detectar una actualización de filas visible mediante una llamada al método [rowUpdated](../../../connect/jdbc/reference/rowupdated-method-sqlserverresultset.md) de la clase [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[usesLocalFilePerTable](../../../connect/jdbc/reference/useslocalfilepertable-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos utiliza un archivo para cada tabla.|  
|[usesLocalFiles](../../../connect/jdbc/reference/useslocalfiles-method-sqlserverdatabasemetadata.md)|Recupera si esta base de datos almacena las tablas en un archivo local.|  
  
## <a name="inherited-methods"></a>Métodos heredados  
  
|Clase heredada de:|Métodos|  
|---------------------------|-------------|  
|java.lang.Object|clone, equals, finalize, getClass, hashCode, notify, notifyAll, toString, wait|  
|java.sql.Wrapper|isWrapperFor, unwrap|  
  
## <a name="see-also"></a>Consulte también  
 [Clase SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
