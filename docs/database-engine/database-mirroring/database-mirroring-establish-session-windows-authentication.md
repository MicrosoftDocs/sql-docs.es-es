---
title: Configuración de la creación de reflejo de la base de datos
description: Pasos para configurar una relación de creación de reflejo de la base de datos entre una entidad de seguridad y un reflejo mediante la autenticación de Windows.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- Windows authentication [SQL Server]
- database mirroring [SQL Server], security
ms.assetid: 143c68a5-589f-4e7f-be59-02707e1a430a
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: baa9d359af3b4003cdeb29d79e6b8a699519a99d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353150"
---
# <a name="configure-database-mirroring"></a>Configuración de la creación de reflejo de la base de datos
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
    
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Se usa [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] en su lugar.  
  
 Una vez preparada la base de datos reflejada (vea [Preparar una base de datos reflejada para la creación de reflejo &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)), puede establecer una sesión de creación de reflejo de la base de datos. Las instancias de servidor principal, reflejado y testigo deben ser instancias de servidor independientes y se deben encontrar en sistemas host distintos.  
  
> [!IMPORTANT]
>  Es recomendable que configure la creación de reflejo de la base de datos durante las horas de menor actividad, ya que puede afectar al rendimiento.  
> 
> [!NOTE]
>  Una determinada instancia de servidor puede participar en varias sesiones simultáneas de creación de reflejo de la base de datos con el mismo asociado o con asociados distintos. Una instancia de servidor puede ser asociado en algunas sesiones y testigo en otras. La instancia del servidor reflejado debe ejecutar la misma edición de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que la instancia del servidor principal. La creación de reflejo de la base de datos no está disponible en todas las ediciones de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener una lista de las características admitidas por las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Características compatibles con las ediciones de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md). Asimismo, se recomienda encarecidamente que se ejecuten en sistemas comparables que puedan administrar cargas de trabajo idénticas.  
  
### <a name="to-establish-a-database-mirroring-session"></a>Para establecer una sesión de creación de reflejo de la base de datos  
  
