---
title: 'Tutorial: Preparación para la replicación'
description: En este tutorial, aprenderá a preparar el publicador, el distribuidor y el suscriptor para la replicación mediante la creación de cuentas de Windows, la preparación de la carpeta de instantáneas y la configuración de la distribución.
ms.custom: seo-lt-2019
ms.date: 04/02/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: quickstart
helpviewer_keywords:
- replication [SQL Server], tutorials
ms.assetid: ce30a095-2975-4387-9377-94a461ac78ee
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 7506b3b57d81187269c26fae3cafc2d3cdbc6628
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348627"
---
# <a name="tutorial-prepare-sql-server-for-replication-publisher-distributor-subscriber"></a>Tutorial: Preparación de SQL Server para la replicación (publicador, distribuidor, suscriptor)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
Es importante planificar la seguridad antes de configurar la topología de replicación. Este tutorial le mostrará cómo asegurar mejor una topología de replicación. También muestra cómo configurar la distribución, que es el primer paso en la replicación de datos. Debe finalizar este tutorial antes que cualquiera de los otros tutoriales.  
  
> [!NOTE]  
> Para replicar datos de forma segura entre servidores, debe implementar todas las recomendaciones de [Prácticas recomendadas de seguridad de replicación](../../relational-databases/replication/security/replication-security-best-practices.md).  
  
## <a name="what-you-will-learn"></a>Aprendizaje  
Este tutorial le enseñará a preparar un servidor de manera que la replicación se ejecute de forma segura con los privilegios mínimos.  

En este tutorial, aprenderá a:
> [!div class="checklist"]
> * Crear cuentas de Windows para replicación
> * Preparar la carpeta de instantáneas
> * Configurar la distribución

## <a name="prerequisites"></a>Prerrequisitos
Este tutorial está pensado para usuarios que están familiarizados con las operaciones básicas de las bases de datos, pero que tienen una experiencia limitada en operaciones de replicación. 

Para completar este tutorial, necesita tener SQL Server, SQL Server Management Studio (SSMS) y una base de datos de AdventureWorks:  
  
- En el servidor del publicador (origen), instale:  
  
   - Cualquier edición de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], excepto SQL Server Express o SQL Server Compact. Estas ediciones no pueden ser publicadores de replicación.   
   - La base de datos de ejemplo [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] . Con el objeto de mejorar la seguridad, las bases de datos de ejemplo no se instalan de forma predeterminada.  
  
- En el servidor del suscriptor (destino), instale cualquier edición de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], excepto [!INCLUDE[ssEW](../../includes/ssew-md.md)]. [!INCLUDE[ssEW](../../includes/ssew-md.md)] no puede ser un suscriptor de una replicación transaccional.  
  
- Instale [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md).
- Instale [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Descargue la [base de datos de ejemplo AdventureWorks](https://github.com/Microsoft/sql-server-samples/releases). Para obtener instrucciones sobre cómo restaurar una base de datos en SSMS, vea [Restaurar una copia de seguridad de base de datos con SSMS](../backup-restore/restore-a-database-backup-using-ssms.md). 
    
>[!NOTE]
> - La replicación no se admite entre instancias de SQL Server que estén separadas por más de dos versiones entre sí. Para más información, vea la entrada de blog [Supported SQL Server versions in Replication Topology](replication-backward-compatibility.md) (Versiones de SQL Server admitidas en la topología de replicación).
> - En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], debe conectarse al publicador y al suscriptor con un inicio de sesión que sea miembro del rol fijo de servidor **sysadmin**. Para más información sobre este rol, vea [Roles de nivel de servidor](../security/authentication-access/server-level-roles.md).  


**Tiempo estimado para completar este tutorial: 30 minutos**
  
## <a name="create-windows-accounts-for-replication"></a>Creación de cuentas de Windows para replicación
En esta sección, crea cuentas de Windows para ejecutar agentes de replicación. Va a crear distintas cuentas de Windows en el servidor local para los siguientes agentes:  
  
|Agente|Location|Nombre de cuenta|  
|---------|------------|----------------|  
|Agente de instantáneas|Publicador|<*nombreDeEquipo*>\repl_snapshot|  
|Agente de registro del LOG|Publicador|<*nombreDeEquipo*>\repl_logreader|  
|Agente de distribución|Publicador y suscriptor|<*nombreDeEquipo*>\repl_distribution|  
|Agente de mezcla|Publicador y suscriptor|<*nombreDeEquipo*>\repl_merge|  
  
> [!NOTE]  
> En los tutoriales de replicación, el publicador y el distribuidor comparten la misma instancia (NODE1\SQL2016) de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La instancia del suscriptor (NODE2\SQL2016) es remota. El publicador y el suscriptor pueden compartir la misma instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], aunque no es necesario. Si el publicador y el suscriptor comparten la misma instancia, no se requieren los pasos que se utilizan para crear las cuentas en el suscriptor.  

