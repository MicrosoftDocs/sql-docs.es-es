---
description: MSSQLSERVER_1418
title: MSSQLSERVER_1418 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1418 (Database Engine error)
ms.assetid: 6e9c7241-0201-44e0-9f8b-b3c4e293f0f6
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: cb8cff51e4b33f2d5aeb99c030073544e195b11d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197003"
---
# <a name="mssqlserver_1418"></a>MSSQLSERVER_1418
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|Id. de evento|1418|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|DBM_PARTNERNOTFOUND|  
|Texto del mensaje|La dirección de red del servidor "%.*ls" no se puede alcanzar o no existe. Compruebe el nombre de la dirección de red y que los puertos de los extremos local y remoto estén operativos.|  
  
## <a name="explanation"></a>Explicación  
El extremo de la red del servidor no ha respondido porque no existe o no se puede alcanzar o no existe la dirección de red del servidor especificada.  
  
> [!NOTE]  
> De forma predeterminada, el sistema operativo de [!INCLUDE[msCoName](../../includes/msconame-md.md)] bloquea todos los puertos.  
  
## <a name="user-action"></a>Acción del usuario  
Compruebe el nombre de la dirección de red y vuelva a emitir el comando.  
  
Es posible que se necesiten acciones correctoras en ambos asociados. Por ejemplo, si se genera este mensaje cuando se intenta ejecutar SET PARTNER en la instancia del servidor principal, puede que el mensaje implique que solo es necesario realizar una acción correctora en la instancia del servidor reflejado. No obstante, es posible que se necesiten acciones correctoras en ambos asociados.  
  
### <a name="additional-corrective-actions"></a>Acciones correctoras adicionales  
  
-   Asegúrese de que la base de datos reflejada está preparada para la creación de reflejo.  
  
-   Asegúrese de que el nombre y el puerto de la instancia del servidor reflejado son correctos.  
  
-   Asegúrese de que la instancia del servidor reflejado de destino no está detrás de un firewall.  
  
-   Asegúrese de que la instancia del servidor principal no está detrás de un firewall.  
  
-   Compruebe que los extremos se inician en los asociados usando la columna **state** o **state_desc** de la vista de catálogo **sys.database_mirroring_endpoints**. Si alguno de los extremos no se ha iniciado, ejecute una instrucción ALTER ENDPOINT para iniciarlo.  
  
-   Asegúrese de que la instancia del servidor principal escucha en el puerto asignado a su extremo de creación de reflejo de la base de datos y que la instancia del servidor reflejado escucha en su puerto. Para obtener más información, vea "Comprobar la disponibilidad de los puertos" más adelante en este tema. Si un asociado no escucha en su puerto asignado, modifique el extremo de creación de reflejo de la base de datos para que escuche en otro puerto.  
  
    > [!IMPORTANT]  
    > Una seguridad configurada incorrectamente puede provocar un mensaje de error de configuración general. Normalmente, la instancia de servidor elimina la solicitud de conexión incorrecta sin responder. Para el autor de la llamada, puede parecer que el error de configuración de seguridad se produce por distintos motivos, como que la base de datos reflejada se encuentra en mal estado o no existe, permisos incorrectos, etc.  
  
### <a name="using-the-error-log-file-for-diagnosis"></a>Usar el archivo de registro de errores para diagnóstico  
En ocasiones, solo hay archivos de registro de errores disponibles para la investigación. En esos casos, determine si el registro de errores contiene el mensaje de error 26023 para el puerto TCP del extremo de creación de reflejo de la base de datos. Este error, cuya gravedad es de 16, podría indicar que no se ha iniciado el extremo de creación de reflejo de la base de datos. Este mensaje se puede generar aunque **sys.database_mirroring_endpoints** muestre el estado del extremo como iniciado.  
  
Después de resolver cualquier problema que surja, vuelva a ejecutar la instrucción ALTER DATABASE *nombreDeBaseDeDatos* SET PARTNER en el servidor principal.  
  
### <a name="verifying-port-availability"></a>Comprobar la disponibilidad de los puertos  
Al configurar la red para una sesión de creación de reflejo de la base de datos, asegúrese de que el extremo de reflejo de la base de datos de cada una de las instancias de servidor solo se usa en el proceso de creación de reflejo de base de datos. Si otro proceso escucha en el puerto asignado a un extremo de reflejo de la base de datos, los procesos de creación de reflejo de la base de datos de las demás instancias de servidor no podrán conectarse al extremo.  
  
Para mostrar todos los puertos en los que escucha un servidor basado en Windows, use la utilidad de símbolo del sistema **netstat**. La sintaxis de **netstat** depende de la versión del sistema operativo Windows. Para obtener más información, vea la documentación del sistema operativo.  
  
#### <a name="windows-server-2003-service-pack-1-sp1"></a>Service Pack 1 (SP1) de Windows Server 2003  
Para enumerar los puertos que escuchan y los procesos que los tienen abiertos, especifique el siguiente comando en el símbolo del sistema de Windows:  
  
**netstat -abn**  
  
#### <a name="windows-server-2003-pre-sp1"></a>Windows Server 2003 (anterior al SP1)  
Para identificar los puertos que escuchan y los procesos que los tienen abiertos, siga estos pasos:  
  
1.  Obtenga el Id. del proceso.  
  
    Para obtener más información sobre el Id. de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], conéctese a la instancia y utilice la siguiente instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)]:  
  
    ```  
    SELECT SERVERPROPERTY('ProcessID')   
    ```  
  
    Para obtener más información, vea "SERVERPROPERTY (Transact-SQL)" en los Libros en pantalla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
2.  Haga coincidir el id. del proceso con el resultado del siguiente comando **netstat**:  
  
    **netstat -ano**  
  
## <a name="see-also"></a>Consulte también  
[ALTER ENDPOINT &#40;Transact-SQL&#41;](~/t-sql/statements/alter-endpoint-transact-sql.md)  
[El punto de conexión de creación de reflejo de la base de datos &#40;SQL Server&#41;](~/database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)  
[Preparar una base de datos reflejada para la creación de reflejo &#40;SQL Server&#41;](~/database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)  
[SERVERPROPERTY &#40;Transact-SQL&#41;](~/t-sql/functions/serverproperty-transact-sql.md)  
[Especificar una dirección de red de servidor &#40;creación de reflejo de la base de datos&#41;](~/database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)  
[sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)  
[Solucionar problemas de configuración de creación de reflejo de la base de datos &#40;SQL Server&#41;](~/database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)  
  
