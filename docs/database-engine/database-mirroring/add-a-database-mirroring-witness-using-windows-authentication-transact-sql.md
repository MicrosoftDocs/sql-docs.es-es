---
title: Configuración de un testigo
description: 'Describe cómo configurar un testigo de creación de reflejo de la base de datos con autenticación de Windows mediante Transact-SQL. '
ms.custom: seo-lt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- witness [SQL Server], establishing
- Windows authentication [SQL Server]
- database mirroring [SQL Server], witness
ms.assetid: bf5e87df-91a4-49f9-ae88-2a6dcf644510
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: a5702769994efd65abeeb330205a2a83362fee6e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353257"
---
# <a name="add-a-database-mirroring-witness-using-windows-authentication-transact-sql"></a>Agregar un testigo de creación de reflejo de la base de datos mediante la autenticación de Windows (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Para configurar un testigo para la base de datos, el propietario de ésta debe asignar una instancia del Motor de base de datos al rol de servidor testigo. La instancia de servidor testigo puede ejecutarse en el mismo equipo que la instancia de servidor principal o reflejado, aunque de este modo se reduce la solidez de la conmutación automática por error.  
  
 Se recomienda que el testigo se aloje en un equipo independiente. Un servidor determinado puede participar en varias sesiones simultáneas de creación de reflejo de la base de datos con el mismo asociado o con asociados distintos. Un servidor específico puede ser asociado en algunas sesiones y testigo en otras.  
  
 El testigo está pensado exclusivamente para el modo de alta seguridad con conmutación automática por error. Antes de definir un testigo, es recomendable que se asegure de que la propiedad SAFETY está definida actualmente como FULL.  
  
> [!IMPORTANT]  
>  Se recomienda configurar la creación de reflejo de la base de datos durante los periodos de poca actividad, dado que este proceso puede afectar al rendimiento.  
  
## <a name="establish-a-witness"></a>Establecimiento de un testigo  
  
