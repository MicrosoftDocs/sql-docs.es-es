---
description: IS_OBJECTSIGNED (Transact-SQL)
title: IS_OBJECTSIGNED (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IS_OBJECTSIGNED
- IS_OBJECTSIGNED_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- IS_OBJECTSIGNED function
ms.assetid: afbc4f7f-8266-4ee6-9802-14a2dbe69ef6
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: e33459d72484363319449d1baba953e0165d6407
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165112"
---
# <a name="is_objectsigned-transact-sql"></a>IS_OBJECTSIGNED (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Indica si un objeto está firmado por un certificado o clave asimétrica especificados.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
IS_OBJECTSIGNED (   
'OBJECT', @object_id, @class, @thumbprint  
  )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 **'OBJECT'**  
 Tipo de la clase protegible.  
  
 *\@object_id*  
 Valor object_id del objeto que se va a probar. *\@object_id* is es de tipo **int**.  
  
 *\@class*  
 Clase del objeto:  
  
-   'certificado'  
  
-   'clave asimétrica'  
  
 *\@class* es **sysname**.  
  
 *\@thumbprint*  
 Huella digital SHA del objeto. *\@thumbprint* es de tipo **varbinary(32)** .  
  
## <a name="returned-types"></a>Tipos devueltos  
 **int**  
  
## <a name="remarks"></a>Observaciones  
 IS_OBJECTSIGNED devuelve los siguientes valores.  
  
|Valor devuelto|Descripción|  
|------------------|-----------------|  
|NULL|El objeto no está firmado o no es válido.|  
|0|El objeto está firmado, pero la firma no es válida.|  
|1|El objeto está firmado.|  
  
## <a name="permissions"></a>Permisos  
 Se requiere VIEW DEFINITION sobre el certificado o la clave asimétrica.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-displaying-extended-properties-on-a-database"></a>A. Mostrar las propiedades extendidas de una base de datos  
 En el ejemplo siguiente se prueba si la tabla spt_fallback_db de la base de datos **master** está firmada por el certificado de firma de esquema.  
  
```sql  
USE master;  
-- Declare a variable to hold a thumbprint and an object name  
DECLARE @thumbprint varbinary(20), @objectname sysname;  
  
-- Populate the thumbprint variable with the thumbprint of   
-- the master database schema signing certificate  
SELECT @thumbprint = thumbprint   
FROM sys.certificates   
WHERE name LIKE '%SchemaSigningCertificate%';  
  
-- Populate the object name variable with a table name in master  
SELECT @objectname = 'spt_fallback_db';  
  
-- Query to see if the table is signed by the thumbprint  
SELECT @objectname AS [object name],  
IS_OBJECTSIGNED(  
'OBJECT', OBJECT_ID(@objectname), 'certificate', @thumbprint  
) AS [Is the object signed?] ;  
```  
  
## <a name="see-also"></a>Consulte también  
 [sys.fn_check_object_signatures &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-check-object-signatures-transact-sql.md)  
  
  
