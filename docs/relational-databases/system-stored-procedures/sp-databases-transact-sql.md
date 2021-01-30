---
description: sp_databases (Transact-SQL)
title: sp_databases (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_databases_TSQL
- sp_databases
dev_langs:
- TSQL
helpviewer_keywords:
- sp_databases
ms.assetid: 2a83b92a-9ecc-43c4-8ff4-e91e3a940b5a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e324cb4660302517daee36072e9b835abc455baf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200325"
---
# <a name="sp_databases-transact-sql"></a>sp_databases (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enumera las bases de datos que residen en una instancia del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o que están accesibles a través de una puerta de enlace de la base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_databases  
```  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 None  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**DATABASE_NAME**|**sysname**|Nombre de la base de datos. En [!INCLUDE[ssDE](../../includes/ssde-md.md)] , esta columna representa el nombre de la base de datos tal y como se almacena en la vista de catálogo **Sys. Databases** .|  
|**DATABASE_SIZE**|**int**|Tamaño de la base de datos, en kilobytes.|  
|**COMENTARIOS**|**VARCHAR (254)**|Para el [!INCLUDE[ssDE](../../includes/ssde-md.md)], este campo siempre devuelve NULL.|  
  
## <a name="remarks"></a>Observaciones  
 Los nombres de bases de datos devueltos pueden utilizarse como parámetros en la instrucción USE para cambiar el contexto de la base de datos actual.  
  
 **sp_databases** no tiene ningún equivalente en Conectividad abierta de bases de datos (ODBC).  
  
## <a name="permissions"></a>Permisos  
 Requiere permiso CREATE DATABASE, ALTER ANY DATABASE o VIEW ANY DEFINITION y debe tener permiso de acceso a la base de datos. No se le puede denegar el permiso VIEW ANY DEFINITION.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se muestra la ejecución de `sp_databases`.  
  
```sql  
USE master;  
GO  
EXEC sp_databases;  
```  
  
## <a name="see-also"></a>Consulte también  
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [HAS_DBACCESS &#40;&#41;de Transact-SQL ](../../t-sql/functions/has-dbaccess-transact-sql.md)  
  
  
