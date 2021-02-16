---
title: Analysis Services con grupos de disponibilidad
description: Si usa grupos de disponibilidad Always On como solución de alta disponibilidad, puede usar una base de datos de ese grupo como origen de datos en una solución tabular o multidimensional de Analysis Services.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: 14d16bfd-228c-4870-b463-a283facda965
author: cawrites
ms.author: chadam
manager: erikre
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 1bcb05458c8e8792975606382e7a1cd24de628a3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338489"
---
# <a name="analysis-services-with-always-on-availability-groups"></a>Analysis Services con grupos de disponibilidad AlwaysOn

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

  El grupo de disponibilidad AlwaysOn es una colección predefinida de bases de datos relacionales de SQL Server que conmutan por error conjuntamente cuando las condiciones desencadenan una conmutación por error en una base de datos, redirigiendo las solicitudes a una base de datos reflejada en otra instancia del mismo grupo de disponibilidad. Si utiliza grupos de disponibilidad como solución de alta disponibilidad, puede usar una base de datos de ese grupo como origen de datos en una solución tabular o multidimensional de Analysis Services. Todas las operaciones de Analysis Services siguientes funcionan de la manera esperada cuando se utiliza una base de datos de disponibilidad: procesando o importando datos, consultando datos relacionales directamente (utilizando el modo DirectQuery o almacenamiento ROLAP) y con reescritura.  
  
 El procesamiento y la consulta son cargas de trabajo de solo lectura. Puede mejorar el rendimiento descargando estas cargas de trabajo en una réplica secundaria legible. Se requiere una configuración adicional para este escenario. Utilice la lista de comprobación de este tema para asegurarse de que sigue todos los pasos.  
  
##  <a name="prerequisites"></a><a name="bkmk_prereq"></a> Requisitos previos  
 Debe tener un inicio de sesión de SQL Server en todas las réplicas. Debe ser **sysadmin** para configurar los grupos de disponibilidad, los agentes de escucha y las bases de datos, pero los usuarios solo necesitan permisos **db_datareader** para tener acceso a la base de datos de un cliente de Analysis Services.  
  
 Use un proveedor de datos que admita la versión (TDS) 7.4 del protocolo de flujo de datos tabular u otra más reciente, como SQL Server Native Client 11.0 o el Proveedor de datos de SQL Server de .NET Framework 4.02.  
  
 **(Para cargas de trabajo de solo lectura)** . El rol de réplica secundaria se debe configurar para las conexiones de solo lectura, el grupo de disponibilidad debe tener una lista de distribución y la conexión del origen de datos de Analysis Services debe especificar el agente de escucha del grupo de disponibilidad. En este tema se proporcionan instrucciones al respecto.  
  
##  <a name="checklist-use-a-secondary-replica-for-read-only-operations"></a><a name="bkmk_UseSecondary"></a> Lista de comprobación: usar una réplica secundaria para las operaciones de solo lectura  
 A menos que la solución de Analysis Services incluya reescritura, puede configurar una conexión a un origen de datos para utilizar una réplica secundaria legible. Si tiene una conexión de red rápida, la replicación secundaria tiene una latencia de datos muy baja, proporcionando datos casi idénticos a los de la réplica primaria. Con la réplica secundaria para las operaciones de Analysis Services, puede reducir la contención de lectura/escritura de la replicación primaria y conseguir una mejor utilización de las réplicas secundarias en el grupo de disponibilidad.  
  
 De forma predeterminada, tanto el acceso de lectura-escritura como de intención de lectura se permiten en la réplica principal y no se permiten conexiones en las réplicas secundarias. Se requiere una configuración adicional para configurar una conexión de cliente de solo lectura en una réplica secundaria. La configuración requiere establecer las propiedades en la réplica secundaria y ejecutar un script T-SQL que defina una lista de enrutamiento de solo lectura. Utilice los procedimientos siguientes para asegurarse de que ha realizado los dos pasos.  
  
> [!NOTE]  
>  En los pasos siguientes se supone que existen las bases de datos y el grupo de disponibilidad AlwaysOn. Si va a configurar un grupo, utilice el Asistente para nuevo grupo de disponibilidad a fin de crear el grupo y para combinar las bases de datos. El asistente comprueba los requisitos previos, proporciona orientación en cada paso y realiza la sincronización inicial. Para obtener más información, vea [Usar el Asistente para grupo de disponibilidad &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md).  
  
