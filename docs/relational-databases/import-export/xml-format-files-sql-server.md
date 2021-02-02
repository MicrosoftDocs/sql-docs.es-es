---
title: Archivos de formato XML (SQL Server) | Microsoft Docs
description: SQL Server 2019 proporciona un esquema XML que define la sintaxis para escribir archivos de formato XML que se usarán para la importación en bloque de datos en una tabla de SQL Server.
ms.custom: ''
ms.date: 01/11/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- format files [SQL Server], XML format files
- bulk importing [SQL Server], format files
- XML format files [SQL Server]
ms.assetid: 69024aad-eeea-4187-8fea-b49bc2359849
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d5f90439c622e9740ffe5d58789ce3d950be0279
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049298"
---
# <a name="xml-format-files-sql-server"></a>XML, archivos de formato (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] proporciona un esquema XML que define la sintaxis para escribir *archivos de formato XML* que se usarán para la importación masiva de datos en una tabla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Los archivos de formato XML deben adherirse a este esquema, que se define en el lenguaje de definición de esquemas XML (XSDL). Los archivos con formato XML solamente se admiten cuando se instalan herramientas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
 Puede usar un archivo de formato XML con un comando **bcp**, una instrucción BULK INSERT o una instrucción INSERT... Instrucción SELECT \* FROM OPENROWSET(BULK...). El comando **bcp** permite generar automáticamente un archivo de formato XML para una tabla; para más información, consulte [bcp Utility](../../tools/bcp-utility.md).  
  
