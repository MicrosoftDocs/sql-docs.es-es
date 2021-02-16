---
title: 'Actualización: Asistente para instalación (configuración)'
description: El Asistente para instalación de SQL Server proporciona un único árbol de características para la actualización local de componentes de SQL Server a la versión más reciente de SQL Server.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- upgrading Database Engine
- Database Engine [SQL Server], upgrading
ms.assetid: cef118a5-a7ce-4bfa-8b9d-c81996284cfc
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: a5621b9444d56307d21855c9b07aefe736d382d0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353852"
---
# <a name="upgrade-sql-server-using-the-installation-wizard-setup"></a>Actualizar SQL Server con el Asistente para instalación (programa de instalación)

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

El Asistente para instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proporciona un único árbol de características para la actualización local de componentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a la versión más reciente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
>[!WARNING]  
>Cuando actualice a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se sobrescribirá y ya no estará en el equipo. 
>
>Antes de actualizar, realice una copia de seguridad de las bases de datos de SQL Server y de otros objetos asociados con la instancia anterior de SQL Server.  
  
> [!CAUTION]  
> Para muchos entornos de producción y algunos de desarrollo, una nueva actualización de la instalación o una actualización gradual es más apropiada que una actualización local.  Para más información sobre los métodos de actualización, vea:
> * [Elegir un método de actualización del motor de base de datos](../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md)
> * [Actualizar Data Quality Services](../../database-engine/install-windows/upgrade-data-quality-services.md)
> * [Actualizar Integration Services](../../integration-services/install-windows/upgrade-integration-services.md)
> * [Actualizar Master Data Services](../../database-engine/install-windows/upgrade-master-data-services.md)
> * [Actualizar y migrar Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)
> * [Actualizar Analysis Services](../../database-engine/install-windows/upgrade-analysis-services.md)
> * [Actualización de PowerPivot para SharePoint](../../database-engine/install-windows/upgrade-power-pivot-for-sharepoint.md)  
  
## <a name="prerequisites"></a>Requisitos previos  
Debe ejecutar el programa de instalación como administrador. Si instala [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] desde un recurso compartido remoto, debe usar una cuenta de dominio que tenga permisos de lectura y ejecución para el recurso compartido remoto y que sea de un administrador local.  
  
> [!WARNING]  
>  Tenga en cuenta que no puede cambiar las características que se van a actualizar, y no puede agregar características durante la operación de actualización. Para más información sobre cómo agregar características a una instancia actualizada de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] una vez completada la operación de actualización, vea [Agregar características a una instancia de SQL Server &#40;programa de instalación&#41;](./add-features-to-an-instance-of-sql-server-setup.md).  
  
 Si va a actualizar el [!INCLUDE[ssDE](../../includes/ssde-md.md)], revise [Planeamiento y prueba del plan de actualización del motor de base de datos](../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md) y, luego, realice las siguientes tareas, según corresponda para su entorno:  
  
-   Realice una copia de seguridad de todos los archivos de base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la instancia que va a actualizar, para que pueda restaurarlos al completo, si fuera necesario.  
  
-   Ejecute los comandos de consola de datos (DBCC) en las bases de datos que vaya a actualizar para asegurarse de que se encuentran en un estado coherente.  
  
-   Calcule el espacio en disco necesario para actualizar los componentes de SQL Server, así como las bases de datos de usuario. Para obtener el espacio en disco necesario para los componentes de SQL Server, vea [Requisitos de hardware y software para instalar SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md).  
  
-   Asegúrese de que las bases de datos del sistema de SQL Server (maestra, de modelos, msdb y tempdb) existentes están configuradas para el crecimiento automático; asegúrese también de que tienen suficiente espacio disponible en disco duro.  
  
-   Asegúrese de que todos los servidores de bases de datos tienen información de inicio de sesión en la base de datos maestra. Esto es especialmente importante para restaurar las bases de datos, ya que la información de inicio de sesión del sistema reside en la base de datos maestra.  
  
-   Deshabilite todos los procedimientos almacenados de inicio, ya que el proceso de actualización se detendrá e iniciará los servicios en la instancia de SQL Server que se vaya a actualizar. Los procedimientos almacenados procesados al inicio podrían impedir el proceso de actualización.  
  
-   Cuando actualice instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en que el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] está dado de alta en relaciones de MSX/TSX, actualice los servidores de destino antes de actualizar los servidores maestros. Si actualiza los servidores maestros antes de actualizar los servidores de destino, el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no podrá conectarse a las instancias maestras de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Cierre todas las aplicaciones, incluidos los servicios que tengan dependencias de SQL Server. La actualización puede ser errónea si hay aplicaciones locales conectadas a la instancia que se va a actualizar.  
  
