---
description: SCOPE_IDENTITY (Transact-SQL)
title: SCOPE_IDENTITY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SCOPE_IDENTITY
- SCOPE_IDENTITY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- identity columns [SQL Server], SCOPE_IDENTITY function
- SCOPE_IDENTITY function
- last-inserted identity values
- identity values [SQL Server], last-inserted
ms.assetid: eef24670-059b-4f10-91d4-a67bc1ed12ab
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: bb49f217e40b13b106743da61169eaf35a58ef1c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183468"
---
# <a name="scope_identity-transact-sql"></a>SCOPE_IDENTITY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve el último valor de identidad insertado en una columna de identidad en el mismo ámbito. Un ámbito es un módulo: un procedimiento almacenado, desencadenador, función o lote. Por tanto, si dos instrucciones se encuentran en el mismo procedimiento almacenado, función o lote, están en el mismo ámbito.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
SCOPE_IDENTITY()  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devuelto
 **numeric(38,0)**  
  
## <a name="remarks"></a>Observaciones  
 SCOPE_IDENTITY, IDENT_CURRENT y @@IDENTITY son funciones parecidas ya que devuelven valores insertados en columnas de identidad.  
  
 IDENT_CURRENT no está limitado por el ámbito y la sesión; se limita a una tabla especificada. IDENT_CURRENT devuelve el valor generado para una tabla específica en cualquier sesión y cualquier ámbito. Para obtener más información, vea [IDENT_CURRENT &#40;Transact-SQL&#41;](../../t-sql/functions/ident-current-transact-sql.md).  
  
 SCOPE_IDENTITY y @@IDENTITY devuelven los últimos valores de identidad generados en una tabla en la sesión actual. No obstante, SCOPE_IDENTITY solo devuelve los valores insertados en el ámbito actual; @@IDENTITY no se limita a un ámbito específico.  
  
 Por ejemplo, suponga que hay dos tablas, T1 y T2, y un desencadenador INSERT definido en T1. Cuando se inserta una fila en T1, el desencadenador se activa e inserta una fila en T2. Este escenario muestra dos ámbitos: la inserción en T1 y la inserción en T2 como resultado del desencadenador.  
  
 Suponiendo que T1 y T2 tienen columnas de identidad, @@IDENTITY y SCOPE_IDENTITY devolverán distintos valores al finalizar una instrucción INSERT en T1. @@IDENTITY devolverá el último valor de la columna de identidad insertado en cualquier ámbito en la sesión actual. Este es el valor insertado en T2. SCOPE_IDENTITY() devuelve el valor IDENTITY insertado en T1. Es la última inserción que se ha producido en el mismo ámbito. La función SCOPE_IDENTITY() devuelve el valor NULL si se llama a la función antes de que se ejecuten las instrucciones INSERT en una columna de identidad del ámbito.  
  
 Las instrucciones y transacciones con errores pueden cambiar la identidad actual de una tabla y crear huecos en los valores de columna de identidad. El valor de identidad jamás se revierte, aun cuando no se haya confirmado la transacción que intentó insertar el valor en la tabla. Por ejemplo, si se produce un error en una instrucción INSERT debido a una infracción de tipo IGNORE_DUP_KEY, el valor de identidad actual de la tabla se sigue incrementando.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-identity-and-scope_identity-with-triggers"></a>A. Uso de @@IDENTITY y SCOPE_IDENTITY con desencadenadores  
 Este ejemplo crea dos tablas, `TZ` y `TY`, y un desencadenador INSERT en `TZ`. Cuando se inserta una fila en `TZ`, el desencadenador `Ztrig` se activa e inserta una fila en `TY`.  
  
```sql  
USE tempdb;  
GO  
CREATE TABLE TZ (  
   Z_id  INT IDENTITY(1,1)PRIMARY KEY,  
   Z_name VARCHAR(20) NOT NULL);  
  
INSERT TZ  
   VALUES ('Lisa'),('Mike'),('Carla');  
  
SELECT * FROM TZ;  
```     
Conjunto de resultados: este es el aspecto de la tabla TZ.  
  
```  
Z_id   Z_name  
-------------  
1      Lisa  
2      Mike  
3      Carla  
```  
```sql 
CREATE TABLE TY (  
   Y_id  INT IDENTITY(100,5)PRIMARY KEY,  
   Y_name VARCHAR(20) NULL);  
  
INSERT TY (Y_name)  
   VALUES ('boathouse'), ('rocks'), ('elevator');  
  
SELECT * FROM TY;  
```   
Conjunto de resultados: este es el aspecto de TY.  
```  
Y_id  Y_name  
---------------  
100   boathouse  
105   rocks  
110   elevator  
```  

Cree el desencadenador que inserta una fila en una tabla TY cuando se inserta una fila en una tabla TZ.  
```sql  
CREATE TRIGGER Ztrig  
ON TZ  
FOR INSERT AS   
   BEGIN  
   INSERT TY VALUES ('')  
   END;  
```  
ACTIVE el desencadenador y determine los valores de identidad que se obtienen con las funciones @@IDENTITY y SCOPE_IDENTITY.   
```sql
INSERT TZ VALUES ('Rosalie');  
  
SELECT SCOPE_IDENTITY() AS [SCOPE_IDENTITY];  
GO  
SELECT @@IDENTITY AS [@@IDENTITY];  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
```
/*SCOPE_IDENTITY returns the last identity value in the same scope. This was the insert on table TZ.*/`  
SCOPE_IDENTITY  
4  

/*@@IDENTITY returns the last identity value inserted to TY by the trigger. 
  This fired because of an earlier insert on TZ.*/
@@IDENTITY  
115  
```  
  
### <a name="b-using-identity-and-scope_identity-with-replication"></a>B. Uso de @@IDENTITY y SCOPE_IDENTITY() con una replicación  
 Los ejemplos siguientes muestran cómo se usan `@@IDENTITY` y `SCOPE_IDENTITY()` para las inserciones en una base de datos publicada para la replicación de mezcla. Las dos tablas de los ejemplos se encuentran en la base de datos de ejemplo [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]: `Person.ContactType` no está publicado y `Sales.Customer` sí. La replicación de mezcla agrega desencadenadores a las tablas publicadas. Por lo tanto, `@@IDENTITY` puede devolver el valor de la inserción en una tabla de sistema de replicación en lugar de la inserción en una tabla de usuario.  
  
 La tabla `Person.ContactType` tiene un valor de identidad máximo de 20. Si inserta una fila en la tabla, `@@IDENTITY` y `SCOPE_IDENTITY()` devolverán el mismo valor.  
  
```sql  
USE AdventureWorks2012;  
GO  
INSERT INTO Person.ContactType ([Name]) VALUES ('Assistant to the Manager');  
GO  
SELECT SCOPE_IDENTITY() AS [SCOPE_IDENTITY];  
GO  
SELECT @@IDENTITY AS [@@IDENTITY];  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
```  
SCOPE_IDENTITY  
21  
@@IDENTITY  
21
```  
  
 La tabla `Sales.Customer` tiene un valor de identidad máximo de 29483. Si inserta una fila en la tabla, `@@IDENTITY` y `SCOPE_IDENTITY()` devolverán valores diferentes. `SCOPE_IDENTITY()` devuelve el valor de la inserción en la tabla de usuario, mientras que `@@IDENTITY` devuelve el valor de la inserción en la tabla del sistema de replicación. Use `SCOPE_IDENTITY()` para las aplicaciones que necesitan obtener acceso al valor de identidad insertado.  
  
```sql  
INSERT INTO Sales.Customer ([TerritoryID],[PersonID]) VALUES (8,NULL);  
GO  
SELECT SCOPE_IDENTITY() AS [SCOPE_IDENTITY];  
GO  
SELECT @@IDENTITY AS [@@IDENTITY];  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
 ```
 SCOPE_IDENTITY  
 29484  
 @@IDENTITY  
 89
 ```  
  
## <a name="see-also"></a>Consulte también  
 [@@IDENTITY &#40;Transact-SQL&#41;](../../t-sql/functions/identity-transact-sql.md)  
  
  
