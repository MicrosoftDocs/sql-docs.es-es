---
description: OPENQUERY (Transact-SQL)
title: OPENQUERY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OPENQUERY_TSQL
- OPENQUERY
dev_langs:
- TSQL
helpviewer_keywords:
- DELETE statement [SQL Server], OPENQUERY function
- OPENQUERY function
- FROM clause, OPENQUERY function
- UPDATE statement [SQL Server], OPENQUERY function
- pass-through queries [SQL Server]
- INSERT statement [SQL Server], OPENQUERY function
ms.assetid: b805e976-f025-4be1-bcb0-3a57b0c57717
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: d7af8ffa6f8fe26bf31c699dece7f24e7f6c32fe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180210"
---
# <a name="openquery-transact-sql"></a>OPENQUERY (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Ejecuta la consulta de paso a través especificada en el servidor vinculado especificado. Este servidor es un origen de datos OLE DB. Se puede hacer referencia a OPENQUERY en la cláusula FROM de una consulta como si fuera un nombre de tabla. También se puede hacer referencia a OPENQUERY como la tabla de destino de una instrucción INSERT, UPDATE, DELETE o TRUNCATE. Esto está sujeto a las capacidades del proveedor OLE DB. Aunque la consulta puede devolver varios conjuntos de resultados, OPENQUERY solo devuelve el primero.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
OPENQUERY ( linked_server ,'query' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *linked_server*  
 Es un identificador que representa el nombre del servidor vinculado.  
  
 **"** *consulta* **"**  
 Es la cadena de consulta que se ejecuta en el servidor vinculado. La longitud máxima de la cadena es de 8 KB.  
  
## <a name="remarks"></a>Comentarios  
 OPENQUERY no acepta variables para sus argumentos.  
  
 No se puede utilizar OPENQUERY para ejecutar procedimientos almacenados extendidos en un servidor vinculado. Sin embargo, se puede ejecutar un procedimiento almacenado extendido en un servidor vinculado mediante un nombre de cuatro partes. Por ejemplo:  
  
```sql  
EXEC SeattleSales.master.dbo.xp_msver  
```  
  
 Las llamadas a OPENDATASOURCE, OPENQUERY u OPENROWSET en la cláusula FROM se evalúan por separado y de forma independiente de otras llamadas a estas funciones utilizadas como destino de la actualización, incluso si se han suministrado argumentos idénticos a las dos llamadas. En particular, las condiciones de filtro o combinación aplicadas en el resultado de una de esas llamadas no tienen ningún efecto en los resultados de la otra llamada.  
  
## <a name="permissions"></a>Permisos  
 Cualquier usuario puede ejecutar OPENQUERY. Los permisos utilizados para conectarse al servidor remoto se obtienen de la configuración definida para el servidor vinculado.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-executing-an-update-pass-through-query"></a>A. Ejecutar una consulta UPDATE de paso a través  
 En el ejemplo siguiente se usa una consulta `UPDATE` de paso a través en el servidor vinculado creado en el ejemplo A.  
  
```sql  
UPDATE OPENQUERY (OracleSvr, 'SELECT name FROM joe.titles WHERE id = 101')   
SET name = 'ADifferentName';  
```  
  
### <a name="b-executing-an-insert-pass-through-query"></a>B. Ejecutar una consulta INSERT de paso a través  
 En el ejemplo siguiente se usa una consulta `INSERT` de paso a través en el servidor vinculado creado en el ejemplo A.  
  
```sql  
INSERT OPENQUERY (OracleSvr, 'SELECT name FROM joe.titles')  
VALUES ('NewTitle');  
```  
  
### <a name="c-executing-a-delete-pass-through-query"></a>C. Ejecutar una consulta DELETE de paso a través  
 En el ejemplo siguiente se usa una consulta `DELETE` de paso a través para eliminar la columna insertada en el ejemplo C.  
  
```sql  
DELETE OPENQUERY (OracleSvr, 'SELECT name FROM joe.titles WHERE name = ''NewTitle''');  
```  
  
### <a name="d-executing-a-select-pass-through-query"></a>D. Ejecutar una consulta SELECT de paso a través  
 En este ejemplo se usa una consulta `SELECT` de paso a través para eliminar la columna insertada en el ejemplo C.  
  
```sql  
SELECT * FROM OPENQUERY (OracleSvr, 'SELECT name FROM joe.titles WHERE name = ''NewTitle''');  
```  
    
## <a name="see-also"></a>Consulte también  
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [OPENDATASOURCE &#40;Transact-SQL&#41;](../../t-sql/functions/opendatasource-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)   
 [sp_serveroption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-serveroption-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)  
  
  
