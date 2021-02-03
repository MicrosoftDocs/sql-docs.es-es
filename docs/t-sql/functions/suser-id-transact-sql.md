---
description: SUSER_ID (Transact-SQL)
title: SUSER_ID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/07/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SUSER_ID_TSQL
- SUSER_ID
dev_langs:
- TSQL
helpviewer_keywords:
- users [SQL Server], IDs
- logins [SQL Server], IDs
- SUSER_ID function
- IDs [SQL Server], logins
- identification numbers [SQL Server], logins
- user IDs [SQL Server]
ms.assetid: 348911ab-b0b6-4867-aee7-e6f42e053a4a
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: c8d76d0a44678a7a43bd365976d8be352d58bf16
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181481"
---
# <a name="suser_id-transact-sql"></a>SUSER_ID (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Devuelve el número de identificación de inicio de sesión del usuario.  
  
> [!NOTE]  
>  A partir de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], SUSER_ID devuelve el valor incluido como **principal_id** en la vista de catálogo **sys.server_principals**.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
SUSER_ID ( [ 'login' ] )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 **'** *login* **'**  
 Nombre de inicio de sesión del usuario. *login* es **nchar**. Si se especifica *login* como **char**, *login* se convierte implícitamente en **nchar**. *login* puede ser cualquier inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o cualquier grupo o usuario de Windows con permiso para conectarse con una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si no se especifica *login*, se devuelve el número de identificación de inicio de sesión para el usuario actual. Si el parámetro contiene la palabra NULL, se devolverá NULL.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 **int**  
  
## <a name="remarks"></a>Observaciones  
 SUSER_ID devuelve un número de identificación solo para los inicios de sesión aprovisionados de forma explícita en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Este Id. se utiliza en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para realizar un seguimiento de la propiedad y los permisos. Este Id. no equivale al SID del inicio de sesión devuelto por SUSER_SID. Si *login* es un inicio de sesión de SQL Server, el SID se asigna a un GUID. Si *login* es un inicio de sesión o un grupo de Windows, el SID se asigna a un identificador de seguridad de Windows.  
  
 SUSER_SID solo devuelve el SUID de los inicios de sesión que tengan una entrada en la tabla de sistema **syslogins**.  
  
 Es posible utilizar funciones de sistema en la lista de selección, en la cláusula WHERE y en cualquier lugar donde se admita una expresión, pero deberán ir seguidas siempre de paréntesis incluso si no se especifica ningún parámetro.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se obtiene el número de identificación del nombre de inicio de sesión `sa`.  
  
```sql
SELECT SUSER_ID('sa');  
```  
  
## <a name="see-also"></a>Consulte también  
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [SUSER_SID &#40;Transact-SQL&#41;](../../t-sql/functions/suser-sid-transact-sql.md)   
 [Funciones del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)  
  
  
