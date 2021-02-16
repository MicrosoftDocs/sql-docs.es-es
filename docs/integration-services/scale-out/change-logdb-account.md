---
title: Cambiar la cuenta para el registro de escalabilidad horizontal de SSIS | Microsoft Docs
description: Obtenga información sobre cómo cambiar la cuenta que usa para el registro de Escalabilidad horizontal de SQL Server Integration Services (SSIS).
ms.custom: performance
ms.date: 06/29/2020
ms.prod: sql
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.reviewer: maghan
ms.openlocfilehash: b36b01a416a5468def2baa66cd4395b0944a49cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354899"
---
# <a name="change-the-account-for-scale-out-logging"></a>Cambiar la cuenta para el registro del escalabilidad horizontal

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


Al ejecutar paquetes de SSIS en escalabilidad horizontal, los mensajes de eventos se registrarán en la base de datos de SSISDB con un usuario creado automáticamente y llamado **##MS_SSISLogDBWorkerAgentLogin##** . El inicio de sesión de este usuario usará la autenticación de SQL Server.

Si quiere cambiar la cuenta que se usa para el registro de la escalabilidad horizontal, siga estos pasos:

> [!NOTE]
> Si usa una cuenta de usuario de Windows para el registro, use la misma cuenta que la que ejecuta el servicio de trabajador de escalabilidad horizontal. En caso contrario, se producirá un error en el inicio de sesión de SQL Server.

## <a name="1-create-a-user-for-ssisdb"></a>1. Crear un usuario de SSISDB
Para obtener instrucciones de creación de un usuario de base de datos, vea [Crear un usuario de base de datos](../../relational-databases/security/authentication-access/create-a-database-user.md).

## <a name="2-add-the-user-to-the-database-role-ssis_cluster_worker"></a>2. Agregar el usuario al rol de base de datos ssis_cluster_worker

Para obtener instrucciones sobre cómo combinar un rol de base de datos, vea [Combinar un rol](../../relational-databases/security/authentication-access/join-a-role.md).

## <a name="3-update-the-logging-information-in-ssisdb"></a>3. Actualizar la información de registro en SSISDB
Realice una llamada al procedimiento almacenado `[catalog].[update_logdb_info]` usando el nombre de SQL Server y la cadena de conexión como parámetros, tal como se muestra en el siguiente ejemplo:

```sql
SET @serverName = CONVERT(sysname, SERVERPROPERTY('servername'))
SET @connectionString = 'Data Source=' + @serverName + ';Initial Catalog=SSISDB;Integrated Security=SSPI;'
EXEC [internal].[update_logdb_info] @serverName, @connectionString
GO
```

## <a name="4-restart-the-scale-out-worker-service"></a>4. Reiniciar el servicio de trabajador de escalabilidad horizontal
Reinicie el servicio de trabajador de escalabilidad horizontal para aplicar el cambio.

## <a name="next-steps"></a>Pasos siguientes
-   [Administrador de escalabilidad horizontal de Integration Services](integration-services-ssis-scale-out-manager.md)
