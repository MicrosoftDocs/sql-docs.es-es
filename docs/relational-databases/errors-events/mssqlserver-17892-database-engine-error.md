---
description: MSSQLSERVER_17892
title: MSSQLSERVER_17892
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 17892 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 389211fb5d261b0ef79926695398b7c6c2101a13
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196576"
---
# <a name="mssqlserver_17892"></a>MSSQLSERVER_17892
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Detalles

|Atributo|Value|
|---|---|
|Nombre de producto|SQL Server|
|Id. de evento|17892|
|Origen de eventos|MSSQLSERVER|
|Componente|SQLEngine|
|Nombre simbólico|SRV_LOGON_FAILED_BY_TRIGGER|
|Texto del mensaje|Error del inicio de sesión \<Login Name> debido a la ejecución del desencadenador.|
||

## <a name="explanation"></a>Explicación

El error 17892 se produce cuando un código del desencadenador de inicio de sesión no se puede ejecutar correctamente. Los [desencadenadores de inicio de sesión](../triggers/logon-triggers.md) activan procedimientos almacenados en respuesta a un evento LOGON. Este evento se genera cuando se establece una sesión de usuario con una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El usuario recibe un mensaje de error similar al siguiente:

> Mensaje 17892, nivel 14, estado 1, servidor \<Server Name>, línea 1  
Error del inicio de sesión \<Login Name> debido a la ejecución del desencadenador.

## <a name="possible-causes"></a>Causas posibles

El problema puede producirse si hay un error al ejecutar el código del desencadenador para esa cuenta de usuario en concreto. Algunos de los escenarios incluyen:

- El desencadenador intenta insertar datos en una tabla que no existe.
- El inicio de sesión no tiene permisos para el objeto al que hace referencia el desencadenador de inicio de sesión.

## <a name="user-action"></a>Acción del usuario

Puede usar una de las siguientes soluciones en función de su escenario.

- **Escenario 1**: Actualmente tiene acceso a una sesión abierta de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en una cuenta de administrador.

  En este caso, puede llevar a cabo la acción correctiva necesaria para corregir el código del desencadenador.

  - Ejemplo 1: Si el objeto al que hace referencia el código del desencadenador no existe, cree el objeto para que el desencadenador de inicio de sesión pueda ejecutarse correctamente.

  - Ejemplo 2: Si el objeto al que hace referencia el código del desencadenador existe, pero los usuarios no tienen permisos, concédales los privilegios necesarios para acceder al objeto.  
  
  De forma alternativa, puede quitar o deshabilitar el desencadenador de inicio de sesión para que los usuarios puedan seguir iniciando sesión en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

- **Escenario 2**: No tiene ninguna sesión actual abierta con privilegios de administración, pero la conexión de administrador dedicada (DAC) está habilitada en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

    En este caso, puede usar la conexión DAC para seguir los pasos descritos en el escenario 1, ya que las conexiones DAC no se ven afectadas por los desencadenadores de inicio de sesión. Para obtener más información sobre la conexión de administrador dedicada (DAC), vea: [Conexión de diagnóstico para administradores de bases de datos](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md).

    Para comprobar si la conexión DAC está habilitada en su instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], revise si hay un mensaje similar al siguiente en el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:

    > 2020-02-09 16:17:44.150 Servidor Se estableció la compatibilidad con la conexión de administración dedicada para escuchar localmente en el puerto 1434.  

- **Escenario 3**: No tiene una conexión DAC habilitada en el servidor ni una sesión de administrador para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

    En este escenario, la única forma de corregir el problema sería realizar los pasos siguientes:
  
    1. Detenga [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y los servicios relacionados.
    2. Inicie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] desde el [símbolo del sistema](/previous-versions/sql/sql-server-2008-r2/ms180965(v=sql.105)) con los parámetros de inicio `-c`, `-m` y `-f`. Esto deshabilita el desencadenador de inicio de sesión y le permite tomar las mismas medidas correctivas descritas anteriormente en el **escenario 1**.
  
        > [!NOTE]
        > El procedimiento anterior requiere una cuenta de *administrador del sistema* o una cuenta de administrador equivalente.
  
         Para obtener más información sobre estas y otras opciones de inicio, vea: [Opciones de inicio del servicio de motor de base de datos](../../database-engine/configure-windows/database-engine-service-startup-options.md).

## <a name="more-information"></a>Más información

Otra situación en la que se produce un error en los desencadenadores de inicio de sesión es cuando se usa la función `EVENTDATA`. Esta función devuelve XML y distingue mayúsculas de minúsculas.  Por lo tanto, si crea el siguiente desencadenador de inicio de sesión, que intenta bloquear el acceso basado en la dirección IP, se puede producir el problema:

``` sql
 CREATE TRIGGER tr_logon_CheckIP  
 ON ALL SERVER  
 FOR LOGON  
 AS
 BEGIN
  IF IS_SRVROLEMEMBER ( 'sysadmin' ) = 1  
     BEGIN
         DECLARE @IP NVARCHAR ( 15 );  
         SET @IP = ( SELECT EVENTDATA ().value ( '(/EVENT_INSTANCE/ClientHost)[1]' , 'NVARCHAR(15)' ));  
         IF NOT EXISTS( SELECT IP FROM DBAWork.dbo.ValidIP WHERE IP = @IP )  
         ROLLBACK ;  
     END ;  
 END ;  
 GO
```

El usuario no respetó las mayúsculas y minúsculas al copiar este script de Internet en esta parte del desencadenador:

```sql
 SELECT EVENTDATA ().value ( '(/event_instance/clienthost)[1]' , 'NVARCHAR(15)' ));  
```

Como consecuencia, `EVENTDATA` siempre devolvía **NULL** y se denegaba el acceso a todos los inicios de sesión equivalentes a cuentas de administrador del sistema. En este caso, la conexión DAC no estaba habilitada, por lo que la única solución era reiniciar el servidor con los parámetros de inicio enumerados anteriormente para quitar el desencadenador.