---
title: Trasvase de registros y replicación (SQL Server) | Microsoft Docs
description: Obtenga información sobre cómo el trasvase de registros aplica el registro de transacciones de todas las inserciones, actualizaciones o eliminaciones realizadas en la base de datos principal a la base de datos secundaria.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- replication [SQL Server], log shipping and
- log shipping [SQL Server], replication and
ms.assetid: 132bebfd-0206-4d23-829a-b38e5ed17bc9
author: cawrites
ms.author: chadam
ms.openlocfilehash: 96f64ac1ade48962f5f8dd58f0af6f0a7f662b5c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346976"
---
# <a name="log-shipping-and-replication-sql-server"></a>Trasvase de registros y replicación (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  El trasvase de registros incluye dos copias de una sola base de datos que suelen residir en diferentes equipos. En cada momento, solo una copia de la base de datos está disponible para los clientes. Esta copia se conoce como la base de datos principal. Las actualizaciones realizadas por los clientes en la base de datos principal se propagan mediante el trasvase de registros a la otra copia de la base de datos, conocida como la base de datos secundaria. El trasvase de registros incluye la aplicación a la base de datos secundaria del registro de transacciones con todas las inserciones, actualizaciones o eliminaciones efectuadas en la base de datos principal.  
  
 El trasvase de registros se puede usar conjuntamente con la replicación, con el siguiente comportamiento:  
  
-   La replicación no continúa después de producirse una conmutación por error de trasvase de registros. Si se produce una conmutación por error, los agentes de replicación no se conectan a la base de datos secundaria, de modo que no se replican transacciones en los suscriptores. Si se produce una conmutación por recuperación en la base de datos principal, se reanuda la replicación. Todas las transacciones que el trasvase de registros vuelve a copiar desde la base de datos secundaria a la base de datos principal se replican en los suscriptores.  
  
-   Si la base de datos principal se pierde de forma permanente, se puede cambiar el nombre de la base de datos secundaria de manera que se pueda continuar con la replicación. En el resto de este tema, se describen los requisitos y procedimientos necesarios para abordar este caso. El ejemplo presentado corresponde a la base de datos de publicaciones, que es la base de datos a la que se aplica el trasvase de registros con mayor frecuencia, aunque también se puede aplicar un proceso similar a las bases de datos de suscripciones y de distribución.  
  
 Para obtener información sobre la recuperación de las bases de datos involucradas en la replicación sin necesidad de reconfigurar la replicación, vea [Hacer copias de seguridad y restaurar bases de datos replicadas](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md).  
  
> [!NOTE]  
>  Se recomienda utilizar la creación de reflejo de la base de datos, en lugar del trasvase de registros, para proporcionar disponibilidad a la base de datos de publicaciones. Para obtener más información, vea [Replicación y creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md).  
  
## <a name="requirements-and-procedures-for-replicating-from-the-secondary-if-the-primary-is-lost"></a>Requisitos y procedimientos para replicar desde la base de datos secundaria si se pierde la base de datos principal  
 Se deben tener en cuenta los siguientes requisitos y consideraciones:  
  
-   Si la base de datos principal contiene más de una base de datos de publicaciones, trasvase los registros de todas las bases de datos de publicaciones a la misma base de datos secundaria.  
  
-   La ruta de instalación de la instancia del servidor secundario debe ser la misma que la del primario. Las ubicaciones de las bases de datos de usuario en el servidor secundario deben ser las mismas que las del primario.  
  
-   Haga una copia de seguridad de la clave maestra de servicio en la base de datos principal. Esta clave se restaurará en la base de datos secundaria. Para obtener más información, vea [BACKUP SERVICE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/backup-service-master-key-transact-sql.md).  
  
-   El trasvase de registros no garantiza que no se produzca una pérdida de datos. Un error en la base de datos principal puede originar la pérdida de los datos que aún no tienen una copia de seguridad o la pérdida de copias de seguridad durante la ejecución del error.  
  
