---
description: SET ANSI_PADDING (Transact-SQL)
title: SET ANSI_PADDING (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/04/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ANSI_PADDING_TSQL
- ANSI_PADDING
- SET_ANSI_PADDING_TSQL
- SET ANSI_PADDING
dev_langs:
- TSQL
helpviewer_keywords:
- data types [SQL Server], short values
- ANSI_PADDING option
- short values [SQL Server]
- SET ANSI_PADDING statement
- trailing blanks
ms.assetid: 92bd29a3-9beb-410e-b7e0-7bc1dc1ae6d0
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0975b21d4d12166a39b8a6099002d0228fa1b7b8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342098"
---
# <a name="set-ansi_padding-transact-sql"></a>SET ANSI_PADDING (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Controla el modo en que la columna almacena valores más cortos que el tamaño definido y el modo en que almacena valores con espacios en blanco finales en datos de tipo **char**, **varchar**, **binary** y **varbinary** .  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis
 
### <a name="syntax-for-ssnoversion-mdmd-and-sssodfull-mdmd"></a>Sintaxis para [!INCLUDE[ssnoversion-md.md](../../includes/ssnoversion-md.md)] y [!INCLUDE[sssodfull-md.md](../../includes/sssodfull-md.md)] 
```syntaxsql
SET ANSI_PADDING { ON | OFF }
```

### <a name="syntax-for-sssdw-mdmd-and-sspdw-mdmd"></a>Sintaxis para [!INCLUDE[sssdw-md.md](../../includes/sssdw-md.md)] y [!INCLUDE[sspdw-md.md](../../includes/sspdw-md.md)]
```syntaxsql
SET ANSI_PADDING ON
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Observaciones
 Las columnas definidas con tipos de datos **char**, **varchar**, **binary** y **varbinary** tienen un tamaño definido.  
  
 Esta opción solo afecta a la definición de nuevas columnas. Una vez creada la columna, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] almacena los valores de acuerdo con la opción establecida en el momento de su creación. Las columnas existentes no se ven afectadas por los cambios posteriores de esta opción.  
  
> [!NOTE]  
> ANSI_PADDING siempre se debe establecer en ON.  
  
 En esta tabla se muestran los efectos del parámetro SET ANSI_PADDING cuando se insertan valores en columnas con los tipos de datos **char**, **varchar**, **binary** y **varbinary**.  
  
|Configuración|char(*n*) NOT NULL o binary(*n*) NOT NULL|char(*n*) NULL o binary(*n*) NULL|varchar(*n*) o varbinary(*n*)|  
|-------------|----------------------------------------------------|--------------------------------------------|----------------------------------------|  
|ACTIVAR|Rellena el valor original (con espacios en blanco finales en las columnas **char** y con ceros finales en las columnas **binary**) hasta completar la longitud de la columna.|Sigue las mismas reglas que para **char(** _n_ **)** o **binary(** _n_ **)** NOT NULL cuando SET ANSI_PADDING es ON.|Los espacios en blanco finales en los valores de caracteres insertados en las columnas **varchar** no se recortan. Los ceros a la derecha en los valores binarios insertados en las columnas **varbinary** no se recortan. Los valores no se rellenan hasta completar la longitud de la columna.|  
|Apagado|Rellena el valor original (con espacios en blanco finales en las columnas **char** y con ceros finales en las columnas **binary**) hasta completar la longitud de la columna.|Sigue las mismas reglas que para **varchar** o **varbinary** cuando SET ANSI_PADDING está en OFF.|Los espacios en blanco finales en los valores de carácter insertados en una columna **varchar** se recortan. Los ceros a la derecha en los valores binarios insertados en una columna **varbinary** se recortan.|  
  
> [!NOTE]  
> Cuando se rellenan las columnas **char**, se incluyen espacios en blanco y en las columnas **binary** se incluyen ceros. Cuando se recortan las columnas **char**, se recortan los espacios en blanco finales; en las columnas **binary** se recortan los ceros finales.  
  
ANSI_PADDING debe ser ON al crear o cambiar índices en columnas calculadas o vista indexadas. Para más información sobre las configuraciones de las opciones SET necesarias con vistas indizadas e índices en columnas calculadas, vea el apartado "Consideraciones al utilizar las instrucciones SET" en [Instrucciones SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md).  
  
El valor predeterminado de SET ANSI_PADDING es ON. El controlador ODBC de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client y el proveedor OLE DB de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] establecen automáticamente ANSI_PADDING en ON al conectarse. Esta opción se puede configurar en los orígenes de datos ODBC, en los atributos de conexión de ODBC o en las propiedades de conexión OLE DB establecidas en la aplicación antes de conectar. SET ANSI_PADDING tiene como opción predeterminada OFF en las conexiones desde aplicaciones DB-Library.  
  
 La opción SET ANSI_PADDING no afecta a los tipos de datos **nchar**, **nvarchar**, **ntext**, **text**, **image**, **varbinary(max)**, **varchar(max)** y **nvarchar(max)**. Siempre muestran el comportamiento SET ANSI_PADDING ON. Esto significa que no se recortan los espacios y ceros a la derecha.  
  
Cuando ANSI_DEFAULTS es ON, se habilita ANSI_PADDING.  
  
El valor de ANSI_PADDING se define en tiempo de ejecución, no en tiempo de análisis.  
  
Para ver la configuración actual de este valor, ejecute la consulta siguiente.  
  
```sql  
DECLARE @ANSI_PADDING VARCHAR(3) = 'OFF';  
IF ( (16 & @@OPTIONS) = 16 ) SET @ANSI_PADDING = 'ON';  
SELECT @ANSI_PADDING AS ANSI_PADDING;  
```  
  
## <a name="permissions"></a>Permisos  
Debe pertenecer al rol **public** .  
  
## <a name="examples"></a>Ejemplos  
En este ejemplo se muestra cómo afecta esta opción a cada uno de los tipos de datos.  

Establezca ANSI_PADDING en ON y pruebe.

```sql  
PRINT 'Testing with ANSI_PADDING ON'  
SET ANSI_PADDING ON;  
GO  
  
CREATE TABLE t1 (  
   charcol CHAR(16) NULL,   
   varcharcol VARCHAR(16) NULL,   
   varbinarycol VARBINARY(8)  
);  
GO  
INSERT INTO t1 VALUES ('No blanks', 'No blanks', 0x00ee);  
INSERT INTO t1 VALUES ('Trailing blank ', 'Trailing blank ', 0x00ee00);  
  
SELECT 'CHAR' = '>' + charcol + '\<', 'VARCHAR'='>' + varcharcol + '\<',  
   varbinarycol  
FROM t1;  
GO  
```

Ahora establezca ANSI_PADDING en OFF y pruebe.

```sql
PRINT 'Testing with ANSI_PADDING OFF';  
SET ANSI_PADDING OFF;  
GO  
  
CREATE TABLE t2 (  
   charcol CHAR(16) NULL,   
   varcharcol VARCHAR(16) NULL,   
   varbinarycol VARBINARY(8)  
);  
GO  
INSERT INTO t2 VALUES ('No blanks', 'No blanks', 0x00ee);  
INSERT INTO t2 VALUES ('Trailing blank ', 'Trailing blank ', 0x00ee00);  
  
SELECT 'CHAR' = '>' + charcol + '\<', 'VARCHAR'='>' + varcharcol + '<',  
   varbinarycol  
FROM t2;  
GO  
  
DROP TABLE t1;  
DROP TABLE t2;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Instrucciones SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SESSIONPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/sessionproperty-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [SET ANSI_DEFAULTS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-defaults-transact-sql.md)  
  
  