#### <a name="step-1-configure-access-on-an-availability-replica"></a>Paso 1: configurar el acceso en una réplica de disponibilidad  
  
1.  En el Explorador de objetos, conéctese a la instancia del servidor que hospeda la réplica principal y expanda el árbol.  
  
    > [!NOTE]  
    >  Estos pasos se siguen en [Configurar el acceso de solo lectura en una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md), que proporciona instrucciones alternativas e información adicional para realizar esta tarea.  
  
2.  Expanda los nodos **Alta disponibilidad de AlwaysOn** y **Grupos de disponibilidad** .  
  
3.  Haga clic en el grupo de disponibilidad cuya réplica desea cambiar. Expanda **Réplicas de disponibilidad**.  
  
4.  Haga clic con el botón derecho en la réplica secundaria y haga clic en **Propiedades**.  
  
5.  En el cuadro de diálogo **Propiedades de réplica de disponibilidad** , cambie el acceso de conexión para el rol secundario, del siguiente modo:  
  
    -   En la lista desplegable **Legible secundaria** , seleccione **Solo lectura**.  
  
    -   En la lista desplegable **Conexiones en el rol principal** , seleccione **Permitir todas las conexiones**. Este es el valor predeterminado.  
  
    -   Opcionalmente, en la lista desplegable **Modo de disponibilidad** , seleccione **Confirmación sincrónica**. Este paso no es necesario pero al configurarlo garantiza que haya paridad de datos entre la replicación primaria y la secundaria.  
  
         Esta propiedad también es un requisito para la conmutación por error planeada. Si desea realizar una conmutación por error manual planeada para pruebas, establezca **Modo de disponibilidad** en **Confirmación sincrónica** tanto para la replicación primaria como para la secundaria.  
  
#### <a name="step-2-configure-read-only-routing"></a>Paso 2: configurar el enrutamiento de solo lectura  
  
1.  Conéctese a la réplica principal.  
  
    > [!NOTE]  
    >  Estos pasos se siguen en [Configurar el enrutamiento de solo lectura para un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md), que proporciona instrucciones alternativas e información adicional para realizar esta tarea.  
  
2.  Abra una ventana de consulta y pegue en ella el siguiente script. Este script hace tres cosas: habilita las conexiones legibles en una réplica secundaria (que está desactivada de forma predeterminada), establece la dirección URL de enrutamiento de solo lectura y crea la lista de enrutamiento que dé prioridad a cómo se dirigen solicitudes de conexión.  La primera instrucción,que permite las conexiones legibles, es redundante si establece ya las propiedades en [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)], pero se incluye para que sea completa.  
  
    ```  
    ALTER AVAILABILITY GROUP [AG1]  
     MODIFY REPLICA ON  
    N'COMPUTER01' WITH   
    (SECONDARY_ROLE (ALLOW_CONNECTIONS = READ_ONLY));  
  
    ALTER AVAILABILITY GROUP [AG1]  
     MODIFY REPLICA ON  
    N'COMPUTER01' WITH   
    (SECONDARY_ROLE (READ_ONLY_ROUTING_URL = N'TCP://COMPUTER01.contoso.com:1433'));  
  
    ALTER AVAILABILITY GROUP [AG1]  
     MODIFY REPLICA ON  
    N'COMPUTER02' WITH   
    (SECONDARY_ROLE (ALLOW_CONNECTIONS = READ_ONLY));  
  
    ALTER AVAILABILITY GROUP [AG1]  
     MODIFY REPLICA ON  
    N'COMPUTER02' WITH   
    (SECONDARY_ROLE (READ_ONLY_ROUTING_URL = N'TCP://COMPUTER02.contoso.com:1433'));  
  
    ALTER AVAILABILITY GROUP [AG1]   
    MODIFY REPLICA ON  
    N'COMPUTER01' WITH   
    (PRIMARY_ROLE (READ_ONLY_ROUTING_LIST=('COMPUTER02','COMPUTER01')));  
  
    ALTER AVAILABILITY GROUP [AG1]   
    MODIFY REPLICA ON  
    N'COMPUTER02' WITH   
    (PRIMARY_ROLE (READ_ONLY_ROUTING_LIST=('COMPUTER01','COMPUTER02')));  
    GO  
    ```  
  