### <a name="create-local-windows-accounts-for-replication-agents-at-the-publisher"></a>Creación de cuentas locales de Windows para agentes de replicación en el publicador
  
1. En el publicador, vaya al Panel de control y abra **Administración de equipos** en **Herramientas administrativas**.  
  
2. En **Herramientas del sistema**, expanda **Usuarios y grupos locales**.  
  
3. Haga clic con el botón derecho en **Usuarios** y, después, seleccione **Usuario nuevo**.  
     
4. Escriba **repl_snapshot** en el cuadro **Nombre de usuario**, proporcione la contraseña y demás información relevante y, después, seleccione **Crear** para crear la cuenta repl_snapshot: 

   ![Cuadro de diálogo Usuario nuevo](media/tutorial-preparing-the-server-for-replication/newuser.png)
  
5. Repita el paso anterior para crear las cuentas repl_logreader, repl_distribution y repl_merge:  
 
   ![Lista de usuarios de replicación](media/tutorial-preparing-the-server-for-replication/replusers.png)
  
6. Seleccione **Cerrar**.  
  
### <a name="create-local-windows-accounts-for-replication-agents-at-the-subscriber"></a>Creación de cuentas locales de Windows para agentes de replicación en el suscriptor  
  
1. En el suscriptor, vaya al Panel de control y abra **Administración de equipos** en **Herramientas administrativas** .  
  
2. En **Herramientas del sistema**, expanda **Usuarios y grupos locales**.  
  
3. Haga clic con el botón derecho en **Usuarios** y, después, seleccione **Usuario nuevo**.  
  
4. Escriba **repl_distribution** en el cuadro **Nombre de usuario**, proporcione la contraseña y demás información relevante y, después, seleccione **Crear** para crear la cuenta repl_distribution.  
  
5. Repita el paso anterior para crear la cuenta repl_merge.  
  
6. Seleccione **Cerrar**.  

Para más información, consulte [Información general sobre los agentes de replicación](../../relational-databases/replication/agents/replication-agents-overview.md).  

## <a name="prepare-the-snapshot-folder"></a>Preparación de la carpeta de instantáneas
En esta sección va a configurar la carpeta de instantáneas que se utiliza para crear y almacenar la instantánea de publicación. 

### <a name="create-a-share-for-the-snapshot-folder-and-assign-permissions"></a>Crear un recurso compartido para la carpeta de instantáneas y asignar permisos  
  
1. En el Explorador de archivos, vaya a la carpeta de datos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La ubicación predeterminada es C:\Archivos de programa\Microsoft SQL Server\MSSQL.X\MSSQL\Data.  
  
2. Cree una carpeta con el nombre **repldata**.  
  
3. Haga clic con el botón derecho en esta carpeta y seleccione **Propiedades**.  
  
   a. En la pestaña **Compartir** del cuadro de diálogo **Propiedades de repldata**, seleccione **Uso compartido avanzado**.  
  
   b. En el cuadro de diálogo **Uso compartido avanzado**, seleccione **Compartir esta carpeta** y, después, seleccione **Permisos**.  

   ![Selecciones para compartir la carpeta repldata](media/tutorial-preparing-the-server-for-replication/repldata.png)

6. En el cuadro de diálogo **Permisos de repldata**, seleccione **Agregar**. En el cuadro de texto **Seleccionar usuarios, equipos, cuentas de servicio o grupos**, escriba el nombre de la cuenta del Agente de instantáneas creado anteriormente, como <*Nombre_De_Equipo_Publicador*> **\repl_snapshot**. Seleccione **Comprobar nombres** y, después, **Aceptar**.  

   ![Selecciones para agregar permisos de uso compartido](media/tutorial-preparing-the-server-for-replication/addshareperms.png)

7. Repita el paso 6 para agregar las otras dos cuentas que se crearon previamente: <*Nombre_De_Equipo_Publicador*> **\repl_merge** y <*Nombre_De_Equipo_Publicador*> **\repl_distribution**.

8. Después de agregar las tres cuentas, asigne los permisos siguientes:      
   - repl_distribution: **Lectura**  
   - repl_merge: **Lectura**  
   - repl_snapshot: **Control total**    

   ![Permisos compartidos para cada cuenta](media/tutorial-preparing-the-server-for-replication/sharedpermissions.png)

