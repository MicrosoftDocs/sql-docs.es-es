---
title: 'Tutorial: Configuración de replicación de mezcla'
description: Este tutorial le enseña a configurar la replicación de combinación entre una instancia de SQL Server y un cliente móvil.
ms.custom: seo-lt-2019
ms.date: 04/03/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: quickstart
helpviewer_keywords:
- replication [SQL Server], tutorials
ms.assetid: af673514-30c7-403a-9d18-d01e1a095115
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3f29e39d8b66b5adafb4c84de6a8f1df99ce95bb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348617"
---
# <a name="tutorial-configure-replication-between-a-server-and-mobile-clients-merge"></a>Tutorial: Configuración de la replicación (de mezcla) entre un servidor y clientes móviles
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
La replicación de mezcla es una buena solución al problema de mover datos entre un servidor central y clientes móviles que solo se conectan en determinadas ocasiones. Al usar los asistentes para replicación, es más fácil configurar y administrar una topología de replicación de mezcla. 

Este tutorial le mostrará cómo configurar una topología de replicación para clientes móviles. Para más información sobre la replicación de mezcla, vea [Replicación de mezcla](./merge/merge-replication.md).
  
## <a name="what-you-will-learn"></a>Aprendizaje  
En este tutorial se explica cómo usar la replicación de mezcla para publicar datos de una base de datos central en uno o más usuarios móviles, de modo que cada usuario obtenga un subconjunto de datos filtrado de manera exclusiva. 

En este tutorial, aprenderá a:
> [!div class="checklist"]
> * Configurar un publicador para la replicación de mezcla
> * Agregar un suscriptor móvil para la publicación de mezcla
> * Sincronizar la suscripción con la publicación de mezcla
  
## <a name="prerequisites"></a>Prerrequisitos  
Este tutorial es para usuarios que están familiarizados con las operaciones básicas de las bases de datos, pero tienen una experiencia limitada en operaciones de replicación. Antes de empezar este tutorial, debe haber completado [Tutorial: Preparación de SQL Server para la replicación](../../relational-databases/replication/tutorial-preparing-the-server-for-replication.md).  
  
Para completar este tutorial, necesita tener SQL Server, SQL Server Management Studio (SSMS) y una base de datos de AdventureWorks: 
  
