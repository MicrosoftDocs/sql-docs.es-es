---
description: SHUTDOWN (Transact-SQL)
title: SHUTDOWN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SHUTDOWN_TSQL
- SHUTDOWN
dev_langs:
- TSQL
helpviewer_keywords:
- SQL Server, stopping
- shutting down SQL Server
- SHUTDOWN statement
- stopping SQL Server
- immediately stopping SQL Server
ms.assetid: c8b03ff9-688c-4fe8-86e8-bd6bd401c9a4
author: cawrites
ms.author: chadam
ms.openlocfilehash: 05a6ab1f592bc1e06c8a0fbbd8b3b4908a0d6156
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184661"
---
# <a name="shutdown-transact-sql"></a>SHUTDOWN (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Detiene SQL Server inmediatamente.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
SHUTDOWN [ WITH NOWAIT ]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 WITH NOWAIT  
 Opcional. Cierra [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sin ejecutar puntos de comprobación en cada base de datos. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se cierra tras intentar finalizar todos los procesos de usuario. Cuando el servidor se reinicia, se produce una operación de reversión para las transacciones incompletas.  
  
## <a name="remarks"></a>Comentarios  
 A menos que se use la opción WITHNOWAIT, SHUTDOWN cierra [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al:  
  
1.  Deshabilitar inicios de sesión (salvo en el caso de los miembros de los roles fijos de servidor **sysadmin** y **serveradmin**).  
  
    > [!NOTE]  
    >  Ejecute **sp_who** para mostrar una lista de todos los usuarios actuales.  
  
2.  Esperar a que terminen las instrucciones Transact-SQL o los procedimientos almacenados que se están ejecutando. Ejecute **sp_who** y **sp_lock** para mostrar una lista de todos los procesos y bloqueos activos respectivamente.  
  
3.  Insertar un punto de comprobación en cada base de datos.  
  
 Al usar la instrucción SHUTDOWN, se reduce el volumen de trabajo de recuperación automática necesario cuando los miembros del rol fijo de servidor **sysadmin** reinician [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 También se pueden utilizar otros métodos y herramientas para detener [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cada uno de ellos emite un punto de comprobación en todas las bases de datos. Puede vaciar los datos confirmados de la caché de datos y detener el servidor.  
  
-   Con el Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Mediante la ejecución de **net stop mssqlserver** desde un símbolo del sistema para una instancia predeterminada, o la ejecución de **net stop mssql$**_nombre_de_instancia_ desde un símbolo del sistema para una instancia con nombre.  
  
-   Con Servicios del Panel de control.  
  
 Si **sqlservr.exe** se ha iniciado desde el símbolo del sistema, al presionar CTRL+C se cierra [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Sin embargo, al presionar CTRL+C no se inserta ningún punto de comprobación.  
  
> [!NOTE]  
>  Al usar cualquiera de estos métodos para detener [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se envía el mensaje `SERVICE_CONTROL_STOP` a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="permissions"></a>Permisos  
 Los permisos de SHUTDOWN se asignan a los miembros de los roles fijos de servidor **sysadmin** y **serveradmin** y no son transferibles.  
  
## <a name="see-also"></a>Consulte también  
 [CHECKPOINT &#40;Transact-SQL&#41;](../../t-sql/language-elements/checkpoint-transact-sql.md)   
 [sp_lock &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-lock-transact-sql.md)   
 [sp_who &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)   
 [sqlservr (aplicación)](../../tools/sqlservr-application.md)   
 [Iniciar, detener, pausar, reanudar y reiniciar el motor de base de datos, Agente SQL Server o el Servicio SQL Server Browser](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)  
  
  
