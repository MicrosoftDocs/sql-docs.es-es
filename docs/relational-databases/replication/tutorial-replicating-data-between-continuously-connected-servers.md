---
title: 'Tutorial: Configuración de replicación transaccional'
description: En este tutorial se explica cómo configurar la replicación transaccional entre dos servidores de SQL Server conectados completamente.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- tutorials [SQL Server replication]
- replication [SQL Server], tutorials
- wizards [SQL Server replication]
ms.assetid: 7b18a04a-2c3d-4efe-a0bc-c3f92be72fd0
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: e2a27d4fccfb24aff29668651ea914e11429910f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347119"
---
# <a name="tutorial-configure-replication-between-two-fully-connected-servers-transactional"></a>Tutorial: Configuración de la replicación entre dos servidores conectados completamente (transaccional)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
La replicación transaccional es una buena solución para el problema de mover datos entre servidores conectados de forma continua. Mediante el Asistente para replicación, puede configurar y administrar fácilmente una topología de replicación. 

Este tutorial le mostrará cómo configurar una topología de replicación transaccional para servidores conectados de forma continua. Para más información sobre cómo funciona la replicación transaccional, vea [Replicación transaccional](./transactional/transactional-replication.md). 
  
## <a name="what-you-will-learn"></a>Aprendizaje  
En este tutorial aprenderá a publicar datos de una base de datos en otra mediante la replicación transaccional.  

En este tutorial, aprenderá a:
> [!div class="checklist"]
> * Crear un publicador mediante la replicación transaccional
> * Crear un suscriptor para el publicador transaccional
> * Validar la suscripción y medir la latencia
  
  
## <a name="prerequisites"></a>Prerrequisitos  
Este tutorial es para usuarios que están familiarizados con las operaciones básicas de las bases de datos, pero tienen poca experiencia con operaciones de replicación. Antes de empezar este tutorial, debe haber completado [Tutorial: Preparación de SQL Server para la replicación](../../relational-databases/replication/tutorial-preparing-the-server-for-replication.md).  
  
Para completar este tutorial, necesita tener SQL Server, SQL Server Management Studio (SSMS) y una base de datos de AdventureWorks:  
  
- En el servidor del publicador (origen), instale:  
  
   - Cualquier edición de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], excepto SQL Server Express o SQL Server Compact. Estas ediciones no pueden ser publicadores de replicación.   
   - La base de datos de ejemplo [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] . Con el objeto de mejorar la seguridad, las bases de datos de ejemplo no se instalan de forma predeterminada.  
  
- En el servidor del suscriptor (destino), instale cualquier edición de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], excepto [!INCLUDE[ssEW](../../includes/ssew-md.md)]. [!INCLUDE[ssEW](../../includes/ssew-md.md)] no puede ser un suscriptor de una replicación transaccional.  
  
