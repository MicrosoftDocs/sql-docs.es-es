---
title: Importación y exportación de documentos XML en bloque
description: Ejemplos de importación y exportación en bloque de documentos XML con SQL Server
ms.date: 10/24/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- field terminators [SQL Server]
- bulk importing [SQL Server], data formats
- row terminators [SQL Server]
- OPENROWSET function, XML bulk load
- terminators [SQL Server]
- bulk exporting [SQL Server], data formats
- XML bulk load [SQL Server]
ms.assetid: dff99404-a002-48ee-910e-f37f013d946d
author: MashaMSFT
ms.author: mathoma
ms.custom: seo-lt-2019
ms.openlocfilehash: 5e2a52bcfa2e95aa364bbdf3079e2f8666f0e1e0
ms.sourcegitcommit: 0b400bb99033f4b836549cb11124a1f1630850a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2021
ms.locfileid: "99978797"
---
# <a name="examples-of-bulk-import-and-export-of-xml-documents-sql-server"></a>Ejemplos de importación y exportación de documentos XML en bloque (SQL Server)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

## <a name="top"></a>

Puede importar documentos XML de forma masiva en una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o exportarlos de igual forma desde una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Este tema proporciona ejemplos de ambos casos.

Para importar datos de forma masiva de un archivo de datos a una tabla o vista sin particiones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , puede utilizar lo siguiente:  
  
- Utilidad **bcp**  
   También puede usar la utilidad **bcp** para exportar datos de cualquier ubicación a una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la que funcione una instrucción SELECT, incluidas las vistas con particiones.  

- BULK INSERT  
  
- INSERT ... SELECT * FROM OPENROWSET(BULK...)  

