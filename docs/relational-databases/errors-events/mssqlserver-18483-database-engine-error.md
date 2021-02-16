---
description: MSSQLSERVER_18483
title: MSSQLSERVER_18483
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 18483 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 82b12abb1121ed8f9523a267b5dbe98464c736f5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345543"
---
# <a name="mssqlserver_18483"></a>MSSQLSERVER_18483
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Detalles

|Atributo|Value|
|---|---|
|Nombre de producto|SQL Server|
|Id. de evento|18483|
|Origen de eventos|MSSQLSERVER|
|Componente|SQLEngine|
|Nombre simbólico|REMLOGIN_INVALID_USER|
|Texto del mensaje|No se pudo conectar al servidor "%.ls". "%. ls" no está definido como inicio de sesión remoto en el servidor. Compruebe que ha especificado el nombre de inicio de sesión correcto. %.*ls.|
||

## <a name="explanation"></a>Explicación

Este error se produce cuando se intenta configurar un distribuidor de replicación en un sistema que se restauró con la imagen de disco duro de otro equipo en el que se instaló originalmente la instancia de SQL. El usuario recibe un mensaje de error similar al siguiente:

> SQL Server Management Studio no pudo configurar "\<Server>\<Instance>" como el distribuidor de "\<Server>\<Instance>". Error 18483: No se pudo conectar al servidor "\<Server>\<Instance>" porque "distributor_admin" no está definido como inicio de sesión remoto en el servidor. Compruebe que ha especificado el nombre de inicio de sesión correcto. %.*ls.

## <a name="cause"></a>Causa

Cuando se implementa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] desde una imagen de disco duro de otro equipo en el que está instalado [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el nombre de red del equipo creado con una imagen se conserva en la instalación nueva. El nombre de red incorrecto hace que se produzca un error en la configuración del distribuidor de la replicación. El mismo problema se produce si cambia el nombre del equipo después de instalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="user-action"></a>Acción del usuario

Para solucionar este problema, reemplace el nombre del servidor [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] por el nombre de red correcto del equipo. Para hacerlo, siga estos pasos:

1. Inicie sesión en el equipo en el que implementó [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] desde la imagen de disco y, a continuación, ejecute la instrucción de Transact-SQL siguiente en SSMS:

    ```sql
    -- Use the Master database
    USE master
    GO

    -- Declare local variables
    DECLARE @serverproperty_servername varchar(100),
    @servername varchar(100);

    -- Get the value returned by the SERVERPROPERTY system function
    SELECT @serverproperty_servername = CONVERT(varchar(100), SERVERPROPERTY('ServerName'));

    -- Get the value returned by @@SERVERNAME global variable
    SELECT @servername = CONVERT(varchar(100), @@SERVERNAME);

    -- Drop the server with incorrect name
    EXEC sp_dropserver @server=@servername;

    -- Add the correct server as a local server
    EXEC sp_addserver @server=@serverproperty_servername, @local='local';
    ```

2. Reinicie el equipo en el que se ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
3. Para comprobar que el nombre de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y el nombre de red del equipo son iguales, ejecute la instrucción de Transact-SQL siguiente:

    ```sql
    SELECT @@SERVERNAME, SERVERPROPERTY('ServerName');
    ```

## <a name="more-information"></a>Más información

Puede usar la variable global `@@SERVERNAME` o la función `SERVERPROPERTY`("ServerName") en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para buscar el nombre de red del equipo en el que se ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La propiedad ServerName de la función `SERVERPROPERTY` notifica automáticamente el cambio del nombre de red del equipo al reiniciar el equipo y el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La variable global `@@SERVERNAME` conserva el nombre de equipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] original hasta que el nombre de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se restablezca manualmente.

### <a name="steps-to-reproduce-the-problem"></a>Pasos para reproducir el problema

En el equipo en el que implementó [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a partir de una imagen de disco, siga estos pasos:

1. Inicie [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)].
2. En el **Explorador de objetos**, expanda el nombre de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
3. Haga clic con el botón derecho en la carpeta **Replicación** y haga clic en la **configuración de replicación de distribución** y, luego, haga clic en **Configurar publicación, suscriptores y distribución**.
4. En el cuadro de diálogo del Asistente para **configuración de la distribución**, haga clic en **Siguiente**.
5. En el cuadro de diálogo **Distribuidor**, haga clic para seleccionar el "\<**Server**>\<**Instance**>" que actuará como su propio distribuidor; SQL Server creará una base de datos de distribución y un botón de radio de registro y, luego, haga clic en **Siguiente**.
6. En el cuadro de diálogo **Inicio del Agente SQL Server**, haga clic en **Siguiente**.
7. En el cuadro de diálogo **Carpeta de instantáneas**, haga clic en **Siguiente**.

    > [!NOTE]
    > Si recibe un mensaje para confirmar la ruta de acceso a la carpeta de instantáneas, haga clic en **Sí**.
8. En el cuadro de diálogo **Base de datos de distribución**, haga clic en **Siguiente**.
9. En el cuadro de diálogo **Publicadores**, haga clic en **Siguiente**.
10. En el cuadro de diálogo **Acciones del Asistente**, haga clic en **Siguiente**.
11. En el cuadro de diálogo **Finalización del asistente**, haga clic en **Finalizar**.

## <a name="see-also"></a>Consulte también

- [Cambiar el nombre de un equipo que hospeda una instancia independiente de SQL Server](../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md)
- [@@SERVERNAME (Transact-SQL)](../../t-sql/functions/servername-transact-sql.md)
- [SERVERPROPERTY (Transact-SQL)](../../t-sql/functions/serverproperty-transact-sql.md)
- [sp_addserver (Transact-SQL)](../system-stored-procedures/sp-addserver-transact-sql.md)