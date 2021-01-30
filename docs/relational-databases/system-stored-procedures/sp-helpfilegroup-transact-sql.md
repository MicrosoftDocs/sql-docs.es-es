---
description: sp_helpfilegroup (Transact-SQL)
title: sp_helpfilegroup (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpfilegroup_TSQL
- sp_helpfilegroup
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpfilegroup
ms.assetid: 619716b5-95dc-4538-82ae-4b90b9da8ebc
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 39899feae9ad7b4cd6f2ab45caf11ddd3e89cd98
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204411"
---
# <a name="sp_helpfilegroup-transact-sql"></a>sp_helpfilegroup (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve los nombres y los atributos de los grupos de archivos asociados con la base de datos actual.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helpfilegroup [ [ @filegroupname = ] 'name' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @filegroupname = ] 'name'` Es el nombre lógico de cualquier grupo de archivos de la base de datos actual. *Name* es de **tipo sysname y su** valor predeterminado es NULL. Si no se especifica *Name* , se muestran todos los grupos de archivos de la base de datos actual y solo se muestra el primer conjunto de resultados que se muestra en la sección conjuntos de resultados.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**GroupName**|**sysname**|Nombre del grupo de archivos.|  
|**GROUPID**|**smallint**|Identificador numérico del grupo de archivos.|  
|**FileCount**|**int**|Número de archivos del grupo de archivos.|  
  
 Si se especifica *Name* , se devuelve una fila por cada archivo del grupo de archivos.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**file_in_group**|**sysname**|Nombre lógico del campo en el grupo de archivos.|  
|**ID**|**smallint**|Identificador numérico del archivo.|  
|**filename**|**NCHAR (260)**|Nombre físico del archivo, incluida la ruta de acceso del directorio.|  
|**size**|**nvarchar(15**|Tamaño del archivo en kilobytes.|  
|**tamañomáximo**|**nvarchar(15**|Tamaño máximo del archivo.<br /><br /> Es el tamaño máximo que puede alcanzar el archivo. El valor UNLIMITED en este campo indica que el archivo puede aumentar hasta que el disco esté lleno.|  
|**crezca**|**nvarchar(15**|Incremento de crecimiento del archivo. Indica la cantidad de espacio que se agrega al archivo cada vez que se necesita espacio.<br /><br /> 0 = El archivo tiene un tamaño fijo y no aumenta.|  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** .  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-returning-all-filegroups-in-a-database"></a>A. Devolver todos los grupos de archivos de una base de datos  
 En el siguiente ejemplo se devuelve información sobre los grupos de archivos de la base de datos de ejemplo [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_helpfilegroup;  
GO  
```  
  
### <a name="b-returning-all-files-in-a-filegroup"></a>B. Devolver todos los archivos de un grupo de archivos  
 En el siguiente ejemplo se devuelve información para todos los archivos del grupo de archivos `PRIMARY` de la base de datos de ejemplo [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_helpfilegroup 'PRIMARY';  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Motor de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sp_helpfile &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpfile-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.master_files &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)   
 [sys.filegroups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Archivos y grupos de archivos de base de datos](../../relational-databases/databases/database-files-and-filegroups.md)  
  
  