- En el servidor del publicador (origen), instale:  
  
   - Cualquier edición de SQL Server, excepto SQL Server Express o SQL Server Compact. Estas ediciones no pueden ser publicadores de replicación.   
   - La base de datos de ejemplo [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . Con el objeto de mejorar la seguridad, las bases de datos de ejemplo no se instalan de forma predeterminada.  
  
- En el servidor de suscriptor (destino), instale cualquier edición de SQL Server, excepto SQL Server Express o SQL Server Compact. La publicación que se crea en este tutorial no es compatible con SQL Server Express ni con SQL Server Compact. 

- Instale [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md).
- Instale [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Descargue la [base de datos de ejemplo AdventureWorks](https://github.com/Microsoft/sql-server-samples/releases). Para obtener instrucciones sobre cómo restaurar una base de datos en SSMS, vea [Restaurar una copia de seguridad de base de datos con SSMS](../backup-restore/restore-a-database-backup-using-ssms.md).  
 
  
>[!NOTE]
> - La replicación no se admite entre instancias de SQL Server que estén separadas por más de dos versiones entre sí. Para más información, vea la entrada de blog [Supported SQL Server versions in Replication Topology](https://blogs.msdn.microsoft.com/repltalk/2016/08/12/suppported-sql-server-versions-in-replication-topology/) (Versiones de SQL Server admitidas en la topología de replicación).
> - En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], debe conectarse al publicador y al suscriptor con un inicio de sesión que sea miembro del rol fijo de servidor **sysadmin**. Para más información sobre este rol, vea [Roles de nivel de servidor](../security/authentication-access/server-level-roles.md).  
  
  
**Tiempo estimado para completar este tutorial: 60 minutos**  
  
## <a name="configure-a-publisher-for-merge-replication"></a>Configurar un publicador para la replicación de mezcla
En esta sección, se crea una publicación de mezcla mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] para publicar un subconjunto de las tablas **Employee**, **SalesOrderHeader** y **SalesOrderDetail** en la base de datos de ejemplo [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Estas tablas están filtradas con filtros de fila con parámetros para que cada suscripción contenga una partición única de los datos. También se agrega el inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usado por el Agente de mezcla a la lista de acceso a la publicación (PAL).  
  
### <a name="create-merge-publication-and-define-articles"></a>Crear publicaciones de mezcla y definir artículos  
  
1. Conéctese al publicador en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] y, después, expanda el nodo de servidor.  
  
2. Inicie el Agente SQL Server haciendo clic con el botón derecho en el Explorador de objetos y seleccionando **Iniciar**. Si esto no inicia el agente, deberá hacerlo manualmente desde el Administrador de configuración de SQL Server.  
3. Expanda la carpeta **Replicación**, haga clic con el botón derecho en la carpeta **Publicaciones locales** y, después, seleccione **Nueva publicación**. Se inicia el Asistente para nueva publicación:  

   ![Selecciones para iniciar el Asistente para nueva publicación](media/tutorial-replicating-data-between-continuously-connected-servers/newpublication.png)
  
3. En la página **Base de datos de publicación**, seleccione [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] y haga clic en **Siguiente**. 

      
4. En la página **Tipo de publicación**, seleccione **Publicación de mezcla** y haga clic en **Siguiente**.  
   
5. En la página **Tipos de suscriptor**, asegúrese de que solo esté seleccionado [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] o posterior y, después, haga clic en **Siguiente**: 

    ![Páginas "Tipo de publicación" y "Tipos de suscriptor"](media/tutorial-replicating-data-with-mobile-clients/mergerpl.png)
  
   
6. En la página **Artículos**, expanda el nodo **Tablas**. Seleccione las tres tablas siguientes: **Employee**, **SalesOrderHeader** y **SalesOrderDetail**. Seleccione **Next** (Siguiente).  

   ![Selecciones de tabla en la página "Artículos"](media/tutorial-replicating-data-with-mobile-clients/mergearticles.png)

   >[!NOTE]
   > La tabla **Employee** contiene una columna (**OrganizationNode**) que tiene el tipo de datos **hierarchyid**. Este tipo de datos se admite para la replicación solo en SQL Server 2017. 
   >
   > Si usa una versión anterior a SQL 2017, verá un mensaje en la parte inferior de la pantalla que le notifica de posibles pérdidas de datos si usa esta columna en la replicación bidireccional. Puede ignorar este mensaje, ya que no afecta al propósito de este tutorial. Pero no olvide que este tipo de datos no debería replicarse en un entorno de producción, a menos que esté usando la versión compatible.
   > 
   > Para más información sobre cómo replicar el tipo de datos **hierarchyid**, vea [Usar columnas hierarchyid en tablas replicadas](../../t-sql/data-types/hierarchyid-data-type-method-reference.md#using-hierarchyid-columns-in-replicated-tables).
    
  
7. En la página **Filtrar filas de tabla**, seleccione **Agregar** y luego **Agregar filtro**.  
  
8. En el cuadro de diálogo **Agregar filtro**, seleccione **Employee (HumanResources)** en **Seleccione la tabla que desea filtrar**. Seleccione la columna **LoginID**, seleccione la flecha derecha para agregar la columna a la cláusula WHERE de la consulta del filtro y modifique la cláusula WHERE de la manera siguiente:  
  
   ```sql 
    WHERE [LoginID] = HOST_NAME()  
   ```  
  
   Seleccione **A row from this table will go to only one subscription** (Una fila de esta tabla irá a una sola suscripción) y luego elija **Aceptar**.  
 
   ![Selecciones para agregar un filtro](media/tutorial-replicating-data-with-mobile-clients/mergeaddfilter.png)

    
  
10. En la página **Filtrar filas de tabla**, seleccione **Employee (Human Resources)** , luego **Agregar** y, después, **Add Join to Extend the Selected Filter** (Agregar combinación para ampliar el filtro seleccionado).  
  
    a. En el cuadro de diálogo **Agregar combinación**, seleccione **Sales.SalesOrderHeader** en **Tabla combinada**. Seleccione **Escribir instrucción de combinación manualmente** y complete la instrucción de combinación de la manera siguiente:  
  
    ```sql  
    ON [Employee].[BusinessEntityID] =  [SalesOrderHeader].[SalesPersonID] 
    ```  
  
    b. En **Especifique las opciones de combinación**, seleccione **Clave única** y, a continuación, seleccione **Aceptar**.

    ![Selecciones para agregar una combinación al filtro](media/tutorial-replicating-data-with-mobile-clients/mergeaddjoin.png)

  
13. En la página **Filtrar filas de tabla**, seleccione **SalesOrderHeader**, elija **Agregar** y luego **Add Join to Extend the Selected Filter** (Agregar combinación para ampliar el filtro seleccionado).  
  
    a. En el cuadro de diálogo **Agregar combinación** , seleccione **Sales.SalesOrderDetail** en **Tabla combinada**.    
    b. Seleccione **Usar generador de instrucciones para crear la instrucción**.  
    c. En el cuadro **Vista previa**, confirme que la instrucción de combinación es como sigue:  
  
    ```sql  
    ON [SalesOrderHeader].[SalesOrderID] = [SalesOrderDetail].[SalesOrderID] 
    ```  
  
    d. En **Especifique las opciones de combinación**, seleccione **Clave única** y, a continuación, seleccione **Aceptar**. Seleccione **Next** (Siguiente). 

    ![Selecciones para agregar otra combinación para pedidos de venta](media/tutorial-replicating-data-with-mobile-clients/joinsalestables.png)
  
21. Seleccione **Crear una instantánea inmediatamente**, desactive **Programar el Agente de instantáneas para ejecutarse** y, a continuación, seleccione **Siguiente**:  

    ![Selección para crear una instantánea inmediatamente](media/tutorial-replicating-data-with-mobile-clients/snapshotagent.png)
  
22. En la página **Seguridad del agente**, elija **Configuración de seguridad**. Escriba <*Nombre_De_Equipo_Publicador>* > **\repl_snapshot** en el cuadro **Cuenta de proceso**, escriba la contraseña de la cuenta y, luego, seleccione **Aceptar**. Seleccione **Next** (Siguiente).  

    ![Selecciones para configurar la seguridad del agente de instantáneas](media/tutorial-replicating-data-with-mobile-clients/snapshotagentsecurity.png)
  
23. En la página **Finalización del asistente**, escriba **AdvWorksSalesOrdersMerge** en el cuadro **Nombre de publicación** y seleccione **Finalizar**:  

    ![Página "Complete the Wizard" (Finalización del asistente) con el nombre de la publicación](media/tutorial-replicating-data-with-mobile-clients/namemergerepl.png)
  
24. Una vez creada la publicación, seleccione **Cerrar**. En el nodo **Replicación** en el **Explorador de objetos**, haga clic con el botón derecho en **Publicaciones locales** y en **Actualizar** para ver la nueva replicación de mezcla.  
  
### <a name="view-the-status-of-snapshot-generation"></a>Ver el estado de la generación de instantáneas  
  
1. Conéctese al publicador en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], expanda el nodo del servidor y luego la carpeta **Replicación**.  
  
2. En la carpeta **Publicaciones locales**, haga clic con el botón derecho en **AdvWorksSalesOrdersMerge** y luego seleccione **Ver estado del Agente de instantáneas**:  

   ![Selecciones para ver el estado del agente de instantáneas](media/tutorial-replicating-data-with-mobile-clients/viewsnapshotagentstatus.png)
  
3. Se muestra el estado actual del trabajo del Agente de instantáneas para la publicación. Compruebe que el trabajo de instantáneas sea correcto antes de continuar con la siguiente lección.  
  
### <a name="add-the-merge-agent-login-to-the-pal"></a>Agregar el inicio de sesión del Agente de mezcla a la lista de acceso de la publicación (PAL)  
  
1. Conéctese al publicador en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], expanda el nodo del servidor y luego la carpeta **Replicación**.  
  
2. En la carpeta **Publicaciones locales**, haga clic con el botón derecho en **AdvWorksSalesOrdersMerge** y seleccione **Propiedades**.  
  
   a. Seleccione la página **Lista de acceso a la publicación** y seleccione **Agregar**. 
  
   b. En el cuadro de diálogo **Agregar acceso de publicación**, seleccione <*Nombre_De_Equipo_Publicador>* > **\repl_merge** y seleccione **Aceptar**. Vuelva a hacer clic en **Aceptar**. 

   ![Selecciones para agregar el inicio de sesión del Agente de mezcla](media/tutorial-replicating-data-with-mobile-clients/mergepal.png) 

  
Para más información, consulte:  
- [Filtrar datos publicados](../../relational-databases/replication/publish/filter-published-data.md) 
- [Filtros con parámetros: filtros de fila con parámetros](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)  
- [Definir un artículo](../../relational-databases/replication/publish/define-an-article.md)  
  
  
## <a name="create-a-subscription-to-the-merge-publication"></a>Crear una suscripción a la publicación de mezcla
En esta sección, se agrega una suscripción a la publicación de mezcla que se creó anteriormente. Este tutorial usa el suscriptor remoto (NODE2\SQL2016). Luego se establecen los permisos en la base de datos de suscripciones y se genera manualmente la instantánea de datos filtrados para la nueva suscripción.   
  
### <a name="add-a-subscriber-for-merge-publication"></a>Agregar un suscriptor para la publicación de mezcla
  
1. Conéctese al suscriptor en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] y expanda el nodo de servidor. Expanda la carpeta **Replicación**, haga clic con el botón derecho en la carpeta **Suscripciones locales** y luego seleccione **Nuevas suscripciones**. Se inicia el Asistente para nueva suscripción:

   ![Selecciones para iniciar el Asistente para nueva suscripción](media/tutorial-replicating-data-with-mobile-clients/newsub.png)
  
