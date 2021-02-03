---
description: FILEGROUPPROPERTY (Transact-SQL)
title: FILEGROUPPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FILEGROUPPROPERTY_TSQL
- FILEGROUPPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- filegroups [SQL Server], property values
- FILEGROUPPROPERTY function
- viewing filegroup properties
- displaying filegroup properties
ms.assetid: b3a930e6-df05-4034-929c-f681f5f6fc6e
author: cawrites
ms.author: chadam
ms.openlocfilehash: cdc8e66d73e7296d269edd420fde4709c11d41f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161866"
---
# <a name="filegroupproperty-transact-sql"></a>FILEGROUPPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Esta función devuelve el valor de propiedad de grupo de archivos de un valor de grupo de archivos y un nombre especificados.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
FILEGROUPPROPERTY ( filegroup_name, property )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *filegroup_name*  
Una expresión de tipo **sysname** que representa el nombre del grupo de archivos del que `FILEGROUPPROPERTY` devuelve información de una propiedad con nombre.  
  
 *property*  
Una expresión de tipo **varchar(128)** que devuelve el nombre de la propiedad del grupo de archivos. *Property* puede devolver uno de estos valores:  
  
|Value|Descripción|Valor devuelto|  
|-----------|-----------------|--------------------|  
|**IsReadOnly**|El grupo de archivos es de solo lectura.|1 = TRUE<br /><br /> 0 = False<br /><br /> NULL = entrada no válida.|  
|**IsUserDefinedFG**|El grupo de archivos es un grupo de archivos definido por el usuario.|1 = TRUE<br /><br /> 0 = False<br /><br /> NULL = entrada no válida.|  
|**IsDefault**|El grupo de archivos es el grupo de archivos predeterminado.|1 = TRUE<br /><br /> 0 = False<br /><br /> NULL = entrada no válida.|  
  
## <a name="return-types"></a>Tipos de valor devuelto  
**int**  
  
## <a name="remarks"></a>Observaciones  
*filegroup_name* corresponde a la columna **name** de la vista de catálogo **sys.filegroups**.  
  
## <a name="examples"></a>Ejemplos  
En este ejemplo se devuelve el valor de la propiedad `IsDefault` del grupo de archivos principal de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
SELECT FILEGROUPPROPERTY('PRIMARY', 'IsDefault') AS 'Default Filegroup';  
GO  
```  

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]   
```  
Default Filegroup   
---------------------   
1  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Consulte también  
 [FILEGROUP_ID &#40;Transact-SQL&#41;](../../t-sql/functions/filegroup-id-transact-sql.md)   
 [FILEGROUP_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/filegroup-name-transact-sql.md)   
 [Funciones de metadatos &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [sys.filegroups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)  
  
  
