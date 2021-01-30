---
description: SchemaEnum
title: SchemaEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- SchemaEnum
helpviewer_keywords:
- SchemaEnum enumeration [ADO]
ms.assetid: 21c97651-297f-469f-b5b5-c48af72b62a8
author: rothja
ms.author: jroth
ms.openlocfilehash: 55a55d39117f2367b1b13c1a25b3785fe2d9cacc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170307"
---
# <a name="schemaenum"></a>SchemaEnum
Especifica el tipo de **conjunto de registros** de esquema que recupera el método [OpenSchema](./openschema-method.md) .  
  
## <a name="remarks"></a>Observaciones  
 Puede encontrar información adicional sobre la función y las columnas devueltas para cada constante de ADO en los temas del [Apéndice B: conjuntos de filas de esquema](/previous-versions/windows/desktop/ms712921(v=vs.85)) de la referencia del programador de OLE DB. El nombre de cada tema se muestra entre paréntesis en la sección Descripción de la tabla siguiente.  
  
 Puede encontrar información adicional sobre la función y las columnas devueltas para cada ADO MD constante en los temas de [OLE DB para los objetos OLAP y los conjuntos de filas de esquema](/previous-versions/windows/desktop/ms723056(v=vs.85)) en la OLE DB para la documentación de procesamiento analítico en línea (OLAP). El nombre de cada tema se muestra entre paréntesis en la columna Descripción de la tabla siguiente.  
  
 Puede traducir los tipos de datos de las columnas de la documentación de OLE DB a tipos de datos de ADO si hace referencia a la columna Description del tema de la [DataTypeEnum](./datatypeenum.md) de ADO. Por ejemplo, un tipo de datos OLE DB de **DBTYPE_WSTR** es equivalente a un tipo de datos ADO de **adWChar**.  
  
 ADO genera resultados similares a los esquemas para las constantes, **adSchemaDBInfoKeywords** y **adSchemaDBInfoLiterals**. ADO crea un **conjunto de registros** y, a continuación, rellena cada fila con los valores devueltos por los métodos **IDBInfo:: GetKeywords** y **IDBInfo:: GetLiteralInfo** . Puede encontrar información adicional sobre estos métodos en la sección [IDBInfo](/previous-versions/windows/desktop/ms713663(v=vs.85)) de la referencia del programador de OLE DB.  
  
