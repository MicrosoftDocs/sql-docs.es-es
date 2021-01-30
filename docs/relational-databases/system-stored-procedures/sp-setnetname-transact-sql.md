---
description: sp_setnetname (Transact-SQL)
title: sp_setnetname (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_setnetname
- sp_setnetname_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_setnetname
ms.assetid: f416ba81-3835-4588-b0a3-2fe75589490e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f84120579d3eae3b2755eaab89eff137af6190db
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193496"
---
# <a name="sp_setnetname-transact-sql"></a>sp_setnetname (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Establece los nombres de red de **Sys. Servers** en sus nombres de equipo de red reales para las instancias remotas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Este procedimiento puede utilizarse para habilitar la ejecución de llamadas a procedimientos almacenados remotos a equipos cuyos nombres de red contienen identificadores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no válidos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_setnetname  
@server = 'server',   
     @netname = 'network_name'  
```  
  
## <a name="arguments"></a>Argumentos  
 **@server = '** *servidor* **'**  
 Es el nombre del servidor remoto tal como se ha especificado en la sintaxis de llamada a procedimiento almacenado remoto codificada por el usuario. Ya debe existir exactamente una fila en **Sys.** servers para usar este *servidor*. *server* es de tipo **sysname** y no tiene ningún valor predeterminado.  
  
 **@netname = '** *network_name* **'**  
 Es el nombre de red del equipo en el que se realizan las llamadas a procedimiento almacenado remoto. *network_name* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
 Este nombre debe coincidir con el nombre del equipo con [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows y puede incluir caracteres que no estén admitidos en los identificadores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 Algunas llamadas a procedimientos almacenados remotos a equipos que ejecutan Windows pueden experimentar problemas si el nombre del equipo contiene identificadores no válidos.  
  
 Debido a que los servidores vinculados y los servidores remotos residen en el mismo espacio de nombres, no pueden tener el mismo nombre. Sin embargo, puede definir un servidor vinculado y un servidor remoto en un servidor especificado asignando nombres diferentes y usando **sp_setnetname** para establecer el nombre de red de uno de ellos en el nombre de red del servidor subyacente.  
  
```  
--Assume sqlserv2 is actual name of SQL Server   
--database server  
EXEC sp_addlinkedserver 'sqlserv2';  
GO  
EXEC sp_addserver 'rpcserv2';  
GO  
EXEC sp_setnetname 'rpcserv2', 'sqlserv2';  
```  
  
> [!NOTE]  
>  No se admite el uso de **sp_setnetname** para volver a apuntar un servidor vinculado al servidor local. Los servidores a los que se hace referencia de esta forma no pueden participar en una transacción distribuida.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia a los roles fijos de servidor **sysadmin** y **setupadmin** .  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se muestra una secuencia administrativa habitual que se utiliza en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para emitir la llamada a procedimiento almacenado remoto.  
  
```  
USE master;  
GO  
EXEC sp_addserver 'Win_1';  
EXEC sp_setnetname 'Win_1','Win-1';  
EXEC Win_1.master.dbo.sp_who;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Motor de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)   
 [sp_addserver &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
