---
description: Replication Management Objects Concepts
title: Conceptos de los Replication Management Objects (RMO) | Microsoft Docs
ms.custom: ''
ms.date: 11/08/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- replication [SQL Server], RMO
- programming interfaces [SQL Server replication]
- replication [SQL Server], how-to topics
- RMO [SQL Server]
- Replication Management Objects
- programming [SQL Server replication], RMO
ms.assetid: 37476d50-fb47-49e3-9504-3b163ac381d8
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: ca46285eacce3f77a07681fef9e75d16eddfd436
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97469146"
---
# <a name="replication-management-objects-concepts"></a>Replication Management Objects Concepts
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]

  Replication Management Objects (RMO) es un ensamblado de código administrado que encapsula las funcionalidades de replicación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. RMO se implementa mediante el espacio de nombres <xref:Microsoft.SqlServer.Replication>.  
  
 En los temas de las secciones siguientes se describe cómo puede utilizar RMO para controlar mediante programación las tareas de replicación:  
  
 [Configurar distribución](../../../relational-databases/replication/configure-distribution.md)  
 Los temas de esta sección muestran cómo utilizar RMO para configurar la publicación y la distribución.  
  
 [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md) (Creación de una publicación)  
 Los temas de esta sección muestran cómo utilizar RMO para crear, eliminar y modificar las publicaciones y los artículos.  
  
 [Suscribirse a publicaciones](../../../relational-databases/replication/subscribe-to-publications.md)  
 Los temas de esta sección muestran cómo utilizar RMO para crear, eliminar y modificar las suscripciones.  
  
 [Proteger una topología de replicación](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)  
 Los temas de esta sección muestran cómo utilizar RMO para ver y modificar la configuración de seguridad.  
  
 [Sincronizar suscripciones &#40;replicación&#41;](../../../relational-databases/replication/synchronize-data.md)  
 Los temas de esta sección muestran cómo sincronizar las suscripciones.  
  
 [Supervisar la replicación](../../../relational-databases/replication/monitor/monitoring-replication.md)  
 Los temas de esta sección muestran cómo supervisar mediante programación una topología de replicación.  
  
## <a name="introduction-to-rmo-programming"></a>Introducción a la programación de RMO  
 RMO está diseñado para programar todos los aspectos de la replicación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. El espacio de nombres de RMO es <xref:Microsoft.SqlServer.Replication> y lo implementa Microsoft.SqlServer.Rmo.dll, que es un ensamblado de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] .NET Framework.  El ensamblado Microsoft.SqlServer.Replication.dll, que también pertenece al espacio de nombres <xref:Microsoft.SqlServer.Replication>, implementa una interfaz de código administrado para programar varios agentes de replicación (Agente de instantáneas, Agente de distribución y Agente de mezcla). Se puede tener acceso a sus clases desde RMO para sincronizar las suscripciones. Las clases en el espacio de nombres <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport>, que implementa el ensamblado Microsoft.SqlServer.Replication.BusinessLogicSupport.dll, se utilizan para crear una lógica empresarial personalizada para la replicación de mezcla. Este ensamblado es independiente de RMO.  
  
## <a name="deploying-applications-based-on-rmo"></a>Implementar aplicaciones basadas en RMO  
 RMO depende de los componentes de replicación y de conectividad de cliente que están incluidos con todas las versiones de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] excepto SQL Server Compact. Para implementar una aplicación basada en RMO, debe instalar una versión de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que incluya componentes de replicación y de conectividad de cliente en el equipo en el que se vaya a ejecutar la aplicación.  
  