- Instale [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md).
- Instale [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Descargue la [base de datos de ejemplo AdventureWorks](https://github.com/Microsoft/sql-server-samples/releases). Para obtener instrucciones sobre cómo restaurar una base de datos en SSMS, vea [Restaurar una copia de seguridad de base de datos con SSMS](../backup-restore/restore-a-database-backup-using-ssms.md). 
 
>[!NOTE]
> - La replicación no se admite entre instancias de SQL Server que estén separadas por más de dos versiones entre sí. Para más información, vea la entrada de blog [Supported SQL Server versions in Replication Topology](https://blogs.msdn.microsoft.com/repltalk/2016/08/12/suppported-sql-server-versions-in-replication-topology/) (Versiones de SQL Server admitidas en la topología de replicación).
> - En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], debe conectarse al publicador y al suscriptor con un inicio de sesión que sea miembro del rol fijo de servidor **sysadmin**. Para más información sobre este rol, vea [Roles de nivel de servidor](../security/authentication-access/server-level-roles.md).  
  
  
**Tiempo estimado para completar este tutorial: 60 minutos**  
  
## <a name="configure-the-publisher-for-transactional-replication"></a>Configurar el publicador para la replicación transaccional
En esta sección, se crea una publicación transaccional mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] para publicar un subconjunto filtrado de la tabla **Product** en la base de datos de ejemplo [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. También se agrega un inicio de sesión de SQL Server que usa el Agente de distribución a la lista de acceso a la publicación (PAL).


### <a name="create-a-publication-and-define-articles"></a>Crear publicaciones y definir artículos
1. Conéctese al publicador en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] y, después, expanda el nodo de servidor.  
  
2. Haga clic con el botón derecho en **Agente SQL Server** y seleccione **Iniciar**. El Agente SQL Server debe estar ejecutándose antes de crear la publicación. Si esto no inicia el agente, deberá hacerlo manualmente desde el Administrador de configuración de SQL Server. 
3. Expanda la carpeta **Replicación**, haga clic con el botón derecho en la carpeta **Publicaciones locales** y, después, seleccione **Nueva publicación**. Este paso inicia el Asistente para nueva publicación:  

   ![Selecciones para iniciar el Asistente para nueva publicación](media/tutorial-replicating-data-between-continuously-connected-servers/newpublication.png)
  
  
3. En la página **Base de datos de publicación**, seleccione [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] y haga clic en **Siguiente**.  
  
4. En la página **Tipo de publicación**, seleccione **Publicación transaccional** y, después, seleccione **Siguiente**:  

   ![Página "Tipo de publicación" con el tipo de publicación seleccionado](media/tutorial-replicating-data-between-continuously-connected-servers/tranrepl.png)
  
5. En la página **Artículos**, expanda el nodo **Tablas** y seleccione la casilla **Producto**. Después, expanda **Producto** y desactive las casillas junto a **ListPrice** y **StandardCost**. Seleccione **Next** (Siguiente).  

   ![Página "Artículos" con artículos seleccionados para publicar](media/tutorial-replicating-data-between-continuously-connected-servers/replarticles.png)
  
6. En la página **Filtrar filas de tabla**, seleccione **Agregar**.   
  
7. En el cuadro de diálogo **Agregar filtro**, seleccione la columna **SafetyStockLevel**. Seleccione la flecha derecha para agregar la columna a la cláusula WHERE de la instrucción de filtro en la consulta del filtro. Después, escriba manualmente en el modificador de la cláusula WHERE lo siguiente:  
  
   ```sql  
   WHERE [SafetyStockLevel] < 500  
   ```
  
   ![Página "Filtrar flujos de tabla" y el cuadro de diálogo "Agregar filtro"](media/tutorial-replicating-data-between-continuously-connected-servers/filter.png)
  
8. Seleccione **Aceptar** y después **Siguiente**.  
  
9. Active la casilla **Crear una instantánea inmediatamente y mantenerla disponible para inicializar suscripciones** y seleccione **Siguiente**:  

   ![Página "Agente de instantáneas" con la casilla seleccionada](media/tutorial-replicating-data-between-continuously-connected-servers/snapshot.png)
  
10. En la página **Seguridad del agente**, desactive la casilla **Use the security settings from the Snapshot Agent** (Usar la configuración de seguridad del Agente de instantáneas).   
  
    Seleccione **Configuración de seguridad** para el Agente de instantáneas. Escriba <*Nombre_De_Equipo_Publicador>* > **\repl_snapshot** en el cuadro **Cuenta de proceso**, escriba la contraseña de la cuenta y, luego, seleccione **Aceptar**.  

    ![Página "Seguridad del agente" y cuadro de diálogo "Seguridad del Agente de instantáneas"](media/tutorial-replicating-data-between-continuously-connected-servers/snapshotagentsecurity.png)
  
12. Repita el paso anterior para establecer <*Nombre_De_Equipo_Publicador*> **\repl_logreader** como la cuenta de proceso para el Agente de registro del LOG. Después, seleccione **Aceptar**.  

    ![Cuadro de diálogo "Seguridad del Agente de registro del LOG" y página "Seguridad del agente"](media/tutorial-replicating-data-between-continuously-connected-servers/logreaderagentsecurity.png)   

  
13. En la página **Complete the Wizard** (Finalización del asistente), escriba **AdvWorksProductTrans** en el cuadro **Nombre de publicación** y seleccione **Finalizar**:  

    ![Página "Complete the Wizard" (Finalización del asistente) con el nombre de la publicación](media/tutorial-replicating-data-between-continuously-connected-servers/advworksproducttrans.png)
  
14. Una vez se haya creado la publicación, seleccione **Cerrar** para finalizar el asistente. 

Podría producirse el siguiente error si el Agente SQL Server no se está ejecutando cuando intenta crear la publicación. Este error indica que la publicación se creó correctamente, pero no se pudo iniciar el Agente de instantáneas. Si esto ocurre, debe iniciar al Agente SQL Server y, después, iniciar manualmente el Agente de instantáneas. En la siguiente sección se proporcionan instrucciones. 

![Advertencia de que el Agente de instantáneas no se pudo iniciar](media/tutorial-replicating-data-between-continuously-connected-servers/snapshotagenterror.png)
    
  
### <a name="view-the-status-of-snapshot-generation"></a>Ver el estado de la generación de instantáneas  
  
1. Conéctese al publicador en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], expanda el nodo del servidor y luego la carpeta **Replicación**.  
  
