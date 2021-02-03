---
description: FILEPROPERTY (Transact-SQL)
title: FILEPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FILEPROPERTY_TSQL
- FILEPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- viewing file properties
- names [SQL Server], files
- displaying file properties
- file properties [SQL Server]
- FILEPROPERTY function
- file names [SQL Server], FILEPROPERTY
ms.assetid: b82244ed-d623-431f-aa06-8017349d847f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0cbbef3b704a8ec174fd9c3e5c1cbf5938a5a4ca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183528"
---
# <a name="fileproperty-transact-sql"></a>FILEPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve el valor de una propiedad de nombre de archivo especificada al especificar un nombre de archivo en la base de datos actual y un nombre de propiedad. Devuelve NULL para los archivos que no estén en la base de datos actual.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
FILEPROPERTY ( file_name , property )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *file_name*  
 Es una expresión que contiene el nombre del archivo asociado a la base de datos actual de la que se va a devolver información de propiedades. *file_name* es **nchar(128)**.  
  
 *property*  
 Es una expresión que contiene el nombre de la propiedad de archivo que se va a devolver. *property* es **varchar (128)** y puede ser uno de estos valores.  
  
|Value|Descripción|Valor devuelto|  
|-----------|-----------------|--------------------|  
|**IsReadOnly**|El grupo de archivos es de solo lectura.|1 = True<br /><br /> 0 = False<br /><br /> NULL = La entrada no es válida.|  
|**IsPrimaryFile**|El archivo es el archivo principal.|1 = True<br /><br /> 0 = False<br /><br /> NULL = La entrada no es válida.|  
|**IsLogFile**|El archivo es un archivo de registro.|1 = True<br /><br /> 0 = False<br /><br /> NULL = La entrada no es válida.|  
|**SpaceUsed**|Cantidad de espacio utilizada por el archivo especificado.|Número de páginas asignadas en el archivo.|  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 **int**  
  
## <a name="remarks"></a>Observaciones  
 *file_name* corresponde a la columna **name** de la vista de catálogo **sys.master_files** o **sys.database_files**.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se devuelve el valor de la propiedad `IsPrimaryFile` del nombre de archivo `AdventureWorks_Data` de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql
SELECT FILEPROPERTY('AdventureWorks2012_Data', 'IsPrimaryFile')AS [Primary File];  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Primary File   
-------------  
1  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Vea también  
 [FILEGROUPPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/filegroupproperty-transact-sql.md)   
 [Funciones de metadatos &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [sp_spaceused &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.master_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
  
  
