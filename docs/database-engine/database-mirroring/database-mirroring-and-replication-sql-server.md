---
title: Replicación y creación de reflejo de la base de datos (SQL Server) | Microsoft Docs
description: Obtenga información sobre cómo utilizar la creación de reflejo de la base de datos conjuntamente con la replicación para mejorar la disponibilidad para la base de datos de publicación en SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], interoperability
- replication [SQL Server], database mirroring and
ms.assetid: 82796217-02e2-4bc5-9ab5-218bae11a2d6
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 16746834acfe8a53511166a35667f136b561c28a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353183"
---
# <a name="database-mirroring-and-replication-sql-server"></a>Replicación y creación de reflejo de la base de datos (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  La creación de reflejo de la base de datos se puede usar conjuntamente con la replicación para mejorar la disponibilidad para la base de datos de publicación. La creación de reflejo de la base de datos incluye la creación de dos copias de una sola base de datos que suelen residir en diferentes equipos. En cada momento, solo una copia de la base de datos está disponible para los clientes. Esta copia se conoce como la base de datos principal. Las actualizaciones realizadas por los clientes en la base de datos de la entidad de seguridad se aplican a la otra copia de la base de datos, conocida como la base de datos reflejada. La creación de reflejo incluye la aplicación a la base de datos reflejada del registro de transacciones con todas las inserciones, actualizaciones o eliminaciones efectuadas en la base de datos de la entidad de seguridad.  
  
 La conmutación por error de replicación en un reflejo se admite totalmente para las bases de datos de publicación, con compatibilidad limitada con las bases de datos de suscripciones. La creación de reflejo de la base de datos no se admite para la base de datos de distribución. Para obtener información sobre la recuperación de una base de datos de distribución o una base de datos de suscripciones sin necesidad de volver configurar la replicación, vea [Back Up and Restore Replicated Databases (Realizar copias de seguridad y restaurar bases de datos replicadas)](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md).   
  
> [!NOTE]  
>  Después de una conmutación por error, la entidad reflejada se convierte en la entidad de seguridad. En este tema, los términos "entidad de seguridad" y "reflejada" siempre hacen referencia a las entidades de seguridad y reflejada originales.  
  
## <a name="requirements-and-considerations-for-using-replication-with-database-mirroring"></a>Requisitos y consideraciones para el uso de la replicación con la creación de reflejo de la base de datos  
 Se deben tener en cuenta los siguientes requisitos y consideraciones al utilizar la replicación con la creación de reflejo de la base de datos:  
  
-   Las entidades de seguridad y reflejada deben compartir un distribuidor. Se recomienda que éste sea un distribuidor remoto, ya que proporciona mayor tolerancia a errores si se produce una conmutación por error imprevista en el publicador.  
  
-   La replicación admite la creación de reflejo de la base de datos de publicación en la replicación de mezcla y en la replicación transaccional con suscriptores de solo lectura o suscriptores de actualización en cola. No se admiten suscriptores de actualización inmediata, publicadores de Oracle, publicadores en una topología punto a punto ni republicación.  
  
