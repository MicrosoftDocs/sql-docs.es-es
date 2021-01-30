---
description: sp_helptrigger (Transact-SQL)
title: sp_helptrigger (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helptrigger
- sp_helptrigger_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helptrigger
ms.assetid: e486d39b-771d-488d-a786-7136433a2203
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8de5ac41fcd22dc721df5c5fc9c56455910157f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204414"
---
# <a name="sp_helptrigger-transact-sql"></a>sp_helptrigger (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve el tipo o los tipos de desencadenadores DML definidos en la tabla especificada de la base de datos actual. no se puede usar sp_helptrigger con desencadenadores DDL. Consulte en su lugar la vista de catálogo [procedimientos almacenados del sistema](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md) .  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helptrigger [ @tabname = ] 'table'   
     [ , [ @triggertype = ] 'type' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @tabname = ] 'table'` Es el nombre de la tabla de la base de datos actual para la que se va a devolver información del desencadenador. *TABLE* es de tipo **nvarchar (776)** y no tiene ningún valor predeterminado.  
  
`[ @triggertype = ] 'type'` Es el tipo de desencadenador DML sobre el que se va a devolver información. el *tipo* es **Char (6)**, su valor predeterminado es NULL y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**DELETE**|Devuelve información de desencadenadores DELETE.|  
|**INSERT**|Devuelve información de desencadenadores INSERT.|  
|**UPDATE**|Devuelve información de desencadenadores UPDATE.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 La siguiente tabla muestra la información del conjunto de resultados.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**trigger_name**|**sysname**|Nombre del desencadenador.|  
|**trigger_owner**|**sysname**|Nombre del propietario de la tabla donde se definió el desencadenador.|  
|**isupdate**|**int**|1=Desencadenador UPDATE<br /><br /> 0=No es un desencadenador UPDATE|  
|**isdelete**|**int**|1=Desencadenador DELETE<br /><br /> 0=No es un desencadenador DELETE|  
|**isinsert**|**int**|1=Desencadenador INSERT<br /><br /> 0=No es un desencadenador INSERT|  
|**isafter**|**int**|1=Desencadenador AFTER<br /><br /> 0=No es un desencadenador AFTER|  
|**isinsteadof**|**int**|1=Desencadenador INSTEAD OF<br /><br /> 0=No es un desencadenador INSTEAD OF|  
|**trigger_schema**|**sysname**|Nombre del esquema al que pertenece el desencadenador.|  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso de configuración de visibilidad de los [metadatos](../../relational-databases/security/metadata-visibility-configuration.md) en la tabla.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se ejecuta `sp_helptrigger` para producir información sobre el o los desencadenadores de la tabla `Person.Person`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_helptrigger 'Person.Person';  
```  
  
## <a name="see-also"></a>Consulte también  
 [Motor de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [DROP TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-trigger-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
