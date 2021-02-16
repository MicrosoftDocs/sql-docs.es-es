---
description: SET ANSI_WARNINGS (Transact-SQL)
title: SET ANSI_WARNINGS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/15/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET ANSI_WARNINGS
- ANSI_WARNINGS_TSQL
- ANSI_WARNINGS
- SET_ANSI_WARNINGS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- errors [SQL Server], ISO standard behavior
- warnings [SQL Server]
- SET ANSI_WARNINGS statement
- ANSI_WARNINGS option
ms.assetid: f82aaab0-334f-427b-89b0-de4af596b4fa
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 13b4638d0ce8076b337b4879893ae23242fa00c4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350010"
---
# <a name="set-ansi_warnings-transact-sql"></a>SET ANSI_WARNINGS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Especifica el comportamiento estándar de ISO para diversas condiciones de error.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis

### <a name="syntax-for-ssnoversion-mdmd-and-sssodfull-mdmd"></a>Sintaxis para [!INCLUDE[ssnoversion-md.md](../../includes/ssnoversion-md.md)] y [!INCLUDE[sssodfull-md.md](../../includes/sssodfull-md.md)]
```syntaxsql
SET ANSI_WARNINGS { ON | OFF }
```

### <a name="syntax-for-sssdw-mdmd-and-sspdw-mdmd"></a>Sintaxis para [!INCLUDE[sssdw-md.md](../../includes/sssdw-md.md)] y [!INCLUDE[sspdw-md.md](../../includes/sspdw-md.md)]
```syntaxsql
SET ANSI_WARNINGS ON
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Observaciones
 SET ANSI_WARNINGS afecta a las condiciones siguientes:  
  
-   Si es ON y aparecen valores NULL en funciones de agregado, como SUM, AVG, MAX, MIN, STDEV, STDEVP, VAR, VARP o COUNT, se genera un mensaje de advertencia. Si es OFF, no se genera ninguna advertencia.  
  
-   Si es ON, los errores de división por cero y desbordamiento aritmético hacen que la instrucción se revierta y que se genere un mensaje de error. Si es OFF, los errores de división por cero y de desbordamiento aritmético hacen que se devuelvan valores NULL. El comportamiento por el que un error de división por cero o desbordamiento aritmético hace que se devuelvan valores NULL tiene lugar cuando se intenta ejecutar una operación INSERT o UPDATE en una columna de tipo **character**, Unicode o **binary**, en la que la longitud del nuevo valor excede el tamaño máximo de la columna. Si SET ANSI_WARNINGS es ON, se cancelan INSERT o UPDATE, tal y como especifica el estándar ISO. No se tienen en cuenta los espacios en blanco a la derecha en las columnas de carácter ni los valores NULL a la derecha en las columnas binarias. Cuando es OFF, los datos se truncan para ajustarlos al tamaño de la columna y la instrucción se ejecuta correctamente.  
  
> [!NOTE]  
> Cuando se produce un truncamiento en alguna conversión desde o hacia datos **binary** o **varbinary**, no se emite ningún error ni advertencia, independientemente de las opciones SET.  
  
> [!NOTE]  
> No se respeta ANSI_WARNINGS al pasar parámetros de un procedimiento almacenado, una función definida por el usuario o al declarar y establecer variables en una instrucción de lote. Por ejemplo, si una variable se define como **char(3)** y después se establece en un valor de más de tres caracteres, los datos se truncan hasta el tamaño definido y la instrucción INSERT o UPDATE se ejecuta correctamente.  
  
Se puede utilizar la opción user options de sp_configure para establecer el valor predeterminado de ANSI_WARNINGS en todas las conexiones al servidor. Para obtener más información, vea [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
ANSI_WARNINGS debe ser ON al crear o manipular índices en columnas calculadas o vistas indexadas. Si SET ANSI_WARNINGS es OFF, las instrucciones CREATE, UPDATE, INSERT y DELETE provocarán errores en tablas con índices en columnas calculadas y vistas indizadas. Para más información sobre las configuraciones de las opciones SET necesarias con vistas indizadas e índices en columnas calculadas, vea el apartado "Consideraciones al utilizar las instrucciones SET" en [Instrucciones SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md).  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] incluye la opción de base de datos ANSI_WARNINGS. Es equivalente a SET ANSI_WARNINGS. Cuando SET ANSI_WARNINGS es ON, se producen errores o advertencias si hay división por cero, cadenas demasiado largas para la columna correspondiente de la base de datos y otras situaciones similares. Cuando SET ANSI_WARNINGS es OFF, no se generan tales errores y advertencias. La opción predeterminada en la base de datos model para SET ANSI_WARNINGS es OFF. Si no se especifica, se aplica la opción de ANSI_WARNINGS. Si SET ANSI_WARNINGS es OFF, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa el valor de la columna is_ansi_warnings_on de la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).  
  
> [!IMPORTANT]
> ANSI_WARNINGS debe establecerse en ON para ejecutar consultas distribuidas.  
  
Los clientes, como el controlador ODBC de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client, el proveedor OLE DB de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y Microsoft JDBC Driver para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] establecen automáticamente ANSI_WARNINGS en ON con una marca de conexión. Esta opción se puede configurar en los orígenes de datos ODBC, en los atributos de conexión de ODBC, establecidos en la aplicación antes de conectar. El valor predeterminado de SET ANSI_WARNINGS es OFF en las conexiones desde aplicaciones DB-Library. Para más información, vea [LOGIN7](/openspecs/windows_protocols/ms-tds/773a62b6-ee89-4c02-9e5e-344882630aac) en las especificaciones del protocolo de flujo TDS. 

Cuando ANSI_DEFAULTS es ON, se habilita ANSI_WARNINGS.  
  
El valor de ANSI_WARNINGS se define en tiempo de ejecución, no en tiempo de análisis.  
  
Si SET ARITHABORT o SET ARITHIGNORE es OFF y SET ANSI_WARNINGS es ON, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devolverá un mensaje de error cuando haya errores de división por cero o desbordamiento.  
  
Para ver la configuración actual de este valor, ejecute la consulta siguiente.  
  
```sql  
DECLARE @ANSI_WARN VARCHAR(3) = 'OFF';  
IF ( (8 & @@OPTIONS) = 8 ) SET @ANSI_WARN = 'ON';  
SELECT @ANSI_WARN AS ANSI_WARNINGS;  
```  
  
## <a name="permissions"></a>Permisos  
Debe pertenecer al rol **public** .  
  
## <a name="examples"></a>Ejemplos  
En este ejemplo se muestran las tres situaciones mencionadas anteriormente con SET ANSI_WARNINGS en ON y en OFF.  
  
```sql  
CREATE TABLE T1   
(  
   a int,   
   b int NULL,   
   c varchar(20)  
);  
GO  
  