2. En la carpeta **Publicaciones locales**, haga clic con el botón derecho en **AdvWorksProductTrans** y luego seleccione **Ver estado del Agente de instantáneas**:  
   ![Comando del menú contextual para ver el estado del Agente de instantáneas](media/tutorial-replicating-data-between-continuously-connected-servers/viewsnapshot.png)
  
3. Se muestra el estado actual del trabajo del Agente de instantáneas para la publicación. Compruebe que el trabajo de instantáneas sea correcto antes de continuar con la siguiente sección.
          
Si el Agente SQL Server no se estaba ejecutando cuando creó la publicación, verá que el Agente de instantáneas nunca se ejecutó cuando compruebe el estado del Agente de instantáneas para su publicación. Si es así, seleccione **Iniciar** para iniciar el Agente de instantáneas: 

![Botón "Iniciar" y cambio en el mensaje de estado para indicar que se ha ejecutado el Agente de instantáneas](media/tutorial-replicating-data-between-continuously-connected-servers/startsnapshotagent.png)
     
Si ve un error aquí, vea [Troubleshooting Snapshot Agent errors](troubleshoot-tran-repl-errors.md#find-errors-with-the-snapshot-agent) (Solucionar errores con el Agente de instantáneas).


  
### <a name="add-the-distribution-agent-login-to-the-pal"></a>Agregar el inicio de sesión del Agente de distribución a la lista de acceso de la publicación (PAL)  
  
1. Conéctese al publicador en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], expanda el nodo del servidor y luego la carpeta **Replicación**.  
  
2. En la carpeta **Publicaciones locales**, haga clic con el botón derecho en **AdvWorksProductTrans** y luego seleccione **Propiedades**.  Aparece el cuadro de diálogo **Propiedades de la publicación**.    
  
   a. Seleccione la página **Lista de acceso a la publicación** y seleccione **Agregar**.  
   b. En el cuadro de diálogo **Agregar acceso de publicación**, seleccione <*Nombre_De_Equipo_Publicador>* > **\repl_distribution** y seleccione **Aceptar**.
   
   ![Selecciones para agregar un inicio de sesión a la lista de acceso a la publicación (PAL).](media/tutorial-replicating-data-between-continuously-connected-servers/tranreplproperties.png)