3.  Modifique el script, reemplazando los marcadores de posición con valores válidos para su implementación:  
  
    -   Reemplace "Computer01" con el nombre de la instancia de servidor que hospeda la replicación primaria.  
  
    -   Reemplace "Computer02" con el nombre de la instancia de servidor que hospeda la replicación secundaria.  
  
    -   Reemplace "contoso.com" con el nombre del dominio u omítalo del script si todos los equipos están en el mismo dominio. Mantenga el número de puerto si el agente de escucha utiliza el puerto predeterminado. El puerto que utiliza realmente el agente de escucha en la página de propiedades de [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)].  
  
4.  Ejecute el script.  
  
     Luego, cree un origen de datos en un modelo de Analysis Services que utilice una base de datos del grupo recién configurado.  
  
##  <a name="create-an-analysis-services-data-source-using-an-always-on-availability-database"></a><a name="bkmk_ssasAODB"></a> Crear un origen de datos de Analysis Services con la base de datos de disponibilidad AlwaysOn  
 En esta sección se explica cómo crear un origen de datos de Analysis Services que se conecta a una base de datos en un grupo de disponibilidad. Puede utilizar estas instrucciones para configurar una conexión a una replicación primaria (valor predeterminado) o a una replicación secundaria legible que se configure en función de los pasos de la sección anterior. La configuración de AlwaysOn, más las propiedades de conexión establecidas en el cliente, determinarán si se usa una réplica primaria o secundaria.  
  
1.  En [!INCLUDE[ssBIDevStudio](../../../includes/ssbidevstudio-md.md)], en el proyecto Modelo de minería de datos y multidimensional de Analysis Services, haga clic con el botón derecho en **Orígenes de datos** y seleccione **Nuevo origen de datos**. haga clic en **Nuevo** para crear un origen de datos nuevo.  
  
     O bien, en un proyecto de modelo tabular, haga clic en el menú Modelo y haga clic **Importar del origen de datos**.  
  
2.  En el Administrador de conexiones, en Proveedor, elija un proveedor que admita el protocolo Flujo TDS. SQL Server 11.0 Native Client admite este protocolo.  
  
3.  En el Administrador de conexiones, en Nombre de Servidor, escriba el nombre del *agente de escucha de grupo de disponibilidad* y elija una base de datos que esté disponible en el grupo.  
  
     El agente de escucha de grupo de disponibilidad redirige una conexión de cliente a una replicación principal para las solicitudes de lectura/escritura o a una replicación secundaria si especifica intento de lectura en la cadena de conexión. Dado que los roles de réplica cambiarán durante una conmutación por error (donde el servidor principal se convierte en el servidor secundario y un elemento secundario se convierte en uno principal), siempre debe especificarse la escucha para redirigir la conexión de cliente en consecuencia.  
  
     Para determinar el nombre del agente de escucha de grupo de disponibilidad, puede solicitarlo a un administrador de base de datos o conectarse a una instancia del grupo de disponibilidad y ver su configuración de disponibilidad AlwaysOn.   
  
4.  Aún en el Administrador de conexiones, haga clic en **todos** en el panel de navegación de la izquierda para ver la cuadrícula de propiedades del proveedor de datos.  
  
     Establezca **Intención de aplicaciones** en **READONLY** si va a configurar una conexión de cliente de solo lectura en una réplica secundaria. En otro caso mantenga el valor predeterminado **READWRITE** para redirigir la conexión a la réplica primaria.  
  
5.  En Información de suplantación, seleccione **Utilizar un nombre de usuario y una contraseña de Windows específicos** y escriba una cuenta de usuario de dominio de Windows que tenga un mínimo de permisos de **db_datareader** en la base de datos.  
  
     No elija **Usar las credenciales del usuario actual** o **Heredar**. Puede elegir **Use la cuenta de servicio**, pero solo si esa cuenta tiene permisos de lectura en la base de datos.  
  
     Finalice el origen de datos y cierre el Asistente para orígenes de datos.  
  
6.  Agregue **MultiSubnetFailover=Yes** a la cadena de conexión para especificar una detección y una conexión más rápidas al servidor activo. Para obtener más información acerca de esta propiedad, vea [Compatibilidad de SQL Server Native Client para la alta disponibilidad con recuperación de desastres](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md).  
  
     Esta propiedad no está visible en la cuadrícula de propiedades. Para agregar la propiedad, haga clic con el botón derecho en el origen de datos y elija **Ver código**. Agregue `MultiSubnetFailover=Yes` a la cadena de conexión.  
  
 El origen de datos está definida ahora. Ahora puede continuar para generar un modelo, a partir de la la vista del origen de datos o, en el caso de los modelos tabulares, creando relaciones. Cuando está en un punto en el que los datos se deban recuperar de la base de datos de la disponibilidad (por ejemplo, cuando esté preparado para procesar o para implementar la solución), puede probar la configuración para comprobar que se accede a los datos a través de la replicación secundaria.  
  