1.  En la instancia de servidor testigo, asegúrese de que existe un extremo para la creación de reflejo de la base de datos. Independientemente del número de sesiones de creación de reflejo que deban admitirse, la instancia del servidor solo debe tener un extremo de creación de reflejo de la base de datos. Si tiene previsto usar esta instancia de servidor exclusivamente como testigo en las sesiones de creación de reflejo de la base de datos, asigne el rol de testigo al punto de conexión (ROLE **=** WITNESS). Si lo que quiere es utilizar la instancia de servidor como asociado en una o más sesiones de creación de reflejo de la base de datos, asigne el rol del extremo a ALL.  
  
     Para ejecutar una instrucción SET WITNESS, es preciso haber iniciado previamente la sesión de creación de reflejo de la base de datos (entre los asociados) y el valor STATE del extremo del testigo debe establecerse en STARTED.  
  
     Para saber si la instancia del servidor testigo tiene su extremo de creación de reflejo de la base de datos y para conocer su rol y su estado, utilice la siguiente instrucción Transact-SQL en dicha instancia:  
  
    ```  
    SELECT role_desc, state_desc FROM sys.database_mirroring_endpoints  
    ```  
  
    > [!IMPORTANT]  
    >  Si existe un extremo para la creación de reflejo de la base de datos y ya se está utilizando, se recomienda utilizar ese extremo para cada sesión en la instancia de servidor. Si quita un extremo en uso, se interrumpirán las conexiones de las sesiones existentes. Si se ha establecido un testigo para una sesión, la eliminación del extremo de creación de reflejo de la base de datos puede hacer que el servidor principal de esa sesión pierda quórum; si sucede esto, la base de datos se queda sin conexión y sus usuarios quedan desconectados. Para más información, vea [Cuórum: cómo un testigo afecta a la disponibilidad de la base de datos &#40;creación de reflejo de la base de datos&#41;](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md).  
  
     Si el asociado carece de un punto de conexión, vea [Crear un punto de conexión de creación de reflejo de la base de datos para la autenticación de Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md).  
  
2.  Si las instancias de asociados se ejecutan en cuentas de usuario de distintos dominios, cree un inicio de sesión para las diferentes cuentas de la base de datos maestra de cada instancia. Para obtener más información, vea [Permitir el acceso de red a un punto de conexión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md).  
  
3.  Conéctese al servidor principal y emita la siguiente instrucción:  
  
     ALTER DATABASE *<nombre_de_base_de_datos>* SET WITNESS **=** _<dirección_de_red_de_servidor>_  
  
     donde *<nombre_de_base_de_datos>* es el nombre de la base de datos de la que se va a crear el reflejo (este nombre es el mismo para ambos asociados) y *<dirección_de_red_de_servidor>* es la dirección de red de servidor de la instancia del servidor testigo.  
  
     La sintaxis para una dirección de red de servidor es la siguiente:  
  
     TCP <b>://</b> _\<system-address>_ <b>:</b> _\<port>_  
  
     donde \<*system-address>* es una cadena que identifica de forma inequívoca el sistema del equipo de destino y \<*port>* es el número de puerto que usa el punto de conexión de la creación de reflejo de la instancia de servidor asociado. Para obtener más información, vea [Especificar una dirección de red de servidor &#40;creación de reflejo de la base de datos&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).  
  
     Por ejemplo, en la instancia del servidor principal, la siguiente instrucción ALTER DATABASE define el testigo. El nombre de la base de datos es **AdventureWorks**, la dirección del sistema es DBSERVER3 (el nombre del sistema testigo) y el puerto que usa el punto de conexión de creación de reflejo de la base de datos del testigo es `7022`:  
  
    ```  
    ALTER DATABASE AdventureWorks   
      SET WITNESS = 'TCP://DBSERVER3:7022'  
    ```  
  
## <a name="example"></a>Ejemplo  
 En el siguiente ejemplo se establece un testigo de creación de reflejo de datos. En la instancia del servidor testigo (instancia predeterminada en `WITNESSHOST4`):  
  
1.  Cree un extremo para esta instancia de servidor solo para el rol WITNESS usando el puerto `7022`.  
  
    ```  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=WITNESS)  
    GO  
    ```  
  
2.  Cree un inicio de sesión para la cuenta de usuario de dominio de las instancias de asociados, si son diferentes; por ejemplo, suponga que el testigo se está ejecutando como `SOMEDOMAIN\witnessuser`, pero los asociados se están ejecutando como `MYDOMAIN\dbousername`. Cree un inicio de sesión para los asociados, como se indica a continuación:  
  
    ```  
    --Create a login for the partner server instances,  
    --which are both running as MYDOMAIN\dbousername:  
    USE master ;  
    GO  
    CREATE LOGIN [MYDOMAIN\dbousername] FROM WINDOWS ;  
    GO  
    --Grant connect permissions on endpoint to login account   
    --of partners  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [MYDOMAIN\dbousername];  
    GO  
    ```  
  
3.  En cada instancia del servidor asociado, cree un inicio de sesión para la instancia del servidor testigo:  
  
    ```  
    --Create a login for the witness server instance,  
    --which is running as SOMEDOMAIN\witnessuser:  
    USE master ;  
    GO  
    CREATE LOGIN [SOMEDOMAIN\witnessuser] FROM WINDOWS ;  
    GO  
    --Grant connect permissions on endpoint to login account   
    --of partners  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [SOMEDOMAIN\witnessuser];  
    GO  
    ```  
  
4.  En el servidor principal, establezca el testigo (que se encuentra en `WITNESSHOST4`):  
  
    ```  
    ALTER DATABASE AdventureWorks   
        SET WITNESS =   
        'TCP://WITNESSHOST4:7022'  
    GO  
    ```  
  
> [!NOTE]  
>  La dirección de red del servidor indica la instancia del servidor de destino mediante el número de puerto, que se asigna al extremo de creación de reflejo de la instancia.  
  
 Para ver un ejemplo completo en el que se muestra la configuración de seguridad, se prepara la base de datos reflejada, se configuran los asociados y se agrega un testigo, vea [Configurar la creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md).  
  
## <a name="see-also"></a>Consulte también  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Permitir el acceso de red a un punto de conexión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md)   
 [Crear un punto de conexión de creación de reflejo de la base de datos para la autenticación de Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)   
 [Establecer una sesión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)   
 [Quitar el testigo de una sesión de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/remove-the-witness-from-a-database-mirroring-session-sql-server.md)   
 [Testigo de creación de reflejo de la base de datos](../../database-engine/database-mirroring/database-mirroring-witness.md)  
  
  
