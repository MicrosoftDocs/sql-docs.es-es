---
description: sp_describe_parameter_encryption (Transact-SQL)
title: sp_describe_parameter_encryption (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/27/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_describe_parameter_encryption
- sp_describe_parameter_encryption_TSQL
- sys.sp_describe_parameter_encryption
- sys.sp_describe_parameter_encryption_TSQL
helpviewer_keywords:
- sp_describe_parameter_encryption
ms.assetid: 706ed441-2881-4934-8d5e-fb357ee067ce
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 41821edc34835f60cef201ae78b3448f3d3f4699
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200776"
---
# <a name="sp_describe_parameter_encryption-transact-sql"></a>sp_describe_parameter_encryption (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Analiza la instrucción especificada [!INCLUDE[tsql](../../includes/tsql-md.md)] y sus parámetros para determinar qué parámetros se corresponden con las columnas de base de datos que están protegidas mediante la característica Always encrypted. Devuelve los metadatos de cifrado de los parámetros que corresponden a las columnas cifradas.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
sp_describe_parameter_encryption   
    [ @tsql = ] N'Transact-SQL_batch' ,   
    [ @params = ] N'parameters'   
[ ;]  
```  
  
## <a name="arguments"></a>Argumentos  
 [ \@ tsql =] ' Transact-SQL_batch '  
 Una o varias instrucciones de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Transact-SQL_batch puede ser nvarchar (n) o nvarchar (Max).  
  
 [ \@ params =] N'parameters '  
 *\@ params* proporciona una cadena de declaración para los parámetros del lote de Transact-SQL, que es similar a sp_executesql. Los parámetros pueden ser nvarchar (n) o nvarchar (Max).  
  
 Es una cadena que contiene las definiciones de todos los parámetros que se han incrustado en el [!INCLUDE[tsql](../../includes/tsql-md.md)] _batch. La cadena debe ser una constante Unicode o una variable Unicode. Cada definición de parámetro se compone de un nombre de parámetro y un tipo de datos. *n* es un marcador de posición que indica definiciones de parámetros adicionales. Todos los parámetros especificados en la instrucción deben definirse en *\@ params*. Si la [!INCLUDE[tsql](../../includes/tsql-md.md)] instrucción o el lote de la instrucción no contiene parámetros, *\@* no es necesario realizar ningún parámetro. NULL es el valor predeterminado para este parámetro.  
  
## <a name="return-value"></a>Valor devuelto  
 0 indica que la operación se ha realizado correctamente. Cualquier otro elemento indica un error.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 **sp_describe_parameter_encryption** devuelve dos conjuntos de resultados:  
  
-   El conjunto de resultados que describe las claves criptográficas configuradas para las columnas de base de datos, los parámetros de la [!INCLUDE[tsql](../../includes/tsql-md.md)] instrucción especificada se corresponden con.  
  
-   Conjunto de resultados que describe cómo se deben cifrar los parámetros concretos. Este conjunto de resultados hace referencia a las claves descritas en el primer conjunto de resultados.  
  
 Cada fila del primer conjunto de resultados describe un par de claves; una clave de cifrado de columna cifrada y su clave maestra de columna correspondiente.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**column_encryption_key_ordinal**|**int**|Identificador de la fila del conjunto de resultados.|  
|**database_id**|**int**|Identificador de base de datos.|  
|**column_encryption_key_id**|**int**|Identificador de la clave de cifrado de la columna. Nota: este identificador denota una fila en la sys.column_encryption_keys &#40;vista de catálogo de [Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md) .|  
|**column_encryption_key_version**|**int**|Reservado para un uso futuro. Actualmente, siempre contiene 1.|  
|**column_encryption_key_metadata_version**|**Binary(8**|Marca de tiempo que representa la hora de creación de la clave de cifrado de columna.|  
|**column_encryption_key_encrypted_value**|**varbinary (4000)**|Valor cifrado de la clave de cifrado de columna.|  
|**column_master_key_store_provider_name**|**sysname**|Nombre del proveedor del almacén de claves que contiene la clave maestra de columna, que se usó para generar el valor cifrado de la clave de cifrado de columna.|  
|**column_master_key_path**|**nvarchar(4000)**|La ruta de acceso de la clave maestra de columna, que se usó para generar el valor cifrado de la clave de cifrado de columna.|  
|**column_encryption_key_encryption_algorithm_name**|**sysname**|Nombre del algoritmo de cifrado que se usa para generar el valor de cifrado de la clave de cifrado de columna.|  
  
 Cada fila del segundo conjunto de resultados contiene metadatos de cifrado para un parámetro.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**parameter_ordinal**|**int**|Identificador de la fila del conjunto de resultados.|  
|**parameter_name**|**sysname**|Nombre de uno de los parámetros especificados en el argumento *\@ params* .|  
|**column_encryption_algorithm**|**tinyint**|Código que indica el algoritmo de cifrado configurado para la columna, el parámetro corresponde a. Los valores admitidos actualmente son: 2 para **AEAD_AES_256_CBC_HMAC_SHA_256**.|  
|**column_encryption_type**|**tinyint**|Código que indica el tipo de cifrado configurado para la columna, el parámetro corresponde a. Los valores admitidos son:<br /><br /> 0: texto sin formato (la columna no está cifrada)<br /><br /> 1: cifrado aleatorio<br /><br /> 2-cifrado determinista.|  
|**column_encryption_key_ordinal**|**int**|Código de la fila del primer conjunto de resultados. La fila a la que se hace referencia describe la clave de cifrado de columnas configurada para la columna, el parámetro corresponde a.|  
|**column_encryption_normalization_rule_version**|**tinyint**|Número de versión del algoritmo de normalización de tipos.|  
  
## <a name="remarks"></a>Observaciones  
 Un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] controlador de cliente, compatible Always Encrypted, llama automáticamente a **sp_describe_parameter_encryption** para recuperar los metadatos de cifrado de las consultas con parámetros, emitidos por la aplicación. Posteriormente, el controlador utiliza los metadatos de cifrado para cifrar los valores de los parámetros que corresponden a las columnas de base de datos protegidas con Always Encrypted y sustituye los valores de parámetro de texto simple, enviados por la aplicación, con los valores de parámetro cifrados, antes de enviar la consulta al motor de base de datos.  
  
## <a name="permissions"></a>Permisos  
 Requiere la **definición de clave de cifrado ver cualquier columna** y ver los permisos de **definición de clave maestra de columna** en la base de datos.  
  
## <a name="examples"></a>Ejemplos  
  
```sql  
CREATE COLUMN MASTER KEY [CMK1]  
WITH  
(  
       KEY_STORE_PROVIDER_NAME = N'MSSQL_CERTIFICATE_STORE',  
    KEY_PATH = N'CurrentUser/my/A66BB0F6DD70BDFF02B62D0F87E340288E6F9305'  
);  
GO  
  
CREATE COLUMN ENCRYPTION KEY [CEK1]  
WITH VALUES  
(  
       COLUMN_MASTER_KEY = [CMK1],  
    ALGORITHM = 'RSA_OAEP',  
    ENCRYPTED_VALUE =   
0x016E000001630075007200720065006E00740075007300650072002F006D007  
9002F00610036003600620062003000660036006400640037003000620064006  
6006600300032006200360032006400300066003800370065003300340030003  
200380038006500360066003900330030003500CA0D0CEC74ECADD1804CF991  
37B4BD06BBAB15D7EA74E0C249A779C7768A5B659E0125D24FF827F5EA8CA51  
7A8E197ECA1353BA814C2B0B2E6C8AB36E3AE6A1E972D69C3C573A963ADAB6  
686CF5D24F95FE43140C4F9AF48FBA7DF2D053F3B4A1F5693A1F905440F8015B  
DB43AF8A04BE4E045B89876A0097E5FBC4E6A3B9C3C0D278C540E46C53938B8  
C957B689C4DC095821C465C73117CBA95B758232F9E5B2FCC7950B8CA00AFE3  
74DE42847E3FBC2FDD277035A2DEF529F4B735C20D980073B4965B4542A3472  
3276A1646998FC6E1C40A3FDB6ABCA98EE2B447F114D2AC7FF8C7D51657550E  
C5C2BABFFE8429B851272086DCED94332CF18FA854C1D545A28B1EF4BE64F8E0  
35175C1650F6FC5C4702ACF99850A4542B3747EAEC0CC726E091B36CE24392D8  
01ECAA684DE344FECE05812D12CD72254A014D42D0EABDA41C89FC4F545E88B  
4B8781E5FAF40D7199D4842D2BFE904D209728ED4F527CBC169E2904F6E711FF8  
1A8F4C25382A2E778DD2A58552ED031AFFDA9D9D891D98AD82155F93C58202F  
C24A77F415D4F8EF22419D62E188AC609330CCBD97CEE1AEF8A18B0195883360  
4707FDF03B2B386487CC679D7E352D0B69F9FB002E51BCD814D077E82A09C14E  
9892C1F8E0C559CFD5FA841CEF647DAB03C8191DC46B772E94D579D8C80FE93C  
3827C9F0AE04D5325BC73111E07EEEDBE67F1E2A73580085  
);  
GO  
  
CREATE TABLE t1 (  
c1 INT ENCRYPTED WITH (  
    COLUMN_ENCRYPTION_KEY = [CEK_Auto1],   
    ENCRYPTION_TYPE = Randomized,   
    ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NULL,  
);  
  
EXEC sp_describe_parameter_encryption N'INSERT INTO t1 VALUES(@c1)',  N'@c1 INT';  
```  
  
 Este es el primer conjunto de resultados:  
  
|column_encryption_key_ordinal|database_id|column_encryption_key_id|column_encryption_key_version|column_encryption_key_metadata_version|column_encryption_key_encrypted_value|  
|--------------------------------------|------------------|---------------------------------|--------------------------------------|------------------------------------------------|-----------------------------------------------|  
|1|5|1|1|0x99EDA60083A50000|0x016E000001630075007200720065006E00740075007300650072002F006D0079002F006100360036006200620030006600360064006400370030006200640066006600300032006200360032006400300066003800370065003300340030003200380038006500360066003900330030003500CA0D0CEC74ECADD1804CF99137B4BD06BBAB15D7EA74E0C249A779C7768A5B659E0125D24FF827F5EA8CA517A8E197ECA1353BA814C2B0B2E6C8AB36E3AE6A1E972D69C3C573A963ADAB6686CF5D24F95FE43140C4F9AF48FBA7DF2D053F3B4A1F5693A1F905440F8015BDB43AF8A04BE4E045B89876A0097E5FBC4E6A3B9C3C0D278C540E46C53938B8C957B689C4DC095821C465C73117CBA95B758232F9E5B2FCC7950B8CA00AFE374DE42847E3FBC2FDD277035A2DEF529F4B735C20D980073B4965B4542A34723276A1646998FC6E1C40A3FDB6ABCA98EE2B447F114D2AC7FF8C7D51657550EC5C2BABFFE8429B851272086DCED94332CF18FA854C1D545A28B1EF4BE64F8E035175C1650F6FC5C4702ACF99850A4542B3747EAEC0CC726E091B36CE24392D801ECAA684DE344FECE05812D12CD72254A014D42D0EABDA41C89FC4F545E88B4B8781E5FAF40D7199D4842D2BFE904D209728ED4F527CBC169E2904F6E711FF81A8F4C25382A2E778DD2A58552ED031AFFDA9D9D891D98 AD82155F93C58202FC24A77F415D4F8EF22419D62E188AC609330CCBD97CEE1AEF8A18B01958833604707FDF03B2B386487CC679D7E352D0B69F9FB002E51BCD814D077E82A09C14E9892C1F8E0C559CFD5FA841CEF647DAB03C8191DC46B772E94D579D8C80FE93C3827C9F0AE04D5325BC73111E07EEEDBE67F1E2A73580085|  
  
 (Los resultados continúan).  
  
|column_master_key_store_provider_name|column_master_key_path|column_encryption_key_encryption_algorithm_name|  
|------------------------------------------------|-------------------------------|----------------------------------------------------------|  
|MSSQL_CERTIFICATE_STORE|CurrentUser/My/A66BB0F6DD70BDFF02B62D0F87E340288E6F9305|RSA_OAEP|  
  
 Este es el segundo conjunto de resultados:  
  
|parameter_ordinal|parameter_name|column_encryption_algorithm|column_encryption_type|  
|------------------------|---------------------|-----------------------------------|------------------------------|  
|1|\@C1|1|1|  
  
 (Los resultados continúan).  
  
|column_encryption_key_ordinal|column_encryption_normalization_rule_version|  
|--------------------------------------|------------------------------------------------------|  
|1|1|  
  
## <a name="see-also"></a>Consulte también  
 [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md)   
 [Desarrollo de aplicaciones con Always Encrypted](../../relational-databases/security/encryption/always-encrypted-client-development.md)  
  
  