## <a name="getting-started-with-rmo"></a>Introducción a RMO  
 En esta sección se describe cómo iniciar un proyecto RMO simple utilizando [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Studio.  
  
#### <a name="to-create-a-new-microsoft-visual-c-project"></a>Para crear un proyecto nuevo de Microsoft Visual C#  
  
1.  Inicie Visual Studio.  
  
2.  En el menú **Archivo**, haga clic en **Nuevo proyecto**. Aparecerá el cuadro de diálogo **Nuevo proyecto** .  
  
3.  En el cuadro de diálogo **Tipos de proyecto**, seleccione **Proyectos de Visual C#**. En el panel **Plantillas**, seleccione **Aplicación para Windows**.  
  
4.  (Opcional) en **Nombre**, escriba el nombre de la nueva aplicación.  
  
5.  Haga clic en **Aceptar** para cargar la plantilla de Windows de Visual C#.  
  
6.  En el menú **Proyecto**, seleccione el elemento **Agregar referencia**. Aparecerá el cuadro de diálogo **Agregar referencia**.  
  
7.  Seleccione los siguientes ensamblados de la lista de la pestaña **.NET** y haga clic en **Aceptar**.  
  
    -   Interfaz de programación Microsoft.SqlServer.Replication de .NET  
  
    -   Microsoft.SqlServer.ConnectionInfo  
  
    -   Biblioteca de agentes de replicación  
  
    > [!NOTE]  
    >  Utilice la tecla CTRL para seleccionar más de un archivo.  
  
8.  (Opcional) Repita el paso 6. Haga clic en la pestaña **Examinar**, navegue a [!INCLUDE[ssInstallPath](../../../includes/ssinstallpath-md.md)]COM, seleccione Microsoft.SqlServer.Replication.BusinessLogicSupport.dll y haga clic en **Aceptar**.  
  
9. En el menú **Ver** , haga clic en **Código**.  
  
10. En el código, antes de la instrucción de espacio de nombres, escriba las siguientes instrucciones **using** para certificar los tipos de los espacios de nombres de RMO:  

    ```  
    // These namespaces are required.  
    using Microsoft.SqlServer.Replication;  
    using Microsoft.SqlServer.Management.Common;  
    // This namespace is only used when creating custom business  
    // logic for merge replication.  
    using Microsoft.SqlServer.Replication.BusinessLogicSupport;   
    ```  
  
#### <a name="to-create-a-new-microsoft-visual-basic-net-project"></a>Para crear un nuevo proyecto de Microsoft Visual Basic .NET  
  
1.  Inicie Visual Studio.  
  
2.  En el menú **Archivo**, seleccione **Nuevo proyecto**. Aparecerá el cuadro de diálogo **Nuevo proyecto** .  
  
3.  En el panel Tipos de proyecto, seleccione **Visual Basic**. En el panel Plantillas, seleccione **Aplicación para Windows**.  
  
4.  (Opcional) en el cuadro **Nombre**, escriba el nombre de la nueva aplicación.  
  
5.  Haga clic en **Aceptar** para cargar la plantilla de Windows de Visual Basic.  
  
6.  En el menú **Proyecto**, seleccione **Agregar referencia**. Aparecerá el cuadro de diálogo **Agregar referencia**.  
  
7.  Seleccione los siguientes ensamblados de la lista de la pestaña **.NET** y haga clic en **Aceptar**.  
  
    -   Interfaz de programación Microsoft.SqlServer.Replication de .NET  
  
    -   Microsoft.SqlServer.ConnectionInfo  
  
    -   Biblioteca de agentes de replicación  
  
    > [!NOTE]  
    >  Utilice la tecla CTRL para seleccionar más de un archivo.  
  
8.  (Opcional) Repita el paso 6. Haga clic en la pestaña **Examinar**, navegue a [!INCLUDE[ssInstallPath](../../../includes/ssinstallpath-md.md)]COM, seleccione Microsoft.SqlServer.Replication.BusinessLogicSupport.dll y haga clic en **Aceptar**.  
  
9. En el menú **Ver** , haga clic en **Código**.  
  
10. En el código, antes de cualquier declaración, escriba las siguientes instrucciones **Imports** para certificar los tipos de los espacios de nombres de RMO.  
  
    ```  
    ' These namespaces are required.  
    Imports Microsoft.SqlServer.Replication  
    Imports Microsoft.SqlServer.Management.Common  
    ' This namespace is only used when creating custom business  
    ' logic for merge replication.  
    Imports Microsoft.SqlServer.Replication.BusinessLogicSupport   
    ```  
  
## <a name="connecting-to-a-replication-server"></a>Conectar a un servidor de replicación  
 Los objetos de programación de RMO requieren que se realice una conexión a una instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] mediante una instancia de la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection>. Esta conexión al servidor se efectúa independientemente de cualquier objeto de programación de RMO. A continuación, se pasa al objeto RMO durante la creación de la instancia o por asignación a la propiedad `P:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContex`t del objeto. De esta manera, se pueden crear y administrar los objetos de programación RMO y las instancias de objeto de conexión por separado y se puede reutilizar un objeto de conexión único con varios objetos de programación RMO. Las reglas siguientes se aplican a las conexiones a un servidor de replicación:  
  