> [!NOTE]  
>  Se admiten dos tipos de archivos de formato para la importación y exportación masivas: *archivos de formato no XML* y *archivos de formato XML*. Los archivos de formato XML proporcionan una alternativa flexible y eficaz a los archivos de formato no XML. Para obtener información sobre los archivos de formato no XML, vea [Archivos de formato no XML &#40;SQL Server&#41;](../../relational-databases/import-export/non-xml-format-files-sql-server.md).  
  
 **En este tema:**  
  
-   [Ventajas de los archivos de formato XML](#BenefitsOfXmlFFs)  
  
-   [Estructura de los archivos de formato XML](#StructureOfXmlFFs)  
  
-   [Sintaxis de esquema para archivos de formato XML](#SchemaSyntax)  
  
-   [Archivos de formato XML de ejemplo](#SampleXmlFFs)  
  
-   [Tareas relacionadas](#RelatedTasks)  
  
-   [Contenido relacionado](#RelatedContent)  
  
##  <a name="benefits-of-xml-format-files"></a><a name="BenefitsOfXmlFFs"></a> Ventajas de los archivos de formato XML  
  
-   Los archivos de formato XML son autodescriptivos, lo que facilita su lectura, creación y ampliación. Los usuarios pueden leerlos, lo que facilita la comprensión del modo en que se interpretan los datos durante las operaciones masivas.  
  
-   Los archivos de formato XML contienen los tipos de datos de las columnas de destino.  La codificación XML describe claramente los tipos de datos y elementos de datos del archivo de datos, así como la asignación entre los elementos de datos y las columnas de las tablas.  
  
     Esta característica habilita la separación entre la representación de los datos en el archivo de datos y el tipo de datos asociado a cada campo del archivo. Por ejemplo, si un archivo de datos contiene una representación de caracteres de los datos, se perderá el tipo de columna SQL correspondiente.  
  
-   Un archivo de formato XML permite cargar un campo que contenga un único tipo de datos de objeto grande (LOB) desde un archivo de datos.  
  
-   Un archivo de formato XML puede mejorarse manteniendo su compatibilidad con sus versiones anteriores. Además, la claridad de la codificación XML facilita la creación de varios archivos de formato para un determinado archivo de datos. Esto es útil si tiene que asignar todos o algunos campos de datos a columnas de diferentes tablas o vistas.  
  
-   La sintaxis XML es independiente de la dirección de la operación; es decir, la sintaxis es la misma para exportaciones e importaciones masivas.  
  
-   Puede usar los archivos de formato XML para importar los datos de forma masiva en tablas o vistas sin particiones y para exportar los datos de forma masiva.  
  
-   Para OPENROWSET(BULK…) la función que especifica una tabla de destino es opcional. Esto se debe a que la función se basa en el archivo de formato XML para leer datos de un archivo de datos.  
  
    > [!NOTE]  
    >  Es necesaria una tabla de destino con el comando **bcp** y la instrucción BULK INSERT, que usa las columnas de la tabla de destino para realizar la conversión de tipos.  
  
##  <a name="structure-of-xml-format-files"></a><a name="StructureOfXmlFFs"></a> Estructura de los archivos de formato XML  
 Al igual que un archivo de formato no XML, un archivo de formato XML define el formato y la estructura de los campos de datos de un archivo de datos y asigna dichos campos a columnas de una sola tabla de destino.  
  
 Un archivo con formato XML posee dos componentes principales, \<RECORD> y \<ROW>:  
  
-   \<RECORD> describe los datos tal y como se almacenan en el archivo de datos.  
  
     Cada elemento \<RECORD> contiene un conjunto de uno o más elementos \<FIELD>. Dichos elementos corresponden a los campos del archivo de datos. La sintaxis básica es la siguiente:  
  
     \<RECORD>  
  
     \<FIELD .../> [ ...*n* ]  
  
     \</RECORD>  
  
     Cada elemento \<FIELD> describe el contenido de un campo de datos concreto. Un campo solo puede asignarse a una columna de la tabla. No es necesario asignar todos los campos a columnas.  
  
     Un campo de un archivo de datos puede tener una longitud fija o variable, o bien terminar mediante un carácter. Un *valor de campo* puede representarse como: un carácter (mediante una representación de un solo byte), un carácter ancho (mediante la representación Unicode de dos bytes), un formato de base de datos nativo o un nombre de archivo. Si un valor de campo se representa como un nombre de archivo, éste apunta al archivo que contiene el valor de una columna BLOB en la tabla de destino.  
  
-   \<ROW> describe el modo de construir filas de datos a partir de un archivo de datos cuando los datos del archivo se importan en una tabla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     Un elemento \<ROW> contiene un conjunto de elementos \<COLUMN>. Estos elementos corresponden a las columnas de la tabla. La sintaxis básica es la siguiente:  
  
     \<ROW>  
  
     \<COLUMN .../> [ ...*n* ]  
  
     \</ROW>  
  
     Cada elemento \<COLUMN> solo se puede asignar a un campo del archivo de datos. El orden de los elementos \<COLUMN> del elemento \<ROW> define el orden en el que la operación en bloque los devuelve. El archivo de formato XML asigna a cada elemento \<COLUMN> un nombre local que no tiene ninguna relación con la columna de la tabla de destino de una operación de importación en bloque.  
  
##  <a name="schema-syntax-for-xml-format-files"></a><a name="SchemaSyntax"></a> Sintaxis de esquema para archivos de formato XML  
 Esta sección contiene un resumen de los elementos y atributos del esquema XML para archivos de formato XML. La sintaxis de un archivo de formato es independiente de la dirección de la operación; es decir, la sintaxis es la misma para exportaciones e importaciones masivas. En esta sección también se describe cómo la importación en bloque usa los elementos \<ROW> y \<COLUMN>, y cómo colocar el valor xsi:type de un elemento en un conjunto de datos.  
  
 Para ver cómo la sintaxis corresponde a los archivos de formato XML reales, vea [Archivos de formato XML de ejemplo](#SampleXmlFFs), más adelante en este tema.  
  
> [!NOTE]  
>  Puede modificar un archivo de formato de forma que le permita importar de forma masiva desde un archivo de datos en el que el número y/o el orden de los campos difieren del número y/o el orden de las columnas de la tabla. Para obtener más información, vea [Archivos de formato para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).  
  
 **En esta sección:**  
  
-   [Sintaxis básica del esquema XML](#BasicSyntax)  
  
-   [Cómo usa la importación en bloque el elemento \<ROW>](#HowUsesROW)  
  
-   [Cómo usa la importación en bloque el elemento \<COLUMN>](#HowUsesColumn)  
  
-   [Colocar el valor xsi:type en un conjunto de datos](#PutXsiTypeValueIntoDataSet)  
  
###  <a name="basic-syntax-of-the-xml-schema"></a><a name="BasicSyntax"></a> Sintaxis básica del esquema XML  
 Las instrucciones de esta sintaxis muestran solo los elementos (\<BCPFORMAT>, \<RECORD>, \<FIELD>, \<ROW> y \<COLUMN>), y sus atributos básicos.  
  
 \<BCPFORMAT ...>  
  
 \<RECORD>  
  
 \<FIELD ID = "*fieldID*" xsi:type = "*fieldType*" [...]  
  
 />  
  
 \</RECORD>  
  
 \<ROW>  
  
 \<COLUMN SOURCE = "*fieldID*" NAME = "*columnName*" xsi:type = "*columnType*" [...]  
  
 />  
  
 \</ROW>  
  
 \</BCPFORMAT>  
  
> [!NOTE]  
>  Los atributos adicionales asociados al valor de xsi:type en un elemento \<FIELD> o \<COLUMN> se describen más adelante en este tema.  
  
 **En esta sección:**  
  
-   [Elementos de esquema](#SchemaElements)  
  
-   [Atributos del elemento \<FIELD>](#AttrOfFieldElement) (y [valores xsi:type del elemento \<FIELD>](#XsiTypeValuesOfFIELD))  
  
-   [Atributos del elemento \<COLUMN>](#AttrOfColumnElement) (y [valores xsi:type del elemento \<COLUMN>](#XsiTypeValuesOfCOLUMN))  
  
####  <a name="schema-elements"></a><a name="SchemaElements"></a> Elementos de esquema  
 En esta sección se resume la finalidad de cada elemento que define el esquema XML para los archivos de formato XML. Los atributos se describen más adelante, en otras secciones de este tema.  
  
 \<BCPFORMAT>  
 Es el elemento de archivo de formato que define la estructura de los registros de un determinado archivo de datos y su correspondencia con las columnas de una fila de tabla en la tabla.  
  
 \<RECORD .../>  
 Define un elemento complejo que contiene uno o más elementos \<FIELD>. El orden en que se declaran los campos en el archivo de formato es el orden en que estos campos aparecen en el archivo de datos.  
  
 \<FIELD .../>  
 Define un campo del archivo de datos que contiene datos.  
  
 Los atributos de este elemento se tratan en la sección [Atributos del elemento \<FIELD>](#AttrOfFieldElement) más adelante en este tema.  
  
 \<ROW .../>  
 Define un elemento complejo que contiene uno o más elementos \<COLUMN>. El orden de los elementos \<COLUMN> es independiente del de los elementos \<FIELD> de una definición RECORD. En su lugar, el orden de los elementos \<COLUMN> de un archivo de formato determina el orden de las columnas del conjunto de filas resultante. Los campos de datos se cargan en el orden en que los elementos \<COLUMN> correspondientes se declaran en el elemento \<COLUMN>.  
  
 Para obtener más información, vea [Cómo usa la importación en bloque el elemento \<ROW>](#HowUsesROW) más adelante en este tema.  
  
 \<COLUMN>  
 Define una columna como un elemento (\<COLUMN>). Cada elemento \<COLUMN> se corresponde a un elemento \<FIELD> (cuyo id. se especifica en el atributo SOURCE del elemento \<COLUMN>).  
  
 Los atributos de este elemento se tratan en la sección [Atributos del elemento \<COLUMN>](#AttrOfColumnElement) más adelante en este tema. Vea también [Cómo usa la importación en bloque el elemento \<COLUMN>](#HowUsesColumn) más adelante en este tema.  
  
 \</BCPFORMAT>  
 Obligatorio para finalizar el archivo de formato.  
  
####  <a name="attributes-of-the-field-element"></a><a name="AttrOfFieldElement"></a> Atributos del elemento \<FIELD>  
 En esta sección se describen los atributos del elemento \<FIELD>, que se resumen en la sintaxis de esquema siguiente:  
  
 <FIELD  
  
 ID **="** _fieldID_ **"**  
  
 xsi **:** type **="** _fieldType_ **"**  
  
 [ LENGTH **="** _n_ **"** ]  
  
 [ PREFIX_LENGTH **="** _p_ **"** ]  
  
 [ MAX_LENGTH **="** _m_ **"** ]  
  
 [ COLLATION **="** _collationName_ **"** ]  
  
 [ TERMINATOR **="** _terminator_ **"** ]  
  
 />  
  
 Cada elemento \<FIELD> es independiente de los demás. Un campo se describe según los atributos siguientes:  
  
|Atributo de FIELD|Descripción|Opcional /<br /><br /> Obligatorio|  
|---------------------|-----------------|------------------------------|  
|ID **="** _fieldID_ **"**|Especifica el nombre lógico del campo incluido en el archivo de datos. El valor de ID de un campo es la clave utilizada para referirse al campo.<br /><br /> \<FIELD ID**="**_fieldID_**"**/> se asigna a \<COLUMN SOURCE**="**_fieldID_**"**/>|Obligatorio|  
|xsi:type **="** _fieldType_ **"**|Es una construcción XML (utilizada como atributo) que identifica el tipo de la instancia del elemento. El valor de *fieldType* determina qué atributos opcionales (a continuación) necesita el usuario en una instancia determinada.|Obligatorio (en función del tipo de datos)|  
|LENGTH **="** _n_ **"**|Este atributo define la longitud de una instancia de un tipo de datos de longitud fija.<br /><br /> El valor de *n* debe ser un entero positivo.|Opcional a no ser que el valor de xsi:type lo requiera|  
|PREFIX_LENGTH **="** _p_ **"**|Este atributo define la longitud del prefijo para una representación de datos binarios. El valor de PREFIX_LENGTH, *p*, debe ser uno de los siguientes: 1, 2, 4 u 8.|Opcional a no ser que el valor de xsi:type lo requiera|  
|MAX_LENGTH **="** _m_ **"**|Este atributo es el número máximo de bytes que se pueden almacenar en un campo determinado. Sin una tabla de destino, la longitud máxima de la columna se desconoce. El atributo MAX_LENGTH restringe la longitud máxima de una columna de caracteres de salida y limita el almacenamiento asignado al valor de la columna. Esto resulta especialmente útil al usar la opción BULK de la función OPENROWSET en una cláusula SELECT FROM.<br /><br /> El valor de *m* debe ser un entero positivo. De forma predeterminada, la longitud máxima es de 8.000 caracteres para una columna **char** y de 4.000 caracteres para una columna **nchar** .|Opcional|  
|COLLATION **="** _collationName_ **"**|COLLATION solo se permite para campos de caracteres. Para ver una lista de los nombres de intercalación de SQL, vea [Nombre de intercalación de SQL Server &#40;Transact-SQL&#41;](../../t-sql/statements/sql-server-collation-name-transact-sql.md).|Opcional|  
|TERMINATOR **= "** _terminator_ **"**|Este atributo especifica el terminador de un campo de datos. El terminador puede ser cualquier carácter. Debe ser un carácter único que no forme parte de los datos.<br /><br /> De forma predeterminada, el terminador del campo es el carácter de tabulación (representado como \t). Para representar una marca de párrafo, utilice \r\n.|Solo se usa con xsi:type de datos de caracteres, que requiere este atributo|  
  
#####  <a name="xsitype-values-of-the-field-element"></a><a name="XsiTypeValuesOfFIELD"></a> Valores xsi:type del elemento \<FIELD>  
 El valor xsi:type es una construcción XML (usada como atributo) que identifica el tipo de datos de una instancia de un elemento. Para obtener más información acerca de su uso, vea "Colocar el valor xsi:type en un conjunto de datos", más adelante en este tema.  
  
 El valor xsi:type del elemento \<FIELD> admite los tipos de datos siguientes.  
  
|Valores xsi:type de \<FIELD>|Atributos XML obligatorios<br /><br /> para el tipo de datos|Atributos XML opcionales<br /><br /> para el tipo de datos|  
|-------------------------------|---------------------------------------------------|---------------------------------------------------|  
|**NativeFixed**|**LENGTH**|Ninguno.|  
|**NativePrefix**|**PREFIX_LENGTH**|MAX_LENGTH|  
|**CharFixed**|**LENGTH**|COLLATION|  
|**NCharFixed**|**LENGTH**|COLLATION|  
|**CharPrefix**|**PREFIX_LENGTH**|MAX_LENGTH, COLLATION|  
|**NCharPrefix**|**PREFIX_LENGTH**|MAX_LENGTH, COLLATION|  
|**CharTerm**|**TERMINATOR**|MAX_LENGTH, COLLATION|  
|**NCharTerm**|**TERMINATOR**|MAX_LENGTH, COLLATION|  
  
 Para obtener más información sobre los tipos de datos de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).  
  
####  <a name="attributes-of-the-column-element"></a><a name="AttrOfColumnElement"></a> Atributos del elemento \<COLUMN>  
 En esta sección se describen los atributos del elemento \<COLUMN>, que se resumen en la sintaxis de esquema siguiente:  
  
 <COLUMN  
  
 SOURCE = "*fieldID*"  
  
 NAME = "*columnName*"  
  
 xsi:type = "*columnType*"  
  
 [ LENGTH = "*n*" ]  
  
 [ PRECISION = "*n*" ]  
  
 [ SCALE = "*value*" ]  
  
 [ NULLABLE = { "YES"  
  
 "NO" } ]  
  
 />  
  
 Un campo se asigna a una columna de la tabla de destino mediante los atributos siguientes:  
  
|Atributo de COLUMN|Descripción|Opcional /<br /><br /> Obligatorio|  
|----------------------|-----------------|------------------------------|  
|SOURCE **="** _fieldID_ **"**|Especifica el Id. del campo que se asigna a la columna.<br /><br /> \<COLUMN SOURCE**="**_fieldID_**"**/> se asigna a \<FIELD ID**="**_fieldID_**"**/>|Obligatorio|  
|NAME = "*columnName*"|Especifica el nombre de la columna en el conjunto de filas representado por el archivo de formato. Este nombre de columna se utiliza para identificar la columna en el conjunto de resultados y no es necesario que corresponda al nombre de columna usado en la tabla de destino.|Obligatorio|  
|xsi **:** type **="** _ColumnType_ **"**|Es una construcción XML (utilizada como atributo) que identifica el tipo de datos de la instancia del elemento. El valor de *ColumnType* determina qué atributos opcionales (a continuación) necesita el usuario en una instancia determinada.<br /><br /> Nota: Los valores posibles de *ColumnType* y sus atributos asociados se enumeran en la tabla de elementos \<COLUMN> de la sección [Valores xsi:type del elemento &lt;COLUMN&gt;](#XsiTypeValuesOfCOLUMN).|Opcional|  
|LENGTH **="** _n_ **"**|Define la longitud de una instancia de un tipo de datos de longitud fija. LENGTH se utiliza solo cuando xsi:type es un tipo de datos de cadena.<br /><br /> El valor de *n* debe ser un entero positivo.|Opcional (solo disponible si xsi:type es un tipo de datos de cadena)|  
|PRECISION **="** _n_ **"**|Indica el número de dígitos de un número. Por ejemplo, el número 123,45 tiene una precisión de 5.<br /><br /> El valor debe ser un entero positivo.|Opcional (solo disponible si xsi:type es un tipo de datos de número variable)|  
|SCALE **="** _int_ **"**|Indica el número de dígitos situados a la derecha de la coma decimal de un número. Por ejemplo, el número 123,45 tiene una escala de 2.<br /><br /> El valor debe ser un entero.|Opcional (solo disponible si xsi:type es un tipo de datos de número variable)|  
|NULLABLE **=** { **"** YES **"**<br /><br /> **"** NO **"** }|Indica si una columna puede aceptar valores NULL. Este atributo es completamente independiente de FIELDS. No obstante, si una columna tiene el valor de NULLABLE establecido en NO y el campo especifica NULL (es decir, no especifica ningún valor), se produce un error de tiempo de ejecución.<br /><br /> El atributo NULLABLE solo se usa si escribe una instrucción SELECT FROM OPENROWSET(BULK...) simple.|Opcional (disponible para cualquier tipo de datos)|  
  
#####  <a name="xsitype-values-of-the-column-element"></a><a name="XsiTypeValuesOfCOLUMN"></a> Valores xsi:type del elemento \<COLUMN>  
 El valor xsi:type es una construcción XML (usada como atributo) que identifica el tipo de datos de una instancia de un elemento. Para obtener más información acerca de su uso, vea "Colocar el valor xsi:type en un conjunto de datos", más adelante en este tema.  
  
 El elemento \<COLUMN> admite tipos de datos SQL nativos, de la forma siguiente:  
  
|Categoría de tipo|Tipos de datos de \<COLUMN>|Atributos XML obligatorios<br /><br /> para el tipo de datos|Atributos XML opcionales<br /><br /> para el tipo de datos|  
|-------------------|---------------------------|---------------------------------------------------|---------------------------------------------------|  
|Corregido|**SQLBIT**, **SQLTINYINT**, **SQLSMALLINT**, **SQLINT**, **SQLBIGINT**, **SQLFLT4**, **SQLFLT8**, **SQLDATETIME**, **SQLDATETIM4**, **SQLDATETIM8**, **SQLMONEY**, **SQLMONEY4**, **SQLVARIANT** y **SQLUNIQUEID**|Ninguno.|NULLABLE|  
|Número de variable|**SQLDECIMAL** y **SQLNUMERIC**|Ninguno.|NULLABLE, PRECISION, SCALE|  
|LOB|**SQLIMAGE**, **CharLOB**, **SQLTEXT** y **SQLUDT**|Ninguno.|NULLABLE|  
|LOB de caracteres|**SQLNTEXT**|Ninguno.|NULLABLE|  
|Cadena binaria|**SQLBINARY** y **SQLVARYBIN**|Ninguno.|NULLABLE, LENGTH|  
|Cadena de caracteres|**SQLCHAR**, **SQLVARYCHAR**, **SQLNCHAR** y **SQLNVARCHAR**|Ninguno.|NULLABLE, LENGTH|  
  
> [!IMPORTANT]  
>  Para importar o exportar de forma masiva datos SQLXML, utilice uno de los tipos de datos siguientes en el archivo de formato: SQLCHAR o SQLVARYCHAR (los datos se envían en la página de códigos del cliente o en la página de códigos implícita en la intercalación), SQLNCHAR o SQLNVARCHAR (los datos se envían como Unicode), o SQLBINARY o SQLVARYBIN (los datos se envían sin ninguna conversión).  
  
 Para obtener más información sobre los tipos de datos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vea [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).  
  
###  <a name="how-bulk-import-uses-the-row-element"></a><a name="HowUsesROW"></a> Cómo usa la importación en bloque el elemento \<ROW>  
 El elemento \<ROW> se omite en algunos contextos. El hecho de que un elemento \<ROW> afecte a una operación de importación en bloque depende de cómo se realice la operación:  
  
-   Comando **bcp**  
  
     Al cargar datos en una tabla de destino, **bcp** omite el componente \<ROW>. En su lugar, **bcp** carga los datos en función de los tipos de columnas de la tabla de destino.  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] instrucciones (proveedor de conjuntos de filas BULK de BULK INSERT y OPENROWSET)  
  
     Al realizar una importación en bloque de datos en una tabla, las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] usan el componente \<ROW> para generar el conjunto de filas de entrada. Además, las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] realizan las conversiones de tipos adecuadas en función de los tipos de columna especificados en \<ROW> y de la columna correspondiente en la tabla de destino. Si los tipos de columna especificados en el archivo de formato y la tabla de destino no coinciden, se realiza una conversión de tipo adicional. Esta conversión de tipo adicional puede llevar a discrepancias (es decir, a una pérdida de precisión) en cuanto al comportamiento en el proveedor de conjuntos de filas BULK de OPENROWSET o BULK INSERT en comparación con **bcp**.  
  
     La información del elemento \<ROW> permite construir una fila sin necesidad de información adicional. Por este motivo, puede generar un conjunto de filas mediante una instrucción SELECT (SELECT \* FROM OPENROWSET(BULK *datafile* FORMATFILE=*xmlformatfile*).  
  
    > [!NOTE]  
    >  La cláusula OPENROWSET BULK requiere un archivo de formato (tenga en cuenta que solo se puede convertir desde el tipo de datos del campo al tipo de datos de una columna con un archivo de formato XML).  
  
###  <a name="how-bulk-import-uses-the-column-element"></a><a name="HowUsesColumn"></a> Cómo usa la importación en bloque el elemento \<COLUMN>  
 Para la importación en bloque de datos en una tabla, los elementos \<COLUMN> de un archivo de formato asignan un campo de archivo de datos a columnas de tabla mediante la especificación de:  
  
-   La posición de cada campo dentro de una fila del archivo de datos.  
  
-   El tipo de columna, que se utiliza para convertir el tipo de datos de campo al tipo de datos de columna deseado.  
  
 Si un campo no tiene asignada ninguna columna, el campo no se copia en las filas generadas. Este comportamiento permite a un archivo de datos generar filas con distintas columnas (en tablas diferentes).  
  
 De forma similar, para exportar de forma masiva datos de una tabla, cada elemento \<COLUMN> del archivo de formato asigna la columna de la fila de la tabla de entrada a su campo correspondiente en el archivo de datos de salida.  
  
###  <a name="putting-the-xsitype-value-into-a-data-set"></a><a name="PutXsiTypeValueIntoDataSet"></a> Colocar el valor xsi:type en un conjunto de datos  
 Si un documento XML se valida con el lenguaje de definición de esquema XML (XSD), el valor xsi:type no se coloca en el conjunto de datos. No obstante, puede colocar la información de xsi:type en el conjunto de datos si carga el archivo de formato XML en un documento XML (por ejemplo, `myDoc`), tal como ilustra el siguiente fragmento de código:  
  
```cs
...;  
myDoc.LoadXml(xmlFormat);  
XmlNodeList ColumnList = myDoc.GetElementsByTagName("COLUMN");  
for(int i=0;i<ColumnList.Count;i++)  
{  
   Console.Write("COLUMN: xsi:type=" +ColumnList[i].Attributes["type",  
      "http://www.w3.org/2001/XMLSchema-instance"].Value+"\n");  
}  
```  
  
##  <a name="sample-xml-format-files"></a><a name="SampleXmlFFs"></a> Archivos de formato XML de ejemplo  
 Esta sección contiene información sobre el uso de archivos de formato XML en diversos casos, incluido un ejemplo de [!INCLUDE[ssSampleDBCoShort](../../includes/sssampledbcoshort-md.md)] .  
  
> [!NOTE]  
>  En los archivos de datos que se muestran en los ejemplos siguientes, `<tab>` indica un carácter de tabulación en un archivo de datos y `<return>` indica un retorno de carro.  
  
 Los ejemplos muestran los aspectos clave del uso de archivos de formato XML, del siguiente modo:  
  
-   [Ordenar campos de datos de caracteres igual que columnas de tabla](#OrderCharFieldsSameAsCols)  
  
-   [Ordenar campos de datos y columnas de tabla de forma diferente](#OrderFieldsAndColsDifferently)  
  
-   [Omitir un campo de datos](#OmitField)  
  
-   [Asignar tipos diferentes de campos a columnas](#MapXSItype)  
  
-   [Asignar datos XML a una tabla](#MapXMLDataToTbl)  
  
-   [Importar campos de longitud fija o de ancho fijo](#ImportFixedFields)  
  
-   [Ejemplo adicional](#AdditionalExamples)  
  
> [!NOTE]  
>  Para obtener información sobre cómo crear archivos de formato, vea [Crear un archivo de formato &#40;SQL Server&#41;](../../relational-databases/import-export/create-a-format-file-sql-server.md).  
  
###  <a name="a-ordering-character-data-fields-the-same-as-table-columns"></a><a name="OrderCharFieldsSameAsCols"></a> A. Ordenar campos de datos de caracteres igual que columnas de tabla  
 En el ejemplo siguiente se muestra un archivo de formato XML que describe un archivo de datos que contiene tres campos de datos de caracteres. El archivo de formato asigna el archivo de datos a una tabla que contiene tres columnas. Los campos de datos se corresponden uno a uno con las columnas de la tabla.  
  
 **Tabla (fila):** Person (Age int, FirstName varchar(20), LastName varchar(30))  
  
 **Archivo de datos (registro):** Age\<tab>Firstname\<tab>Lastname\<return>  
  
 El siguiente archivo de formato XML lee del archivo de datos a la tabla.  
  
 En el elemento `<RECORD>` , el archivo de formato representa los valores de datos de los tres campos como datos de caracteres. Para cada campo, el atributo `TERMINATOR` indica el terminador que sigue al valor de datos.  
  
 Los campos de datos se corresponden uno a uno con las columnas de la tabla. En el elemento `<ROW>` , el archivo de formato asigna la columna `Age` al primer campo, la columna `FirstName` al segundo campo y la columna `LastName` al tercer campo.  
  
```xml
<?xml version="1.0"?>  
<BCPFORMAT   
xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format"   
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  <RECORD>  
    <FIELD ID="1" xsi:type="CharTerm" TERMINATOR="\t"   
      MAX_LENGTH="12"/>   
    <FIELD ID="2" xsi:type="CharTerm" TERMINATOR="\t"   
      MAX_LENGTH="20" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
    <FIELD ID="3" xsi:type="CharTerm" TERMINATOR="\r\n"   
      MAX_LENGTH="30"   
      COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
  </RECORD>  
  <ROW>  
    <COLUMN SOURCE="1" NAME="age" xsi:type="SQLINT"/>  
    <COLUMN SOURCE="2" NAME="firstname" xsi:type="SQLVARYCHAR"/>  
    <COLUMN SOURCE="3" NAME="lastname" xsi:type="SQLVARYCHAR"/>  
  </ROW>  
</BCPFORMAT>  
```  
  
> [!NOTE]  
>  Para ver un ejemplo equivalente de [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] , vea [Crear un archivo de formato &#40;SQL Server&#41;](../../relational-databases/import-export/create-a-format-file-sql-server.md).  
  
###  <a name="b-ordering-data-fields-and-table-columns-differently"></a><a name="OrderFieldsAndColsDifferently"></a> B. Ordenar campos de datos y columnas de tabla de forma diferente  
 En el ejemplo siguiente se muestra un archivo de formato XML que describe un archivo de datos que contiene tres campos de datos de caracteres. El archivo de formato asigna el archivo de datos a una tabla que contiene tres columnas que están ordenadas de forma diferente a los campos del archivo de datos.  
  
 **Tabla (fila):** Person (Age int, FirstName varchar(20), LastName varchar(30))  
  
 **Archivo de datos** (registro): Age\<tab>Lastname\<tab>Firstname\<return>  
  
 En el elemento `<RECORD>` , el archivo de formato representa los valores de datos de los tres campos como datos de caracteres.  
  
 En el elemento `<ROW>` , el archivo de formato asigna la columna `Age` al primer campo, la columna `FirstName` al tercer campo y la columna `LastName` al segundo campo.  
  
```xml
<?xml version="1.0"?>  
<BCPFORMAT   
xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format"   
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  <RECORD>  
    <FIELD ID="1" xsi:type="CharTerm" TERMINATOR="\t"   
      MAX_LENGTH="12"/>  
    <FIELD ID="2" xsi:type="CharTerm" TERMINATOR="\t" MAX_LENGTH="20"   
      COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
    <FIELD ID="3" xsi:type="CharTerm" TERMINATOR="\r\n"   
      MAX_LENGTH="30" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
  </RECORD>  
  <ROW>  
    <COLUMN SOURCE="1" NAME="age" xsi:type="SQLINT"/>  
    <COLUMN SOURCE="3" NAME="firstname" xsi:type="SQLVARYCHAR"/>  
    <COLUMN SOURCE="2" NAME="lastname" xsi:type="SQLVARYCHAR"/>  
  </ROW>  
</BCPFORMAT>  
```  
  
> [!NOTE]  
>  Para ver un ejemplo equivalente de [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] , vea [Usar un archivo de formato para asignar columnas de tabla a campos de un archivo de datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md).  
  
###  <a name="c-omitting-a-data-field"></a><a name="OmitField"></a> C. Omitir un campo de datos  
 En el ejemplo siguiente se muestra un archivo de formato XML que describe un archivo de datos que contiene cuatro campos de datos de caracteres. El archivo de formato asigna el archivo de datos a una tabla que contiene tres columnas. El segundo campo de datos no se corresponde con ninguna columna de la tabla.  
  
 **Tabla (fila):** Person (Age int, FirstName Varchar(20), LastName Varchar(30))  
  
 **Archivo de datos (registro):** Age\<tab>employeeID\<tab>Firstname\<tab>Lastname\<return>  
  
 En el elemento `<RECORD>` , el archivo de formato representa los valores de datos de los cuatro campos como datos de caracteres. Para cada campo, el atributo TERMINATOR indica el terminador que sigue al valor de datos.  
  
 En el elemento `<ROW>` , el archivo de formato asigna la columna `Age` al primer campo, la columna `FirstName` al tercer campo y la columna `LastName` al cuarto campo.  
  
```xml
<?xml version = "1.0"?>  
<BCPFORMAT   
xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format"   
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  <RECORD>  
    <FIELD ID="1" xsi:type="CharTerm" TERMINATOR="\t"   
      MAX_LENGTH="12"/>  
    <FIELD ID="2" xsi:type="CharTerm" TERMINATOR="\t"   
      MAX_LENGTH="10"   
      COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
    <FIELD ID="3" xsi:type="CharTerm" TERMINATOR="\t"   
      MAX_LENGTH="20"   
      COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
    <FIELD ID="4" xsi:type="CharTerm" TERMINATOR="\r\n"   
      MAX_LENGTH="30"   
      COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
  </RECORD>  
  <ROW>  
    <COLUMN SOURCE="1" NAME="age" xsi:type="SQLINT"/>  
    <COLUMN SOURCE="3" NAME="firstname" xsi:type="SQLVARYCHAR"/>  
    <COLUMN SOURCE="4" NAME="lastname" xsi:type="SQLVARYCHAR"/>  
  </ROW>  
</BCPFORMAT>  
```  
  
> [!NOTE]  
>  Para ver un ejemplo equivalente de [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] , vea [Usar un archivo de formato para omitir un campo de datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md).  
  
###  <a name="d-mapping-field-xsitype-to-column-xsitype"></a><a name="MapXSItype"></a> D. Asignación de \<FIELD> xsi:type a \<COLUMN> xsi:type  
 En el ejemplo siguiente se muestran tipos de campos diferentes y sus asignaciones a columnas.  
  
```xml
<?xml version = "1.0"?>  
<BCPFORMAT  
xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format"   
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
   <RECORD>  
      <FIELD xsi:type="CharTerm" ID="C1" TERMINATOR="\t"   
            MAX_LENGTH="4"/>  
      <FIELD xsi:type="CharFixed" ID="C2" LENGTH="10"   
         COLLATION="SQL_LATIN1_GENERAL_CP1_CI_AS"/>  
      <FIELD xsi:type="CharPrefix" ID="C3" PREFIX_LENGTH="2"   
         MAX_LENGTH="32" COLLATION="SQL_LATIN1_GENERAL_CP1_CI_AS"/>  
      <FIELD xsi:type="NCharTerm" ID="C4" TERMINATOR="\t"   
         MAX_LENGTH="4"/>  
      <FIELD xsi:type="NCharFixed" ID="C5" LENGTH="10"   
         COLLATION="SQL_LATIN1_GENERAL_CP1_CI_AS"/>  
      <FIELD xsi:type="NCharPrefix" ID="C6" PREFIX_LENGTH="2"   
         MAX_LENGTH="32" COLLATION="SQL_LATIN1_GENERAL_CP1_CI_AS"/>  
      <FIELD xsi:type="NativeFixed" ID="C7" LENGTH="4"/>  
   </RECORD>  
   <ROW>  
      <COLUMN SOURCE="C1" NAME="Age" xsi:type="SQLTINYINT"/>  
      <COLUMN SOURCE="C2" NAME="FirstName" xsi:type="SQLVARYCHAR"   
      LENGTH="16" NULLABLE="NO"/>  
      <COLUMN SOURCE="C3" NAME="LastName" />  
      <COLUMN SOURCE="C4" NAME="Salary" xsi:type="SQLMONEY"/>  
      <COLUMN SOURCE="C5" NAME="Picture" xsi:type="SQLIMAGE"/>  
      <COLUMN SOURCE="C6" NAME="Bio" xsi:type="SQLTEXT"/>  
      <COLUMN SOURCE="C7" NAME="Interest"xsi:type="SQLDECIMAL"   
      PRECISION="5" SCALE="3"/>  
   </ROW>  
</BCPFORMAT>  
```  
  
###  <a name="e-mapping-xml-data-to-a-table"></a><a name="MapXMLDataToTbl"></a> E. Asignar datos XML a una tabla  
 En el ejemplo siguiente se crea una tabla vacía de dos columnas (`t_xml`), en la que la primera columna se asigna al tipo de datos `int` y la segunda columna se asigna al tipo de datos `xml` .  
  
```sql
CREATE TABLE t_xml (c1 int, c2 xml)  
```  
  
 El siguiente archivo de formato XML carga un archivo de datos en una tabla `t_xml`.  
  
```xml
<?xml version="1.0"?>  
<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format"   
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
 <RECORD>  
  <FIELD ID="1" xsi:type="NativePrefix" PREFIX_LENGTH="1"/>  
  <FIELD ID="2" xsi:type="NCharPrefix" PREFIX_LENGTH="8"/>  
 </RECORD>  
 <ROW>  
  <COLUMN SOURCE="1" NAME="c1" xsi:type="SQLINT"/>  
  <COLUMN SOURCE="2" NAME="c2" xsi:type="SQLNCHAR"/>  
 </ROW>  
</BCPFORMAT>  
```  
  
###  <a name="f-importing-fixed-length-or-fixed-width-fields"></a><a name="ImportFixedFields"></a> F. Importar campos de longitud fija o de ancho fijo  
 En el siguiente ejemplo se describen campos fijos de `10` o `6` caracteres cada uno. El archivo de formato representa estas longitudes y anchos de campo como `LENGTH="10"` y `LENGTH="6"`, respectivamente. Cada una de las filas de los archivos de datos termina con una combinación de retorno de carro y avance de línea, {CR}{LF}, que el archivo de formato representa como `TERMINATOR="\r\n"`.  
  
```xml
<?xml version="1.0"?>  
<BCPFORMAT  
       xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  <RECORD>  
    <FIELD ID="1" xsi:type="CharFixed" LENGTH="10"/>  
    <FIELD ID="2" xsi:type="CharFixed" LENGTH="6"/>  
    <FIELD ID="3" xsi:type="CharTerm" TERMINATOR="\r\n"/>  
  </RECORD>  
  <ROW>  
    <COLUMN SOURCE="1" NAME="C1" xsi:type="SQLINT" />  
    <COLUMN SOURCE="2" NAME="C2" xsi:type="SQLINT" />  
  </ROW>  
</BCPFORMAT>  
```  
  
###  <a name="additional-examples"></a><a name="AdditionalExamples"></a> Otros ejemplos  
 Para obtener más ejemplos tanto de archivos de formato XML como de formato no XML, vea los siguientes temas:  
  
-   [Usar un archivo de formato para omitir una columna de tabla &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)  
  
-   [Usar un archivo de formato para omitir un campo de datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)  
  
-   [Usar un archivo de formato para asignar columnas de tabla a campos de un archivo de datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
  
-   [Crear un archivo de formato &#40;SQL Server&#41;](../../relational-databases/import-export/create-a-format-file-sql-server.md)  
  
-   [Usar un archivo de formato para importar datos en bloque &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)  
  
-   [Usar un archivo de formato para omitir una columna de tabla &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)  
  
-   [Usar un archivo de formato para omitir un campo de datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)  
  
-   [Usar un archivo de formato para asignar columnas de tabla a campos de un archivo de datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenido relacionado  
 Ninguno.  
  
## <a name="see-also"></a>Consulte también  
 [Importar y exportar datos en bloque &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)   
 [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Archivos de formato no XML &#40;SQL Server&#41;](../../relational-databases/import-export/non-xml-format-files-sql-server.md)   
 [Archivos de formato para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)  
  
  