Para más información, vea [Conceptos de la programación de replicación](../../relational-databases/replication/concepts/replication-programming-concepts.md).  
  

## <a name="create-a-subscription-to-the-transactional-publication"></a>Creación de una suscripción a la publicación transaccional
En esta sección se agrega un suscriptor a la publicación que creada anteriormente. Este tutorial usa un suscriptor remoto (NODE2\SQL2016), pero también puede agregar una suscripción localmente en el publicador. 

### <a name="create-the-subscription"></a>Creación de la suscripción  
  
1. Conéctese al publicador en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], expanda el nodo del servidor y luego la carpeta **Replicación**.  
  
2. En la carpeta **Publicaciones locales**, haga clic con el botón derecho en la publicación **AdvWorksProductTrans** y, después, seleccione **Nuevas suscripciones**. Se inicia el Asistente para nueva suscripción: 
 
   ![Selecciones para iniciar el Asistente para nueva suscripción](media/tutorial-replicating-data-between-continuously-connected-servers/newsub.png)     
  
3. En la página **Publicación**, seleccione **AdvWorksProductTrans** y, después, seleccione **Siguiente**:  

   ![Página "Publicación" con la publicación seleccionada](media/tutorial-replicating-data-between-continuously-connected-servers/selectpub.png)
  
4. En la página **Ubicación del Agente de distribución**, seleccione **Ejecutar todos los agentes en el distribuidor** y luego seleccione **Siguiente**.  Para más información sobre las suscripciones de inserción y de extracción, vea [Suscribirse a publicaciones](./subscribe-to-publications.md).

   ![Página "Ubicación del Agente de distribución" con la opción seleccionada para ejecutar todos los agentes en el distribuidor](media/tutorial-replicating-data-between-continuously-connected-servers/runagentsatdist.png)
  
5. En la página **Suscriptores**, si no se muestra el nombre de la instancia del suscriptor, seleccione **Agregar suscriptor** y, después, seleccione **Agregar suscriptor de SQL Server** en la lista desplegable. Este paso abre el cuadro de diálogo **Conectar al servidor**. Escriba el nombre de instancia del suscriptor y, después, seleccione **Conectar**.  
    
   Después de agregar el suscriptor, active la casilla situada junto al nombre de instancia del suscriptor. Después, seleccione **Nueva base de datos** en **Base de datos de suscripción**.   

   ![Página "Suscriptores" con selecciones para agregar un servidor de suscriptor](media/tutorial-replicating-data-between-continuously-connected-servers/addsub.png)

7. Aparece el cuadro de diálogo **Nueva base de datos**. Escriba **ProductReplica** en el cuadro **Nombre de base de datos**, seleccione **Aceptar** y, después, seleccione **Siguiente**: 
  
   ![Escribir un nombre para la base de datos de suscripción](media/tutorial-replicating-data-between-continuously-connected-servers/productreplica.png)
  
8. En la página **Seguridad del Agente de distribución**, haga clic en el botón de puntos suspensivos ( **...** ). Escriba <*Nombre_De_Equipo_Publicador>* >**rpl_distribution** el cuadro **Cuenta de proceso**, escriba la contraseña para esta cuenta, seleccione **Aceptar** y, a luego, **Siguiente**.

   ![Especifique una cuenta y una contraseña en el cuadro de diálogo "Seguridad del Agente de distribución".](media/tutorial-replicating-data-between-continuously-connected-servers/adddistaccount.png)
  
9. Seleccione **Finalizar** para aceptar los valores predeterminados en las páginas restantes y finalizar el asistente.  
  
### <a name="set-database-permissions-at-the-subscriber"></a>Establecer permisos de base de datos en el suscriptor  
  