### <a name="log-shipping-with-transactional-replication"></a>Trasvase de registros con la replicación transaccional  
 En la replicación transaccional, el comportamiento del trasvase de registros depende de la opción **sync with backup** . Esta opción se puede establecer en la base de datos de publicaciones y en la base de datos de distribución. En lo que respecta al trasvase de registros para el publicador, solo tiene relevancia la configuración de la base de datos de publicaciones.  
  
 Establecer esta opción en la base de datos de publicaciones garantiza que las transacciones no se enviarán a la base de datos de distribución hasta que se hayan incluido en la copia de seguridad en la base de datos de publicaciones. La última copia de seguridad de la base de datos de publicaciones se puede restaurar posteriormente en el servidor secundario sin que exista ninguna posibilidad de que la base de datos de distribución contenga transacciones que no existan en la base de datos de publicaciones restaurada. Esta opción garantiza que si el publicador genera un error en el servidor secundario, se mantendrá la coherencia entre el publicador, el distribuidor y los suscriptores. La latencia y el rendimiento se verán afectados porque no será posible entregar transacciones a la base de datos de distribución hasta que se haya hecho una copia de seguridad de ellas en el publicador. Si la aplicación puede tolerar esta latencia, se recomienda establecer esta opción en la base de datos de publicaciones. Si no se establece la opción **sync with backup** , los suscriptores podrían recibir cambios que ya no están incluidos en la base de datos recuperada en el servidor secundario. Para más información, consulte [Estrategias para hacer copias de seguridad y restaurar replicación de instantáneas o replicación transaccional](../../relational-databases/replication/administration/strategies-for-backing-up-and-restoring-snapshot-and-transactional-replication.md).  
  
 **Para configurar la replicación transaccional y el trasvase de registros con la opción sync with backup:**  
  
1.  Si la opción sync with backup no está establecida en la base de datos de publicaciones, ejecute `sp_replicationdboption '<publicationdatabasename>', 'sync with backup', 'true'`. Para obtener más información, vea [sp_replicationdboption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md).  
  
2.  Configure el trasvase de registros para la base de datos de publicaciones. Para obtener más información, vea [Configurar el trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md).  
  