-   Los metadatos y los objetos que existen fuera de la base de datos, incluidos inicios de sesión, trabajos, servidores vinculados, etc., no se copian en la entidad reflejada. Si se requieren los metadatos y los objetos en la entidad reflejada, se deben copiar manualmente. Para obtener más información, vea [Administración de inicios de sesión y trabajos tras la conmutación de roles &#40;SQL Server&#41;](../../sql-server/failover-clusters/management-of-logins-and-jobs-after-role-switching-sql-server.md).  
  
## <a name="configuring-replication-with-database-mirroring"></a>Configurar la replicación con la creación de reflejo de la base de datos  
 La configuración de la replicación y la creación de reflejo de la base de datos implica cinco pasos. Cada paso se describe en detalle en la siguiente sección.  
  
1.  Configurar el publicador  
  
2.  Configurar la creación de reflejo de la base de datos.  
  
3.  Configurar la entidad reflejada de manera que utilice el mismo distribuidor que la entidad de seguridad  
  
4.  Configurar los agentes de replicación para la conmutación por error  
  
5.  Agregue las entidades de seguridad y reflejada al Monitor de replicación.  
  
 El orden de los pasos 1 y 2 se puede invertir.  
  
#### <a name="to-configure-database-mirroring-for-a-publication-database"></a>Para configurar la creación de reflejo de la base de datos para una base de datos de publicación  
  
1.  Configure el publicador:  
  
    1.  Se recomienda el uso de un distribuidor remoto. Para obtener más información sobre cómo configurar la distribución, vea [Configure Distribution (Configurar la distribución)](../../relational-databases/replication/configure-distribution.md).  
  
    2.  Se puede habilitar una base de datos para publicaciones transaccionales y de instantáneas y/o para publicaciones de combinación. Para las bases de datos reflejadas que incluirán más de un tipo de publicación, se debe habilitar la base de datos para ambos tipos en el mismo nodo mediante [sp_replicationdboption](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md). Por ejemplo, puede ejecutar el siguiente procedimiento almacenado en la entidad de seguridad:  
  
        ```  
        exec sp_replicationdboption @dbname='<PublicationDatabase>', @optname='publish', @value=true;  
        exec sp_replicationdboption @dbname='<PublicationDatabase>', @optname='mergepublish', @value=true;  
        ```  
  
         Para obtener más información sobre la creación de publicaciones, vea [Publish Data and Database Objects (Publicar datos y objetos de base de datos)](../../relational-databases/replication/publish/publish-data-and-database-objects.md).  
  
2.  Configurar la creación de reflejo de la base de datos. Para obtener más información, vea [Establecer una sesión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md) y [Configurar la creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md).  
  
3.  Configurar la distribución para la entidad reflejada. Indique el nombre de la entidad reflejada como el publicador y especifique el mismo distribuidor y la misma carpeta de instantáneas que se utilizan en la entidad de seguridad. Por ejemplo, si está configurando la replicación con procedimientos almacenados, ejecute [sp_adddistpublisher](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md) en el distribuidor y, después, ejecute [sp_adddistributor](../../relational-databases/system-stored-procedures/sp-adddistributor-transact-sql.md) en la entidad reflejada. Para **sp_adddistpublisher**:  
  
    -   Establezca el valor del parámetro **\@publisher** en el nombre de red de la entidad reflejada.  
  
    -   Establezca el valor del parámetro **\@working_directory** en la carpeta de instantáneas que se usa en la entidad de seguridad.  
  
4.  Especifique el nombre de la entidad reflejada para el parámetro de agente **- PublisherFailoverPartner**. Este parámetro es necesario para que los siguientes agentes identifiquen la entidad reflejada después de una conmutación por error:  
  
    -   Agente de instantáneas (para todas las publicaciones)  
  
    -   Agente de registro del LOG (para todas las publicaciones transaccionales)  
  
    -   Agente de lectura de cola (para las publicaciones transaccionales que admiten suscripciones de actualización en cola)  
  
    -   Agente de mezcla (para suscripciones de mezcla)  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Escucha de replicación (replisapi.dll: para suscripciones de mezcla sincronizadas mediante sincronización web)  
  
    -   Control ActiveX de mezcla de SQL (para suscripciones de mezcla sincronizadas con el control)  
  
     El Agente de distribución y el Control ActiveX de distribución de SQL no tienen este parámetro porque no se conectan al publicador.  
  
     Los cambios en los parámetros del agente tendrán efecto la próxima vez que se inicie el agente. Si el agente se ejecuta sin interrupción, debe detenerlo y reiniciarlo. Los parámetros se pueden especificar en perfiles de agente y desde el símbolo del sistema. Para más información, consulte:  
  
    -   [Ver y modificar parámetros del símbolo del sistema de los agentes de replicación &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/agents/view-and-modify-replication-agent-command-prompt-parameters.md)  
  
    -   [Conceptos de los ejecutables del Agente de replicación](../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)  
  
     Se recomienda agregar el parámetro **-PublisherFailoverPartner** a un perfil de agente y, después, especificar el nombre de la entidad reflejada en el perfil. Por ejemplo, si configura la replicación con procedimientos almacenados:  
  
    ```  
    -- Execute sp_help_agent_profile in the context of the distribution database to get the list of profiles.  
    -- Select the profile id of the profile that needs to be updated from the result set.  
    -- In the agent_type column returned by sp_help_agent_profile:   
    -- 1 = Snapshot Agent; 2 = Log Reader Agent; 3 = Distribution Agent; 4 = Merge Agent; 9 = Queue Reader Agent.  
  
    exec sp_help_agent_profile;  
  
    -- Setting the -PublisherFailoverPartner parameter in the default Snapshot Agent profile (profile 1).  
    -- Execute sp_add_agent_parameter in the context of the distribution database.  
    exec sp_add_agent_parameter @profile_id = 1, @parameter_name = N'-PublisherFailoverPartner', @parameter_value = N'<Failover Partner Name>';  
  
    -- Setting the -PublisherFailoverPartner parameter in the default Merge Agent profile (profile 6).  
    -- Execute sp_add_agent_parameter in the context of the distribution database.  
    exec sp_add_agent_parameter @profile_id = 6, @parameter_name = N'-PublisherFailoverPartner', @parameter_value = N'<Failover Partner Name>';  
    ```  
  
5.  Agregue las entidades de seguridad y reflejada al Monitor de replicación. Para obtener más información, vea [Agregar y quitar publicadores del Monitor de replicación](../../relational-databases/replication/monitor/add-and-remove-publishers-from-replication-monitor.md).  
  
## <a name="maintaining-a-mirrored-publication-database"></a>Mantener una base de datos de publicación reflejada  
 El mantenimiento de una base de datos de publicación reflejada se realiza básicamente de la misma forma que para una base de datos no reflejada, con las siguientes salvedades:  
  
-   La administración y la supervisión deben tener lugar en el servidor activo. En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], las publicaciones aparecen debajo de la carpeta **Publicaciones locales** solo para el servidor activo. Por ejemplo, si produce la conmutación por error a la entidad reflejada, las publicaciones se muestran en la entidad reflejada y dejan de aparecer en la entidad de seguridad. Si se produce la conmutación por error de la base de datos a la entidad reflejada, puede que sea necesario actualizar manualmente [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] y el Monitor de replicación para que se refleje el cambio.  
  
