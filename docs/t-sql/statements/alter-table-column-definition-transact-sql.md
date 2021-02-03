---
description: ALTER TABLE column_definition (Transact-SQL)
title: column_definition (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/24/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- column_definition
- column_definition_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- column_definition
- ALTER TABLE statement
- column properties [SQL Server]
- column definitions [SQL Server]
ms.assetid: a1742649-ca29-4d9b-9975-661cdbf18f78
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b3c5c23b2cdccf464ec86d88f50073143a979ca8
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237118"
---
# <a name="alter-table-column_definition-transact-sql"></a>ALTER TABLE column_definition (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Especifica las propiedades de una columna que se agregan a una tabla mediante [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
column_name <data_type>  
[ FILESTREAM ]  
[ COLLATE collation_name ]   
[ NULL | NOT NULL ]  
[   
    [ CONSTRAINT constraint_name ] DEFAULT constant_expression [ WITH VALUES ]   
    | IDENTITY [ ( seed , increment ) ] [ NOT FOR REPLICATION ]   
]  
[ ROWGUIDCOL ]   
[ SPARSE ]   
[ ENCRYPTED WITH  
  ( COLUMN_ENCRYPTION_KEY = key_name ,  
      ENCRYPTION_TYPE = { DETERMINISTIC | RANDOMIZED } ,   
      ALGORITHM =  'AEAD_AES_256_CBC_HMAC_SHA_256'   
  ) ]  
[ MASKED WITH ( FUNCTION = ' mask_function ') ]  
[ <column_constraint> [ ...n ] ]  
  
<data type> ::=   
[ type_schema_name . ] type_name   
    [ ( precision [ , scale ] | max |   
        [ { CONTENT | DOCUMENT } ] xml_schema_collection ) ]   
  
<column_constraint> ::=   
[ CONSTRAINT constraint_name ]   
{     { PRIMARY KEY | UNIQUE }   
        [ CLUSTERED | NONCLUSTERED ]   
        [   
            WITH FILLFACTOR = fillfactor    
          | WITH ( < index_option > [ , ...n ] )   
        ]   
        [ ON { partition_scheme_name ( partition_column_name )   
            | filegroup | "default" } ]  
  | [ FOREIGN KEY ]   
        REFERENCES [ schema_name . ] referenced_table_name [ ( ref_column ) ]   
        [ ON DELETE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]   
        [ ON UPDATE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]   
        [ NOT FOR REPLICATION ]   
  | CHECK [ NOT FOR REPLICATION ] ( logical_expression )   
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *column_name*  
 Es el nombre de la columna que se va a modificar, agregar o quitar. *column_name* puede tener entre 1 y 128 caracteres. Si se trata de columnas nuevas creadas con un tipo de datos de marca de tiempo, *column_name* se puede omitir. Si no se especifica el argumento *column_name* en una columna con un tipo de datos **timestamp**, se usa el nombre **timestamp**.  
  
 [ _type_schema_name_ **.** ] *type_name*  
 Es el tipo de datos de la columna agregada y el esquema al que pertenece.  
  
 *type_name* puede ser:  
  
-   Un tipo de datos del sistema de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Un tipo de datos del alias basado en el tipo de datos del sistema de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los tipos de datos de alias deben crearse mediante CREATE TYPE para poder utilizarse en una definición de tabla.  
  
-   Un tipo definido por el usuario de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] y el esquema al que pertenece. Un tipo de datos [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] definido por el usuario debe crearse mediante CREATE TYPE para poder utilizarse en una definición de tabla.  
  
 Si no se especifica *type_schema_name*, [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] hace referencia a *type_name* en este orden:  
  
-   El tipo de datos del sistema de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   El esquema predeterminado del usuario actual en la base de datos actual.  
  
-   El esquema **dbo** de la base de datos actual.  
  
*precisión*  
 Es la precisión del tipo de datos especificado. Para obtener más información sobre los valores de precisión válidos, vea [Precisión, escala y longitud &#40;Transact-SQL&#41;](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).  
  
*scale*  
 Es la escala del tipo de datos especificado. Para obtener más información sobre los valores de escala válidos, vea [Precisión, escala y longitud &#40;Transact-SQL&#41;](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).  
  
**max**  
 Solo se aplica a los tipos de datos **varchar**, **nvarchar** y **varbinary**. Éstos se utilizan para almacenar 2^31 bytes de datos de caracteres y binarios y 2^30 bytes de datos Unicode.  
  
**CONTENT**  
 Especifica que cada instancia del tipo de datos **xml** en *column_name* puede incluir varios elementos de nivel superior. CONTENT se aplica solamente al tipo de datos **xml** y únicamente se puede especificar si también se especifica *xml_schema_collection*. Si no se especifica este último parámetro, CONTENT es el comportamiento predeterminado.  
  
DOCUMENT  
 Especifica que cada instancia del tipo de datos **xml** en *column_name* puede incluir un solo elemento de nivel superior. DOCUMENT se aplica solamente al tipo de datos **xml** y únicamente se puede especificar si también se especifica *xml_schema_collection*.  
  
 *xml_schema_collection*  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Solo se aplica al tipo de datos **xml** para asociar una colección de esquemas XML al tipo. Antes de escribir una columna **xml** para un esquema, primero debe crear el esquema en la base de datos mediante [CREATE XML SCHEMA COLLECTION](../../t-sql/statements/create-xml-schema-collection-transact-sql.md).  
  
FILESTREAM  
 Opcionalmente, especifica un atributo de almacenamiento FILESTREAM para una columna que tiene un *type_name* de **varbinary(max)** .  
  
 Si se especifica FILESTREAM para una columna, la tabla debe tener también una columna del tipo de datos **uniqueidentifier** con el atributo ROWGUIDCOL. Esta columna no debe permitir valores nulos y debe tener una restricción de columna única UNIQUE o PRIMARY KEY. El valor GUID para la columna debe ser suministrado por una aplicación cuando se insertan los datos o por una restricción DEFAULT que utiliza la función NEWID ().  
  
 No se puede quitar la columna ROWGUIDCOL ni se pueden cambiar las restricciones relacionadas si hay definida una columna FILESTREAM para la tabla. Solamente se puede quitar la columna ROWGUIDCOL después de quitarse la última columna FILESTREAM.  
  
 Si se especifica el atributo de almacenamiento FILESTREAM para una columna, todos los valores para dicha columna se almacenan en un contenedor de datos de FILESTREAM del sistema de archivos.  
  
 Para ver un ejemplo de cómo usar la definición de columna, vea [FILESTREAM &#40;SQL Server&#41;](../../relational-databases/blob/filestream-sql-server.md).  
  
COLLATE *collation_name*  
 Especifica la intercalación de la columna. Si no se especifica, se asigna a la columna la intercalación predeterminada de la base de datos. El nombre de intercalación puede ser un nombre de intercalación de Windows o un nombre de intercalación de SQL. Para obtener una lista y más información, vea [Windows Collation Name &#40;Transact-SQL&#41;](../../t-sql/statements/windows-collation-name-transact-sql.md) (Nombre de intercalación de Windows &#40;Transact-SQL&#41;) y [SQL Server Collation Name &#40;Transact-SQL&#41;](../../t-sql/statements/sql-server-collation-name-transact-sql.md) (Nombre de intercalación de SQL Server &#40;Transact-SQL&#41;).  
  
 La cláusula COLLATE se puede usar para especificar únicamente las intercalaciones de las columnas con tipos de datos **char**, **varchar**, **nchar** y **nvarchar**.  
  
 Para más información sobre la cláusula COLLATE, vea [COLLATE &#40;Transact-SQL&#41;](~/t-sql/statements/collations.md).  
  
 NULL | NOT NULL  
 Determina si se permiten valores NULL en la columna. NULL no es estrictamente una restricción, pero se puede especificar, al igual que NOT NULL.  
  
[ CONSTRAINT *constraint_name* ]  
 Especifica el inicio de una definición de valor DEFAULT. Para mantener la compatibilidad con las versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se puede asignar un nombre de restricción a DEFAULT. *constraint_name* debe seguir las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md), excepto en que el nombre no puede empezar por un signo de número (#). Si no se especifica *constraint_name*, se asigna a la definición DEFAULT un nombre generado por el sistema.  
  
DEFAULT  
 Es una palabra clave que especifica el valor predeterminado de la columna. Las definiciones DEFAULT se pueden utilizar para proporcionar valores a una nueva columna de las filas existentes de datos. Las definiciones DEFAULT no se pueden aplicar a columnas de tipo **timestamp** o columnas con una propiedad IDENTITY. Si se especifica un valor predeterminado para una columna de un tipo definido por el usuario, dicho tipo debe admitir la conversión implícita de *constant_expression* al tipo definido por el usuario.  
  
*constant_expression*  
 Es un valor literal, un valor NULL o una función del sistema que se usa como valor predeterminado de la columna. Si se usa junto con una columna definida como de tipo [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] definido por el usuario, la implementación del tipo debe ser compatible con la conversión implícita de *constant_expression* en el tipo definido por el usuario.  
  
WITH VALUES   
 Al agregar una columna Y una restricción DEFAULT, si la columna permite valores NULL con WITH VALUES, para las filas existentes, establecerá el valor de la columna nueva en el valor determinado en la expresión *constant_expression* DEFAULT. Si la columna que se agrega no permite valores NULL, para las filas existentes, el valor de la columna siempre se establecerá en el valor determinado en la *expresión constante* DEFAULT. Desde SQL Server 2012, está puede ser una operación de metadatos [adding-not-null-columns-as-an-online-operation](alter-table-transact-sql.md#adding-not-null-columns-as-an-online-operation).
Si se usa cuando no se está agregando también la columna relacionada, no tiene ningún impacto.
 
 Especifica que el valor dado en DEFAULT *constant_expression* se almacena en una nueva columna que se agrega a las filas existentes. Si la columna agregada permite valores NULL y se ha especificado WITH VALUES, el valor predeterminado se almacena en la nueva columna que se agrega a las filas existentes. Si no se especifica WITH VALUES para las columnas que permiten valores NULL, el valor NULL se almacena en la nueva columna en las filas existentes. Si la nueva columna no permite valores NULL, el valor predeterminado se almacena en las nuevas filas, independientemente de que se especifique o no WITH VALUES.  
  
IDENTITY  
 Especifica que la nueva columna es una columna de identidad. [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] proporciona un valor incremental único para la columna. Al agregar columnas de identificadores a tablas existentes, los números de identidad se agregan a las filas existentes de la tabla con los valores de inicialización e incremento. No se garantiza el orden en que las filas se actualizan. También se generan números de identidad para las filas nuevas que se añadan.  
  
 Las columnas de identidad se utilizan normalmente junto con restricciones PRIMARY KEY para que actúen como identificador exclusivo de fila para la tabla. La propiedad IDENTITY se puede asignar a una columna **tinyint**, **smallint**, **int**, **bigint**, **decimal(p,0)** o **numeric(p,0)** . Solo se puede crear una columna de identidad para cada tabla. La palabra clave DEFAULT y los valores predeterminados enlazados no se pueden utilizar con una columna de identidad. En este caso, debe especificarse tanto el valor de inicialización como el incremento o ninguno. Si no se especifica ninguno, el valor predeterminado es (1,1).  
  
> [!NOTE]  
>  No puede modificar una columna de tabla existente para agregar la propiedad IDENTITY.  
  
 No se permite agregar una columna de identidad a una tabla publicada, puesto que esta operación puede dar lugar a una falta de convergencia cuando la columna se replica en el suscriptor. Los valores de la columna de identidad en el publicador dependen del orden en que se almacenen físicamente las filas de la tabla afectada. Las filas se pueden almacenar de forma diferente en el suscriptor; por lo tanto, el valor de la columna de identidad puede ser distinto para las mismas filas.  
  
 Para deshabilitar la propiedad IDENTITY de una columna al permitir que los valores se inserten de forma explícita, use [SET IDENTITY_INSERT](../../t-sql/statements/set-identity-insert-transact-sql.md).  
  
*seed*  
 Es el valor que se usa para la primera fila cargada en la tabla.  
  
*increment*  
 Es el valor incremental que se agrega al valor de identidad de la fila cargada anteriormente.  
  
NOT FOR REPLICATION  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Se puede especificar para la propiedad IDENTITY. Si se especifica esta cláusula para la propiedad IDENTITY, los valores de las columnas de identidad no aumentan cuando los agentes de replicación realizan operaciones de inserción.  
  
ROWGUIDCOL  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Especifica que la columna es una columna de identificador único global de la fila. ROWGUIDCOL solo se puede asignar a una columna **uniqueidentifier** y solo es posible designar una columna **uniqueidentifier** por tabla como columna ROWGUIDCOL. ROWGUIDCOL no se puede asignar a columnas con tipos de datos definidos por el usuario.  
  
 ROWGUIDCOL no impone la unicidad de los valores almacenados en la columna. Asimismo, tampoco genera automáticamente valores para nuevas filas que se insertan en la tabla. Para generar valores únicos para cada columna, utilice la función NEWID en instrucciones INSERT o especifique la función NEWID como la predeterminada para la columna. Para más información, vea [NEWID &#40;Transact-SQL&#41;](../../t-sql/functions/newid-transact-sql.md) e [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md).  
  
SPARSE  
 Indica que la columna es una columna dispersa. El almacenamiento de columnas dispersas está optimizado para los valores NULL. Las columnas dispersas no se pueden designar como NOT NULL. Para conocer otras restricciones y leer más información sobre columnas dispersas, vea [Usar columnas dispersas](../../relational-databases/tables/use-sparse-columns.md).  
  
\<column_constraint>  
 Para obtener las definiciones de los argumentos de restricción de columna, vea [column_constraint &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-column-constraint-transact-sql.md).  
  
 ENCRYPTED WITH  
 Especifica columnas de cifrado mediante la característica [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md).  
  
 COLUMN_ENCRYPTION_KEY = *key_name*  
 Especifica la clave de cifrado de columna. Para más información, vea [CREATE COLUMN ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-column-encryption-key-transact-sql.md).  
  
ENCRYPTION_TYPE = { DETERMINISTIC | RANDOMIZED }  
 El **cifrado determinista** usa un método que genera siempre el mismo valor cifrado para cualquier valor de texto no cifrado concreto. Al usar cifrado determinista se pueden realizar búsquedas mediante comparación de igualdad, agrupar y unir tablas mediante combinaciones de igualdad basadas en valores cifrados, y además se puede permitir a usuarios no autorizados que averigüen la información sobre valores cifrados mediante el análisis de patrones en la columna cifrada. La combinación de dos tablas en columnas cifradas de manera determinista solo es posible si ambas columnas están cifradas con la misma clave de cifrado de columna. El cifrado determinista debe usar una intercalación de columna con un criterio de ordenación binario 2 para columnas de caracteres.  
  
 El **cifrado aleatorio** utiliza un método que cifra los datos de una manera menos predecible. El cifrado aleatorio es más seguro, pero impide todos los cálculos y la indexación en columnas cifradas, a menos que la instancia de SQL Server admita [Always Encrypted con enclaves seguros](../../relational-databases/security/encryption/always-encrypted-enclaves.md).
  
 Si usa Always Encrypted (sin los enclaves seguros), utilice el cifrado determinista para que se busquen columnas con parámetros o parámetros de agrupación, por ejemplo, un número de identificación gubernamental. Use el cifrado aleatorio para datos como números de tarjeta de crédito, que no estén agrupados con otros registros ni se estén usando para combinar tablas, y en los que no se realicen búsquedas porque se estén usando otras columnas (por ejemplo, un número de transacción) para buscar la fila que contiene la columna cifrada en cuestión.  

 Si usa Always Encrypted con enclaves seguros, se recomienda, por ejemplo, el cifrado aleatorio.  
  
 Las columnas deben ser de un tipo de datos aplicable.  
  
ALGORITHM  
**Se aplica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores, [!INCLUDE[ssSDS](../../includes/sssds-md.md)]  
Debe ser **'AEAD_AES_256_CBC_HMAC_SHA_256'** .  
  
 Para más información, incluidas restricciones de características, vea [Always Encrypted &#40;motor de base de datos&#41;](../../relational-databases/security/encryption/always-encrypted-database-engine.md).  
  
   
ADD MASKED WITH ( FUNCTION = ' *mask_function* ')  
 **Se aplica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]  
  
 Especifica una máscara dinámica de datos. *mask_function* es el nombre de la función de máscara con los parámetros adecuados. Las siguientes funciones están disponibles:  
  
-   default()  
  
-   email()  
  
-   partial()  
  
-   random()  
  
 Para conocer más parámetros de función, vea [Enmascaramiento de datos dinámicos](../../relational-databases/security/dynamic-data-masking.md).  
  
## <a name="remarks"></a>Observaciones  
 Si se agrega una columna que tiene un tipo de datos **uniqueidentifier**, se puede definir con un valor predeterminado que use la función NEWID() para proporcionar los valores de identificador único de la nueva columna para cada fila existente en la tabla.  
  
 El [!INCLUDE[ssDE](../../includes/ssde-md.md)] no aplica un orden para especificar DEFAULT, IDENTITY, ROWGUIDCOL o las restricciones de columna en una definición de columna.  
  
 La instrucción ALTER TABLE generará un error si la adición de la columna hace que el tamaño de las filas de datos supere los 8060 bytes.  
  
## <a name="examples"></a>Ejemplos  
 Para consultar otros ejemplos, vea [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md).  
  
## <a name="see-also"></a>Consulte también  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
  
