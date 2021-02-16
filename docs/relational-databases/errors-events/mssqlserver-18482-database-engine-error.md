---
description: MSSQLSERVER_18482
title: MSSQLSERVER_18482
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 18482 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: ff7d1cf9be26bd9301d16ae21dcbd6af96f57bc3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345549"
---
# <a name="mssqlserver_18482"></a>MSSQLSERVER_18482
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Detalles

|Atributo|Value|
|---|---|
|Nombre de producto|SQL Server|
|Id. de evento|18482|
|Origen de eventos|MSSQLSERVER|
|Componente|SQLEngine|
|Nombre simbólico|REMLOGIN_INVALID_SITE|
|Texto del mensaje|No se pudo conectar al servidor "%.ls". "%. ls" no está definido como servidor remoto. Compruebe que ha especificado el nombre de servidor correcto. %.*ls|
||

## <a name="explanation"></a>Explicación

Este error se produce cuando se intenta ejecutar una llamada a procedimiento remoto (RPC) desde un servidor a otro (por ejemplo, mediante la ejecución de un procedimiento almacenado en un equipo remoto con una instrucción como `EXEC SERV_REMOTE.pubs..byroyalty`). El usuario recibe un mensaje de error similar al siguiente

> Error 18482: No se pudo conectar al servidor \<SERV_REMOTE> porque \<SERV_REMOTE> no está definido como servidor remoto. Compruebe que ha especificado el nombre de servidor correcto.

## <a name="cause"></a>Causa

Este error se produce cuando SQL Server no puede ejecutar una llamada a procedimiento remoto. Esto puede deberse a un servidor local configurado incorrectamente. Para hacer una llamada a procedimiento remoto, SQL Server primero determina quién es el servidor local buscando el nombre del servidor con srvid = 0 en sysservers. Si no se encuentra una entrada con srvid = 0 en sysservers, o si el nombre del servidor con srvid = 0 pertenece a un nombre de servidor distinto del nombre del equipo Windows local, recibirá el error.

## <a name="user-action"></a>Acción del usuario

Para determinar si el servidor local está configurado correctamente, examine la columna `srvstatus` en master..sysservers. Este valor debe ser 0 para el servidor local.

Por ejemplo, supongamos que al servidor local se le asignó el nombre SERV_LOCAL, al servidor remoto el nombre *SERV_REMOTE* y que sysservers contenía la siguiente información:

|srvid|srvstatus|srvname|srvname|
|---|---|---|---|
|1|2|SERV_LOCAL|SERV_LOCAL|
|2|1|SERV_REMOTE|SERV_REMOTE|
||||

En la salida anterior, SERV_LOCAL es el servidor local, pero tiene un srvid de 1; debe ser 0. Para corregirlo, siga estos pasos:

1. Ejecute `sp_dropserver` local_server_name, droplogins (en este ejemplo, ejecutaría `sp_dropserver SERV_LOCAL, droplogins`).
1. Ejecute `sp_addserver` local_server_name, LOCAL (en este ejemplo, ejecutaría `sp_addserver SERV_LOCAL, LOCAL`).
1. Detenga y reinicie SQL Server.

Después de ejecutar esos pasos, la tabla sysservers debe tener el siguiente aspecto:

|srvid|srvstatus|srvname|srvname|
|---|---|---|---|
|0|0|SERV_LOCAL|SERV_LOCAL|
|2|1|SERV_REMOTE|SERV_REMOTE|
||||

> [!NOTE]
> El identificador de servidor (srvid) debe ser 0 para el servidor local.

Es posible que haya algunos casos en los que las entradas de la tabla sysservers parezcan correctas, pero cuando ejecute `select @@servername`, devolverá NULL. En estos escenarios, todavía debe continuar con los pasos 1 a 3 mostrados anteriormente para corregir el problema.

## <a name="more-information"></a>Más información

Es posible que reciba este mensaje de error al instalar la replicación, ya que el proceso de instalación realiza llamadas a procedimiento remoto entre los servidores implicados en la replicación.