9. Después de configurar correctamente los permisos de los recursos compartidos, seleccione **Aceptar** para cerrar el cuadro de diálogo **Permisos de repldata**. Seleccione **Aceptar** para cerrar el cuadro de diálogo **Uso compartido avanzado**. 

10. En el cuadro de diálogo **Propiedades de repldata**, seleccione la pestaña **Seguridad** y seleccione **Editar**:  

    ![Botón Editar en la pestaña Seguridad](media/tutorial-preparing-the-server-for-replication/editsecurity.png)   

11. En el cuadro de diálogo **Permisos de repldata**, seleccione **Agregar**. En el cuadro de texto **Seleccionar usuarios, equipos, cuentas de servicio o grupos**, escriba el nombre de la cuenta del Agente de instantáneas creado anteriormente, como <*Nombre_De_Equipo_Publicador*> **\repl_snapshot**. Seleccione **Comprobar nombres** y, después, **Aceptar**.  

    ![Selecciones para agregar permisos de seguridad](media/tutorial-preparing-the-server-for-replication/addsecuritypermissions.png)

  
12. Repita el paso anterior para agregar permisos para el Agente de distribución, por ejemplo <*Nombre_De_Equipo_Publicador*> **\repl_distribution** y, para el Agente de mezcla, <*Nombre_De_Equipo_Publicador*> **\repl_merge**.  
    
  
13. Compruebe que se admiten los siguientes permisos:  
  
    - repl_distribution: **Lectura**
    - repl_merge: **Lectura**
    - repl_snapshot: **Control total**   
 
    ![Permisos de usuario para datos de replicación](media/tutorial-preparing-the-server-for-replication/replpermissions.png) 

14. Vuelva a seleccionar la pestaña **Compartir** y anote la **Ruta de acceso a la red** para el recurso compartido. Necesitará esta ruta de acceso más adelante cuando configure la carpeta de instantáneas.  

    ![Ruta de acceso de red en la pestaña Uso compartido](media/tutorial-replicating-data-between-continuously-connected-servers/networkpath.png)

15. Seleccione **Aceptar** para cerrar el cuadro de diálogo **Propiedades de repldata**. 
 
Para más información, consulte [Proteger la carpeta de instantáneas](../../relational-databases/replication/security/secure-the-snapshot-folder.md).  
  

## <a name="configure-distribution"></a>Configuración de distribución
En esta sección va a configurar la distribución en el publicador y establecer los permisos requeridos en las bases de datos de publicación y distribución. Si ya ha configurado el distribuidor, debe deshabilitar la publicación y distribución antes de iniciar esta sección. No lo haga si debe mantener una topología de replicación existente, especialmente en producción.   
  
En este tutorial no se contempla la configuración de un publicador con un distribuidor remoto.  

### <a name="configure-distribution-at-the-publisher"></a>Configuración de la distribución en el publicador  
  
1. Conéctese al publicador en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] y, después, expanda el nodo de servidor.  
  
2. Haga clic con el botón derecho en la carpeta **Replicación** y luego seleccione **Configurar distribución**:  

   ![Comando Configurar la distribución en el menú contextual](media/tutorial-preparing-the-server-for-replication/configuredistribution.png)
  
   > [!NOTE]  
   > - Si se ha conectado a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante **localhost** en lugar del nombre real del servidor, aparece una advertencia que indica que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no se puede conectar a **localhost o dirección IP**. En el cuadro de diálogo de advertencia, seleccione **Aceptar**. En el cuadro de diálogo **Conectar al servidor**, cambie **Nombre del servidor** de **localhost o dirección IP** al nombre del servidor. A continuación, seleccione **Conectar**.  
   > - Actualmente hay un problema conocido con SQL Server Management Studio (SSMS) 18.0 (y versiones posteriores) que consiste en que _no_ se muestra un mensaje de advertencia al conectarse al distribuidor con la dirección IP, aunque esto sigue sin ser válido. Se debe usar el nombre real del servidor al conectarse al distribuidor. 
   
   Se inicia el Asistente para configurar la distribución.  
  
3. En la página **Distribuidor**, seleccione < *'NombreServidor'* >  **actuará como su propio distribuidor; SQL Server creará una base de datos y un registro de distribución**. Luego, seleccione **Siguiente**.  

   ![Opción para que el servidor actúe como su propio distribuidor](media/tutorial-preparing-the-server-for-replication/serverdistributor.png)
  
