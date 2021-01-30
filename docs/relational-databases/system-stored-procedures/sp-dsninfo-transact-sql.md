---
description: sp_dsninfo (Transact-SQL)
title: sp_dsninfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dsninfo
- sp_dsninfo_TSQL
helpviewer_keywords:
- sp_dsninfo
ms.assetid: 34648615-814b-42bc-95a3-50e86b42ec4d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 65a3f12555d1dbb0e26702229fdcc47664cc4c82
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186970"
---
# <a name="sp_dsninfo-transact-sql"></a>sp_dsninfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información del origen de datos ODBC u OLE DB tomada del distribuidor asociado con el servidor actual. Este procedimiento almacenado se ejecuta en el distribuidor de cualquier base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_dsninfo [ @dsn =] 'dsn'   
    [ , [ @infotype =] 'info_type']   
    [ , [ @login =] 'login']   
    [ , [ @password =] 'password']  
    [ , [ @dso_type=] dso_type]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @dsn = ] 'dsn'` Es el nombre del DSN ODBC o del servidor vinculado OLE DB. *DSN* es de tipo **VARCHAR (128)** y no tiene ningún valor predeterminado.  
  
`[ @infotype = ] 'info_type'` Es el tipo de información que se va a devolver. Si no se especifica *info_type* o si se especifica null, se devuelven todos los tipos de información. *info_type* es de tipo **VARCHAR (128)**, su valor predeterminado es NULL y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**DBMS_NAME**|Especifica el nombre del proveedor del origen de datos.|  
|**DBMS_VERSION**|Especifica la versión del origen de datos.|  
|**DATABASE_NAME**|Especifica el nombre de la base de datos.|  
|**SQL_SUBSCRIBER**|Especifica que el origen de datos puede ser un suscriptor.|  
  
`[ @login = ] 'login'` Es el inicio de sesión del origen de datos. Si el origen de datos incluye un inicio de sesión, especifique NULL u omita el parámetro. *login* es de tipo **VARCHAR (128)** y su valor predeterminado es NULL.  
  
`[ @password = ] 'password'` Es la contraseña para el inicio de sesión. Si el origen de datos incluye un inicio de sesión, especifique NULL u omita el parámetro. *password* es de tipo **VARCHAR (128)** y su valor predeterminado es NULL.  
  
`[ @dso_type = ] dso_type` Es el tipo de origen de datos. *dso_type* es de **tipo int** y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1** (predeterminado)|Origen de datos ODBC|  
|**3**|Origen de datos OLE DB|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**Tipo de información**|**nvarchar (64)**|Tipos de información, como DBMS_NAME, DBMS_VERSION, DATABASE_NAME o SQL_SUBSCRIBER.|  
|**Valor**|**nvarchar(512)**|Valor del tipo de información asociado.|  
  
## <a name="remarks"></a>Observaciones  
 **sp_dsninfo** se utiliza en todos los tipos de replicación.  
  
 **sp_dsninfo** recupera la información del origen de datos ODBC o OLE DB que muestra si la base de datos se puede usar para la replicación o la consulta.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar **sp_dsninfo**.  
  
## <a name="see-also"></a>Consulte también  
 [sp_enumdsn &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-enumdsn-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