-   Asegúrese de que la replicación está vigente y, a continuación, detenga la replicación.   
    Para obtener instrucciones detalladas para realizar una actualización gradual en un entorno replicado, vea [Actualizar bases de datos replicadas](../../database-engine/install-windows/upgrade-replicated-databases.md).
  
## <a name="procedure"></a>Procedimiento  
  
### <a name="to-upgrade-ssnoversion"></a>Para actualizar [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)]  
  
1.  Inserte el disco de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y, en la carpeta raíz, haga doble clic en Setup.exe. Para realizar la instalación desde un recurso compartido de red, vaya a la carpeta raíz de dicho recurso y, a continuación, haga doble clic en Setup.exe.  
  
2.  El Asistente para la instalación inicia el Centro de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para actualizar una instancia existente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], haga clic en **Instalación** en el área de navegación de la izquierda y, después, haga clic en **Actualizar desde...** versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
3.  En la página Clave del producto, haga clic en una opción para indicar si va a actualizar a una edición gratuita de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]o si tiene una clave de PID para una versión de producción del producto. Para obtener más información, vea [Ediciones y componentes de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md) y [Actualizaciones de ediciones y versiones admitidas](../../database-engine/install-windows/supported-version-and-edition-upgrades.md).  
  
4.  En la página Términos de licencia, revise el contrato de licencia y, si está de acuerdo, active la casilla **Acepto los términos de licencia** y haga clic en **Siguiente**. Para ayudar a mejorar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], también puede habilitar la opción de uso de características y enviar informes a [!INCLUDE[msCoName](../../includes/msconame-md.md)].  
  
5.  En la ventana Reglas globales, el procedimiento de instalación avanzará automáticamente hasta la ventana Actualizaciones de productos si no hay ningún error de regla.  
  
6.  La página [!INCLUDE[msCoName](../../includes/msconame-md.md)] Update aparecerá a continuación si la casilla [!INCLUDE[msCoName](../../includes/msconame-md.md)] Update del Panel de control\Todos los elementos de Panel de control\Windows Update\Cambiar configuración no está activada. Si se pone una marca de verificación en la página [!INCLUDE[msCoName](../../includes/msconame-md.md)] Update, cambiará la configuración del equipo para incluir las últimas actualizaciones al buscar actualizaciones en Windows Update.  
  
7.  En la página Actualizaciones del producto se muestran las actualizaciones del producto [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] más recientes disponibles. Si no quiere incluir las actualizaciones, desactive la casilla **Incluir actualizaciones de productos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** . Si no se detectan actualizaciones de producto, el programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no muestra esta página y pasa automáticamente a la página **Instalar archivos de instalación** .  
  
8.  En la página Instalar archivos de instalación, el programa de instalación proporciona el progreso de descarga, extracción e instalación de los archivos de instalación. Si se encuentra una actualización para la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], y se especifica que debe incluirse, esa actualización también se instalará.  
  
9. En la ventana Reglas de actualización, el procedimiento de instalación avanzará automáticamente hasta la ventana Seleccionar instancia si no hay ningún error de regla.  
  
10. En la página Seleccionar instancia, especifique la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que va a actualizar. Para actualizar las herramientas de administración y las características compartidas, seleccione **Actualizar solo características compartidas**.  
  
11. En la página Seleccionar características, aparecen seleccionadas previamente las características que se van a actualizar. Después de seleccionar el nombre de la característica se muestra una descripción de cada grupo de componentes en el panel derecho.  
  
     Los requisitos previos para las características seleccionadas se muestran en el recuadro del lado derecho. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] El programa de instalación instalará los requisitos previos que no se hayan instalado todavía durante el paso de instalación que se describe más adelante en este procedimiento.  
  
    > [!NOTE]  
    >  Si optó por actualizar las características compartidas seleccionando **\<Upgrade shared features only>** en la página **Seleccionar instancia**, todas las características compartidas estarán ya seleccionadas en la página Selección de características. Todos los componentes compartidos se actualizan al mismo tiempo.  
  