2. En la página **Publicación**, seleccione **Buscar publicador de SQL Server** en la lista **Publicador**.  
  
   En el cuadro de diálogo **Conectar al servidor**, escriba el nombre de la instancia del publicador en el cuadro **Nombre del servidor** y, después, seleccione **Conectar**: 

   ![Selecciones para agregar un publicador](media/tutorial-replicating-data-with-mobile-clients/publication.png)
  
4. Seleccione **AdvWorksSalesOrdersMerge** y **Siguiente**.  
  
5. En la página **Ubicación del Agente de mezcla**, seleccione **Ejecutar cada agente en su suscriptor** y, luego, **Siguiente**:  

   ![Opción "Ejecutar cada agente como su suscriptor"](media/tutorial-replicating-data-with-mobile-clients/pullsub.png)
  
6. En la página **Suscriptores**, seleccione el nombre de instancia del servidor del suscriptor. En **Base de datos de suscripciones**, seleccione **Nueva base de datos** en la lista.  
  
   En el cuadro de diálogo **Nueva base de datos**, escriba **SalesOrdersReplica** en el cuadro **Nombre de la base de datos**. Seleccione **Aceptar** y después **Siguiente**. 

   ![Selecciones para agregar una base de datos al suscriptor](media/tutorial-replicating-data-with-mobile-clients/addsubdb.png)
  