##  <a name="test-the-configuration"></a><a name="bkmk_test"></a> Probar la configuración  
 Después de configurar la replicación secundaria y crear una conexión a un origen de datos en Analysis Services, puede confirmar que los comandos de consulta y procesamiento se redirigen a la réplica secundaria. También puede realizar una conmutación por error manual planeada para comprobar el plan de recuperación para este escenario.  
  
#### <a name="step-1-confirm-the-data-source-connection-is-redirected-to-the-secondary-replica"></a>Paso 1: confirmar que la conexión a un origen de datos se redirige a la réplica secundaria  
  
1.  Inicie SQL Server Profiler y conéctese a la instancia de SQL Server que hospeda la réplica secundaria.  
  
     Cuando se ejecute el seguimiento la traza, los eventos **SQL:BatchStarting** y de **SQL:BatchCompleting** mostrarán las consultas emitidas desde Analysis Services que se ejecuta en la instancia del motor de base de datos. Estos eventos están seleccionados de forma predeterminada de modo que todo lo que debe hacer es iniciar el seguimiento.  
  
2.  En [!INCLUDE[ssBIDevStudio](../../../includes/ssbidevstudio-md.md)], abra el proyecto de Analysis Services o la solución que contenga una conexión a un origen de datos que desee probar. Asegúrese de que el origen de datos especifica el agente de escucha de grupo de disponibilidad y no una instancia del grupo.  
  
     Este paso es importante. El enrutamiento a la réplica secundaria no aparecerá si especifica un nombre de instancia de servidor.  
  
3.  Organice las ventanas de aplicación para poder ver SQL Server Profiler y [!INCLUDE[ssBIDevStudio](../../../includes/ssbidevstudio-md.md)] uno al lado del otro.  
  
4.  Implemente la solución y, cuando se complete, detenga el seguimiento.  
  
     En la ventana de seguimiento debería ver los eventos de la aplicación **Microsoft SQL Server Analysis Services**. Debe ver las instrucciones de **SELECT** que recuperan los datos de una base de datos en la instancia del servidor que hospeda la replicación secundaria, lo que prueba que la conexión se realiza a través del agente escucha a la réplica secundaria.  
  
#### <a name="step-2-perform-a-planned-failover-to-test-the-configuration"></a>Paso 2: realizar una conmutación por error planeada para probar la configuración  
  
1.  En [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] compruebe las réplicas primaria y secundaria para asegurarse de que ambas están configuradas para el modo de confirmación sincrónico y que están sincronizadas.  
  
     En los pasos siguientes se supone que una réplica secundaria está configurada para la confirmación sincrónica.  
  
     Para comprobar la sincronización, abra una conexión a cada instancia que hospeda las réplicas primarias y secundaria, expanda la carpeta de bases de datos y asegúrese de que la base de datos tenga anexado **(Sincronizado)** y **(Sincronizando)** al nombre de cada réplica.  
  
    > [!NOTE]  
    >  Estos pasos se siguen en [Realizar una conmutación por error manual planeada de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md), que proporciona instrucciones alternativas e información adicional para realizar esta tarea.  
  
2.  En SQL Server Profiler, inicie los seguimientos para cada réplica y vea los seguimientos en paralelo. En los pasos siguientes, comparará los seguimientos, confirmando que las consultas SQL usadas para el procesamiento o la consulta desde Analysis Services pasan de una réplica a la otra.  
  
3.  Ejecuta un comando de procesamiento o consulta desde Analysis Services. Dado que configuró el origen de datos para una conexión de solo lectura, debería ver el comando ejecutarse en la réplica secundaria.  
  
4.  En [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)], conéctese a la réplica secundaria.  
  
5.  Expanda los nodos **Alta disponibilidad de AlwaysOn** y **Grupos de disponibilidad** .  
  
6.  Haga clic con el botón derecho en el grupo de disponibilidad que va a ser objeto de conmutación por error y seleccione el comando **Conmutación por error** . Esto inicia el Asistente para conmutación por error del grupo de disponibilidad. Use el asistente para elegir qué réplica convertir en la la nueva réplica primaria.  
  
