---
description: LOGINPROPERTY (Transact-SQL)
title: LOGINPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- BadPasswordCount_TSQL
- BadPasswordTime_TSQL
- IsLockedIsMustChange
- PasswordLastSetTime
- LOGINPROPERTY_TSQL
- LockoutTime
- BadPasswordCount
- PasswordHash
- HistoryLengthIsExpired
- LockoutTime_TSQL
- PasswordHash_TSQL
- HistoryLengthIsExpired_TSQL
- PasswordLastSetTime_TSQL
- BadPasswordTime
- IsLockedIsMustChange_TSQL
- LOGINPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- default database
- LOGINPROPERTY function
ms.assetid: b34df777-79b0-49a5-88db-b99998479a5d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: bb6436b8785a6e550e0011635d3c2e1b80c98c84
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193158"
---
# <a name="loginproperty-transact-sql"></a>LOGINPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información sobre la configuración de la directiva de inicio de sesión.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
LOGINPROPERTY ( 'login_name' , 'property_name' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *login_name*  
 Es el nombre de un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para el que se devolverá el estado de la propiedad login.  
  
 *propertyname*  
 Es una expresión que contiene la información de propiedad que se va a devolver para el inicio de sesión. *propertyname* puede ser uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**BadPasswordCount**|Devuelve el número de intentos consecutivos de inicio de sesión con una contraseña incorrecta.|  
|**BadPasswordTime**|Devuelve la hora del último intento de inicio de sesión con una contraseña incorrecta.|  
|**DaysUntilExpiration**|Devuelve el número de días que faltan para que expire la contraseña.|  
|**DefaultDatabase**|Devuelve la base de datos predeterminada de inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tal y como se almacena en los metadatos, o **master** si no se ha especificado ninguna base de datos. Devuelve NULL para los usuarios que no se han autenticado a través de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (como los usuarios autenticados a través de Windows).|  
|**DefaultLanguage**|Devuelve el idioma predeterminado de inicio de sesión tal y como se almacena en los metadatos. Devuelve NULL para los usuarios que no se han autenticado a través de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (como los usuarios autenticados a través de Windows).|  
|**HistoryLength**|Devuelve el número de contraseñas comprobadas para el inicio de sesión con el mecanismo de imposición de la directiva de contraseñas. 0 si la directiva de contraseñas no se exige. La exigencia de aplicar la directiva de contraseñas se vuelve a iniciar con 1.|  
|**IsExpired**|Indica si el inicio de sesión ha expirado.|  
|**IsLocked**|Indica si el inicio de sesión está bloqueado.|  
|**IsMustChange**|Indica si el inicio de sesión debe cambiar la contraseña la próxima vez que se conecte.|  
|**LockoutTime**|Devuelve la fecha en la que se bloqueó el inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] por haber superado el número permitido de intentos de inicio de sesión erróneos.|  
|**PasswordHash**|Devuelve el hash de la contraseña.|  
|**PasswordLastSetTime**|Devuelve la fecha en la que se estableció la contraseña actual.|  
|**PasswordHashAlgorithm**|Devuelve el algoritmo de hash utilizado para la contraseña.|  
  
## <a name="returns"></a>Devoluciones  
 El tipo de datos depende del valor solicitado.  
  
 **IsLocked**, **IsExpired** e **IsMustChange** son de tipo **int**.  
  
-   1 si el inicio de sesión está en el estado especificado.  
  
-   0 si el inicio de sesión está en el estado especificado.  
  
 **BadPasswordCount** e **HistoryLength** son de tipo **int**.  
  
 **BadPasswordTime**, **LockoutTime**, **PasswordLastSetTime** son de tipo **datetime**.  
  
 **PasswordHash** es de tipo **varbinary**.  
  
 NULL si el inicio de sesión no es un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] válido.  
  
 **DaysUntilExpiration** es de tipo **int**.  
  
-   0 si el inicio de sesión está expirado o si expirará en el día en que se hace la consulta.  
  
-   -1 si la directiva de seguridad local de Windows determina que la contraseña nunca expira.  
  
-   NULL si se ha desactivado CHECK_POLICY o CHECK_EXPIRATION para un inicio de sesión, o si el sistema operativo no admite la directiva de contraseñas.  
  
 **PasswordHashAlgorithm** es de tipo int.  
  
-   0 si es un hash SQL7.0  
  
-   1 si es un hash SHA-1  
  
-   2 si es un hash SHA-2  
  
-   NULL si el inicio de sesión no es un inicio de sesión de SQL Server válido.  
  
## <a name="remarks"></a>Comentarios  
 Esta función integrada devuelve información sobre la configuración de la directiva de contraseñas de un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los nombres de las propiedades no distinguen mayúsculas de minúsculas, por lo que nombres de propiedades como **BadPasswordCount** y **badpasswordcount** son equivalentes. Los valores de las propiedades **PasswordHash, PasswordHashAlgorithm** y **PasswordLastSetTime** están disponibles en todas las configuraciones compatibles de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], pero las demás propiedades solo están disponibles cuando se ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en [!INCLUDE[winxpsvr](../../includes/winxpsvr-md.md)] y tanto CHECK_POLICY como CHECK_EXPIRATION están habilitadas. Para obtener más información, vea [Password Policy](../../relational-databases/security/password-policy.md).  
  
## <a name="permissions"></a>Permisos  
 Requiere permiso VIEW en el inicio de sesión. También se necesita permiso CONTROL SERVER para solicitar el hash de contraseña.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-checking-whether-a-login-must-change-its-password"></a>A. Comprobar si se debe cambiar la contraseña de un inicio de sesión  
 En el ejemplo siguiente se comprueba si el inicio de sesión `John3` de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe cambiar la contraseña la próxima vez que se conecte a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql  
SELECT LOGINPROPERTY('John3', 'IsMustChange');  
GO  
```  
  
### <a name="b-checking-whether-a-login-is-locked-out"></a>B. Comprobar si un inicio de sesión está bloqueado  
 En el ejemplo siguiente se comprueba si el inicio de sesión `John3` de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] está bloqueado.  
  
```sql  
SELECT LOGINPROPERTY('John3', 'IsLocked');  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)  
  
  
