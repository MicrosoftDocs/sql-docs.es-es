---
description: MSSQLSERVER_15401
title: MSSQLSERVER_15401
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 15401 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: ab9739d17fbe36bb8a703dd06ee85a32e93e7662
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349036"
---
# <a name="mssqlserver_15401"></a>MSSQLSERVER_15401
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Detalles

|Atributo|Value|
|---|---|
|Nombre de producto|SQL Server|
|Id. de evento|15401|
|Origen de eventos|MSSQLSERVER|
|Componente|SQLEngine|
|Nombre simbólico|SEC_INVALIDLOGINORGROUP|
|Texto del mensaje|No se encuentra el usuario o grupo de Windows NT '%s'. Compruebe de nuevo el nombre.|
||

## <a name="explanation"></a>Explicación

Este error se produce cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede crear un inicio de sesión basado en una entidad de seguridad de Windows (como un usuario de dominio o un grupo de dominio de Windows). El usuario recibe un mensaje de error similar al siguiente

> Error 15401: No se encuentra el usuario o grupo de Windows NT '%s'. Compruebe de nuevo el nombre.

## <a name="cause"></a>Causa

El error puede producirse por alguno de los siguientes motivos:

- El inicio de sesión no existe en Active Directory.
- El controlador de dominio no está disponible.
- No utiliza BUILTIN como nombre de dominio al agregar una cuenta local.
- Problemas de resolución de nombres.

## <a name="user-action"></a>Acción del usuario

Revise las siguientes secciones para ver las acciones que puede realizar para cada una de las distintas causas mencionadas anteriormente.

### <a name="verify-the-login-you-are-trying-to-add"></a>Compruebe el inicio de sesión que intenta agregar

1. Compruebe que el inicio de sesión de Windows todavía existe en el dominio. Es posible que el administrador de red haya quitado el inicio de sesión de Windows por motivos específicos y no pueda conceder ese inicio de sesión a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
1. Compruebe que ha escrito correctamente el nombre de inicio de sesión y dominio y que usa el siguiente formato: `Domain\User`
1. Si el inicio de sesión existe y es correcto, y sigue recibiendo el error, continúe con las siguientes secciones de este artículo.

### <a name="verify-if-the-domain-controller-is-available"></a>Comprobación de la disponibilidad del controlador de dominio

Es posible que reciba el error 15401 cuando el controlador de dominio del dominio en el que reside el inicio de sesión (el mismo dominio o uno diferente) no está disponible por algún motivo.

Si el inicio de sesión se encuentra en un dominio distinto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], compruebe que existen las confianzas correctas entre los dominios.

Compruebe que el controlador de dominio del inicio de sesión es accesible mediante el comando ping desde el equipo que ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Compruebe la dirección IP y el nombre del controlador de dominio.

### <a name="verify-the-domain-name-for-local-accounts"></a>Comprobación del nombre de dominio para las cuentas locales

Las cuentas locales (que no son de dominio) requieren un tratamiento especial. Si intenta agregar una cuenta local desde el equipo local que ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], asegúrese de que usa BUILTIN como nombre de dominio.

### <a name="check-for-name-resolution-issues"></a>Comprobación de problemas de resolución de nombres

Si tiene problemas para resolver el nombre de un equipo implicado en la incorporación del inicio de sesión o grupo, es posible que reciba el error 15401.

Compruebe que el mecanismo de resolución de nombres (como WINS, DNS, HOSTS o LMHOSTS) está configurado correctamente.

## <a name="see-also"></a>Consulte también

- [Prueba de un canal entre el equipo local y su dominio](/powershell/module/microsoft.powershell.management/test-computersecurechannel#example-1--test-a-channel-between-the-local-computer-and-its-domain)
- [LogonSessions v1.4](/sysinternals/downloads/logonsessions)
- [sp_change_users_login (Transact-SQL)](../system-stored-procedures/sp-change-users-login-transact-sql.md)