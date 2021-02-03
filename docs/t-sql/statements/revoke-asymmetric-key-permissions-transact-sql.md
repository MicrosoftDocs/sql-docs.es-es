---
description: REVOKE (permisos de clave asimétrica de Transact-SQL)
title: REVOKE (permisos de clave asimétrica de Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- permissions [SQL Server], asymmetric keys
- asymmetric keys [SQL Server], permissions
- REVOKE statement, asymmetric keys
ms.assetid: 1a1063e8-ffc7-4775-a40d-e155740ad7b2
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 3ab78d858d084a139e3932dab30990e495f6717c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209943"
---
# <a name="revoke-asymmetric-key-permissions-transact-sql"></a>REVOKE (permisos de clave asimétrica de Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Revoca los permisos de una clave asimétrica.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
  
REVOKE [ GRANT OPTION FOR ] { permission  [ ,...n ] }   
    ON ASYMMETRIC KEY :: asymmetric_key_name   
    { TO | FROM } database_principal [ ,...n ]  
    [ CASCADE ]  
    [ AS revoking_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 GRANT OPTION FOR  
 Indica que se revocará la capacidad de conceder el permiso especificado.  
  
> [!IMPORTANT]  
>  Si la entidad de seguridad dispone del permiso especificado sin la opción GRANT, se revocará el permiso.  
  
 *permission*  
 Especifica un permiso que se puede revocar para un ensamblado. Se muestra a continuación.  
  
 ON ASYMMETRIC KEY **::** _asymmetric_key_name_  
 Especifica la clave asimétrica en la que se va a revocar el permiso. El calificador de ámbito **::** es obligatorio.  
  
 *database_principal*  
 Especifica la entidad de seguridad desde la que se revoca el permiso. Uno de los siguientes:  
  
-   Usuario de la base de datos  
  
-   Rol de base de datos  
  
-   Rol de aplicación  
  
-   Usuario de la base de datos asignado a un inicio de sesión de Windows  
  
-   Usuario de la base de datos asignado a un grupo de Windows  
  
-   Usuario de la base de datos asignado a un certificado  
  
-   Usuario de la base de datos asignado a una clave asimétrica  
  
-   Usuario de la base de datos no asignado a una entidad de seguridad de servidor.  
  
 CASCADE  
 Indica que el permiso que se va a revocar también se revocará de otras entidades de seguridad a las que esta entidad de seguridad ha concedido o denegado permisos. No se revocará el permiso.  
  
> [!CAUTION]  
>  Una revocación en cascada de un permiso concedido WITH GRANT OPTION revocará tanto GRANT como DENY de dicho permiso.  
  
 AS *revoking_principal*  
 Especifica una entidad de seguridad de la que la entidad de seguridad que ejecuta esta consulta deriva su derecho de revocar el permiso. Uno de los siguientes:  
  
-   Usuario de la base de datos  
  
-   Rol de base de datos  
  
-   Rol de aplicación  
  
-   Usuario de la base de datos asignado a un inicio de sesión de Windows  
  
-   Usuario de la base de datos asignado a un grupo de Windows  
  
-   Usuario de la base de datos asignado a un certificado  
  
-   Usuario de la base de datos asignado a una clave asimétrica  
  
-   Usuario de la base de datos no asignado a una entidad de seguridad de servidor.  
  
## <a name="remarks"></a>Observaciones  
 Una clave asimétrica es un elemento protegible de nivel de base de datos que contiene la base de datos que es su entidad primaria en la jerarquía de permisos. La mayoría de permisos limitados y específicos que se pueden revocar en una clave asimétrica se muestran a continuación, junto con permisos más generales que los incluyen por implicación.  
  
|Permiso de clave asimétrica|Implícito en el permiso de clave asimétrica|Implícito en el permiso de base de datos|  
|-------------------------------|------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY ASYMMETRIC KEY|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Permisos  
 Requiere permiso CONTROL en la clave asimétrica.  
  
## <a name="see-also"></a>Consulte también  
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [Permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)   
 [Jerarquía de cifrado](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
