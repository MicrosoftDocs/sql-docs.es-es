---
description: PERMISSIONS (Transact-SQL)
title: PERMISSIONS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- PERMISSIONS_TSQL
- PERMISSIONS
dev_langs:
- TSQL
helpviewer_keywords:
- permissions [SQL Server], verifying
- current permission status
- users [SQL Server], permissions status
- checking permission status
- security [SQL Server], permissions
- verifying permission status
- testing permissions
- PERMISSIONS function
ms.assetid: 81625a56-b160-4424-91c5-1ce8b259a8e6
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 2fa0effd0cd559ae6f52e422ce96684d6b9b9e0f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211451"
---
# <a name="permissions-transact-sql"></a>PERMISSIONS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve un valor que contiene un mapa de bits que indica los permisos del usuario actual sobre una instrucción, objeto o columna.  
  
 **Importante** [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Use en su lugar [fn_my_permissions](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md) y [Has_Perms_By_Name](../../t-sql/functions/has-perms-by-name-transact-sql.md). El uso continuado de la función PERMISSIONS puede producir un rendimiento más lento.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
PERMISSIONS ( [ objectid [ , 'column' ] ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *objectid*  
 Es el Id. de un elemento protegible. Si no se especifica *objectid*, el valor de mapa de bits contiene permisos de instrucción para el usuario actual; en caso contrario, contiene permisos sobre el elemento protegible para el usuario actual. El elemento protegible especificado se debe encontrar en la base de datos actual. Use la función [OBJECT_ID](../../t-sql/functions/object-id-transact-sql.md) para averiguar el valor de *objectid*.  
  
 **'** *column* **'**  
 Es el nombre opcional de la columna cuya información de permisos se devuelve. Debe ser un nombre de columna válido de la tabla especificada con *objectid*.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 **int**  
  
## <a name="remarks"></a>Observaciones  
 Se puede utilizar PERMISSIONS para determinar si el usuario actual cuenta con los permisos necesarios para ejecutar una instrucción o para otorgar, con GRANT, un permiso a otro usuario.  
  
 La información de permisos devuelta es un mapa de bits de 32 bits.  
  
 Los 16 bits inferiores reflejan permisos concedidos al usuario y también permisos que se aplican a grupos de Windows o a roles fijos de servidor de los que es miembro el usuario actual. Por ejemplo, si se devuelve el valor 66 (valor hexadecimal 0x42) cuando no se especifica *objectid*, indica que el usuario tiene permiso para ejecutar las instrucciones CREATE TABLE (valor decimal 2) y BACKUP DATABASE (valor decimal 64).  
  
 Los 16 bits superiores reflejan los permisos que el usuario puede otorgar a otros usuarios con la instrucción GRANT. Los 16 bits superiores se interpretan exactamente de la misma forma que los 16 bits inferiores descritos en las tablas siguientes, excepto en que están desplazados 16 bits hacia la izquierda (multiplicados por 65.536). Por ejemplo, 0x8 (valor decimal 8) es el bit que indica el permiso INSERT cuando se especifica *objectid*. Por su parte, 0x80000 (valor decimal 524288) indica que se puede usar el permiso GRANT INSERT, porque 524288 = 8 x 65536.  
  
 Debido a la pertenencia a roles, un usuario que no tiene permiso para ejecutar una instrucción puede conceder ese permiso a otro usuario.  
  
 En la siguiente tabla se muestran los bits usados para los permisos sobre instrucciones (no se especifica *objectid*).  
  
|Bit (dec)|Bit (hex)|Permiso de la instrucción|  
|-----------------|-----------------|--------------------------|  
|1|0x1|CREATE DATABASE (solo base de datos maestra)|  
|2|0x2|CREATE TABLE|  
|4|0x4|CREATE PROCEDURE|  
|8|0x8|CREATE VIEW|  
|16|0x10|CREATE RULE|  
|32|0x20|CREATE DEFAULT|  
|64|0x40|BACKUP DATABASE|  
|128|0x80|BACKUP LOG|  
|256|0x100|Reserved|  
  
 En la siguiente tabla se indican los bits usados para los permisos de objetos que se devuelven cuando solo se especifica *objectid*.  
  
|Bit (dec)|Bit (hex)|Permiso de la instrucción|  
|-----------------|-----------------|--------------------------|  
|1|0x1|SELECT ALL|  
|2|0x2|UPDATE ALL|  
|4|0x4|REFERENCES ALL|  
|8|0x8|INSERT|  
|16|0x10|Delete|  
|32|0x20|EXECUTE (solo procedimientos)|  
|4096|0x1000|SELECT ANY (al menos una columna)|  
|8192|0x2000|UPDATE ANY|  
|16384|0x4000|REFERENCES ANY|  
  
 En la siguiente tabla se muestran los bits usados para los permisos de objeto de nivel de columnas, devueltos cuando se especifican *objectid* y columna.  
  
|Bit (dec)|Bit (hex)|Permiso de la instrucción|  
|-----------------|-----------------|--------------------------|  
|1|0x1|SELECT|  
|2|0x2|UPDATE|  
|4|0x4|REFERENCES|  
  
 Se devuelve NULL cuando alguno de los parámetros especificados es NULL o no es válido (por ejemplo, un *objectid* o columna que no existen). Los valores de bits para permisos que no son aplicables (por ejemplo, el permiso EXECUTE, bit 0x20,) para una tabla, no están definidos.  
  
 Puede usar el operador de bits AND (&) para determinar cada bit establecido en el mapa de bits que se devuelve mediante la función PERMISSIONS.  
  
 También puede usar el procedimiento almacenado de sistema sp_helprotect para obtener una lista de permisos para un usuario de la base de datos actual.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-the-permissions-function-with-statement-permissions"></a>A. Usar la función PERMISSIONS con permisos de instrucciones  
 En el siguiente ejemplo se determina si el usuario actual puede ejecutar la instrucción `CREATE TABLE`.  
  
```sql  
IF PERMISSIONS()&2=2  
   CREATE TABLE test_table (col1 INT)  
ELSE  
   PRINT 'ERROR: The current user cannot create a table.';  
```  
  
### <a name="b-using-the-permissions-function-with-object-permissions"></a>B. Usar la función PERMISSIONS con permisos de objeto  
 En el siguiente ejemplo se determina si el usuario actual puede insertar una fila de datos en la tabla `Address` de la base de datos `AdventureWorks2012`.  
  
```sql  
IF PERMISSIONS(OBJECT_ID('AdventureWorks2012.Person.Address','U'))&8=8   
   PRINT 'The current user can insert data into Person.Address.'  
ELSE  
   PRINT 'ERROR: The current user cannot insert data into Person.Address.';  
```  
  
### <a name="c-using-the-permissions-function-with-grantable-permissions"></a>C. Usar la función PERMISSIONS con permisos que se pueden otorgar  
 En el siguiente ejemplo se determina si el usuario actual puede otorgar a otro usuario el permiso INSERT en la tabla `Address` de la base de datos `AdventureWorks2012`.  
  
```sql  
IF PERMISSIONS(OBJECT_ID('AdventureWorks2012.Person.Address','U'))&0x80000=0x80000  
   PRINT 'INSERT on Person.Address is grantable.'  
ELSE  
   PRINT 'You may not GRANT INSERT permissions on Person.Address.';  
```  
  
## <a name="see-also"></a>Consulte también  
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [OBJECT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/object-id-transact-sql.md)   
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [sp_helprotect &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helprotect-transact-sql.md)   
 [Funciones del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)  
  
  