|Constante|Value|Descripción|Columnas de restricción|  
|--------------|-----------|-----------------|------------------------|  
|**adSchemaAsserts**|0|Devuelve las aserciones definidas en el catálogo que son propiedad de un usuario determinado.<br /><br /> (Conjunto de filas ASERCIONES)|CONSTRAINT_CATALOG CONSTRAINT_SCHEMA CONSTRAINT_NAME|  
|**adSchemaCatalogs**|1|Devuelve los atributos físicos asociados a los catálogos accesibles desde DBMS.<br /><br /> (Conjunto de filas CATALOGs)|CATALOG_NAME|  
|**adSchemaCharacterSets**|2|Devuelve los juegos de caracteres definidos en el catálogo a los que puede tener acceso un usuario determinado.<br /><br /> (CHARACTER_SETS conjunto de filas)|CHARACTER_SET_CATALOG CHARACTER_SET_SCHEMA CHARACTER_SET_NAME|  
|**adSchemaCheckConstraints**|5|Devuelve las restricciones check definidas en el catálogo que son propiedad de un usuario determinado.<br /><br /> (CHECK_CONSTRAINTS) MultiRow|CONSTRAINT_CATALOG CONSTRAINT_SCHEMA CONSTRAINT_NAME|  
|**adSchemaCollations**|3|Devuelve las intercalaciones de caracteres definidas en el catálogo a las que puede tener acceso un usuario determinado.<br /><br /> (Conjunto de filas COLLAtions)|COLLATION_CATALOG COLLATION_SCHEMA COLLATION_NAME|  
|**adSchemaColumnPrivileges**|13|Devuelve los privilegios de las columnas de las tablas definidas en el catálogo que están disponibles para un usuario determinado o que se le conceden.<br /><br /> (COLUMN_PRIVILEGES conjunto de filas)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME COLUMN_NAME GRANTOR GRANTEE|  
|**adSchemaColumns**|4|Devuelve las columnas de las tablas (incluidas las vistas) definidas en el catálogo a las que puede tener acceso un usuario determinado.<br /><br /> (Conjunto de filas COLUMNs)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME COLUMN_NAME|  
|**adSchemaColumnsDomainUsage**|11|Devuelve las columnas definidas en el catálogo que dependen de un dominio definido en el catálogo y que pertenecen a un usuario determinado.<br /><br /> (COLUMN_DOMAIN_USAGE conjunto de filas)|DOMAIN_CATALOG DOMAIN_SCHEMA DOMAIN_NAME COLUMN_NAME|  
|**adSchemaConstraintColumnUsage**|6|Devuelve las columnas utilizadas por las restricciones referenciales, restricciones únicas, restricciones Check y aserciones definidas en el catálogo y que pertenecen a un usuario determinado.<br /><br /> (CONSTRAINT_COLUMN_USAGE conjunto de filas)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME COLUMN_NAME|  
|**adSchemaConstraintTableUsage**|7|Devuelve las tablas utilizadas por las restricciones referenciales, restricciones únicas, restricciones Check y aserciones definidas en el catálogo y que pertenecen a un usuario determinado.<br /><br /> (CONSTRAINT_TABLE_USAGE conjunto de filas)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME|  
|**adSchemaCubes**|32|Devuelve información acerca de los cubos disponibles en un esquema (o el catálogo, si el proveedor no admite esquemas).<br /><br /> (Conjunto de filas CUBEs *)|CATALOG_NAME SCHEMA_NAME CUBE_NAME|  
|**adSchemaDBInfoKeywords**|30|Devuelve una lista de palabras clave específicas del proveedor.<br /><br /> (IDBInfo:: GetKeywords)|\<None>|  
|**adSchemaDBInfoLiterals**|31|Devuelve una lista de literales específicos del proveedor utilizados en los comandos de texto.<br /><br /> (IDBInfo:: GetLiteralInfo)|\<None>|  
|**adSchemaDimensions**|33|Devuelve información sobre las dimensiones de un cubo determinado. Tiene una fila para cada dimensión.<br /><br /> (Conjunto de filas DIMENSIONs)|CATALOG_NAME SCHEMA_NAME CUBE_NAME DIMENSION_NAME DIMENSION_UNIQUE_NAME|  
|**adSchemaForeignKeys**|27|Devuelve las columnas de clave externa definidas en el catálogo por un usuario determinado.<br /><br /> (FOREIGN_KEYS conjunto de filas)|PK_TABLE_CATALOG PK_TABLE_SCHEMA PK_TABLE_NAME FK_TABLE_CATALOG FK_TABLE_SCHEMA FK_TABLE_NAME|  
|**adSchemaHierarchies**|34|Devuelve información sobre las jerarquías disponibles en una dimensión.<br /><br /> (Conjunto de filas Hierarchies)|CATALOG_NAME SCHEMA_NAME CUBE_NAME DIMENSION_UNIQUE_NAME HIERARCHY_NAME HIERARCHY_UNIQUE_NAME|  
|**adSchemaIndexes**|12|Devuelve los índices definidos en el catálogo que son propiedad de un usuario determinado.<br /><br /> (Conjunto de filas INDEXes)|TABLE_CATALOG TIPO DE INDEX_NAME DE TABLE_SCHEMA TABLE_NAME|  
|**adSchemaKeyColumnUsage**|8|Devuelve las columnas definidas en el catálogo que un usuario determinado restringe como claves.<br /><br /> (KEY_COLUMN_USAGE conjunto de filas)|CONSTRAINT_CATALOG CONSTRAINT_SCHEMA CONSTRAINT_NAME TABLE_CATALOG TABLE_SCHEMA TABLE_NAME COLUMN_NAME|  
|**adSchemaLevels**|35|Devuelve información sobre los niveles disponibles en una dimensión.<br /><br /> (Conjunto de filas LEVELs)|CATALOG_NAME SCHEMA_NAME CUBE_NAME DIMENSION_UNIQUE_NAME HIERARCHY_UNIQUE_NAME LEVEL_NAME LEVEL_UNIQUE_NAME|  
|**adSchemaMeasures**|36|Devuelve información sobre las medidas disponibles.<br /><br /> (Conjunto de filas MEASUREs)|CATALOG_NAME SCHEMA_NAME CUBE_NAME MEASURE_NAME MEASURE_UNIQUE_NAME|  
|**adSchemaMembers**|38|Devuelve información sobre los miembros disponibles.<br /><br /> (Conjunto de filas MEMBERs)|CATALOG_NAME SCHEMA_NAME CUBE_NAME DIMENSION_UNIQUE_NAME HIERARCHY_UNIQUE_NAME LEVEL_UNIQUE_NAME LEVEL_NUMBER MEMBER_NAME MEMBER_UNIQUE_NAME MEMBER_CAPTION MEMBER_TYPE. Para obtener más información, vea OLE DB para el procesamiento analítico en línea (OLAP).|  
|**adSchemaPrimaryKeys**|28|Devuelve las columnas de clave principal definidas en el catálogo por un usuario determinado.<br /><br /> (PRIMARY_KEYS conjunto de filas)|PK_TABLE_CATALOG PK_TABLE_SCHEMA PK_TABLE_NAME|  
|**adSchemaProcedureColumns**|29|Devuelve información sobre las columnas de los conjuntos de filas que devuelven los procedimientos.<br /><br /> (PROCEDURE_COLUMNS conjunto de filas)|PROCEDURE_CATALOG PROCEDURE_SCHEMA PROCEDURE_NAME COLUMN_NAME|  
|**adSchemaProcedureParameters**|26|Devuelve información sobre los parámetros y los códigos devueltos de los procedimientos.<br /><br /> (PROCEDURE_PARAMETERS conjunto de filas)|PROCEDURE_CATALOG PROCEDURE_SCHEMA PROCEDURE_NAME PARAMETER_NAME|  
|**adSchemaProcedures**|16|Devuelve los procedimientos definidos en el catálogo que son propiedad de un usuario determinado.<br /><br /> (Conjunto de filas PROCEDUREs)|PROCEDURE_CATALOG PROCEDURE_SCHEMA PROCEDURE_NAME PROCEDURE_TYPE|  
|**adSchemaProperties**|37|Devuelve información sobre las propiedades disponibles para cada nivel de la dimensión.<br /><br /> (Conjunto de filas PROPERTIES)|CATALOG_NAME SCHEMA_NAME CUBE_NAME DIMENSION_UNIQUE_NAME HIERARCHY_UNIQUE_NAME LEVEL_UNIQUE_NAME MEMBER_UNIQUE_NAME PROPERTY_TYPE PROPERTY_NAME|  
|**adSchemaProviderSpecific**|-1|Se utiliza si el proveedor define sus propias consultas de esquema no estándar.|\<Provider specific>|  
|**adSchemaProviderTypes**|22|Devuelve los tipos de datos (base) admitidos por el proveedor de datos.<br /><br /> (PROVIDER_TYPES conjunto de filas)|DATA_TYPE BEST_MATCH|  
|**AdSchemaReferentialConstraints**|9|Devuelve las restricciones referenciales definidas en el catálogo que son propiedad de un usuario determinado.<br /><br /> (REFERENTIAL_CONSTRAINTS conjunto de filas)|CONSTRAINT_CATALOG CONSTRAINT_SCHEMA CONSTRAINT_NAME|  
|**adSchemaSchemata**|17|Devuelve los esquemas (objetos de base de datos) que son propiedad de un usuario determinado.<br /><br /> (Conjunto de filas de esquema)|CATALOG_NAME SCHEMA_NAME SCHEMA_OWNER|  
|**adSchemaSQLLanguages**|18|Devuelve los niveles, opciones y dialectos de compatibilidad que admiten los datos de procesamiento de la implementación SQL definidos en el catálogo.<br /><br /> (SQL_LANGUAGES conjunto de filas)|\<None>|  
|**adSchemaStatistics**|19|Devuelve las estadísticas definidas en el catálogo que son propiedad de un usuario determinado.<br /><br /> (Conjunto de filas STATISTICs)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME|  
|**adSchemaTableConstraints**|10|Devuelve las restricciones de tabla definidas en el catálogo que son propiedad de un usuario determinado.<br /><br /> (TABLE_CONSTRAINTS conjunto de filas)|CONSTRAINT_CATALOG CONSTRAINT_SCHEMA CONSTRAINT_NAME TABLE_CATALOG TABLE_SCHEMA TABLE_NAME CONSTRAINT_TYPE|  
|**adSchemaTablePrivileges**|14|Devuelve los privilegios en tablas definidas en el catálogo que están disponibles para un usuario determinado o que se le conceden.<br /><br /> (TABLE_PRIVILEGES conjunto de filas)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME GRANTOR GRANTEE|  
|**adSchemaTables**|20|Recupera las tablas (incluidas las vistas) definidas en el catálogo a las que puede tener acceso un usuario determinado.<br /><br /> (Conjunto de filas TABLEs)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME TABLE_TYPE|  
|**adSchemaTranslations**|21|Devuelve las traducciones de caracteres definidas en el catálogo a las que puede tener acceso un usuario determinado.<br /><br /> (Conjunto de filas Translations)|TRANSLATION_CATALOG TRANSLATION_SCHEMA TRANSLATION_NAME|  
|**adSchemaTrustees**|39|Reservado para un uso futuro.||  
|**adSchemaUsagePrivileges**|15|Devuelve los privilegios de uso de los objetos definidos en el catálogo que están disponibles para un usuario determinado o que se le conceden.<br /><br /> (USAGE_PRIVILEGES conjunto de filas)|OBJECT_CATALOG OBJECT_SCHEMA OBJECT_NAME OBJECT_TYPE RECEPTOR DE GRANTOR|  
|**adSchemaViewColumnUsage**|24|Devuelve las columnas de las que dependen las tablas vistas, definidas en el catálogo y que pertenecen a un usuario determinado.<br /><br /> (VIEW_COLUMN_USAGE conjunto de filas)|VIEW_CATALOG VIEW_SCHEMA VIEW_NAME|  
|**adSchemaViews**|23|Devuelve las vistas definidas en el catálogo a las que puede tener acceso un usuario determinado.<br /><br /> (Conjunto de filas VIEWs)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME|  
|**adSchemaViewTableUsage**|25|Devuelve las tablas de las que dependen las tablas vistas, definidas en el catálogo y que pertenecen a un usuario determinado.<br /><br /> (VIEW_TABLE_USAGE conjunto de filas)|VIEW_CATALOG VIEW_SCHEMA VIEW_NAME|  
  
