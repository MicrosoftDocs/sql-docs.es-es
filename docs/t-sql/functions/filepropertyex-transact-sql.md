---
description: FILEPROPERTYEX (Transact-SQL)
title: FILEPROPERTYEX (Transact-SQL) | Microsoft Docs
ms.date: 07/23/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FILEPROPERTYEX_TSQL
- FILEPROPERTYEX
dev_langs:
- TSQL
helpviewer_keywords:
- viewing file properties
- displaying file properties
- file properties [SQL Server]
- FILEPROPERTYEX function
- file names [SQL Server], FILEPROPERTYEX
author: stevestein
ms.author: sstein
ms.openlocfilehash: 7335b9e78a8b0cdce4ca341a8de62254003e2e1c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209578"
---
# <a name="filepropertyex-transact-sql"></a>FILEPROPERTYEX (Transact-SQL)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Devuelve el valor de propiedad de archivo extendida especificado al especificar un nombre de archivo en la base de datos actual y un nombre de propiedad. Devuelve NULL para los archivos que no están en la base de datos actual o para las propiedades de archivo extendidas que no existen. Actualmente, las propiedades de archivo extendidas solo se aplican a las bases de datos que están en Azure Blob Storage.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
FILEPROPERTYEX ( name , property )  
```  
  
## <a name="arguments"></a>Argumentos  
 *name*  
 Es una expresión que contiene el nombre del archivo asociado a la base de datos actual de la que se va a devolver información de propiedades. *file_name* es **nchar(128)**.  
  
 *property*  
 Es una expresión que contiene el nombre de la propiedad de archivo que se va a devolver. *property* es **varchar (128)** y puede ser uno de estos valores.  


  
|Value|Descripción|
|-----------|-----------------|  
|**BlobTier**|Nivel de blob en páginas de Azure de destino. Solo se aplica a las bases de datos Standard y GeneralPurpose que usa el almacenamiento de blobs en páginas de Azure.|
|**AccountType**|Tipo de cuenta de almacenamiento que indica si se trata de almacenamiento de blobs o de almacenamiento de archivos y si es un almacenamiento Premium o estándar.|
|**IsInferredTier**|Indica si el nivel es un nivel implícito (deducido) que podría crecer con el tamaño de los datos o un nivel explícito (fijo).|
|**IsPageBlob**|Indica si el blob de destino es un blob en páginas o no.|
  
## <a name="return-types"></a>Tipos de valor devuelto  
 **sql_variant**  
  
## <a name="remarks"></a>Observaciones  
 *file_name* corresponde a la columna **name** de la vista de catálogo **sys.master_files** o **sys.database_files**.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se devuelve el valor de los archivos de base de datos:
```sql
SELECT s.file_id,
       s.type_desc,
       s.name,
       FILEPROPERTYEX(s.name, 'BlobTier') AS BlobTier,
       FILEPROPERTYEX(s.name, 'AccountType') AS AccountType,
       FILEPROPERTYEX(s.name, 'IsInferredTier') AS IsInferredTier,
       FILEPROPERTYEX(s.name, 'IsPageBlob') AS IsPageBlob
FROM sys.database_files AS s
WHERE s.type_desc IN ('ROWS', 'LOG');
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
file_id  type_desc  name  BlobTier  AccountType  IsInferredTier  IsPageBlob
--------------------------------------------------------------------------------------
1     ROWS      data_0  P30  PremiumBlobStorage  0   1
2     LOG       log     P30  PremiumBlobStorage  0   1

(2 rows affected)
```  
  
## <a name="see-also"></a>Vea también  
 [FILEGROUPPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/filegroupproperty-transact-sql.md)   
 [Funciones de metadatos &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [sp_spaceused &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.master_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
  
  
