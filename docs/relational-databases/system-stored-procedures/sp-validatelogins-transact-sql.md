---
description: sp_validatelogins (Transact-SQL)
title: sp_validatelogins (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_validatelogins
- sp_validatelogins_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_validatelogins
ms.assetid: 6ac52e21-e20d-469b-ad40-5aa091e06b61
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 46ed2b0577918630cc9c938047b740618eda45a3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201843"
---
# <a name="sp_validatelogins-transact-sql"></a>sp_validatelogins (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ofrece información acerca de los usuarios y grupos de Windows asignados a entidades de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que ya no existen en el entorno de Windows.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_validatelogins  
```  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**SID**|**varbinary(85)**|Identificador de seguridad (SID) de Windows del usuario o grupo de Windows.|  
|**NT Login**|**sysname**|Nombre del usuario o grupo de Windows.|  
  
## <a name="remarks"></a>Observaciones  
 Si la entidad de seguridad a nivel de servidor huérfana es propietaria de un usuario de base de datos, es necesario quitar el usuario de la base de datos antes de quitar la entidad de seguridad de servidor huérfana. Para quitar un usuario de base de datos, utilice [Drop User](../../t-sql/statements/drop-user-transact-sql.md). Si la entidad de seguridad a nivel de servidor es propietaria de elementos protegibles en la base de datos, la propiedad de estos elementos debe transferirse o quitarse. Para transferir la propiedad de los elementos protegibles de base de datos, use [ALTER Authorization](../../t-sql/statements/alter-authorization-transact-sql.md).  
  
 Para quitar las asignaciones a los usuarios y grupos de Windows que ya no existen, utilice [Drop login](../../t-sql/statements/drop-login-transact-sql.md).  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de servidor **sysadmin** o **securityadmin** .  
  
## <a name="examples"></a>Ejemplos  
 En este ejemplo se muestran los usuarios y los grupos de Windows que ya no existen pero que siguen disponiendo de acceso a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```  
EXEC sp_validatelogins;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procedimientos almacenados de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [DROP USER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-user-transact-sql.md)   
 [DROP LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/drop-login-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)  
  
  