4. Si el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no se está ejecutando, en la página[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **Inicio del Agente**, seleccione **Sí[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y configure el servicio del Agente**  para que se inicie automáticamente. Seleccione **Next** (Siguiente).  

     
5. Escriba la ruta de acceso \\\\<*Nombre_De_Equipo_Publicador*> **\repldata** en el cuadro de texto **Carpeta de instantáneas** y, después, seleccione **Siguiente**. Esta ruta de acceso debe coincidir con lo que vimos anteriormente en **Ruta de acceso a la red** de la carpeta de propiedades de repldata después de configurar las propiedades del recurso compartido. 

   ![Comparación de las rutas de acceso de red en el cuadro de diálogo "Propiedades de repldata" y en el Asistente para configurar la distribución](media/tutorial-preparing-the-server-for-replication/repldatasnapshot.png)
  
6. Acepte los valores predeterminados en las páginas restantes del asistente.  
    
   ![Última página del Asistente](media/tutorial-preparing-the-server-for-replication/distributionwizarddefaults.png)
  
7. Seleccione **Finalizar** para habilitar la distribución. 

El siguiente error puede aparecer al configurar el distribuidor. Es una indicación de que la cuenta que se utilizó para iniciar la cuenta del Agente SQL Server no es un administrador del sistema. Necesitará iniciar manualmente el Agente SQL Server, conceder dichos permisos a la cuenta existente o modificar la cuenta que utiliza el Agente SQL Server. 

![Mensaje de error para configurar al Agente SQL Server](media/tutorial-preparing-the-server-for-replication/startingagenterror.png)

Si su instancia de SQL Server Management Studio se ejecuta con derechos administrativos, puede iniciar el Agente de SQL manualmente desde dentro de SSMS:  
    
![Selección de "Iniciar" en el menú contextual para el agente en SSMS](media/tutorial-preparing-the-server-for-replication/ssmsstartagent.png) 

>[!NOTE]
> Si no se inicia el Agente SQL de forma visible, haga clic con el botón derecho en el Agente SQL Server en SSMS y seleccione **Actualizar**. Si está todavía en estado detenido, inícielo manualmente desde el Administrador de configuración de SQL Server.    
  
## <a name="set-database-permissions"></a>Establecer los permisos para la base de datos  
  
1. En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], expanda **Seguridad**, haga clic con el botón derecho en **Inicios de sesión** y, después, seleccione **Nuevo inicio de sesión**:  

   ![Comando Nuevo inicio de sesión en el menú contextual](media/tutorial-preparing-the-server-for-replication/newlogin.png)
  
2. En la página **General**, seleccione **Buscar**. Escriba <*Nombre_De_Equipo_Publicador*> **\repl_snapshot** en el cuadro **Escriba el nombre de objeto a seleccionar**, seleccione **Comprobar nombres** y, después, seleccione **Aceptar**.  

   ![Selecciones para escribir el nombre del objeto](media/tutorial-preparing-the-server-for-replication/addsnapshotlogin.png)
  
3. En la página **Asignación de usuarios**, en la lista **Usuarios asignados a este inicio de sesión**, seleccione las bases de datos de **distribución** y de [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
   En la lista Pertenencia al rol de la base de datos, seleccione el rol **db_owner** para el inicio de sesión en ambas bases de datos.  

   ![Selección de las bases de datos y su rol](media/tutorial-preparing-the-server-for-replication/dbowner.png)
  
4. Seleccione **Aceptar** para crear el inicio de sesión.  
  
5. Repita los pasos del 1 al 4 para crear un inicio de sesión para el resto de cuentas locales (repl_distribution, repl_logreader y repl_merge). Estos inicios de sesión también se deben asignar a usuarios que son miembros del rol fijo de base de datos **db_owner** en las bases de datos **distribution** y **AdventureWorks**.  

   ![Vista de las cuatro cuentas en el Explorador de objetos](media/tutorial-preparing-the-server-for-replication/usersinssms.png)
   
  
Para más información, consulte:
- [Configuración de la distribución](../../relational-databases/replication/configure-distribution.md) 
- [Modelo de seguridad del Agente de replicación](../../relational-databases/replication/security/replication-agent-security-model.md)  

## <a name="next-steps"></a>Pasos siguientes
Ya ha preparado correctamente el servidor para la replicación. El artículo siguiente le enseñará a configurar la replicación transaccional: 

> [!div class="nextstepaction"]
> [Tutorial: Configuración de la replicación entre dos servidores conectados completamente (transaccional)](tutorial-replicating-data-between-continuously-connected-servers.md)

  
