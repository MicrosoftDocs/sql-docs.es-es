---
description: MSSQLSERVER_15581
title: MSSQLSERVER_15581
ms.custom: ''
ms.date: 09/03/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha, VenCher
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 15581 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: e2308e1cbdd139983df14f7620bd4a6b9168ac92
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196892"
---
# <a name="mssqlserver_15581"></a>MSSQLSERVER_15581
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Detalles

|Atributo|Value|
|---|---|
|Nombre de producto|SQL Server|
|Id. de evento|15581|
|Origen de eventos|MSSQLSERVER|
|Componente|SQLEngine|
|Nombre simbólico|SEC_NODBMASTERKEYERR|
|Texto del mensaje|Cree una clave maestra en la base de datos o abra la clave maestra en la sesión antes de realizar esta operación.|
||

## <a name="explanation"></a>Explicación

Se produce el error 15581 cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede recuperar una base de datos habilitada para el cifrado de datos transparente (TDE). En el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se registra un mensaje de error similar al siguiente

> 2020-01-14 22:16:26.47 spid20s Error: 15581, Gravedad: 16, estado: 3.  
2020-01-14 22:16:26.47 spid20s Cree una clave maestra en la base de datos o abra la clave maestra en la sesión antes de realizar esta operación.

## <a name="possible-cause"></a>Causa posible

Este problema se produce cuando se quita el cifrado de claves maestras de servicio para la clave maestra de base de datos en la base de datos maestra cuando se ejecuta el comando siguiente:

```sql
Use master
go
alter master key drop encryption by service master key
```

La clave maestra de servicio se usa para cifrar el certificado utilizado por la clave maestra de la base de datos. Cualquier intento de usar la base de datos habilitada para TDE requiere acceso a la clave maestra de la base de datos en la base de datos maestra. Una clave maestra que no esté cifrada por la clave maestra de servicio debe abrirse mediante la instrucción [OPEN MASTER KEY (Transact-SQL)](../../t-sql/statements/open-master-key-transact-sql.md) junto con una contraseña en cada sesión que necesite acceso a la clave maestra. Como este comando no se puede ejecutar en sesiones del sistema, la recuperación no se puede completar en las bases de datos habilitadas para TDE.

## <a name="user-action"></a>Acción del usuario

Para resolver el problema, habilite el descifrado automático de la clave maestra. Para ello, ejecute los siguientes comandos:

```sql
Use master
go
open master key DECRYPTION BY PASSWORD = 'password'
alter master key add encryption by service master key
```

Use la consulta siguiente para determinar si el descifrado automático de la clave maestra por medio de la clave maestra de servicio se ha deshabilitado para la base de datos maestra:

```sql
select is_master_key_encrypted_by_server from sys.databases where name = 'master'
```

Si esta consulta devuelve un valor de 0, se ha deshabilitado el descifrado automático de la clave maestra por medio de la clave maestra de servicio.

## <a name="more-information"></a>Más información

En algunos casos, puede parecer que la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no responde. Si consulta la vista de administración dinámica `sys.dm_exec_requests`, observará que el subproceso `LogWriter` y otros subprocesos que realizan operaciones de DML espera indefinidamente con el valor wait_type WRITELOG. Es posible que otras sesiones también estén en espera mientras intentan obtener bloqueos.