7.  Confirme que la conmutación por error se realizó correctamente:  
  
    -   En [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)], expanda los grupos disponibilidad para ver las designaciones (primaria) y (secundaria). La instancia que era anteriormente primaria ahora debe ser ahora secundaria.  
  
    -   Vea el panel para determinar si se ha detectado algún problema de mantenimiento. Haga clic con el botón derecho en el grupo de disponibilidad y seleccione **Mostrar el panel**.  
  
8.  Espere uno o dos minutos a que se complete la conmutación por error en el servidor.  
  
9. Repita el comando de procesamiento o de consulta en la solución Analysis Services y después observe los seguimientos en SQL Server Profiler. Debe ver la prueba del procesamiento en la otra instancia, que ahora es la nueva réplica secundaria.  
  
##  <a name="what-happens-after-a-failover-occurs"></a><a name="bkmk_whathappens"></a> Qué ocurre después de una conmutación por error  
 Durante una conmutación por error, una réplica secundaria realiza la transición al rol principal y la réplica principal anterior realiza la transición al rol secundario. Todas las conexiones de cliente se terminan, la propiedad del agente de grupo de disponibilidad pasa con el rol de réplica principal a una nueva instancia de SQL Server y el punto de conexión del agente de escucha se enlaza a los puertos TCP y las direcciones IP virtuales de la nueva instancia. Para obtener más información, vea [Acerca del acceso de conexión de cliente a réplicas de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md).  
  
 Si la conmutación por error se produce durante el procesamiento, aparece el siguiente error en Analysis Services en el archivo de registro o la ventana de resultados: "Error de OLE DB: OLE DB o error de ODBC: Error de vínculo de comunicación; 08S01; proveedor de TPC: el host remoto cerró a la fuerza una conexión existente. ; 08S01".  
  
 Este error se debe resolver si espera un minuto y vuelve a intentarlo. Si el grupo de disponibilidad se configura correctamente para la réplica secundaria legible, el procesamiento se reanudará en la nueva réplica secundaria cuando se reintente el procesamiento.  
  
 Los errores persistentes es más probable uqe se deban a un problema de configuración. Puede intentar volver a ejecutar el script T-SQL para solucionar los problemas de la lista de enrutamiento, las direcciones URL de enrutamiento de solo lectura y el intento de lectura en la réplica secundaria. También debe comprobar que la replicación primaria permite todas las conexiones.  
  
##  <a name="writeback-when-using-an-always-on-availability-database"></a><a name="bkmk_writeback"></a> Reescritura al usar una base de datos de disponibilidad AlwaysOn  
 La reescritura es una característica de Analysis Services que admite realizar análisis Y si en Excel. También es de utilidad para tareas de presupuesto y previsión en aplicaciones personalizadas.  
  
 La compatibilidad con la reescritura requiere una conexión de cliente READWRITE. En Excel, si intenta reescribir en una conexión de solo lectura, aparecerá un error similar al siguiente: "No se pudieron recuperar datos del origen de datos externo".  
  
 Si configuró una conexión para acceder siempre en una replica secundaria legible, ahora debe configurar una nueva conexión que use una conexión READWRITE a la replicación primaria.  
  
 Para ello, cree un origen de datos adicional en un modelo de Analysis Services para admitir la conexión de lectura/escritura. Al crear el origen de datos adicional, use la misma base de datos y nombre de agente de escucha especificados en la conexión de solo lectura pero, en lugar de modificar **Intención de aplicaciones**, mantenga el valor predeterminado que admite las conexiones READWRITE. Ahora puede agregar nuevas tablas de dimensiones o hechos a la vista del origen de datos que se basen en el origen de datos de lectura/escritura y luego habilitar la reescritura en las nuevas tablas.  
  
## <a name="see-also"></a>Consulte también  
 [Agentes de escucha de grupo de disponibilidad, conectividad de cliente y conmutación por error de una aplicación &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [Secundarias activas: réplicas secundarias legibles &#40;Grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)   
 [Directivas de AlwaysOn para problemas operativos con Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-policies-for-operational-issues-always-on-availability.md)   
 [Crear un origen de datos &#40;SSAS multidimensional&#41;](/analysis-services/multidimensional-models/create-a-data-source-ssas-multidimensional)   
 [Habilitar reescritura en la dimensión](/analysis-services/multidimensional-models/bi-wizard-enable-dimension-writeback)  
  