1. Conéctese al suscriptor en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Expanda **Seguridad**, haga clic con el botón derecho en **Inicios de sesión** y, después, seleccione **Nuevo inicio de sesión**.     
  
   a. En la página **General**, en **Nombre de inicio de sesión**, seleccione **Buscar** y agregue el inicio de sesión para <*Nombre_De_Equipo_Suscriptor>* > **\repl_distribution**.

   b. En la página **Asignaciones de usuario**, otorgue el inicio de sesión al miembro **db_owner** para la base de datos **ProductReplica**. 

   ![Selecciones para configurar el inicio de sesión en el suscriptor](media/tutorial-replicating-data-between-continuously-connected-servers/loginforsub.png)

2. Seleccione **Aceptar** para cerrar el cuadro de diálogo **Nuevo inicio de sesión**. 

  
### <a name="view-the-synchronization-status-of-the-subscription"></a>Ver el estado de sincronización de la suscripción  
  
1. Conéctese al publicador en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Expanda el nodo del servidor y luego la carpeta **Replicación**.  
  
2. En la carpeta **Publicaciones locales**, expanda la publicación **AdvWorksProductTrans**, haga clic con el botón derecho en la suscripción de la base de datos **ProductReplica** y, después, seleccione **Ver estado de sincronización**. Se muestra el estado de sincronización actual de la suscripción:

   ![Selecciones para abrir el cuadro de diálogo "Ver estado de sincronización"](media/tutorial-replicating-data-between-continuously-connected-servers/viewsyncstatus.png)
3. Si la suscripción no está visible en **AdvWorksProductTrans**, presione F5 para actualizar la lista.  
  
Para más información, consulte:  
- [Inicializar una suscripción con una instantánea](../../relational-databases/replication/initialize-a-subscription-with-a-snapshot.md)  
- [Crear una suscripción de inserción](../../relational-databases/replication/create-a-push-subscription.md)  
- [Suscribirse a publicaciones](../../relational-databases/replication/subscribe-to-publications.md)  

## <a name="measure-replication-latency"></a>Medir la latencia de replicación
En esta sección se emplean testigos de seguimiento para comprobar que los cambios se replican en el suscriptor y para determinar la latencia. La latencia es el tiempo necesario para que un cambio realizado en el publicador aparezca en el suscriptor.
  
1. Conéctese al publicador en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Expanda el nodo del servidor, haga clic con el botón derecho en la carpeta **Replicación** y luego seleccione **Iniciar Monitor de replicación**:

   ![Comando "Iniciar el Monitor de replicación" en el menú contextual](media/tutorial-replicating-data-between-continuously-connected-servers/launchreplmonitor.png)

2. Expanda un grupo de publicador en el panel izquierdo, expanda la instancia del publicador y, después, seleccione la publicación **AdvWorksProductTrans**.  
  
   a. Seleccione la pestaña **Testigos de seguimiento**.  
   b. Seleccione **Insertar seguimiento**.    
   c. Vea el tiempo transcurrido para el testigo de seguimiento en las siguientes columnas: **Publicador a distribuidor**, **Distribuidor a suscriptor**, **Latencia total**. El valor **Pendiente** indica que el testigo no ha alcanzado un punto especificado.

   ![Información para el testigo de seguimiento](media/tutorial-replicating-data-between-continuously-connected-servers/tracertoken.png)


Para más información, consulte: 
- [Medir la latencia y validar las conexiones de la replicación transaccional](../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md)
- [Buscar errores con los agentes de replicación transaccional](troubleshoot-tran-repl-errors.md)


## <a name="next-steps"></a>Pasos siguientes
Ha configurado correctamente el publicador y el suscriptor para la replicación transaccional. Ahora puede insertar, actualizar o eliminar datos en la tabla **Product** del publicador. Después, puede consultar la tabla **Product** en el suscriptor para ver los cambios replicados. 

En este artículo se muestra cómo configurar la replicación de mezcla:  

> [!div class="nextstepaction"]
> [Tutorial: Configuración de la replicación (de mezcla) entre un servidor y clientes móviles](tutorial-replicating-data-with-mobile-clients.md)