-   Todas las propiedades de la conexión se definen para un objeto <xref:Microsoft.SqlServer.Management.Common.ServerConnection> dado.  
  
-   Una conexión a cada instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] debe tener su propio objeto <xref:Microsoft.SqlServer.Management.Common.ServerConnection>.  
  
-   El objeto <xref:Microsoft.SqlServer.Management.Common.ServerConnection> se asigna a la propiedad `P:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext` del objeto de programación de RMO que se va a crear o al que se va a tener acceso en el servidor.  
  
-   El método <xref:Microsoft.SqlServer.Management.Common.ConnectionManager.Connect%2A> abre la conexión al servidor. Se debe llamar a este método antes de llamar a cualquier método que tenga acceso al servidor en cualquier objeto de programación de RMO que use la conexión.  
  
-   Dado que tanto RMO como Objetos de administración de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (SMO) usan la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> para las conexiones a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], los objetos SMO y RMO pueden utilizar la misma conexión. Para obtener más información, vea [Connecting to an Instance of SQL Server](../../../relational-databases/server-management-objects-smo/create-program/connecting-to-an-instance-of-sql-server.md) (Conectarse a una instancia de SQL Server).  
  
-   Toda la información de autenticación para realizar la conexión e iniciar sesión en el servidor correctamente se proporciona en el objeto <xref:Microsoft.SqlServer.Management.Common.ServerConnection>.  
  
-   La autenticación de Windows es el valor predeterminado. Para utilizar la autenticación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], <xref:Microsoft.SqlServer.Management.Common.ConnectionSettings.LoginSecure%2A> debe estar establecido en **False**, y <xref:Microsoft.SqlServer.Management.Common.ConnectionSettings.Login%2A> y <xref:Microsoft.SqlServer.Management.Common.ConnectionSettings.Password%2A> deben estar establecidos como inicio de sesión y contraseña de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] válidos. Las credenciales de seguridad siempre deben almacenarse y administrarse de forma segura, y se deben proporcionar en tiempo de ejecución cuando sea posible.  
  
-   Para las aplicaciones multiproceso, se debe utilizar un objeto <xref:Microsoft.SqlServer.Management.Common.ServerConnection> independiente en cada subproceso.  
  
 Llame al método <xref:Microsoft.SqlServer.Management.Common.ConnectionManager.Disconnect%2A> en el objeto <xref:Microsoft.SqlServer.Management.Common.ServerConnection> para cerrar las conexiones al servidor activas que usan los objetos RMO.  
  
## <a name="setting-rmo-properties"></a>Establecer las propiedades de RMO  
 Las propiedades de los objetos de programación de RMO representan las propiedades de estos objetos de replicación en el servidor. Al crear nuevos objetos de replicación en el servidor, se utilizan las propiedades de RMO para definirlos. En los objetos existentes, las propiedades de RMO representan sus propiedades, que solo se pueden modificar en el caso de que se puedan escribir o configurar. Las propiedades se pueden establecer en los objetos nuevos o en los objetos existentes.  
  
### <a name="setting-properties-for-new-replication-objects"></a>Establecer propiedades para los nuevos objetos de replicación  
 Al crear un nuevo objeto de replicación en el servidor, debe especificar todas las propiedades necesarias antes de llamar al método **Create** del objeto. Para obtener más información sobre cómo establecer las propiedades para un nuevo objeto de replicación, vea [Configurar la publicación y la distribución](../../../relational-databases/replication/configure-publishing-and-distribution.md).  
  