8. En la página **Seguridad del agente de mezcla**, seleccione el botón de puntos suspensivos ( **...** ). Escriba <*Nombre_De_Equipo_Suscriptor*> **\repl_merge** en el cuadro **Cuenta de proceso** y especifique la contraseña para esta cuenta. Seleccione **Aceptar**, **Siguiente** y, después, elija otra vez **Siguiente**.  

   ![Selecciones para la seguridad del Agente de mezcla](media/tutorial-replicating-data-with-mobile-clients/mergeagentsecurity.png)

9. En la página **Programación de sincronización**, establezca **Programación del agente** en **Run on demand only** (Ejecutar solamente a petición). Seleccione **Next** (Siguiente).  

   ![Selección de "Run on demand only" (Ejecutar solamente a petición) para el agente](media/tutorial-replicating-data-with-mobile-clients/mergesyncschedule.png)
  
9. En la página **Inicializar suscripciones**, seleccione **En la primera sincronización** de la lista **Inicializar cuando**. Seleccione **Siguiente** para continuar con la página **Tipo de suscripción** y seleccione el tipo de suscripción adecuado. En este tutorial se usa **Cliente**. Después de seleccionar el tipo de suscripción, seleccione **Siguiente** otra vez. 

   ![Selecciones para inicializar suscripciones en la primera sincronización](media/tutorial-replicating-data-with-mobile-clients/firstsync.png)

10. En la página **Valores de HOST_NAME**, escriba un valor para **adventure-works\pamela0** en el cuadro **Valor de HOST_NAME**. Después, seleccione **Finalizar**.  

    ![Página "Valores de HOST_NAME"](media/tutorial-replicating-data-with-mobile-clients/hostname.png)
  
11. Seleccione **Finalizar** otra vez. Una vez creada la suscripción, seleccione **Cerrar**.  

### <a name="set-server-permissions-at-the-subscriber"></a>Establecer permisos de servidor en el suscriptor  
  