-   El Monitor de replicación muestra los nodos del publicador en el árbol de objetos de la entidad de seguridad y reflejada. Si la entidad de seguridad es el servidor activo, la información de publicación solo se mostrará debajo del nodo de la entidad de seguridad en el Monitor de replicación.  
  
     Si la entidad reflejada es el servidor activo:  
  
    -   Si se produce un error en un agente, solo se indicará en el nodo de la entidad de seguridad, no en el nodo de la entidad reflejada.  
  
    -   Si la entidad de seguridad no está disponible, los nodos de la entidad de seguridad y reflejada muestran listas de publicaciones idénticas. La supervisión debe realizarse en las publicaciones debajo del nodo de la entidad reflejada.  
  
-   Si se utilizan procedimientos almacenados o Replication Management Objects (RMO) para administrar la replicación en la entidad reflejada, en los casos en que se especifica el nombre del publicador, se debe especificar el nombre de la instancia en la que la base de datos se habilitó para la replicación. Para determinar el nombre correcto, use la función [publishingservername](../../t-sql/functions/replication-functions-publishingservername.md).  
  
     Cuando se crea el reflejo de una base de datos de publicación, los metadatos de la replicación que se encuentran almacenados en la base de datos reflejada son idénticos a los que se encuentran almacenados en la base de datos de la entidad de seguridad. En consecuencia, para las bases de datos de publicación habilitadas para replicación en la entidad de seguridad, el nombre de la instancia del publicador que está almacenado en las tablas del sistema en la entidad reflejada es el nombre de la entidad de seguridad, en lugar del nombre de la entidad reflejada. Esto afecta a la configuración y al mantenimiento de la replicación si se produce la conmutación por error de la base de datos de publicación a la entidad reflejada. Por ejemplo, si se configura la replicación con procedimientos almacenados en la entidad reflejada después de una conmutación por error y se quiere agregar una suscripción de extracción a una base de datos de publicación que estaba habilitada en la entidad de seguridad, se debe especificar el nombre de la entidad de seguridad, en lugar del nombre de la entidad reflejada, para el parámetro **\@publisher** de **sp_addpullsubscription** o **sp_addmergepullsubscription**.  
  
     Si se habilita una base de datos de publicación en la entidad reflejada después de una conmutación por error a dicha entidad, el nombre de la instancia del publicador que está almacenado en las tablas del sistema es el nombre de la entidad reflejada. En este caso, se debe usar el nombre de la entidad reflejada para el parámetro **\@publisher**.  
  
    > [!NOTE]  
    >  En algunos casos, por ejemplo **sp_addpublication**, el parámetro **\@publisher** solo se admite para publicadores que no sean de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En estos casos, no es relevante para la creación de reflejo de la base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Para sincronizar una suscripción en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] tras una conmutación por error: sincronice las suscripciones de extracción del suscriptor y sincronice las suscripciones de inserción del publicador activo.  
  
