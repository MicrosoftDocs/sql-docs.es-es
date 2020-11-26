---
title: Realizar operaciones de copia masiva
description: Obtenga información sobre cómo realizar operaciones de copia masiva con OLE DB Driver for SQL Server y cómo habilita la transferencia rápida de datos a la base de datos.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- bulk copy [OLE DB Driver for SQL Server]
- data access [OLE DB Driver for SQL Server], bulk copy operations
- OLE DB Driver for SQL Server, bulk copy operations
- MSOLEDBSQL, bulk copy operations
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6035f5ac8723e9ccb84735080569468c1b7b33b3
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123854"
---
# <a name="performing-bulk-copy-operations"></a>Realizar operaciones de copia masiva
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  La característica de copia masiva de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] admite la transferencia de grandes cantidades de datos a una tabla o vista de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] o desde ella. Los datos también pueden transferirse especificando una instrucción SELECT. Los datos pueden moverse entre [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y un archivo de datos del sistema operativo, como un archivo ASCII. El archivo de datos puede tener diferentes formatos; el formato para una copia masiva se define en un archivo de formato. Opcionalmente, los datos pueden cargarse en variables de programa y transferirse a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] mediante funciones y métodos de copia masiva.  
  
 Para obtener una aplicación de ejemplo que muestra esta característica, consulte [Copia de datos masiva con IRowsetFastLoad &#40;OLE DB&#41;](../../oledb/ole-db-how-to/bulk-copy-data-using-irowsetfastload-ole-db.md).  
  
 Normalmente, una aplicación utiliza la copia masiva de una de las siguientes formas:  
  
-   Copia masiva de una tabla, una vista o el conjunto de resultados de una instrucción Transact-SQL a un archivo de datos donde los datos se almacenan en el mismo formato que la tabla o vista.  
  
     Recibe el nombre de archivo de datos en modo nativo.  
  
-   Copia masiva de una tabla, una vista o el conjunto de resultados de una instrucción Transact-SQL a un archivo de datos donde los datos se almacenan en un formato distinto al de la tabla o vista.  
  
     En este caso, se crea un archivo de formato independiente que define las características (tipo de datos, posición, longitud, terminador, etc.) de cada columna cuando se almacena en el archivo de datos. Si todas las columnas se convierten a un formato de caracteres, el archivo resultante se denomina archivo de datos en modo de carácter.  
  
-   Copia masiva de un archivo de datos a una tabla o vista.  
  
     Si es necesario, se usa un archivo de formato para determinar el diseño del archivo de datos.  
  
-   Carga de datos en variables de programa y, a continuación, importación de los datos en una tabla o vista mediante las funciones de copia masiva a fin de copiar de forma masiva una fila cada vez.  
  
 Los archivos de datos utilizados por las funciones de copia masiva no tienen que ser creados por otro programa de copia masiva. Cualquier otro sistema puede generar un archivo de datos y un archivo de formato según las definiciones de copia masiva; estos archivos pueden utilizarse posteriormente con un programa de copia masiva de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] para importar datos en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Por ejemplo, puede exportar datos de una hoja de cálculo a un archivo delimitado por tabuladores, generar un archivo de formato que describa el archivo delimitado por tabuladores y, a continuación, usar un programa de copia masiva para importar rápidamente los datos en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Los archivos de datos generados por la copia masiva también pueden importarse en otras aplicaciones. Por ejemplo, puede usar las funciones de copia masiva para exportar datos de una tabla o vista a un archivo delimitado por tabuladores que, a su vez, puede cargarse en una hoja de cálculo.  
  
 Los programadores que codifican aplicaciones para usar las funciones de copia masiva deben seguir las reglas generales para obtener un rendimiento adecuado de la copia masiva. Para más información sobre operaciones de carga masiva en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], consulte [Importar y exportar datos de forma masiva &#40;SQL Server&#41;](../../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md).  
  
## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones  
 Debe enlazarse un tipo definido por el usuario CLR (UDT) en forma de datos binarios. Aunque un archivo de formato especifique SQLCHAR como tipo de datos de una columna UDT de destino, la utilidad BCP considerará los datos como binarios.  
  
 No use SET FMTONLY OFF con operaciones de copia masiva. SET FMTONLY OFF puede hacer que la operación de copia masiva no se ejecute correctamente o genere resultados inesperados.  
  
## <a name="ole-db-driver-for-sql-server"></a>Controlador OLE DB para SQL Server 
 OLE DB Driver for SQL Server implementa dos métodos para realizar operaciones de copia masiva con una base de datos de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. El primer método implica el uso de la interfaz [IRowsetFastLoad](../../oledb/ole-db-interfaces/irowsetfastload-ole-db.md) en operaciones de copia masiva basadas en memoria; el segundo, implica el uso de la interfaz [IBCPSession](../../oledb/ole-db-interfaces/ibcpsession-ole-db.md) en operaciones de copia masiva basadas en archivos.  
  
### <a name="using-memory-based-bulk-copy-operations"></a>Usar operaciones de copia masiva basadas en memoria  
 El controlador OLE DB para SQL Server implementa la interfaz **IRowsetFastLoad** para exponer la compatibilidad de las operaciones de copia masiva basadas en memoria de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. La interfaz **IRowsetFastLoad** implementa los métodos [IRowsetFastLoad::Commit](../../oledb/ole-db-interfaces/irowsetfastload-commit-ole-db.md) e [IRowsetFastLoad::InsertRow](../../oledb/ole-db-interfaces/irowsetfastload-insertrow-ole-db.md).  
  
#### <a name="enabling-a-session-for-irowsetfastload"></a>Habilitar una sesión para IRowsetFastLoad  
 El consumidor notifica a OLE DB Driver for SQL Server su necesidad de realizar una copia masiva estableciendo la propiedad SSPROP_ENABLEFASTLOAD específica de OLE DB Driver for SQL Server en VARIANT_TRUE. Con la propiedad establecida en el origen de datos, el consumidor crea una sesión de OLE DB Driver for SQL Server. La nueva sesión permite al consumidor acceder a la interfaz **IRowsetFastLoad**.  
  
> [!NOTE]  
>  Si se usa la interfaz **IDataInitialize** para inicializar el origen de datos, es necesario establecer la propiedad SSPROP_IRowsetFastLoad en el parámetro *rgPropertySets* del método **IOpenRowset::OpenRowset**; de lo contrario, la llamada al método **OpenRowset** devolverá E_NOINTERFACE.  
  
 Al habilitar una sesión para la copia masiva, se restringe la compatibilidad del proveedor OLE DB de OLE DB Driver for SQL Server con las interfaces en la sesión. Una sesión habilitada para copia masiva solo expone las interfaces siguientes:  
  
-   **IDBSchemaRowset**  
  
-   **IGetDataSource**  
  
-   **IOpenRowset**  
  
-   **ISupportErrorInfo**  
  
-   **ITransactionJoin**  
  
 Para deshabilitar la creación de conjuntos de filas habilitados para copia masiva y revertir la sesión del controlador OLE DB para SQL Server al procesamiento estándar, vuelva a establecer SSPROP_ENABLEFASTLOAD en VARIANT_FALSE.  
  
#### <a name="irowsetfastload-rowsets"></a>Conjuntos de filas IRowsetFastLoad  
 Los conjuntos de filas de copia masiva del controlador OLE DB para SQL Server son de solo escritura, pero exponen interfaces que permiten al consumidor determinar la estructura de una tabla de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. En un conjunto de filas de OLE DB Driver for SQL Server habilitado para copia masiva se exponen las siguientes interfaces:  
  
-   **IAccessor**  
  
-   **IColumnsInfo**  
  
-   **IColumnsRowset**  
  
-   **IConvertType**  
  
-   **IRowsetFastLoad**  
  
-   **IRowsetInfo**  
  
-   **ISupportErrorInfo**  
  
 Las propiedades específicas del proveedor SSPROP_FASTLOADOPTIONS, SSPROP_FASTLOADKEEPNULLS y SSPROP_FASTLOADKEEPIDENTITY controlan el comportamiento de los conjuntos de filas de copia masiva del controlador OLE DB para SQL Server. Las propiedades se especifican en el miembro *rgProperties* de un miembro de parámetro de *rgPropertySets***IOpenRowset**.  
  
|Id. de propiedad|Descripción|  
|-----------------|-----------------|  
|SSPROP_FASTLOADKEEPIDENTITY|Columna: No<br /><br /> R (lectura) y W (escritura): Lectura/escritura<br /><br /> Escriba:  VT_BOOL<br /><br /> Valor predeterminado: VARIANT_FALSE<br /><br /> Descripción: Mantiene valores de identidad suministrados por el consumidor.<br /><br /> VARIANT_FALSE: Los valores para una columna de identidad de la tabla [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se generan por [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. El proveedor OLE DB Driver for SQL Server omite cualquier valor enlazado para la columna.<br /><br /> VARIANT_TRUE: El consumidor enlaza un descriptor de acceso que proporciona un valor para una columna de identidad de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. La propiedad de identidad no está disponible en las columnas que aceptan valores NULL, por lo que el consumidor proporciona un valor único en cada llamada a **IRowsetFastLoad::Insert**.|  
|SSPROP_FASTLOADKEEPNULLS|Columna: No<br /><br /> R (lectura) y W (escritura): Lectura/escritura<br /><br /> Escriba:  VT_BOOL<br /><br /> Valor predeterminado: VARIANT_FALSE<br /><br /> Descripción: Mantiene NULL en las columnas con una restricción DEFAULT. Solo afecta a las columnas [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que aceptan valores NULL y tienen aplicada una restricción DEFAULT.<br /><br /> VARIANT_FALSE: [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] inserta el valor predeterminado de la columna cuando el consumidor del controlador OLE DB para SQL Server inserta una fila que contiene un valor NULL para la columna.<br /><br /> VARIANT_TRUE: [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] inserta el valor NULL como valor de columna cuando el consumidor del controlador OLE DB para SQL Server inserta una fila que contiene un valor NULL para la columna.|  
|SSPROP_FASTLOADOPTIONS|Columna: No<br /><br /> R (lectura) y W (escritura): Lectura/escritura<br /><br /> Escriba:  VT_BSTR<br /><br /> Valor predeterminado: ninguno<br /><br /> Descripción: Esta propiedad es la misma que la opción **-h** "*hint*[,...*n*]" de la utilidad **bcp**. Las cadenas que se indican a continuación pueden utilizarse como opciones para la copia masiva de datos en una tabla.<br /><br /> **ORDER**(*columna*[**ASC** &#124; **DESC**][,...*n*]): Criterio de ordenación de los datos en el archivo de datos. El rendimiento de la copia masiva mejora si el archivo de datos que se carga se ordena según el índice clúster de la tabla.<br /><br /> **ROWS_PER_BATCH** = *bb*: Número de filas de datos por lote (como *bb*). El servidor optimiza la carga masiva según el valor *bb*. De forma predeterminada, el valor de **ROWS_PER_BATCH** es desconocido.<br /><br /> **KILOBYTES_PER_BATCH** = *cc*: Número de kilobytes (KB) de datos por lote (como cc). De forma predeterminada, el valor de **KILOBYTES_PER_BATCH** es desconocido.<br /><br /> **TABLOCK**: Se adquiere un bloqueo de nivel de tabla durante la operación de copia masiva. Esta opción mejora notablemente el rendimiento ya que, al mantenerse el bloqueo solamente durante la operación de copia masiva, se reduce la contención en la tabla por bloqueo. Varios clientes pueden cargar una tabla de forma simultánea si esta no tiene índices y se especifica **TABLOCK**. De forma predeterminada, el comportamiento del bloqueo viene determinado por la opción de tabla **table lock on bulk load**.<br /><br /> **CHECK_CONSTRAINTS**: Durante la operación de copia masiva se comprueban todas las restricciones de *table_name*. De forma predeterminada, se omiten las restricciones.<br /><br /> **FIRE_TRIGGER**: [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] usa las versiones de fila para los desencadenadores y almacena las versiones de fila en el almacén de versiones, en **tempdb**. Por lo tanto, las optimizaciones de registro masivo están disponibles incluso si están habilitados los desencadenadores. Antes de realizar una importación masiva de un lote con un número elevado de filas y los desencadenadores habilitados, puede que tenga que ampliar el tamaño de **tempdb**.|  
  
### <a name="using-file-based-bulk-copy-operations"></a>Usar operaciones de copia masiva basadas en archivos  
 El controlador OLE DB para SQL Server implementa la interfaz **IBCPSession** para exponer la compatibilidad de las operaciones de copia masiva basadas en archivos de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. La interfaz **IBCPSession** implementa los métodos [IBCPSession::BCPColFmt](../../oledb/ole-db-interfaces/ibcpsession-bcpcolfmt-ole-db.md), [IBCPSession::BCPColumns](../../oledb/ole-db-interfaces/ibcpsession-bcpcolumns-ole-db.md), [IBCPSession::BCPControl](../../oledb/ole-db-interfaces/ibcpsession-bcpcontrol-ole-db.md), [IBCPSession::BCPDone](../../oledb/ole-db-interfaces/ibcpsession-bcpdone-ole-db.md), [IBCPSession::BCPExec](../../oledb/ole-db-interfaces/ibcpsession-bcpexec-ole-db.md), [IBCPSession::BCPInit](../../oledb/ole-db-interfaces/ibcpsession-bcpinit-ole-db.md), [IBCPSession::BCPReadFmt](../../oledb/ole-db-interfaces/ibcpsession-bcpreadfmt-ole-db.md) e [IBCPSession::BCPWriteFmt](../../oledb/ole-db-interfaces/ibcpsession-bcpwritefmt-ole-db.md).  
  
  
## <a name="see-also"></a>Consulte también  
 [Características del controlador OLE DB para SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md)   
 [Propiedades del origen de datos &#40;OLE DB&#41;](../../oledb/ole-db-data-source-objects/data-source-properties-ole-db.md)   
 [Importar y exportar datos en bloque &#40;SQL Server&#41;](../../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)   
 [IRowsetFastLoad &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/irowsetfastload-ole-db.md)   
 [IBCPSession &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/ibcpsession-ole-db.md)   