1. Conéctese al suscriptor en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Expanda **Seguridad**, haga clic con el botón derecho en **Inicios de sesión** y, después, seleccione **Nuevo inicio de sesión**.  
  
   En la página **General**, seleccione **Buscar** y escriba <*Nombre_De_Equipo_Suscriptor>* > **\repl_merge** en el campo **Escriba el nombre de objeto**. Seleccione **Comprobar nombres** y, después, **Aceptar**. 
    
   ![Selecciones para configurar el inicio de sesión](media/tutorial-replicating-data-with-mobile-clients/sublogin.png)
  
1. En la página **Asignación de usuario**, seleccione la base de datos **SalesOrdersReplica** y seleccione el rol **db_owner**. En la página **Elementos protegibles**, conceda el permiso **Explícito** para **Modificar seguimiento**. Seleccione **Aceptar**.

   ![Páginas "Asignación de usuario" y "Elementos protegibles"](media/tutorial-replicating-data-with-mobile-clients/setdbo.png)
  
### <a name="create-the-filtered-data-snapshot-for-the-subscription"></a>Crear la instantánea de datos filtrados para la suscripción  
  
1. Conéctese al publicador en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], expanda el nodo del servidor y luego la carpeta **Replicación**.  
  
2. En la carpeta **Publicaciones locales**, haga clic con el botón derecho en la publicación **AdvWorksSalesOrdersMerge** y, luego, seleccione **Propiedades**.  
   
   a. Seleccione la página **Particiones de datos** y seleccione **Agregar**.   
   b. En el cuadro de diálogo **Agregar partición de datos**, escriba **adventure-works\pamela0** en el cuadro **Valor de HOST_NAME** y, después, seleccione **Aceptar**.  
   c. Seleccione la partición agregada recientemente, elija **Generar instantáneas seleccionadas ahora** y, después, seleccione **Aceptar**. 

   ![Selecciones para agregar una partición](media/tutorial-replicating-data-with-mobile-clients/partition.png)
  
  
Para más información, consulte:  
- [Suscribirse a publicaciones](../../relational-databases/replication/subscribe-to-publications.md)  
- [Crear una suscripción de extracción](../../relational-databases/replication/create-a-pull-subscription.md)  
- [Instantáneas para publicaciones de combinación con filtros con parámetros](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)  

## <a name="synchronize-the-subscription-to-the-merge-publication"></a>Sincronizar la suscripción con la publicación de mezcla

En esta sección, se iniciará el Agente de mezcla para inicializar la suscripción mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Este procedimiento también se usa para sincronizar con el publicador.   
  
### <a name="start-synchronization-and-initialize-the-subscription"></a>Iniciar la sincronización e inicializar la suscripción  
  
1. Conéctese al suscriptor en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
2. Asegúrese de que el Agente SQL Server se está ejecutando. Si no lo está, haga clic con el botón derecho en el Agente SQL Server desde el Explorador de objetos y seleccione **Iniciar**. Si esto no inicia el agente, deberá hacerlo manualmente mediante el Administrador de configuración de SQL Server. 
  
2. Expanda el nodo **Replicación**. En la carpeta **Suscripciones locales**, haga clic con el botón derecho en la suscripción de la base de datos **SalesOrdersReplica** y, después, seleccione **Ver estado de sincronización**.  
  
   Seleccione **Iniciar** para inicializar la suscripción. 

   ![Estado de sincronización con el botón "Iniciar"](media/tutorial-replicating-data-with-mobile-clients/mergesyncstatus.png)
    
  
  
## <a name="next-steps"></a>Pasos siguientes  
Ha configurado correctamente el publicador y el suscriptor para la replicación de mezcla. También puede:

1. Insertar, actualizar o eliminar datos en la tabla **SalesOrderHeader** o **SalesOrderDetail** en el publicador o el suscriptor.
2. Repetir este procedimiento cuando la conectividad de red esté disponible para sincronizar los datos entre el publicador y el suscriptor.
3. Consultar la tabla **SalesOrderHeader** o **SalesOrderDetail** en el otro servidor para ver los cambios replicados.  
  
Para más información, consulte:   
- [Inicializar una suscripción con una instantánea](../../relational-databases/replication/initialize-a-subscription-with-a-snapshot.md)  
- [Sincronizar datos](../../relational-databases/replication/synchronize-data.md)  
- [Sincronizar una suscripción de extracción](../../relational-databases/replication/synchronize-a-pull-subscription.md)  
  
  
  