1.  Cree la base de datos reflejada. Para obtener más información, vea [Preparar una base de datos reflejada para la creación de reflejo &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
2.  Configure la seguridad en cada instancia de servidor.  
  
     Cada instancia de servidor de una sesión de creación de reflejo de la base de datos requiere un extremo de creación de reflejo de la base de datos. Si el extremo no existe, deberá crearlo.  
  
    > [!NOTE]  
    >  El tipo de autenticación que utilice la instancia de servidor para la creación de reflejo de la base de datos es una propiedad del extremo de reflejo de la base de datos. Existen dos tipos de seguridad de transporte para la creación de reflejo de la base de datos: autenticación de Windows o autenticación basada en certificados. Para obtener más información, vea [Seguridad de transporte para la creación de reflejo de la base de datos y grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md).  
  
     Asegúrese de que exista un extremo para el reflejo de bases de datos en cada servidor asociado. La instancia de servidor solo puede tener un extremo de reflejo de la base de datos, independientemente del número de sesiones de creación de reflejo que se admitirán. Si tiene pensado usar esta instancia de servidor exclusivamente para los asociados en las sesiones de creación de reflejo de la base de datos, puede asignar el rol de asociado al punto de conexión (ROLE **=** PARTNER). Si también tiene pensado usar este servidor para el testigo en otras sesiones de creación de reflejo de la base de datos, asigne el rol del extremo como ALL.  
  
     Para ejecutar una instrucción SET PARTNER, el valor de STATE de los extremos de ambos asociados debe ser STARTED.  
  
     Para saber si una instancia de servidor tiene un extremo de creación de reflejo de la base de datos y cuáles son su rol y su estado, use en esa instancia la siguiente instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] :  
  
    ```  
    SELECT role_desc, state_desc FROM sys.database_mirroring_endpoints  
    ```  
  
    > [!IMPORTANT]  
    >  No reconfigure un extremo de creación de reflejo de la base de datos en uso. Si existe un extremo para la creación de reflejo de la base de datos y ya se está utilizando, se recomienda utilizar ese extremo para cada sesión en la instancia de servidor. Quitar un extremo en uso puede dar lugar a que el extremo se reinicie y conllevar la interrupción de las conexiones de las sesiones existentes, que pueden aparecer como un error en las otras instancias de servidor. Esto es especialmente importante en el modo de alta seguridad con conmutación automática por error, en el que reconfigurar el extremo en un asociado podría dar lugar a una conmutación automática por error. Asimismo, si se ha establecido un testigo para una sesión, la eliminación del extremo de creación de reflejo de la base de datos puede hacer que el servidor principal de esa sesión pierda quórum; si sucede esto, la base de datos se queda sin conexión y se desconecta a sus usuarios. Para más información, vea [Cuórum: cómo un testigo afecta a la disponibilidad de la base de datos &#40;creación de reflejo de la base de datos&#41;](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md).  
  
     Si alguno de los asociados no tiene un punto de conexión, vea [Crear un punto de conexión de creación de reflejo de la base de datos para la autenticación de Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md).  
  
3.  Si las instancias de servidor se ejecutan con distintas cuentas de usuario de dominio, cada una requiere un inicio de sesión en la base de datos **master** de las demás. Si el inicio de sesión no existe, deberá crearlo. Para obtener más información, vea [Permitir el acceso de red a un punto de conexión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md).  
  
4.  Para establecer el servidor principal como asociado en la base de datos reflejada, conéctese al servidor reflejado y emita la instrucción siguiente:  
  
     ALTER DATABASE *\<database_name\>* SET PARTNER **=** _\<server\_network\_address\>_  
  
     donde _\<database\_name\>_ es el nombre de la base de datos de la que se va a crear el reflejo (este nombre es el mismo para ambos asociados) y _\<server\_network\_address\>_ es la dirección de red del servidor principal.  
  
     La sintaxis para una dirección de red de servidor es la siguiente:  
  
     TCP <b>\://</b> _\<system-address\>_ <b>\:</b> _\<port\>_  
  
     donde _\<system-address>_ es una cadena que identifica de forma inequívoca el sistema del equipo de destino y _\<port>_ es el número de puerto que usa el punto de conexión de la creación de reflejo de la instancia de servidor asociado. Para obtener más información, vea [Especificar una dirección de red de servidor &#40;creación de reflejo de la base de datos&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).  
  
     Por ejemplo, en la instancia del servidor reflejado, el siguiente parámetro ALTER DATABASE establece el asociado como la instancia de servidor principal original. El nombre de la base de datos es **AdventureWorks**, la dirección del sistema es DBSERVER1 (el nombre del sistema del asociado) y el puerto usado por el punto de conexión de reflejo de la base de datos del asociado es 7022:  
  
    ```  
    ALTER DATABASE AdventureWorks   
       SET PARTNER = 'TCP://DBSERVER1:7022'  
    ```  
  
     Esta instrucción prepara el servidor reflejado para que cree una sesión cuando el servidor principal se ponga en contacto con él.  
  
5.  Para establecer el servidor reflejado como asociado en la base de datos principal, conéctese al servidor principal y emita la instrucción siguiente:  
  
     ALTER DATABASE _\<database\_name\>_ SET PARTNER **=** _\<server\_network\_address\>_  
  
     Para obtener más información, vea el paso 4.  
  
     Por ejemplo, en la instancia de servidor principal, la siguiente instrucción ALTER DATABASE establece el asociado como la instancia del servidor reflejado original. El nombre de la base de datos es **AdventureWorks**, la dirección del sistema es DBSERVER2 (el nombre del sistema del asociado) y el puerto usado por el punto de conexión de reflejo de la base de datos del asociado es 7025:  
  
    ```  
    ALTER DATABASE AdventureWorks SET PARTNER = 'TCP://DBSERVER2:7022'  
    ```  
  
     Si escribe esta instrucción en el servidor principal, comenzará la sesión de creación de reflejo de la base de datos.  
  
6.  De forma predeterminada, una sesión se configura con la seguridad de las transacciones completa (SAFETY se establece en FULL), lo que inicia la sesión en modo sincrónico de alta seguridad sin conmutación automática por error. Puede volver a configurar la sesión para ejecutarla en modo de alta seguridad con conmutación automática por error o en modo asincrónico de alto rendimiento, como se indica a continuación:  
  
    -   **Modo de seguridad alta con conmutación automática por error**  
  
         Si desea que una sesión en modo de alta seguridad admita la conmutación automática por error, agregue una instancia de servidor testigo. Para obtener más información, vea [Agregar un testigo de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md).  
  
    -   **Modo de alto rendimiento**  
  
         Otra opción, si no desea la conmutación automática por error y prefiere poner más énfasis en el rendimiento en lugar de la disponibilidad, es desactivar la seguridad de las transacciones. Para obtener más información, vea [Cambiar la seguridad de las transacciones en una sesión de creación de reflejo de la base de datos &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/change-transaction-safety-in-a-database-mirroring-session-transact-sql.md).  
  
        > [!NOTE]  
        >  En el modo de alto rendimiento, WITNESS debe establecerse en OFF. Para más información, vea [Cuórum: cómo un testigo afecta a la disponibilidad de la base de datos &#40;creación de reflejo de la base de datos&#41;](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md).  
  
## <a name="example"></a>Ejemplo  
  
> [!NOTE]  
>  En el siguiente ejemplo se establece una sesión de creación de reflejo de la base de datos entre asociados de una base de datos reflejada existente. Para obtener información sobre la creación de una base de datos reflejada, vea [Preparar una base de datos reflejada para la creación de reflejo &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
 En el ejemplo se muestran los pasos básicos para crear una sesión de creación de reflejo de la base de datos sin un testigo. Los dos asociados son las instancias de servidor predeterminadas en dos equipos (PARTNERHOST1 y PARTNERHOST5). Las dos instancias de asociado se ejecutan con la misma cuenta de usuario de dominio de Windows (MYDOMAIN\dbousername).  
  
1.  En la instancia de servidor principal (instancia predeterminada en PARTNERHOST1), cree un extremo que admita todos los roles y que utilice el puerto 7022:  
  
    ```  
    --create an endpoint for this instance  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=ALL)  
    GO  
    --Partners under same domain user; login already exists in master.  
    ```  
  
    > [!NOTE]  
    >  Para obtener un ejemplo de cómo configurar un inicio de sesión, vea [Permitir el acceso de red a un punto de conexión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md).  
  
2.  En la instancia del servidor reflejado (instancia predeterminada en PARTNERHOST5), cree un extremo que admita todos los roles y que utilice el puerto 7022:  
  
    ```  
    --create an endpoint for this instance  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=ALL)  
    GO  
    --Partners under same domain user; login already exists in master.  
    ```  
  
3.  En la instancia de servidor principal (en PARTNERHOST1), cree una copia de seguridad de la base de datos:  
  
    ```  
    BACKUP DATABASE AdventureWorks   
        TO DISK = 'C:\AdvWorks_dbmirror.bak'   
        WITH FORMAT  
    GO  
    ```  
  
4.  En la instancia del servidor reflejado (en `PARTNERHOST5`), restaure la base de datos:  
  
    ```  
    RESTORE DATABASE AdventureWorks   
        FROM DISK = 'Z:\AdvWorks_dbmirror.bak'   
        WITH NORECOVERY  
    GO  
    ```  
  
5.  Tras crear la copia de seguridad completa de base de datos, debe crear una copia de seguridad de registros en la base de datos principal. Por ejemplo, las siguientes instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] hacen una copia de seguridad del registro en el mismo archivo que ha utilizado la copia de seguridad de base de datos anterior:  
  
    ```  
    BACKUP LOG AdventureWorks   
        TO DISK = 'C:\AdventureWorks.bak'   
    GO  
    ```  
  
6.  Para poder iniciar la creación de reflejo, se debe aplicar la copia de seguridad de registros obligatoria (y las copias de seguridad de registros subsiguientes).  
  
     Por ejemplo, la siguiente instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] restaura el primer registro desde C:\AdventureWorks.bak:  
  
    ```  
    RESTORE LOG AdventureWorks   
        FROM DISK = 'C:\ AdventureWorks.bak'   
        WITH FILE=1, NORECOVERY  
    GO  
    ```  
  
7.  En la instancia del servidor reflejado, establezca la instancia de servidor en PARTNERHOST1 como asociado (para convertirla en el servidor principal inicial):  
  
    ```  
    USE master;  
    GO  
    ALTER DATABASE AdventureWorks   
        SET PARTNER =   
        'TCP://PARTNERHOST1:7022'  
    GO  
    ```  
  
    > [!IMPORTANT]  
    >  De forma predeterminada, una sesión de creación de reflejo de la base de datos se ejecuta en modo sincrónico. Esto exige tener la seguridad de las transacciones completa (SAFETY establecido en FULL). Para que una sesión se ejecute en modo asincrónico de alto rendimiento, establezca SAFETY en OFF. Para más información, consulte [Database Mirroring Operating Modes](../../database-engine/database-mirroring/database-mirroring-operating-modes.md).  
  
8.  En la instancia de servidor principal, establezca la instancia de servidor en `PARTNERHOST5` como asociado (para convertirla en el servidor reflejado inicial):  
  
    ```  
    USE master;  
    GO  
    ALTER DATABASE AdventureWorks   
        SET PARTNER = 'TCP://PARTNERHOST5:7022'  
    GO  
    ```  
  
9. Opcionalmente, si va a usar el modo de alta seguridad con conmutación automática por error, configure la instancia de servidor testigo. Para obtener más información, vea [Agregar un testigo de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md).  
  
> [!NOTE]  
>  Para ver un ejemplo completo en el que se muestra la configuración de seguridad, se prepara la base de datos reflejada, se configuran los asociados y se agrega un testigo, vea [Configurar la creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md).  
  
## <a name="see-also"></a>Consulte también  
 [Configurar la creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Permitir el acceso de red a un punto de conexión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md)   
 [Preparar una base de datos reflejada para la creación de reflejo &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)   
 [Crear un punto de conexión de creación de reflejo de la base de datos para la autenticación de Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)   
 [Creación de reflejo de la base de datos y trasvase de registros &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-log-shipping-sql-server.md)   
 [Creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Replicación y creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md)   
 [Configurar la creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [Especificar una dirección de red de servidor &#40;creación de reflejo de la base de datos&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)   
 [Database Mirroring Operating Modes](../../database-engine/database-mirroring/database-mirroring-operating-modes.md)  
  
  