## <a name="adowfc-equivalent"></a>Equivalente de ADO/WFC  
 Paquete: **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums. Schema. Asserts|  
|AdoEnums. Schema. CATALOGs|  
|AdoEnums. Schema. CHARACTERSETS|  
|AdoEnums. Schema. CHECKCONSTRAINTS|  
|AdoEnums. Schema. COLLAtions|  
|AdoEnums. Schema. COLUMNPRIVILEGES|  
|AdoEnums. Schema. COLUMNs|  
|AdoEnums. Schema. COLUMNSDOMAINUSAGE|  
|AdoEnums. Schema. CONSTRAINTCOLUMNUSAGE|  
|AdoEnums. Schema. CONSTRAINTTABLEUSAGE|  
|AdoEnums. Schema. CUBEs|  
|AdoEnums. Schema. DBINFOKEYWORDS|  
|AdoEnums. Schema. DBINFOLITERALS|  
|AdoEnums. Schema. DIMENSIONs|  
|AdoEnums. Schema. FOREIGNKEYS|  
|AdoEnums. Schema. HIERARCHYs|  
|AdoEnums. Schema. INDEXes|  
|AdoEnums. Schema. KEYCOLUMNUSAGE|  
|AdoEnums. Schema. LEVELs|  
|AdoEnums. Schema. MEASUREs|  
|AdoEnums. Schema. MEMBERs|  
|AdoEnums. Schema. PRIMARYKEYS|  
|AdoEnums. Schema. PROCEDURECOLUMNS|  
|AdoEnums. Schema. PROCEDUREPARAMETERS|  
|AdoEnums. Schema. PROCEDUREs|  
|AdoEnums. Schema. PROPERTIES|  
|AdoEnums. Schema. PROVIDERSPECIFIC|  
|AdoEnums. Schema. PROVIDERTYPES|  
|AdoEnums. Schema. REFERENTIALCONTRAINTS|  
|AdoEnums. Schema. esquemas|  
|AdoEnums. Schema. SQLLANGUAGES|  
|AdoEnums. Schema. STATISTICs|  
|AdoEnums. Schema. TABLECONSTRAINTS|  
|AdoEnums. Schema. TABLEPRIVILEGES|  
|AdoEnums. Schema. TABLEs|  
|AdoEnums. Schema. Translations|  
|AdoEnums. Schema. TRUSTEEs|  
|AdoEnums. Schema. USAGEPRIVILEGES|  
|AdoEnums. Schema. VIEWCOLUMNUSAGE|  
|AdoEnums. Schema. VIEWs|  
|AdoEnums. Schema. VIEWTABLEUSAGE|  
  
## <a name="applies-to"></a>Se aplica a  
 [Método OpenSchema](./openschema-method.md)