SET NOCOUNT ON;  
  
INSERT INTO T1   
VALUES (1, NULL, '')   
      ,(1, 0, '')  
      ,(2, 1, '')  
      ,(2, 2, '');  
  
SET NOCOUNT OFF;  
GO  
```

Ahora establezca ANSI_WARNINGS en ON y pruebe.

```sql
PRINT '**** Setting ANSI_WARNINGS ON';  
GO  
  
SET ANSI_WARNINGS ON;  
GO  
  
PRINT 'Testing NULL in aggregate';  
GO  
SELECT a, SUM(b)   
FROM T1   
GROUP BY a;  
GO  
  
PRINT 'Testing String Overflow in INSERT';  
GO  
INSERT INTO T1   
VALUES (3, 3, 'Text string longer than 20 characters');  
GO  
  
PRINT 'Testing Divide by zero';  
GO  
SELECT a / b AS ab   
FROM T1;  
GO  
```

Ahora establezca ANSI_WARNINGS en OFF y pruebe.

```sql
PRINT '**** Setting ANSI_WARNINGS OFF';  
GO  
SET ANSI_WARNINGS OFF;  
GO  
  
PRINT 'Testing NULL in aggregate';  
GO  
SELECT a, SUM(b)   
FROM T1   
GROUP BY a;  
GO  
  
PRINT 'Testing String Overflow in INSERT';  
GO  
INSERT INTO T1   
VALUES (4, 4, 'Text string longer than 20 characters');  
GO  
SELECT a, b, c   
FROM T1  
WHERE a = 4;  
GO  
  
PRINT 'Testing Divide by zero';  
GO  
SELECT a / b AS ab   
FROM T1;  
GO  
  
DROP TABLE T1;  
```  
  
## <a name="see-also"></a>Consulte también  
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [Instrucciones SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET ANSI_DEFAULTS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-defaults-transact-sql.md)   
 [SESSIONPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/sessionproperty-transact-sql.md)  
  
