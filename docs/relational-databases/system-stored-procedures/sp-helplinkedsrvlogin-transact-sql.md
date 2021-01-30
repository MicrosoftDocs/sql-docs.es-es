---
description: sp_helplinkedsrvlogin (Transact-SQL)
title: sp_helplinkedsrvlogin (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helplinkedsrvlogin_TSQL
- sp_helplinkedsrvlogin
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helplinkedsrvlogin
ms.assetid: a2b1eba0-bf71-47e7-a4c7-9f55feec82a3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 274d1d9a39feac333d5b7d572ec475bb89444c2b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190016"
---
# <a name="sp_helplinkedsrvlogin-transact-sql"></a>sp_helplinkedsrvlogin (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Proporciona información acerca de las asignaciones de inicio de sesión definidas para un servidor vinculado específico que se utiliza para consultas distribuidas y para llamadas a procedimientos almacenados remotos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helplinkedsrvlogin [ [ @rmtsrvname = ] 'rmtsrvname' ]   
     [ , [ @locallogin = ] 'locallogin' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @rmtsrvname = ] 'rmtsrvname'` Es el nombre del servidor vinculado al que se aplica la asignación de inicio de sesión. *rmtsrvname* es de **tipo sysname y su** valor predeterminado es NULL. Si es NULL, se presentan todas las asignaciones de inicio de sesión establecidas con todos los servidores vinculados definidos en el equipo local que ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
`[ @locallogin = ] 'locallogin'` Es el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Inicio de sesión en el servidor local que tiene una asignación al servidor vinculado *rmtsrvname*. *locallogin* es de **tipo sysname y su** valor predeterminado es NULL. NULL especifica que se devuelven todas las asignaciones de inicio de sesión definidas en *rmtsrvname* . Si no es NULL, ya debe existir una asignación para *locallogin* a *rmtsrvname* . *locallogin* puede ser un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Inicio de sesión de o un usuario de Windows. El usuario de Windows tiene que haber recibido acceso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] directamente o a través de su pertenencia a un grupo de Windows al que se haya concedido acceso.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**Servidor vinculado**|**sysname**|Nombre del servidor vinculado.|  
|**Inicio de sesión local**|**sysname**|Inicio de sesión local al que se aplica la asignación.|  
|**Is Self Mapping**|**smallint**|0 = el **Inicio de sesión local** se asigna a **sesión remota** al conectarse al **servidor vinculado**.<br /><br /> 1 = el **Inicio de sesión local** se asigna al mismo inicio de sesión y contraseña cuando se conecta al **servidor vinculado**.|  
|**Remote Login**|**sysname**|Nombre de inicio de sesión en **LinkedServer** que se asigna a **LocalLogin** cuando **IsSelfMapping** es 0. Si **IsSelfMapping** es 1, **RemoteLogin** es NULL.|  
  
## <a name="remarks"></a>Observaciones  
 Antes de eliminar las asignaciones de inicio de sesión, utilice **sp_helplinkedsrvlogin** para determinar los servidores vinculados que están implicados.  
  
## <a name="permissions"></a>Permisos  
 No se comprueba ningún permiso.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-displaying-all-login-mappings-for-all-linked-servers"></a>A. Mostrar todas las asignaciones de inicio de sesión de todos los servidores vinculados  
 En el siguiente ejemplo se muestran todas las asignaciones de inicio de sesión de todos los servidores vinculados definidos en el equipo local que ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```  
EXEC sp_helplinkedsrvlogin;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Linked Server    Local Login   Is Self Mapping Remote Login   
---------------- ------------- --------------- --------------   
Accounts         NULL          1               NULL  
Sales            NULL          1               NULL  
Sales            Mary          0               sa  
Marketing        NULL          1               NULL  
  
(4 row(s) affected)  
```  
  
### <a name="b-displaying-all-login-mappings-for-a-linked-server"></a>B. Mostrar todas las asignaciones de inicio de sesión de un servidor vinculado  
 En el siguiente ejemplo se muestran todas las asignaciones de inicio de sesión definidas localmente para el servidor vinculado `Sales`.  
  
```  
EXEC sp_helplinkedsrvlogin 'Sales';  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Linked Server    Local Login   Is Self Mapping Remote Login   
---------------- ------------- --------------- --------------   
Sales            NULL          1               NULL  
Sales            Mary          0               sa  
  
(2 row(s) affected)  
```  
  
### <a name="c-displaying-all-login-mappings-for-a-local-login"></a>C. Mostrar todas las asignaciones de inicio de sesión de un inicio de sesión local  
 En el siguiente ejemplo se muestran todas las asignaciones de inicio de sesión definidas localmente para el inicio de sesión `Mary`.  
  
```  
EXEC sp_helplinkedsrvlogin NULL, 'Mary';  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Linked Server    Local Login   Is Self Mapping Remote Login   
---------------- ------------- --------------- --------------   
Sales            NULL          1               NULL  
Sales            Mary          0               sa  
  
(2 row(s) affected)  
```  
  
## <a name="see-also"></a>Vea también  
 [Procedimientos almacenados de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)   
 [sp_droplinkedsrvlogin &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-droplinkedsrvlogin-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
