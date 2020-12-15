---
title: Creación de tablas de SQL Server (proveedor de OLE DB de Native Client) | Microsoft Docs
description: Obtenga información sobre cómo el proveedor de OLE DB de SQL Server Native Client expone las funciones que permiten a los consumidores crear SQL Server tablas.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- tables [OLE DB]
- SQL Server Native Client OLE DB provider, tables
- DBCOLUMNDESC usage
- adding tables
- CreateTable function
ms.assetid: a7b8d142-d76a-44d9-a583-86ac5109fbe8
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a777ec24b0099e6ad4c98ed4f9d42d38f785de36
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484937"
---
# <a name="creating-sql-server-native-client-tables"></a>Crear tablas de SQL Server Native Client
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  El [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client expone la función **ITableDefinition:: CreateTable** , lo que permite a los consumidores crear [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tablas. Los consumidores usan **createTable** para crear tablas permanentes con nombre de consumidor y tablas permanentes o temporales con nombres únicos generados por el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client.  
  
 Cuando el consumidor llama a **ITableDefinition:: CreateTable**, si el valor de la propiedad DBPROP_TBL_TEMPTABLE es VARIANT_TRUE, el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client genera un nombre de tabla temporal para el consumidor. El consumidor establece el parámetro *pTableID* del método **CreateTable** en NULL. Las tablas temporales con nombres generados por el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client no aparecen en el conjunto de filas **tables** , pero son accesibles a través de la interfaz **IOpenRowset** .  
  
 Cuando los consumidores especifican el nombre de tabla en el miembro *pwszName* de la Unión *uname* en el parámetro *pTableID* , el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client crea una [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tabla con ese nombre. Se aplican las restricciones de denominación de tabla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y el nombre de tabla puede indicar una tabla permanente o una tabla temporal local o global. Para obtener más información, vea [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md). El parámetro *ppTableID* puede ser NULL.  
  
 El [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client puede generar los nombres de tablas permanentes o temporales. Cuando el consumidor establece el parámetro *pTableID* en NULL y establece *ppTableID* para que apunte a un DBID válido \* , el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client devuelve el nombre generado de la tabla en el miembro *PWSZNAME* de la Unión *uname* del DBID señalado por el valor de *ppTableID*. Para crear una [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tabla con nombre de proveedor de OLE DB de Native Client temporal, el consumidor incluye la propiedad OLE DB table DBPROP_TBL_TEMPTABLE en un conjunto de propiedades de tabla al que se hace referencia en el parámetro *rgPropertySets* . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Las tablas temporales con nombre del proveedor de OLE DB de Native Client son locales.  
  
 **CreateTable** devuelve DB_E_BADTABLEID si el miembro *eKind* del parámetro *pTableID* no indica DBKIND_NAME.  
  
## <a name="dbcolumndesc-usage"></a>Uso de DBCOLUMNDESC  
 El consumidor puede indicar un tipo de datos de columna con el miembro *pwszTypeName* o el miembro *wType*. Si el consumidor especifica el tipo de datos en *pwszTypeName*, el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client omite el valor de *wType*.  
  
 Si usa el miembro *pwszTypeName*, el consumidor especifica el tipo de datos con los nombres de tipo de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los nombres de tipo de datos válidos son aquéllos devueltos en la columna TYPE_NAME del conjunto de filas de esquema PROVIDER_TYPES.  
  
 El [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client reconoce un subconjunto de valores DbType enumerados OLE DB en el miembro *wType* . Para más información, consulte [Asignación de tipos de datos en ITableDefinition](../../relational-databases/native-client-ole-db-data-types/data-type-mapping-in-itabledefinition.md).  
  
> [!NOTE]  
>  **CreateTable** devuelve DB_E_BADTYPE si el consumidor establece el miembro *pTypeInfo* o *pclsid* para especificar el tipo de datos de columna.  
  
 El consumidor especifica el nombre de columna en el miembro *pwszName* de la unión *uName* del miembro *dbcid* de DBCOLUMNDESC. El nombre de columna se especifica como una cadena de caracteres Unicode. El miembro *eKind* de *dbcid* debe ser DBKIND_NAME. **CreateTable** devuelve DB_E_BADCOLUMNID si *eKind* no es válido, *pwszName* es NULL o si el valor de *pwszName* no es un identificador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] válido.  
  
 Todas las propiedades de columna están disponibles en todas las columnas definidas para la tabla. **CreateTable** puede devolver DB_S_ERRORSOCCURRED o DB_E_ERRORSOCCURRED si los valores de la propiedad se han establecido de un modo que están en conflicto. **CreateTable** devuelve un error cuando la configuración de la propiedad de columna no válida produce un error en la creación de la tabla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Las propiedades de columna de un DBCOLUMNDESC se interpretan como sigue.  
  
|Id. de propiedad|Descripción|  
|-----------------|-----------------|  
|DBPROP_COL_AUTOINCREMENT|R (lectura) y W (escritura): Lectura/escritura<br /><br /> Valor predeterminado: Descripción de VARIANT_FALSE: Establece la propiedad de identidad en la columna creada. Para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la propiedad de identidad es válida para una columna única dentro de una tabla. Si se establece la propiedad en VARIANT_TRUE para más de una sola columna, se genera un error cuando el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client intenta crear la tabla en el servidor.<br /><br /> La propiedad de identidad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] solo es válida para los tipos de datos **integer**, **numeric** y **decimal** cuando la escala es 0. Si se establece la propiedad en VARIANT_TRUE en una columna de cualquier otro tipo de datos, se genera un error cuando el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client intenta crear la tabla en el servidor.<br /><br /> El [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client devuelve DB_S_ERRORSOCCURRED cuando DBPROP_COL_AUTOINCREMENT y DBPROP_COL_NULLABLE son VARIANT_TRUE y el *dwOption* de DBPROP_COL_NULLABLE no se DBPROPOPTIONS_REQUIRED. Se devuelve DB_E_ERRORSOCCURRED cuando DBPROP_COL_AUTOINCREMENT y DBPROP_COL_NULLABLE son VARIANT_TRUE y *dwOption* de DBPROP_COL_NULLABLE es igual a DBPROPOPTIONS_REQUIRED. La columna se define con la propiedad de identidad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y el miembro DBPROP_COL_NULLABLE de *dwStatus* se establece en DBPROPSTATUS_CONFLICTING.|  
|DBPROP_COL_DEFAULT|R (lectura) y W (escritura): Lectura/escritura<br /><br /> Valor predeterminado: None<br /><br /> Descripción: crea una restricción DEFAULT de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para la columna.<br /><br /> El miembro DBPROP de *vValue* puede ser cualquiera de varios tipos. El miembro *vValue.vt* tiene que especificar un tipo compatible con el tipo de datos de la columna. Por ejemplo, la definición de BSTR N/A como el valor predeterminado para una columna definida como DBTYPE_WSTR es una coincidencia compatible. Al definir el mismo valor predeterminado en una columna definida como DBTYPE_R8 se genera un error cuando el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client intenta crear la tabla en el servidor.|  
|DBPROP_COL_DESCRIPTION|R (lectura) y W (escritura): Lectura/escritura<br /><br /> Valor predeterminado: None<br /><br /> Descripción: el proveedor de OLE DB de Native Client no implementa la propiedad DBPROP_COL_DESCRIPTION columna [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> El miembro *dwStatus* de la estructura DBPROP devuelve DBPROPSTATUS_NOTSUPPORTED cuando el consumidor intenta escribir el valor de propiedad.<br /><br /> El establecimiento de la propiedad no constituye un error irrecuperable en el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client. Si todos los demás valores de parámetro son válidos, se crea la tabla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|DBPROP_COL_FIXEDLENGTH|R (lectura) y W (escritura): Lectura/escritura<br /><br /> Valor predeterminado: VARIANT_FALSE<br /><br /> Descripción: el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client utiliza DBPROP_COL_FIXEDLENGTH para determinar la asignación de tipos de datos cuando el consumidor define el tipo de datos de una columna mediante el miembro *WTYPE* de DBCOLUMNDESC. Para más información, consulte [Asignación de tipos de datos en ITableDefinition](../../relational-databases/native-client-ole-db-data-types/data-type-mapping-in-itabledefinition.md).|  
|DBPROP_COL_NULLABLE|R (lectura) y W (escritura): Lectura/escritura<br /><br /> Valor predeterminado: None<br /><br /> Descripción: al crear la tabla, el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client indica si la columna debe aceptar valores NULL si se establece la propiedad. Cuando no se establece la propiedad, la capacidad de la columna de aceptar valores NULL como valores está determinada por la opción de base de datos predeterminada ANSI_NULLS de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> El [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client es un proveedor compatible con ISO. Las sesiones conectadas exhiben los comportamientos ISO. Si el consumidor no establece DBPROP_COL_NULLABLE, las columnas aceptan valores nulos.|  
|DBPROP_COL_PRIMARYKEY|R (lectura) y W (escritura): Lectura/escritura<br /><br /> Default: VARIANT_FALSE Description: cuando se VARIANT_TRUE, el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client crea la columna con una restricción primary key.<br /><br /> Cuando se define como una propiedad de columna, solo una columna única puede determinar la restricción. Al establecer la propiedad VARIANT_TRUE para más de una sola columna, se devuelve un error cuando el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client intenta crear la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tabla.<br /><br /> Nota: El consumidor puede usar **IIndexDefinition::CreateIndex** para crear una restricción PRIMARY KEY sobre dos o más columnas.<br /><br /> El [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client devuelve DB_S_ERRORSOCCURRED cuando DBPROP_COL_PRIMARYKEY y DBPROP_COL_UNIQUE son VARIANT_TRUE y el *dwOption* de DBPROP_COL_UNIQUE no se DBPROPOPTIONS_REQUIRED.<br /><br /> Se devuelve DB_E_ERRORSOCCURRED cuando DBPROP_COL_PRIMARYKEY y DBPROP_COL_UNIQUE son VARIANT_TRUE y *dwOption* de DBPROP_COL_UNIQUE es igual a DBPROPOPTIONS_REQUIRED. La columna se define con la propiedad de identidad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y el miembro *dwStatus* de DBPROP_COL_PRIMARYKEY se establece en DBPROPSTATUS_CONFLICTING.<br /><br /> El [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client devuelve un error cuando DBPROP_COL_PRIMARYKEY y DBPROP_COL_NULLABLE VARIANT_TRUE.<br /><br /> El [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client devuelve un error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cuando el consumidor intenta crear una restricción de clave principal en una columna de tipo de datos no válido [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . No se pueden definir restricciones PRIMARY KEY en las columnas creadas con los tipos de datos **bit**, **text**, **ntext** e **image** de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|DBPROP_COL_UNIQUE|R (lectura) y W (escritura): Lectura/escritura<br /><br /> Valor predeterminado: Descripción de VARIANT_FALSE: aplica una restricción UNIQUE de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a la columna.<br /><br /> Cuando se define como una propiedad de columna, la restricción solo se aplica en una columna única. El consumidor puede usar **IIndexDefinition::CreateIndex** para aplicar una restricción UNIQUE en los valores combinados de dos o más columnas.<br /><br /> El [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client devuelve DB_S_ERRORSOCCURRED cuando DBPROP_COL_PRIMARYKEY y DBPROP_COL_UNIQUE son VARIANT_TRUE y *dwOption* no DBPROPOPTIONS_REQUIRED.<br /><br /> Se devuelve DB_E_ERRORSOCCURRED cuando DBPROP_COL_PRIMARYKEY y DBPROP_COL_UNIQUE son VARIANT_TRUE y *dwOption* es igual a DBPROPOPTIONS_REQUIRED. La columna se define con la propiedad de identidad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y el miembro *dwStatus* de DBPROP_COL_PRIMARYKEY se establece en DBPROPSTATUS_CONFLICTING.<br /><br /> El [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client devuelve DB_S_ERRORSOCCURRED cuando DBPROP_COL_NULLABLE y DBPROP_COL_UNIQUE son VARIANT_TRUE y *dwOption* no DBPROPOPTIONS_REQUIRED.<br /><br /> Se devuelve DB_E_ERRORSOCCURRED cuando DBPROP_COL_NULLABLE y DBPROP_COL_UNIQUE son VARIANT_TRUE y *dwOption* es igual a DBPROPOPTIONS_REQUIRED. La columna se define con la propiedad de identidad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y el miembro DBPROP_COL_NULLABLE de *dwStatus* se establece en DBPROPSTATUS_CONFLICTING.<br /><br /> El [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client devuelve un error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cuando el consumidor intenta crear una restricción UNIQUE en una columna de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipo de datos no válido. No se pueden definir restricciones UNIQUE en las columnas creadas con el tipo de datos **bit** de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
 Cuando el consumidor llama a **ITableDefinition:: CreateTable**, el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client interpreta las propiedades de la tabla como se indica a continuación.  
  
|Id. de propiedad|Descripción|  
|-----------------|-----------------|  
|DBPROP_TBL_TEMPTABLE|R (lectura) y W (escritura): Lectura/escritura<br /><br /> Default: VARIANT_FALSE Description: de forma predeterminada, el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client crea tablas denominadas por el consumidor. Cuando se VARIANT_TRUE, el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client genera un nombre de tabla temporal para el consumidor. El consumidor establece el parámetro *pTableID* de **CreateTable** en NULL. El parámetro *ppTableID* tiene que contener un puntero válido.|  
  
 Si el consumidor solicita que se abra un conjunto de filas en una tabla creada correctamente, el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client abre un conjunto de filas compatible con el cursor. Las propiedades del conjunto de filas se pueden indicar en los conjuntos de propiedades pasados.  
  
 En este ejemplo se crea una tabla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```  
// This CREATE TABLE statement shows the details of the table created by   
// the following example code.  
//  
// CREATE TABLE OrderDetails  
// (  
//    OrderID      int      NOT NULL  
//    ProductID   int      NOT NULL  
//    CONSTRAINT PK_OrderDetails  
//         PRIMARY KEY CLUSTERED (OrderID, ProductID),  
//    UnitPrice   money      NOT NULL,  
//    Quantity   int      NOT NULL,  
//    Discount   decimal(2,2)   NOT NULL  
//        DEFAULT 0  
// )  
//  
// The PRIMARY KEY constraint is created in an additional example.  
HRESULT CreateTable  
    (  
    ITableDefinition* pITableDefinition  
    )  
    {  
    DBID            dbidTable;  
    const ULONG     nCols = 5;  
    ULONG           nCol;  
    ULONG           nProp;  
    DBCOLUMNDESC    dbcoldesc[nCols];  
  
    HRESULT         hr;  
  
    // Set up column descriptions. First, set default property values for  
    //  the columns.  
    for (nCol = 0; nCol < nCols; nCol++)  
        {  
        dbcoldesc[nCol].pwszTypeName = NULL;  
        dbcoldesc[nCol].pTypeInfo = NULL;  
        dbcoldesc[nCol].rgPropertySets = new DBPROPSET;  
        dbcoldesc[nCol].pclsid = NULL;  
        dbcoldesc[nCol].cPropertySets = 1;  
        dbcoldesc[nCol].ulColumnSize = 0;  
        dbcoldesc[nCol].dbcid.eKind = DBKIND_NAME;  
        dbcoldesc[nCol].wType = DBTYPE_I4;  
        dbcoldesc[nCol].bPrecision = 0;  
        dbcoldesc[nCol].bScale = 0;  
  
        dbcoldesc[nCol].rgPropertySets[0].rgProperties =   
            new DBPROP[NCOLPROPS_MAX];  
        dbcoldesc[nCol].rgPropertySets[0].cProperties = NCOLPROPS_MAX;  
        dbcoldesc[nCol].rgPropertySets[0].guidPropertySet =  
            DBPROPSET_COLUMN;  
  
        for (nProp = 0; nProp < NCOLPROPS_MAX; nProp++)  
            {  
            dbcoldesc[nCol].rgPropertySets[0].rgProperties[nProp].  
                dwOptions = DBPROPOPTIONS_REQUIRED;  
            dbcoldesc[nCol].rgPropertySets[0].rgProperties[nProp].colid  
                 = DB_NULLID;  
  
            VariantInit(  
                &(dbcoldesc[nCol].rgPropertySets[0].rgProperties[nProp].  
                    vValue));  
  
            dbcoldesc[nCol].rgPropertySets[0].rgProperties[nProp].  
                vValue.vt = VT_BOOL;  
            }  
        }  
  
    // Set the column-specific information.  
    dbcoldesc[0].dbcid.uName.pwszName = L"OrderID";  
    dbcoldesc[0].rgPropertySets[0].rgProperties[0].dwPropertyID =   
        DBPROP_COL_NULLABLE;  
    dbcoldesc[0].rgPropertySets[0].rgProperties[0].vValue.boolVal =   
        VARIANT_FALSE;  
    dbcoldesc[0].rgPropertySets[0].cProperties = 1;  
  
    dbcoldesc[1].dbcid.uName.pwszName = L"ProductID";  
    dbcoldesc[1].rgPropertySets[0].rgProperties[0].dwPropertyID =   
        DBPROP_COL_NULLABLE;  
    dbcoldesc[1].rgPropertySets[0].rgProperties[0].vValue.boolVal =   
        VARIANT_FALSE;  
    dbcoldesc[1].rgPropertySets[0].cProperties = 1;  
  
    dbcoldesc[2].dbcid.uName.pwszName = L"UnitPrice";  
    dbcoldesc[2].wType = DBTYPE_CY;  
    dbcoldesc[2].rgPropertySets[0].rgProperties[0].dwPropertyID =   
        DBPROP_COL_NULLABLE;  
    dbcoldesc[2].rgPropertySets[0].rgProperties[0].vValue.boolVal =   
        VARIANT_FALSE;  
    dbcoldesc[2].rgPropertySets[0].cProperties = 1;  
  
    dbcoldesc[3].dbcid.uName.pwszName = L"Quantity";  
    dbcoldesc[3].rgPropertySets[0].rgProperties[0].dwPropertyID =   
        DBPROP_COL_NULLABLE;  
    dbcoldesc[3].rgPropertySets[0].rgProperties[0].vValue.boolVal =   
        VARIANT_FALSE;  
    dbcoldesc[3].rgPropertySets[0].cProperties = 1;  
  
    dbcoldesc[4].dbcid.uName.pwszName = L"Discount";  
    dbcoldesc[4].wType = DBTYPE_NUMERIC;  
    dbcoldesc[4].bPrecision = 2;  
    dbcoldesc[4].bScale = 2;  
    dbcoldesc[4].rgPropertySets[0].rgProperties[0].dwPropertyID =   
        DBPROP_COL_NULLABLE;  
    dbcoldesc[4].rgPropertySets[0].rgProperties[0].vValue.boolVal =   
        VARIANT_FALSE;  
    dbcoldesc[4].rgPropertySets[0].rgProperties[1].dwPropertyID =   
        DBPROP_COL_DEFAULT;  
    dbcoldesc[4].rgPropertySets[0].rgProperties[1].vValue.vt = VT_BSTR;  
    dbcoldesc[4].rgPropertySets[0].rgProperties[1].vValue.bstrVal =  
        SysAllocString(L"0");  
    dbcoldesc[4].rgPropertySets[0].cProperties = 2;  
  
    // Set up the dbid for OrderDetails.  
    dbidTable.eKind = DBKIND_NAME;  
    dbidTable.uName.pwszName = L"OrderDetails";  
  
    if (FAILED(hr = pITableDefinition->CreateTable(NULL, &dbidTable,  
        nCols, dbcoldesc, NULL, 0, NULL, NULL, NULL)))  
        {  
        DumpError(pITableDefinition, IID_ITableDefinition);  
        goto SAFE_EXIT;  
        }  
  
SAFE_EXIT:  
    // Clean up dynamic allocation in the property sets.  
    for (nCol = 0; nCol < nCols; nCol++)  
        {  
        for (nProp = 0; nProp < NCOLPROPS_MAX; nProp++)  
            {  
            if (dbcoldesc[nCol].rgPropertySets[0].rgProperties[nProp].  
                vValue.vt == VT_BSTR)  
                {  
                SysFreeString(dbcoldesc[nCol].rgPropertySets[0].  
                    rgProperties[nProp].vValue.bstrVal);  
                }  
            }  
  
        delete [] dbcoldesc[nCol].rgPropertySets[0].rgProperties;  
        delete [] dbcoldesc[nCol].rgPropertySets;  
        }  
  
    return (hr);  
    }  
```  
  
## <a name="see-also"></a>Consulte también  
 [Tablas e índices](../../relational-databases/native-client-ole-db-tables-indexes/tables-and-indexes.md)  
  
  