### <a name="replication-behavior-if-mirroring-is-removed"></a>Comportamiento de la replicación si se quita la creación de reflejo  
 Tenga en cuenta las siguientes consideraciones si se quita la creación de reflejo de la base de datos de una base de datos publicada:  
  
-   Si la base de datos de publicación de la entidad de seguridad ya no está reflejada, la replicación continúa funcionando sin variaciones con la entidad de seguridad original.  
  
-   Si se produce la conmutación por error de la base de datos de publicación de la entidad de seguridad a la entidad reflejada y, por consiguiente, se deshabilita o quita la relación de creación de reflejo, los agentes de replicación no funcionarán con la entidad reflejada. Si la entidad de seguridad se pierde de forma permanente, deshabilite y, a continuación, vuelva a configurar la replicación con la entidad reflejada especificada como publicador.  
  
-   Si se quita por completo la creación de reflejo de la base de datos, la base de datos reflejada se encontrará en un estado de recuperación y deberá restaurarse para ser funcional. El comportamiento de la base de datos recuperada con respecto a la replicación depende de si se ha especificado la opción KEEP_REPLICATION. Esta opción obliga a la operación de restauración a conservar la configuración de la replicación cuando restaure una base de datos publicada en un servidor distinto del servidor en el que se creó la copia de seguridad. Utilice la opción KEEP_REPLICATION solo cuando la otra base de datos de publicación no esté disponible. Esta opción no es compatible si la otra base de datos de publicación sigue intacta y continúa con la replicación. Para obtener más información sobre KEEP_REPLICATION, vea [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
## <a name="log-reader-agent-behavior"></a>Comportamiento del Agente de registro del LOG  
 En la siguiente tabla se describe el comportamiento del Agente de registro del LOG en los distintos modos de funcionamiento de la creación de reflejo de la base de datos.  
  
|Modo de funcionamiento|Comportamiento del Agente de registro del LOG si la entidad reflejada no está disponible|  
|--------------------|------------------------------------------------------------|  
|Modo de seguridad alta con conmutación automática por error|Si la entidad reflejada no está disponible, el Agente de registro del LOG propaga los comandos a la base de datos de distribución. La entidad de seguridad no puede realizar la conmutación por error a la entidad reflejada hasta que la entidad reflejada vuelva a estar en línea e incluya todas las transacciones de la entidad de seguridad.|  
|Modo de alto rendimiento|Si la entidad reflejada no está disponible, la base de datos de la entidad de seguridad se ejecuta de forma expuesta (es decir, sin reflejo). Sin embargo, el Agente de registro del LOG solo replica las transacciones reforzadas en la entidad reflejada. Si se fuerza el servicio y el servidor reflejado asume el rol de la entidad de seguridad, el Agente de registro del LOG trabajará con la entidad reflejada y comenzará a recoger las transacciones nuevas.<br /><br /> Tenga en cuenta que aumentará la latencia de replicación si la entidad reflejada se retrasa con respecto a la entidad de seguridad.|  
|Modo de alta seguridad sin conmutación automática por error|Se garantiza que todas las transacciones confirmadas se refuerzan en disco en la entidad reflejada. El Agente de registro del LOG solo replica las transacciones reforzadas en la entidad reflejada. Si la entidad reflejada no está disponible, la entidad de seguridad no permite que continúe la actividad en la base de datos; por lo tanto, el Agente de registro del LOG no contará con transacciones para replicar.|  
  
## <a name="see-also"></a>Consulte también  
 [Replicación de SQL Server](../../relational-databases/replication/sql-server-replication.md)   
 [Trasvase de registros y replicación &#40;SQL Server&#41;](../../database-engine/log-shipping/log-shipping-and-replication-sql-server.md)  
  
  