3.  Si se produce un error en el publicador, restaure el último registro de la base de datos en el servidor secundario mediante la opción KEEP_REPLICATION de RESTORE LOG. De esta manera se conservará toda la configuración de la replicación correspondiente a la base de datos. Para obtener más información, vea [Conmutar por error a una base de datos secundaria de trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md) y [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
4.  Restaure la base de datos **msdb** y las bases de datos **maestras** del servidor principal al secundario. Para obtener más información, vea [Realizar copias de seguridad y restaurar bases de datos del sistema &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md). Si la base de datos principal también era un distribuidor, restaure la base de datos de distribución del servidor principal al secundario.  
  
     Las bases de datos deben ser coherentes con la base de datos de publicaciones del servidor principal en lo que respecta a las opciones de configuración de la replicación.  
  
5.  En el servidor secundario, cambie el nombre del equipo y, a continuación, cambie el nombre de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de manera que coincida con el nombre del servidor principal. Para obtener información sobre cómo cambiar el nombre del equipo, vea la documentación de Windows. Para obtener más información sobre cómo cambiar el nombre de servidor, vea [Cambiar el nombre de un equipo que hospeda una instancia independiente de SQL Server](../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md) y [Cambiar el nombre de una instancia de clúster de conmutación por error de SQL Server](../../sql-server/failover-clusters/install/rename-a-sql-server-failover-cluster-instance.md).  
  
6.  En el servidor secundario, restaure la clave maestra de servicio que se copió desde el servidor principal. Para obtener más información, vea [RESTORE SERVICE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-service-master-key-transact-sql.md).  
  
 **Para configurar la replicación transaccional y el trasvase de registros sin la opción sync with backup**  
  
1.  Configure el trasvase de registros para la base de datos de publicaciones. Para obtener más información, vea [Configurar el trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md).  
  
2.  Si se produce un error en el publicador, restaure el último registro de la base de datos en el servidor secundario mediante la opción KEEP_REPLICATION de RESTORE LOG. De esta manera se conservará toda la configuración de la replicación correspondiente a la base de datos. Para obtener más información, vea [Conmutar por error a una base de datos secundaria de trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md) y [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
3.  Restaure la base de datos **msdb** y las bases de datos **maestras** del servidor principal al secundario. Para obtener más información, vea [Realizar copias de seguridad y restaurar bases de datos del sistema &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md). Si la base de datos principal también era un distribuidor, restaure la base de datos de distribución del servidor principal al secundario.  
  
     Las bases de datos deben ser coherentes con la base de datos de publicaciones del servidor principal en lo que respecta a las opciones de configuración de la replicación.  
  
4.  En el servidor secundario, cambie el nombre del equipo y, a continuación, cambie el nombre de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de manera que coincida con el nombre del servidor principal. Para obtener información sobre cómo cambiar el nombre del equipo, vea la documentación de Windows. Para obtener más información sobre cómo cambiar el nombre de servidor, vea [Cambiar el nombre de un equipo que hospeda una instancia independiente de SQL Server](../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md) y [Cambiar el nombre de una instancia de clúster de conmutación por error de SQL Server](../../sql-server/failover-clusters/install/rename-a-sql-server-failover-cluster-instance.md).  
  
     Es probable que reciba un mensaje de error del Agente de registro del LOG comunicándole que la base de datos de publicaciones y la base de datos de distribución no están sincronizadas.  
  
5.  En el servidor secundario, restaure la clave maestra de servicio que se copió desde el servidor principal. Para obtener más información, vea [RESTORE SERVICE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-service-master-key-transact-sql.md).  
  
6.  Ejecute **sp_replrestart**. Este procedimiento almacenado se puede utilizar para forzar que el Agente de registro del LOG omita todas las transacciones replicadas anteriormente en el registro de la base de datos de publicaciones. Las transacciones aplicadas tras la finalización del procedimiento almacenado se procesan mediante el Agente de registro del LOG. Para obtener más información, vea [sp_replrestart &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replrestart-transact-sql.md).  
  
7.  Reinicie el Agente de registro del LOG después de que el procedimiento almacenado se haya ejecutado correctamente. Para obtener más información, vea [Iniciar y detener un agente de replicación &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md).  
  
8.  Las transacciones que ya se distribuyeron al suscriptor podrían aplicarse al publicador. Para asegurarse de que el Agente de distribución no genere un error cuando intente volver a aplicar estas transacciones en el suscriptor, especifique el perfil de agente llamado **Continuar después de errores de coherencia de datos**.  
  
### <a name="log-shipping-with-merge-replication"></a>Trasvase de registros con la replicación de mezcla  
 Realice los pasos del siguiente procedimiento para configurar la replicación de mezcla y el trasvase de registros.  
  
 **Para configurar la replicación de mezcla y el trasvase de registros**  
  
1.  Configure el trasvase de registros para la base de datos de publicaciones. Para obtener más información, vea [Configurar el trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md).  
  
2.  En el publicador que da error, en el servidor secundario cambie el nombre del equipo y, a continuación, cambie el nombre de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de manera que coincida con el nombre del servidor principal. Para obtener información sobre cómo cambiar el nombre del equipo, vea la documentación de Windows. Para obtener más información sobre cómo cambiar el nombre de servidor, vea [Cambiar el nombre de un equipo que hospeda una instancia independiente de SQL Server](../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md) y [Cambiar el nombre de una instancia de clúster de conmutación por error de SQL Server](../../sql-server/failover-clusters/install/rename-a-sql-server-failover-cluster-instance.md).  
  
3.  Restaure el último registro de la base de datos en el servidor secundario mediante la opción KEEP_REPLICATION de RESTORE LOG. De esta manera se conservará toda la configuración de la replicación correspondiente a la base de datos. Para obtener más información, vea [Conmutar por error a una base de datos secundaria de trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md) y [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
4.  Restaure la base de datos **msdb** y las bases de datos **maestras** del servidor principal al secundario. Para obtener más información, vea [Realizar copias de seguridad y restaurar bases de datos del sistema &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md). Si la base de datos principal también era un distribuidor, restaure la base de datos de distribución del servidor principal al secundario.  
  
     Las bases de datos deben ser coherentes con la base de datos de publicaciones del servidor principal en lo que respecta a las opciones de configuración de la replicación.  
  
5.  En el servidor secundario, restaure la clave maestra de servicio que se copió desde el servidor principal. Para obtener más información, vea [RESTORE SERVICE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-service-master-key-transact-sql.md).  
  
6.  Sincronice la base de datos de publicaciones con una o más bases de datos de suscripciones. Esto permite cargar los cambios realizados anteriormente en la base de datos de publicaciones, pero que no se muestran en la copia de seguridad restaurada. Los datos que se pueden cargar dependen de la forma en que se filtra una publicación:  
  
    -   Si la publicación no está filtrada, debe poder actualizar la base de datos de publicaciones sincronizándola con el suscriptor más actualizado.  
  
    -   Si la publicación está filtrada, es posible que no pueda actualizar la base de datos de publicaciones. Considere una tabla dividida de forma que cada suscripción reciba únicamente datos de clientes de una región: norte, este, sur y oeste. Si hay al menos un suscriptor para cada partición de datos, al sincronizar cada partición con un suscriptor se debería actualizar la base de datos de publicaciones. Sin embargo, si los datos de la partición oeste, por ejemplo, no se han replicado en ningún suscriptor, no se podrán actualizar estos datos en el publicador. En este caso, se recomienda reinicializar todas las suscripciones de manera que los datos del publicador y de los suscriptores converjan. Para obtener más información, vea [Reinicializar suscripciones](../../relational-databases/replication/reinitialize-subscriptions.md).  
  
     Si se sincroniza con un suscriptor que ejecute una versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] anterior a [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], la suscripción no puede ser anónima, sino que debe ser una suscripción de cliente o de servidor (llamadas suscripciones locales y suscripciones globales en versiones anteriores). Para obtener más información, vea [Sincronizar datos](../../relational-databases/replication/synchronize-data.md).  
  
## <a name="see-also"></a>Consulte también  
 [Replicación de SQL Server](../../relational-databases/replication/sql-server-replication.md)   
 [Acerca del trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Replicación y creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md)  
  
  