Para obtener más información, vea los siguientes temas.
- [Importar y exportar datos de forma masiva con la utilidad bcp (SQL Server)](../../relational-databases/import-export/import-and-export-bulk-data-by-using-the-bcp-utility-sql-server.md)
- [Importación en bloque de datos mediante las instrucciones BULK INSERT o OPENROWSET(BULK...) (SQL Server)](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md) 
- [Importar XML en SQL Server con el componente de carga masiva XML](https://mskb.pkisolutions.com/kb/316005)
- [Colecciones de esquemas XML (SQL Server)](../xml/xml-schema-collections-sql-server.md)
  
## <a name="examples"></a>Ejemplos  
 Los ejemplos son los siguientes:  
  
-  [A. Importar en bloque datos XML como un flujo de bytes binario](#binary_byte_stream)  
  
-  [B. Importar en bloque datos XML en una fila existente](#existing_row)  
  
-  [C. Importar en bloque datos XML de un archivo que contiene una DTD](#file_contains_dtd)  
  
- [D. Especificar el terminador de campo explícitamente mediante el uso de un archivo de formato](#field_terminator_in_format_file)  
  
-  [E. Exportar en bloque datos XML](#bulk_export_xml_data)  
  
## <a name="bulk-importing-xml-data-as-a-binary-byte-stream"></a><a name="binary_byte_stream"></a>Importar de forma masiva datos XML como un flujo de bytes binario  
 Cuando importa datos XML de forma masiva de un archivo que contiene una declaración de codificación que quiere aplicar, especifique la opción SINGLE_BLOB en la cláusula OPENROWSET(BULK...). La opción SINGLE_BLOB garantiza que el analizador de XML de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] importa los datos según el esquema de codificación especificado en la declaración XML.  
  
#### <a name="sample-table"></a>Tabla de ejemplo  
 Para probar el ejemplo A de aquí, debe crear una tabla de ejemplo `T`.  
  
```sql
USE tempdb  
CREATE TABLE T (IntCol int, XmlCol xml);  
GO  
```  
  
#### <a name="sample-data-file"></a>Archivo de datos de ejemplo  
 Para poder ejecutar el ejemplo A, debe crear un archivo codificado con UTF-8 (`C:\SampleFolder\SampleData3.txt`) que contenga el ejemplo de instancia siguiente que especifica el esquema de codificación `UTF-8` .  
  
```xml  
<?xml version="1.0" encoding="UTF-8"?>  
<Root>  
          <ProductDescription ProductModelID="5">  
             <Summary>Some Text</Summary>  
          </ProductDescription>  
</Root>  
```  
  
#### <a name="example-a"></a>Ejemplo A  
 Este ejemplo utiliza la opción `SINGLE_BLOB` en una instrucción `INSERT ... SELECT * FROM OPENROWSET(BULK...)` para importar datos de un archivo llamado `SampleData3.txt` e insertar una instancia XML en la tabla de ejemplo de una sola columna `T`.  
  
```sql
INSERT INTO T(XmlCol)  
SELECT * FROM OPENROWSET(  
   BULK 'c:\SampleFolder\SampleData3.txt',  
   SINGLE_BLOB) AS x;  
```  
  
#### <a name="remarks"></a>Observaciones  
 Si usa SINGLE_BLOB en este caso, puede evitar una discrepancia entre la codificación del documento XML (especificada por la declaración de codificación XML) y la página de códigos de cadena establecida implícitamente por el servidor.  
  
 Si utiliza tipos de datos NCLOB o CLOB y se produce un conflicto de página de códigos o de codificación, deberá realizar una de las siguientes acciones:  
  
-   Elimine la declaración XML para poder importar correctamente el contenido del archivo de datos XML.  
  
-   Especifique una página de códigos en la opción CODEPAGE de la consulta que coincida con el esquema de codificación que se utiliza en la declaración XML.  
  
-   Compare o resuelva los valores de intercalación de base de datos con un esquema de codificación XML distinto de Unicode.  
  
 [&#91;Principio&#93;](#top)  
  
##  <a name="bulk-importing-xml-data-in-an-existing-row"></a><a name="existing_row"></a> Importar de forma masiva datos XML en una fila existente  
 Este ejemplo utiliza el el proveedor de conjuntos de filas BULK `OPENROWSET` para agregar una instancia XML a una fila o filas existentes en la tabla de ejemplo `T`.  
  
> [!NOTE]  
>  Para ejecutar este ejemplo, es necesario que complete primero el script de prueba del ejemplo A. Ese ejemplo crea la tabla `tempdb.dbo.T` e importa los datos de forma masiva de `SampleData3.txt`.  
  
#### <a name="sample-data-file"></a>Archivo de datos de ejemplo  
 El ejemplo B utiliza una versión modificada del archivo de datos de ejemplo `SampleData3.txt` del ejemplo anterior. Para ejecutar este ejemplo, modifique el contenido de este archivo de la siguiente manera:  
  
```xml
<Root>  
          <ProductDescription ProductModelID="10">  
             <Summary>Some New Text</Summary>  
          </ProductDescription>  
</Root>  
```  
  
#### <a name="example-b"></a>Ejemplo B  
  
```sql  
-- Query before update shows initial state of XmlCol values.  
SELECT * FROM T  
UPDATE T  
SET XmlCol =(  
SELECT * FROM OPENROWSET(  
   BULK 'C:\SampleFolder\SampleData3.txt',  
           SINGLE_BLOB  
) AS x  
)  
WHERE IntCol = 1;  
GO  
```  
  
 [&#91;Principio&#93;](#top)  
  
## <a name="bulk-importing-xml-data-from-a-file-that-contains-a-dtd"></a><a name="file_contains_dtd"></a> Importar de forma masiva datos XML a partir de un archivo que contiene una DTD  
  
> [!IMPORTANT]  
>  Se recomienda no habilitar la compatibilidad con definiciones de tipo de documento (DTD) si no es necesaria en el entorno XML. Si se activa la compatibilidad con DTD, se aumenta el área expuesta susceptible de ataques del servidor, que puede quedar expuesta a un ataque por denegación de servicio. En caso de que sea necesario habilitar la compatibilidad con DTD, este riesgo de seguridad puede reducirse procesando únicamente los documentos XML de confianza.  
  
 Al intentar utilizar un comando [bcp](../../tools/bcp-utility.md) para importar datos XML de un archivo que contenga una DTD, puede producirse un error similar al siguiente:  
  
 "SQLState = 42000, NativeError = 6359"  
  
 "Error = [Microsoft][SQL Server Native Client][ SQL Server]No se permite analizar XML con DTD de un subconjunto interno. Utilice CONVERT con la opción de estilo 2 para habilitar la compatibilidad limitada con DTD de subconjuntos internos."  
  
 "Error de copia de %s en BCP"  
  
 Para solucionar este problema, puede importar datos XML de un archivo de datos que contenga una DTD mediante la función `OPENROWSET(BULK...)` y especificando posteriormente la opción `CONVERT` en la cláusula `SELECT` del comando. La sintaxis básica para el comando es la siguiente:  
  
 `INSERT ... SELECT CONVERT(...) FROM OPENROWSET(BULK...)`  
  
#### <a name="sample-data-file"></a>Archivo de datos de ejemplo  
 Para probar este ejemplo de importación en bloque, cree un archivo (`C:\temp\Dtdfile.xml`) que contenga la siguiente instancia de ejemplo:  
  
```xml 
<!DOCTYPE DOC [<!ATTLIST elem1 attr1 CDATA "defVal1">]><elem1>January</elem1>  
```  
  
#### <a name="sample-table"></a>Tabla de ejemplo  
 El ejemplo C utiliza la tabla de ejemplo `T1` que crea la instrucción `CREATE TABLE` siguiente:  
  
```sql  
USE tempdb;  
CREATE TABLE T1(XmlCol xml);  
GO  
```  
  
#### <a name="example-c"></a>Ejemplo C  
 Este ejemplo utiliza `OPENROWSET(BULK...)` y especifica la opción `CONVERT` en la cláusula `SELECT` para importar los datos XML desde `Dtdfile.xml` al ejemplo de tabla `T1`.  
  
```sql
INSERT T1  
  SELECT CONVERT(xml, BulkColumn, 2) FROM   
    OPENROWSET(Bulk 'c:\temp\Dtdfile.xml', SINGLE_BLOB) [rowsetresults];  
```  
  
 Después de ejecutar la instrucción `INSERT` , se quitará la DTD del XML y se almacenará en la tabla `T1` .  
  
 [&#91;Principio&#93;](#top)  
  
## <a name="specifying-the-field-terminator-explicitly-using-a-format-file"></a><a name="field_terminator_in_format_file"></a> Especificar el terminador de campo explícitamente mediante el uso de un archivo de formato  
 El ejemplo siguiente muestra cómo importar de forma masiva el siguiente documento XML, `Xmltable.dat`.  
  
#### <a name="sample-data-file"></a>Archivo de datos de ejemplo  
 El documento en `Xmltable.dat` contiene dos valores XML, uno para cada fila. El primer valor XML está codificado con UTF-16 y el segundo con UTF-8.  
  
 El contenido de este archivo de datos se muestra en el volcado Hex siguiente:  
  
```  
FF FE 3C 00 3F 00 78 00-6D 00 6C 00 20 00 76 00  *..\<.?.x.m.l. .v.*  
65 00 72 00 73 00 69 00-6F 00 6E 00 3D 00 22 00  *e.r.s.i.o.n.=.".*  
31 00 2E 00 30 00 22 00-20 00 65 00 6E 00 63 00  *1...0.". .e.n.c.*  
6F 00 64 00 69 00 6E 00-67 00 3D 00 22 00 75 00  *o.d.i.n.g.=.".u.*  
74 00 66 00 2D 00 31 00-36 00 22 00 3F 00 3E 00  *t.f.-.1.6.".?.>.*  
3C 00 72 00 6F 00 6F 00-74 00 3E 00 A2 4F 9C 76  *\<.r.o.o.t.>..O.v*  
0C FA 77 E4 80 00 89 00-00 06 90 06 91 2E 9B 2E  *..w.............*  
99 34 A2 34 86 00 83 02-92 20 7F 02 4E C5 E4 A3  *.4.4..... ..N...*  
34 B2 B7 B3 B7 FE F8 FF-F8 00 3C 00 2F 00 72 00  *4.........\<./.r.*  
6F 00 6F 00 74 00 3E 00-00 00 00 00 7A EF BB BF  *o.o.t.>.....z...*  
3C 3F 78 6D 6C 20 76 65-72 73 69 6F 6E 3D 22 31  *\<?xml version="1*  
2E 30 22 20 65 6E 63 6F-64 69 6E 67 3D 22 75 74  *.0" encoding="ut*  
66 2D 38 22 3F 3E 3C 72-6F 6F 74 3E E4 BE A2 E7  *f-8"?><root>....*  
9A 9C EF A8 8C EE 91 B7-C2 80 C2 89 D8 80 DA 90  *................*  
E2 BA 91 E2 BA 9B E3 92-99 E3 92 A2 C2 86 CA 83  *................*  
E2 82 92 C9 BF EC 95 8E-EA 8F A4 EB 88 B4 EB 8E  *................*  
B7 EF BA B7 EF BF B8 C3-B8 3C 2F 72 6F 6F 74 3E  *.........</root>*  
00 00 00 00 7A                                   *....z*  
```  
  
#### <a name="sample-table"></a>Tabla de ejemplo  
 Al realizar una importación o exportación masiva de un documento XML, hay que usar un [terminador de campo](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md) que no pueda aparecer en ninguno de los documentos; por ejemplo, una serie de cuatro valores NULL (`\0`) seguidos de la letra `z`: `\0\0\0\0z`.  
  
 Este ejemplo muestra cómo utilizar el terminador de campo para la tabla de ejemplo `xTable` . Para crear esta vista de ejemplo, utilice la siguiente instrucción `CREATE TABLE` :  
  
```sql
USE tempdb;  
CREATE TABLE xTable (xCol xml);  
GO  
```  
  
#### <a name="sample-format-file"></a>Archivo de formato de ejemplo  
 El terminador de campo se debe especificar en el archivo de formato. El ejemplo D utiliza un archivo de formato no XML denominado `Xmltable.fmt` que contiene lo siguiente:  
  
```  
9.0  
1  
1       SQLBINARY     0       0       "\0\0\0\0z"    1     xCol         ""  
```  
  
 Puede utilizar este archivo de formato para realizar importaciones masivas de documentos XML a la tabla `xTable` mediante el comando `bcp` o una instrucción `BULK INSERT` o `INSERT ... SELECT * FROM OPENROWSET(BULK...)` .  
  
#### <a name="example-d"></a>Ejemplo D  
 Este ejemplo utiliza el archivo de formato `Xmltable.fmt` en una instrucción `BULK INSERT` para importar el contenido de un archivo de datos XML denominado `Xmltable.dat`.  
  
```sql
BULK INSERT xTable   
FROM 'C:\Xmltable.dat'  
WITH (FORMATFILE = 'C:\Xmltable.fmt');  
GO  
```  
  
 [&#91;Principio&#93;](#top)  
  
## <a name="bulk-exporting-xml-data"></a><a name="bulk_export_xml_data"></a> Exportar de forma masiva datos XML  
 En el ejemplo siguiente se utiliza [bcp](../../tools/bcp-utility.md) para realizar la exportación masiva de datos XML a partir de la tabla creada en el ejemplo anterior con el mismo archivo de formato XML. En el siguiente comando `bcp` , `<server_name>` y `<instance_name>` representan los marcadores de posición que deben ser reemplazados con los valores adecuados:  
  
```cmd
bcp bulktest..xTable out a-wn.out -N -T -S<server_name>\<instance_name>  
```  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no guarda la codificación XML cuando se mantienen datos XML en la base de datos. Por lo tanto, la codificación original de los campos XML no estará disponible cuando se exporten datos XML. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa la codificación UTF-16 al exportar datos XML.  
  

## <a name="see-also"></a>Consulte también  
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [SELECT &#40;cláusula de Transact-SQL&#41;](../../t-sql/queries/select-clause-transact-sql.md)   
 [bcp (utilidad)](../../tools/bcp-utility.md)   
 [Importar y exportar datos en bloque &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)  
  
  