### <a name="setting-properties-for-existing-replication-objects"></a>Establecer propiedades para los objetos de replicación existentes  
 Para algunos de los objetos de replicación que existen en el servidor, RMO podría admitir la capacidad de cambiar algunas o todas sus propiedades. Solo es posible cambiar las propiedades que se puedan escribir o configurar. Antes de poder cambiar las propiedades, se debe llamar al método **Load** o `M:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties` para recibir las propiedades actuales del servidor. Llamar a estos métodos indica que un objeto existente se está modificando.  
  
 De forma predeterminada, al cambiar las propiedades del objeto, RMO confirma estos cambios en el servidor según el modo de ejecución de <xref:Microsoft.SqlServer.Management.Common.ServerConnection> que se esté usando. El método `P:Microsoft.SqlServer.Replication.ReplicationObject.IsExistingObject` se puede utilizar para comprobar que un objeto existe en el servidor antes de intentar recuperar o cambiar sus propiedades. Para obtener más información sobre cómo cambiar las propiedades de un objeto de replicación, vea [Ver y modificar las propiedades del distribuidor y del publicador](../../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md).  
  
> [!NOTE]  
>  Cuando varios clientes de RMO o varias instancias de un objeto de programación RMO están accediendo al mismo objeto de replicación del servidor, se puede llamar al método **Refresh** del objeto RMO para actualizar las propiedades en función del estado actual del objeto del servidor.  
  
### <a name="caching-property-changes"></a>Almacenar en memoria caché los cambios de propiedades  
 Cuando la propiedad <xref:Microsoft.SqlServer.Management.Common.SqlExecutionModes> está establecida en <xref:Microsoft.SqlServer.Management.Common.SqlExecutionModes.CaptureSql> todas las instrucciones [!INCLUDE[tsql](../../../includes/tsql-md.md)] generadas por RMO se capturan para que se puedan ejecutar manualmente en un lote único utilizando alguno de los métodos de ejecución. RMO le permite almacenar en memoria caché los cambios de las propiedades y las confirma conjuntamente en un único lote utilizando el método `M:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges` del objeto. Para almacenar en caché los cambios de las propiedades, la propiedad `P:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges` del objeto debe estar establecida en **true**. Al almacenar en memoria caché los cambios de las propiedades en RMO, el objeto <xref:Microsoft.SqlServer.Management.Common.ServerConnection> todavía controla cuándo se envían los cambios al servidor. Para obtener más información sobre cómo almacenar en caché los cambios de las propiedades de un objeto de replicación, vea [Ver y modificar las propiedades del distribuidor y del publicador](../../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md).  
  
> [!IMPORTANT]  
>  Aunque la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> permite declarar las transacciones explícitas al establecer las propiedades, tales transacciones pueden interferir con las transacciones de replicación internas, producir resultados imprevistos y no se deberían utilizar con RMO.  

### <a name="enabling-tls-12-support-for-rmo-components"></a>Habilitación de la compatibilidad de TLS 1.2 con componentes RMO 
 La compatibilidad de TLS 1.2 con componentes RMO en Windows 2012 e inferior puede habilitarse mediante la instalación de la actualización [KB 3140245](https://support.microsoft.com/help/3140245) y la creación de las claves del Registro, como se ha mencionado en el artículo. En Windows 2012 R2 y versiones posteriores, solo es necesario crear las claves del Registro, como se ha mencionado en el artículo anterior.
 
## <a name="example"></a>Ejemplo  
 En este ejemplo se muestra el almacenamiento en memoria caché de los cambios de las propiedades. Los cambios realizados en los atributos de una publicación transaccional se almacenan en memoria caché hasta que se envían explícitamente al servidor.  
  
 [!code-cs[HowTo#rmo_ChangeTranPub_cached](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_changetranpub_cached)]  
  
## <a name="see-also"></a>Consulte también  
 [Replication System Stored Procedures Concepts](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [Conceptos de la programación de replicación](../../../relational-databases/replication/concepts/replication-programming-concepts.md)  
  
  