12. En la página Configuración de instancia, especifique el identificador de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     **Id. de instancia** : de manera predeterminada, el nombre de instancia se usa como identificador de la instancia. Se usa para identificar los directorios de instalación y las claves del Registro para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Es así en las instancias predeterminadas y en las instancias con nombre. Con una instancia predeterminada, el nombre y el identificador serían MSSQLSERVER. Para usar un identificador de instancia no predeterminado, proporcione un valor para el cuadro de texto **Id. de instancia** .  
  
     Todos los Service Pack y actualizaciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se aplicarán a cada componente de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     **Instancias instaladas**: la cuadrícula mostrará las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que están en el equipo en el que se ejecuta el programa de instalación. Si ya hay una instancia predeterminada instalada en el equipo, debe instalar una instancia con nombre de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
13. El flujo de trabajo para el resto del artículo depende de las características que haya especificado para la instalación. Dependiendo de las selecciones, es posible que no vea todas las páginas.  
  
14. En la página Configuración del servidor - Cuentas de servicio, las cuentas de servicio predeterminadas se muestran para los servicios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los servicios reales que se configuran en esta página dependen de las características que se van a actualizar.  
  
     La información sobre autenticación e inicio de sesión se obtiene de la instancia anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Puede asignar la misma cuenta de inicio de sesión a todos los servicios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , o configurar cada cuenta de servicio individualmente. También puede especificar si los servicios se inician de forma automática o manual, o si están deshabilitados. [!INCLUDE[msCoName](../../includes/msconame-md.md)] recomienda configurar las cuentas de servicio de forma individual para que se concedan a los servicios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] los permisos mínimos que necesitan para completar sus tareas. Para obtener más información, consulte [Configurar los permisos y las cuentas de servicio de Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
     Para especificar la misma cuenta de inicio de sesión para todas las cuentas de servicio en esta instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se han de proporcionar credenciales en los campos de la parte inferior de la página.  
  
     **Nota de seguridad** [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]  
  
     Cuando termine de especificar la información de inicio de sesión para los servicios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , haga clic en **Siguiente**.  
  
15. En la página Opción de actualización de búsqueda de texto completo, especifique las opciones de actualización para las bases de datos que se van a actualizar. Para obtener más información, vea [Opciones de actualización de búsqueda de texto completo](./install-sql-server.md).  
  
16. La ventana Reglas de características se pasará automáticamente si se cumplen todas las reglas.  
  
17. La página Listo para actualizar muestra una vista de árbol de las opciones de instalación que se especificaron durante la instalación. Para continuar, haga clic en **Instalar**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] El programa de instalación instalará primero los requisitos previos necesarios para las características seleccionadas y a continuación realizará la instalación de características.  
  
18. Durante la instalación, la página de progreso muestra el estado para que pueda supervisar el progreso de la instalación durante la ejecución del programa de instalación.  
  
19. Después de la instalación, la página Operación completada proporciona un vínculo al archivo de registro de resumen para la instalación y otras notas importantes. Para completar el proceso de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , haga clic en **Cerrar**.  
  
20. Si el programa indica que se reinicie el equipo, hágalo ahora. Es importante leer el mensaje del Asistente para la instalación tras finalizar el programa de instalación. Para obtener más información sobre los archivos de registro de instalación, vea [Ver y leer los archivos de registro de instalación de SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  
  
## <a name="next-steps"></a>Pasos siguientes  
 Después de actualizar a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], complete las tareas siguientes:  
  
-   **Registrar los servidores**: la actualización quita la configuración del Registro de la instancia anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Tras la actualización, debe volver a registrar los servidores.  
  
-   **Actualizar las estadísticas**: para poder optimizar el rendimiento de las consultas, es recomendable actualizar las estadísticas de todas las bases de datos tras la actualización. Use el procedimiento almacenado **sp_updatestats** para actualizar las estadísticas de las tablas definidas por el usuario en las bases de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   **Configurar la nueva instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** : para reducir el área expuesta del sistema susceptible de recibir ataques, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instala y habilita los servicios y características clave de forma selectiva. Para obtener más información sobre la configuración del área expuesta, vea el archivo Léame correspondiente a esta versión.  
  
## <a name="see-also"></a>Consulte también  
 [Actualizar SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)   
 [Compatibilidad con versiones anteriores](/previous-versions/sql/sql-server-2016/cc280407(v=sql.130))  
  
