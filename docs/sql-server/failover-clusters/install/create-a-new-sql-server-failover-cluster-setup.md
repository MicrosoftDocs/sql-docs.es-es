---
title: Creación de una nueva instancia de clúster de conmutación por error
description: En este artículo se describe cómo usar el programa de instalación para instalar o actualizar una instancia de clúster de conmutación por error Always On de SQL Server o agregar un nodo a una instancia de clúster de conmutación por error existente.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.reviewer: ''
ms.prod: sql
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- adding nodes
- failover clustering [SQL Server], creating clusters
- nodes [Faillover Clustering], removing
- nodes [Faillover Clustering], adding
- clusters [SQL Server], creating
- removing nodes
ms.assetid: 30e06a7d-75e9-44e2-bca3-b3b0c4a33f61
author: cawrites
ms.author: chadam
ms.openlocfilehash: 63b850d847e5ba0980f002e02a2f2604a845dc0b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351719"
---
# <a name="create-a-new-always-on-failover-cluster-instance-setup"></a>Creación de una nueva instancia de clúster de conmutación por error Always On (programa de instalación)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Para instalar o actualizar una instancia de clúster de conmutación por error (FCI) de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], debe ejecutar el programa de instalación en cada nodo del clúster de conmutación por error de Windows Server subyacente. Para agregar un nodo a una instancia de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] existente, debe ejecutar el programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en el nodo que se va a agregar a la instancia del clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. No ejecute el programa de instalación en el nodo activo para administrar los demás nodos.  
  
 Dependiendo de cómo se agrupen en clústeres los nodos, la instancia de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se configurará de las maneras siguientes:  
  
-   Nodos en la misma subred o el mismo conjunto de subredes: la dependencia de recursos de dirección IP se establece en AND para estos tipos de configuraciones.  
  
-   Nodos en subredes diferentes: la dependencia de recursos de dirección IP se establece en OR y esta configuración se denomina configuración de instancias de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Para obtener más información, vea [Agrupación en clústeres de varias subredes de SQL Server &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/sql-server-multi-subnet-clustering-sql-server.md).  
  
 Las opciones siguientes están disponibles para la instalación de clústeres de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] :  
  
 **Opción 1: instalación integrada con Agregar nodo**   
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] consta de los pasos siguientes:  
  
-   Cree y configure una instancia en clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] de un único nodo. Cuando se configura el nodo correctamente, se tiene una instancia totalmente funcional de clústeres de conmutación por error. En ese momento no tendrá una alta disponibilidad porque solamente hay un nodo en la instancia de clúster de conmutación por error.  
  
-   En cada nodo que se va a agregar a la instancia de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], ejecute el programa de instalación con la funcionalidad Agregar nodo para agregar ese nodo.  
  
    -   Si el nodo que agrega tiene subredes adicionales o diferentes, el programa de instalación le permite especificar más direcciones IP. Si el nodo que va a agregar está en una subred diferente, también debe confirmar el cambio de dependencias de recursos de dirección IP a OR. Para obtener más información sobre los posibles escenarios durante las operaciones de adición de un nodo, vea [Agregar o quitar nodos en un clúster de conmutación por error de SQL Server (programa de instalación)](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md).  
  
 **Opción 2: Instalación de Advanced/Enterprise**  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] La instalación de clústeres de conmutación por error de Advanced/Enterprise consta de los pasos siguientes:  
  
-   En cada nodo que es un posible propietario del nuevo clústeres de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , siga los pasos de instalación referidos a la preparación de clústeres de conmutación por error que se enumeran en la [sección Preparar](#prepare). Una vez ejecutada la preparación de clústeres de conmutación por error en un nodo, el programa de instalación crea el archivo Configuration.ini, que enumera todos los valores de configuración especificados. En los nodos adicionales que se van a preparar, en lugar de seguir estos pasos, puede proporcionar el archivo Configuration.ini autogenerado del primer nodo como entrada a la línea de comandos del programa de instalación. Para obtener más información, vea [Instalar SQL Server 2016 mediante un archivo de configuración](../../../database-engine/install-windows/install-sql-server-using-a-configuration-file.md). En este paso se preparan los nodos para su agrupación en clústeres, pero al final de este paso no hay ninguna instancia operativa de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
-   Una vez preparados los nodos para la agrupación en clústeres, ejecute el programa de instalación en uno de los nodos preparados. En este paso se configura y se finaliza la instancia de los clústeres de conmutación por error. Al final de este paso, tendrá una instancia operativa de los clústeres de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y todos los nodos que se prepararon previamente para esa instancia serán los posibles propietarios de la instancia de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que se acaba de crear.  
  
     Si está creando una instancia de clúster de conmutación por error que abarca varias subredes, el programa de instalación detectará la unión de todas las subredes en todos los nodos que tienen la instancia en clúster de conmutación por error preparada de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Podrá especificar varias direcciones IP para las subredes. Cada nodo preparado debe ser el posible propietario de al menos una dirección IP.  
  
     Si cada una de las direcciones IP especificadas para las subredes se admite en todos los nodos preparados, la dependencia se establece en AND.  
  
     Si cada una de las direcciones IP especificadas para las subredes no se admite en todos los nodos preparados, la dependencia se establece en OR. Para obtener más información, vea [Agrupación en clústeres de varias subredes de SQL Server &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/sql-server-multi-subnet-clustering-sql-server.md).  
  
    > [!NOTE]  
    >  La funcionalidad Completar los clústeres de conmutación por error requiere la existencia de los clústeres de conmutación por error de Windows Server subyacente. Si el clúster de conmutación por error de Windows Server no existe, el programa de instalación produce un error y se cierra.  
  
 Para obtener más información sobre cómo agregar o quitar nodos en una instancia de clúster de conmutación por error existente, vea [Agregar o quitar nodos en un clúster de conmutación por error de SQL Server (programa de instalación)](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md).  
  
 Para obtener más información sobre la instalación remota, vea [Actualizaciones de ediciones y versiones admitidas](../../../database-engine/install-windows/supported-version-and-edition-upgrades.md).  
  
 Para obtener más información acerca de cómo instalar [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] en un clúster WSFC, vea [Organizar en clúster SQL Server Analysis Services](/previous-versions/sql/sql-server-2012/dn736073(v=msdn.10)).  
  
## <a name="prerequisites"></a>Requisitos previos  
 Antes de empezar, revise los siguientes temas de los Libros en pantalla de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] :  
  
-   [Planear una instalación de SQL Server](../../../sql-server/install/planning-a-sql-server-installation.md)  
  
-   [Antes de instalar los clústeres de conmutación por error](../../../sql-server/failover-clusters/install/before-installing-failover-clustering.md)  
  
-   [Consideraciones de seguridad para una instalación de SQL Server](../../../sql-server/install/security-considerations-for-a-sql-server-installation.md)  
  
-   [Agrupación en clústeres de varias subredes de SQL Server &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/sql-server-multi-subnet-clustering-sql-server.md)  
  
> [!NOTE]  
>  Tome nota de la ubicación de la unidad compartida en el Administrador de clústeres antes de ejecutar el programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Debe tener esta información para crear un nueva instancia de clúster de conmutación por error.  
  
### <a name="to-install-a-new-ssnoversion-failover-cluster-instance-using-integrated-install-with-add-node"></a>Para instalar una nueva instancia de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] utilizando la instalación integrada con Agregar nodo.  
  
1.  Inserte el disco de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y, en la carpeta raíz, haga doble clic en Setup.exe. Para realizar la instalación desde un recurso compartido de red, desplácese a la carpeta raíz de dicho recurso y, a continuación, haga doble clic en Setup.exe. Para obtener más información acerca de cómo instalar los requisitos previos, vea [Before Installing Failover Clustering](../../../sql-server/failover-clusters/install/before-installing-failover-clustering.md).  
  
2.  El Asistente para la instalación inicia el Centro de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Para crear una nueva instalación de clústeres de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], haga clic en **Nueva instalación de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]** en la página de instalación.  
  
3.  El Comprobador de configuración del sistema ejecuta una operación de detección en el equipo. Para continuar, [!INCLUDE[clickOK](../../../includes/clickok-md.md)]. Puede ver los detalles en la pantalla haciendo clic en **Mostrar detalles** o, como un informe HTML, haciendo clic en **Ver informe detallado**.  
  
4.  Para continuar, haga clic en **Siguiente**.  
  
5.  En la página Archivos auxiliares del programa de instalación, haga clic en **Instalar** para instalar los archivos auxiliares del programa de instalación.  
  
6.  El Comprobador de configuración del sistema comprueba el estado del sistema del equipo antes de seguir con la instalación. Cuando se haya completado la comprobación, haga clic en **Siguiente** para continuar. Puede ver los detalles en la pantalla haciendo clic en **Mostrar detalles** o, como un informe HTML, haciendo clic en **Ver informe detallado**.  
  
7.  En la página Clave del producto, indique si va a instalar una edición gratuita de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]o si tiene una clave de PID para una versión de producción del producto. Para obtener más información, vea [Ediciones y componentes de SQL Server 2016](../../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
8.  En la página Términos de licencia, lea el contrato de licencia y active la casilla para aceptar los términos y condiciones de la licencia. Para ayudar a mejorar [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], también puede habilitar la opción de uso de características y enviar informes a [!INCLUDE[msCoName](../../../includes/msconame-md.md)]. Haga clic en **Siguiente** para continuar. Para salir del programa de instalación, haga clic en **Cancelar**.  
  
9. En la página Selección de características, seleccione los componentes de la instalación. Después de seleccionar el nombre de la característica se muestra una descripción de cada grupo de componentes en el panel derecho. Puede activar cualquier combinación de casillas, pero solo [!INCLUDE[ssDE](../../../includes/ssde-md.md)], [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] en modo tabular y [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] en modo multidimensional admiten los clústeres de conmutación por error. Los demás componentes seleccionados se ejecutarán como una característica independiente sin la funcionalidad de conmutación por error en el nodo actual en el que esté ejecutando el programa de instalación. Para obtener más información sobre modos de [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] , vea [Determinar el modo de servidor de una instancia de Analysis Services](/analysis-services/instances/determine-the-server-mode-of-an-analysis-services-instance).  
  
     Los requisitos previos para las características seleccionadas se muestran en el recuadro del lado derecho. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] El programa de instalación instalará los requisitos previos que no se hayan instalado todavía durante el paso de instalación que se describe más adelante en este procedimiento.  
  
     Si desea especificar un directorio personalizado para los componentes compartidos, use el campo situado en la parte inferior de esta página. Para cambiar la ruta de instalación de los componentes compartidos, actualícela en el campo que se proporciona en la parte inferior del cuadro de diálogo o haga clic en el botón de puntos suspensivos para desplazarse a un directorio de instalación. La ruta de instalación predeterminada es C:\Archivos de programa\\[!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] \\.  
  
     [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] también admite la instalación de bases de datos del sistema (Master, Model, MSDB y TempDB) y de bases de datos de usuario de [!INCLUDE[ssDE](../../../includes/ssde-md.md)] en un recurso compartido de archivos de Bloque de mensajes del servidor (SMB). Para obtener más información sobre cómo instalar [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] con el recurso compartido de archivos de SMB como almacenamiento, vea [Instalar SQL Server con el recurso compartido de archivos SMB como opción de almacenamiento](../../../database-engine/install-windows/install-sql-server-with-smb-fileshare-as-a-storage-option.md).  
  
     La ruta de acceso especificada para los componentes compartidos debe ser una ruta de acceso absoluta. La carpeta no se debe comprimir ni cifrar. No se admiten las unidades asignadas. Si está instalando [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en un sistema operativo de 64 bits, verá las opciones siguientes:  
  
    1.  Directorio de características compartidas  
  
    2.  Directorio de características compartidas (x86)  
  
     La ruta de acceso especificada para cada una de las opciones anteriores debe ser diferente.  
  
    > [!NOTE]  
    >  Si selecciona la característica Servicios de [!INCLUDE[ssDE](../../../includes/ssde-md.md)] , se seleccionan automáticamente la replicación y la búsqueda de texto completo. Data Quality Services (DQS) también se selecciona al seleccionar la característica Servicios de [!INCLUDE[ssDE](../../../includes/ssde-md.md)] . Si anula la selección de cualquiera de estas subcaracterísticas, también se anula la selección de la característica Servicios de [!INCLUDE[ssDE](../../../includes/ssde-md.md)] .  
  
10. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ejecuta uno o varios conjuntos de reglas que se basan en las características que seleccionó para validar la configuración.  
  
11. En la página Configuración de instancia, especifique si desea instalar una instancia predeterminada o una instancia con nombre. Para obtener más información, vea [Instance Configuration](../../install/instance-configuration.md).  
  
     **[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Nombre de red**: especifique un nombre de red para la nueva instancia de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Este es el nombre que se usa para identificar la instancia de clúster de conmutación por error en la red.  
  
    > [!NOTE]  
    >  Se conoce como el nombre de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] virtual en versiones anteriores de instancias de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
     **Id. de instancia** : de manera predeterminada, el nombre de instancia se usa como identificador de la instancia. Se usa para identificar los directorios de instalación y las claves del Registro para la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Es así en las instancias predeterminadas y en las instancias con nombre. Con una instancia predeterminada, el nombre y el identificador serían MSSQLSERVER. Para utilizar un identificador de instancia no predeterminado, seleccione el cuadro **Id. de instancia** y proporcione un valor.  
  
    > [!NOTE]  
    >  Las instancias independientes típicas de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], tanto si son predeterminadas como si son instancias con nombre, no usan un valor no predeterminado para el cuadro **Id. de instancia** .  
  
     **Directorio raíz de instancia**: de forma predeterminada, el directorio raíz de la instancia es C:\Archivos de programa\\[!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]\\. Para especificar un directorio raíz no predeterminado, utilice el campo proporcionado o haga clic en el botón de puntos suspensivos para buscar una carpeta de instalación.  
  
     **Características e instancias de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] detectadas en este equipo**: la cuadrícula muestra las instancias de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que están en el equipo en el que se ejecuta el programa de instalación. Si ya hay una instancia predeterminada instalada en el equipo, debe instalar una instancia con nombre de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Haga clic en **Siguiente** para continuar.  
  
12. Use la página Grupo de recursos de clúster para especificar el nombre, o rol, del grupo de recursos de clúster donde se ubicarán los recursos de servidor virtual de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Para especificar el nombre del grupo de recursos de clúster de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , tiene dos opciones:  
  
    -   Use la lista desplegable para especificar un grupo existente.  
  
    -   Escriba el nombre del grupo que desea crear. Tenga en cuenta que el nombre "Available storage" no es un nombre de grupo válido.  
  
13. En la página Selección de disco de clúster, seleccione el recurso de disco de clúster compartido para la instancia de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. El disco de clúster es donde se colocarán los datos de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Se puede especificar más de un disco. La cuadrícula Discos compartidos disponibles muestra la lista de discos disponibles, si están calificados todos ellos como discos compartidos, y una descripción de cada recurso de disco. Haga clic en **Siguiente** para continuar.  
  
    > [!NOTE]  
    >  La primera unidad se utiliza como unidad predeterminada para todas las bases de datos, pero se puede cambiar en las páginas de configuración del [!INCLUDE[ssDE](../../../includes/ssde-md.md)] o de [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] .  
    >   
    >  En la página Selección de disco de clúster, puede omitir la selección de cualquier disco compartido si desea usar el recurso compartido de archivos de SMB como carpeta de datos.  
  
14. En la página Configuración de red en clúster, especifique los recursos de red para la instancia de clústeres de conmutación por error:  
  
    -   **Configuración de red**: especifique el tipo de IP y la dirección IP de la instancia del clúster de conmutación por error.  
  
     Haga clic en **Siguiente** para continuar.  
  
15. Utilice esta página para especificar la Directiva de seguridad de clúster.  
  
    -   [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] y versiones posteriores: los SID (identificadores de seguridad de servidor) de servicio son la configuración predeterminada y recomendada. No hay ninguna opción para cambiarlo para grupos de seguridad. Para obtener información sobre la funcionalidad de SID de servicio en [!INCLUDE[nextref_longhorn](../../../includes/nextref-longhorn-md.md)], vea [Configurar los permisos y las cuentas de servicio de Windows](../../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md). Se ha probado en configuraciones independientes y de clúster de [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)].  
  
    -   En [!INCLUDE[winxpsvr](../../../includes/winxpsvr-md.md)], especifique grupos de dominio para los servicios de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Todos los permisos de recursos se controlan mediante grupos de nivel de dominio que incluyen cuentas de servicio de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] como miembros de grupo.  
  
     Haga clic en **Siguiente** para continuar.  
  
16. El flujo de trabajo para el resto del tema depende de las características que haya especificado para la instalación. Dependiendo de las selecciones ([!INCLUDE[ssDE](../../../includes/ssde-md.md)], [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]y [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]), es posible que no vea todas las páginas.  
  
17. En la página Configuración del servidor - Cuentas de servicio, especifique las cuentas de inicio de sesión para los servicios de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Los servicios reales que se configuran en esta página dependen de las características que se van a instalar.  
  
     Puede asignar la misma cuenta de inicio de sesión a todos los servicios de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , o configurar cada cuenta de servicio individualmente. El tipo de inicio se establece en manual para todos los servicios que reconocen clústeres, incluidos la búsqueda de texto completo y el Agente [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], y no se puede cambiar durante la instalación. [!INCLUDE[msCoName](../../../includes/msconame-md.md)] recomienda configurar las cuentas de servicio de forma individual para proporcionar a cada servicio los privilegios mínimos; así, los servicios de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] obtendrán los permisos mínimos que necesitan para completar sus tareas. Para obtener más información, vea [Configuración del servidor - Cuentas de servicio](../../../database-engine/install-windows/install-sql-server.md) y [Configurar los permisos y las cuentas de servicio de Windows](../../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
     Para especificar la misma cuenta de inicio de sesión para todas las cuentas de servicio en esta instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], las credenciales se proporcionan en los campos de la parte inferior de la página.  
  
     **Nota de seguridad** [!INCLUDE[ssNoteStrongPass](../../../includes/ssnotestrongpass-md.md)]  
  
     Cuando termine de especificar la información de inicio de sesión para los servicios de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , haga clic en **Siguiente**.  
  
18. Use la pestaña **Configuración del servidor - Intercalación** para especificar intercalaciones no predeterminadas para [!INCLUDE[ssDE](../../../includes/ssde-md.md)] y [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].  
  
19. Use la página Configuración de [!INCLUDE[ssDE](../../../includes/ssde-md.md)] - Aprovisionamiento de cuentas para especificar lo siguiente:  
  
    -   Modo de Seguridad: seleccione la autenticación de Windows o la autenticación de modo mixto para la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Si selecciona la autenticación de modo mixto, debe proporcionar una contraseña segura para la cuenta de administrador del sistema de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] integrada.  
  
         Una vez que un dispositivo establece una conexión correcta con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], el mecanismo de seguridad es el mismo para la autenticación de Windows y para el modo mixto.  
  
    -   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Administradores: debe especificar al menos un administrador del sistema para la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Para agregar la cuenta en la que se ejecuta el programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , haga clic en **Agregar usuario actual**. Para agregar o quitar cuentas de la lista de administradores del sistema, haga clic en **Agregar** o en **Quitar** y, a continuación, modifique la lista de usuarios, grupos o equipos que tendrán privilegios de administrador para la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
     Cuando haya terminado de modificar la lista, haga clic en [!INCLUDE[clickOK](../../../includes/clickok-md.md)]. Compruebe la lista de administradores en el cuadro de diálogo de configuración. Cuando la lista esté completa, haga clic en **Siguiente**.  
  
20. Use la página Configuración de [!INCLUDE[ssDE](../../../includes/ssde-md.md)] - Directorios de datos para especificar los directorios de instalación no predeterminados. Para instalar en los directorios predeterminados, haga clic en **Siguiente**.  
  
    > [!IMPORTANT]  
    >  Si especifica directorios de instalación no predeterminados, asegúrese de que las carpetas de instalación sean únicas para esta instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Ninguno de los directorios de este cuadro de diálogo se debe compartir con los de otras instancias de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Los directorios de datos se deben encontrar en el disco de clúster compartido para la instancia de clúster de conmutación por error.  
  
    > [!NOTE]  
    >  Para especificar un servidor de archivos de Bloque de mensajes del servidor (SMB) como directorio de datos, establezca **directorio raíz predeterminado de datos** en el recurso compartido de archivos con el formato \\\Servername\ShareName\\...  
   
21. Use la página Configuración de [!INCLUDE[ssDE](../../../includes/ssde-md.md)] - FILESTREAM para habilitar FILESTREAM para la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Haga clic en **Siguiente** para continuar.  
  
22. Use la página Configuración de [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] - Aprovisionamiento de cuentas para especificar los usuarios o las cuentas que tendrán permisos de administrador para [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]. Debe especificar al menos un administrador del sistema para [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]. Para agregar la cuenta en la que se ejecuta el programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , haga clic en **Agregar usuario actual**. Para agregar o quitar cuentas de la lista de administradores del sistema, haga clic en **Agregar** o **Quitar** y, a continuación, modifique la lista de usuarios, grupos o equipos que tendrán privilegios de administrador para [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].
  
     Cuando haya terminado de modificar la lista, haga clic en [!INCLUDE[clickOK](../../../includes/clickok-md.md)]. Compruebe la lista de administradores en el cuadro de diálogo de configuración. Cuando la lista esté completa, haga clic en **Siguiente**.  
  
23. Use la página Configuración de [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] - Directorios de datos para especificar los directorios de instalación no predeterminados. Para instalar en los directorios predeterminados, haga clic en **Siguiente**.  
  
    > [!IMPORTANT]  
    >  Si especifica directorios de instalación no predeterminados, asegúrese de que las carpetas de instalación sean únicas para esta instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Ninguno de los directorios de este cuadro de diálogo se debe compartir con los de otras instancias de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Los directorios de datos se deben encontrar en el disco de clúster compartido para la instancia de clúster de conmutación por error.  
   
24. Use la página Configuración de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] para especificar el tipo de instalación de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] que se creará. Para instalaciones de instancias de clúster de conmutación por error, la opción se establece en Instalación de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] sin configurar. Debe configurar los servicios de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] una vez completada la instalación.  
  
 
25. El Comprobador de configuración del sistema ejecuta uno o varios conjuntos de reglas para validar la configuración con las características de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que se han especificado.  
  
26. La página Listo para instalar muestra una vista de árbol de las opciones de instalación que se especificaron durante la instalación. Para continuar, haga clic en **Instalar**. El programa de instalación instalará primero los requisitos previos necesarios para las características seleccionadas y a continuación realizará la instalación de características.  
  
27. La página Progreso de la instalación muestra el estado para que pueda supervisar el progreso de la instalación durante la ejecución del programa de instalación.  
  
28. Después de la instalación, en la página **Operación completada** se proporciona un vínculo al archivo de registro de resumen de la instalación y a otras notas importantes. Para completar el proceso de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , haga clic en **Cerrar**.  
  
29. Si el programa indica que se reinicie el equipo, hágalo ahora. Es importante leer el mensaje del Asistente para la instalación tras finalizar el programa de instalación. Para obtener más información sobre los archivos de registro de instalación, vea [Ver y leer los archivos de registro de instalación de SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  
  
30. Para agregar nodos a la conmutación por error de nodo único que acaba de crear, ejecute el programa de instalación en cada nodo adicional y siga los pasos para la operación AddNode. Para más información, vea [Agregar o quitar nodos en un clúster de conmutación por error de SQL Server (programa de instalación)](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md).  
  
    > [!NOTE]  
    >  Si va a agregar más de un nodo, puede utilizar el archivo de configuración para implementar las instalaciones. Para obtener más información, vea [Instalar SQL Server 2016 mediante un archivo de configuración](../../../database-engine/install-windows/install-sql-server-using-a-configuration-file.md).  
    >   
    >  La edición de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que va a instalar debe coincidir en todos los nodos de una instancia de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Si agrega un nuevo nodo a una instancia de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] existente, asegúrese de especificar que la edición coincida con la edición de la instancia de clúster de conmutación por error existente.  
  
##  <a name="prepare"></a><a name="prepare"></a> Preparar  
  
#### <a name="advancedenterprise-failover-cluster-instance-install-step-1-prepare"></a>Paso 1 de la instalación de instancias de clúster de conmutación por error de Advanced/Enterprise: Preparación  
  
1.  Inserte el disco de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y, en la carpeta raíz, haga doble clic en Setup.exe. Para realizar la instalación desde un recurso compartido de red, desplácese a la carpeta raíz de dicho recurso y, a continuación, haga doble clic en Setup.exe. Para obtener más información acerca de cómo instalar los requisitos previos, vea [Before Installing Failover Clustering](../../../sql-server/failover-clusters/install/before-installing-failover-clustering.md). Puede que se le solicite que instale los requisitos previos si no se han instalado previamente.  
  
2.  Se requiere Windows Installer 4.5, que puede instalarse con el Asistente para instalación. Si se le solicita que reinicie el equipo, hágalo y, a continuación, vuelva a iniciar el programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
3.  Una vez instalados los requisitos previos, el Asistente para instalación inicia el Centro de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Para preparar el nodo para la agrupación en clústeres, vaya a la página **Opciones avanzadas** y, a continuación, haga clic en **Preparación de clúster para avanzada**.  
  
4.  El Comprobador de configuración del sistema ejecuta una operación de detección en el equipo. Para continuar, [!INCLUDE[clickOK](../../../includes/clickok-md.md)]. Puede ver los detalles en la pantalla haciendo clic en **Mostrar detalles** o, como un informe HTML, haciendo clic en **Ver informe detallado**.  
  
5.  En la página Archivos auxiliares del programa de instalación, haga clic en **Instalar** para instalar los archivos auxiliares del programa de instalación.  
  
6.  El Comprobador de configuración del sistema comprueba el estado del sistema del equipo antes de seguir con la instalación. Cuando se haya completado la comprobación, haga clic en **Siguiente** para continuar. Puede ver los detalles en la pantalla haciendo clic en **Mostrar detalles** o, como un informe HTML, haciendo clic en **Ver informe detallado**.  
  
7.  En la página Selección de idioma, puede especificar el idioma para su instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] si va a realizar la instalación en un sistema operativo localizado y el medio de instalación incluye paquetes de idioma en inglés y en el idioma correspondiente al sistema operativo. Para obtener más información sobre la compatibilidad entre idiomas y las consideraciones sobre la instalación, vea [Versiones de idioma local en SQL Server](../../../sql-server/install/local-language-versions-in-sql-server.md).  
  
     Para continuar, haga clic en **Siguiente**.  
  
8.  En la página Clave del producto, haga clic para indicar si va a instalar una edición gratuita de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]o si tiene una clave de PID para una versión de producción del producto. Para obtener más información, vea [Ediciones y componentes de SQL Server 2016](../../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
    > [!NOTE]  
    >  Debe especificar la misma clave del producto en todos los nodos que va a preparar para la misma instancia de clúster de conmutación por error.  
  
9. En la página Términos de licencia, lea el contrato de licencia y active la casilla para aceptar los términos y condiciones de la licencia. Para ayudar a mejorar [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], también puede habilitar la opción de uso de características y enviar informes a [!INCLUDE[msCoName](../../../includes/msconame-md.md)]. Haga clic en **Siguiente** para continuar. Para salir del programa de instalación, haga clic en **Cancelar**.  
  
10. En la página Selección de características, seleccione los componentes de la instalación. Después de seleccionar el nombre de la característica se muestra una descripción de cada grupo de componentes en el panel derecho. Puede activar cualquier combinación de casillas, pero solo [!INCLUDE[ssDE](../../../includes/ssde-md.md)], [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] en modo tabular y [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] en modo multidimensional admiten los clústeres de conmutación por error. Los demás componentes seleccionados se ejecutarán como una característica independiente sin la funcionalidad de conmutación por error en el nodo actual en el que esté ejecutando el programa de instalación. Para obtener más información sobre modos de [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] , vea [Determinar el modo de servidor de una instancia de Analysis Services](/analysis-services/instances/determine-the-server-mode-of-an-analysis-services-instance).  
  
     Los requisitos previos para las características seleccionadas se muestran en el recuadro del lado derecho. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] El programa de instalación instalará los requisitos previos que no se hayan instalado todavía durante el paso de instalación que se describe más adelante en este procedimiento.  
  
     Si desea especificar un directorio personalizado para los componentes compartidos, use el campo situado en la parte inferior de esta página. Para cambiar la ruta de instalación de los componentes compartidos, actualícela en el campo que se proporciona en la parte inferior del cuadro de diálogo o haga clic en el botón de puntos suspensivos para desplazarse a un directorio de instalación. La ruta de instalación predeterminada es C:\Archivos de programa\\[!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]\\.  
  
    > [!NOTE]  
    >  Si selecciona la característica Servicios de [!INCLUDE[ssDE](../../../includes/ssde-md.md)] , se seleccionan automáticamente la replicación y la búsqueda de texto completo. Si anula la selección de cualquiera de estas subcaracterísticas, también se anula la selección de la característica Servicios de [!INCLUDE[ssDE](../../../includes/ssde-md.md)] .  
  
11. En la página Configuración de instancia, especifique si desea instalar una instancia predeterminada o una instancia con nombre.
  
     **Id. de instancia** : de manera predeterminada, el nombre de instancia se usa como identificador de la instancia. Se usa para identificar los directorios de instalación y las claves del Registro para la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Es así en las instancias predeterminadas y en las instancias con nombre. Con una instancia predeterminada, el nombre y el identificador serían MSSQLSERVER. Para utilizar un identificador de instancia no predeterminado, seleccione el cuadro de texto **Id. de instancia** y proporcione un valor.  
  
    > [!NOTE]  
    >  Las instancias independientes típicas de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], tanto si son predeterminadas como si son instancias con nombre, no usan un valor no predeterminado para el cuadro de texto **Id. de instancia** .  
  
    > [!IMPORTANT]  
    >  Use el mismo identificador de instancia para todos los nodos que se preparen para la instancia de clúster de conmutación por error.  
  
     **Directorio raíz de instancia**: de forma predeterminada, el directorio raíz de la instancia es C:\Archivos de programa\\[!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]\\. Para especificar un directorio raíz no predeterminado, utilice el campo proporcionado o haga clic en el botón de puntos suspensivos para buscar una carpeta de instalación.  
  
     **Instancias instaladas:** la cuadrícula muestra las instancias de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que están en el equipo en el que se ejecuta el programa de instalación. Si ya hay una instancia predeterminada instalada en el equipo, debe instalar una instancia con nombre de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Haga clic en **Siguiente** para continuar.  
  
12. La página Requisitos de espacio en disco calcula el espacio en disco necesario para las características especificadas y compara los requisitos con el espacio en disco disponible en el equipo donde se ejecuta el programa de instalación.  
  
13. Utilice esta página para especificar la Directiva de seguridad de clúster.  
  
    -   [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] y versiones posteriores: los SID (identificadores de seguridad de servidor) de servicio son la configuración predeterminada y recomendada. No hay ninguna opción para cambiarlo para grupos de seguridad. Para obtener información sobre la funcionalidad de SID de servicio en [!INCLUDE[nextref_longhorn](../../../includes/nextref-longhorn-md.md)], vea [Configurar los permisos y las cuentas de servicio de Windows](../../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md). Se ha probado en configuraciones independientes y de clúster de [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)].  
  
    -   En [!INCLUDE[winxpsvr](../../../includes/winxpsvr-md.md)], especifique grupos de dominio para los servicios de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Todos los permisos de recursos se controlan mediante grupos de nivel de dominio que incluyen cuentas de servicio de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] como miembros de grupo.  
  
     Haga clic en **Siguiente** para continuar.  
  
14. El flujo de trabajo para el resto del tema depende de las características que haya especificado para la instalación. Dependiendo de las selecciones, es posible que no vea todas las páginas.  
  
15. En la página Configuración del servidor - Cuentas de servicio, especifique las cuentas de inicio de sesión para los servicios de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Los servicios reales que se configuran en esta página dependen de las características que se van a instalar.  
  
     Puede asignar la misma cuenta de inicio de sesión a todos los servicios de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , o configurar cada cuenta de servicio individualmente. El tipo de inicio se establece en manual para todos los servicios que reconocen clústeres, incluidos la búsqueda de texto completo y el Agente [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], y no se puede cambiar durante la instalación. [!INCLUDE[msCoName](../../../includes/msconame-md.md)] recomienda configurar las cuentas de servicio de forma individual para proporcionar a cada servicio los privilegios mínimos; así, los servicios de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] obtendrán los permisos mínimos que necesitan para completar sus tareas. Para obtener más información, consulte [Configurar los permisos y las cuentas de servicio de Windows](../../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
     Para especificar la misma cuenta de inicio de sesión para todas las cuentas de servicio en esta instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], las credenciales se proporcionan en los campos de la parte inferior de la página.  
  
     **Nota de seguridad** [!INCLUDE[ssNoteStrongPass](../../../includes/ssnotestrongpass-md.md)]  
  
     Cuando termine de especificar la información de inicio de sesión para los servicios de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , haga clic en **Siguiente**.  
  
16. Use la pestaña **Configuración del servidor - Intercalación** para especificar intercalaciones no predeterminadas para [!INCLUDE[ssDE](../../../includes/ssde-md.md)] y [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].  
  
17. Use **Configuración del servidor - Secuencia de archivo** para habilitar FILESTREAM para la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  Haga clic en **Siguiente** para continuar.  
  
18. Use la página Configuración de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] para especificar el tipo de instalación de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] que se creará. Para instalaciones de instancias de clúster de conmutación por error, la opción se establece en Instalación de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] sin configurar. Debe configurar los servicios de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] una vez completada la instalación.  
   
19. En la página Informes de errores, especifique la información que desea enviar a [!INCLUDE[msCoName](../../../includes/msconame-md.md)] y que ayudará a mejorar [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. De forma predeterminada, se habilitan las opciones de informes de errores.  
  
20. El Comprobador de configuración del sistema ejecuta uno o varios conjuntos de reglas para validar la configuración con las características de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que se han especificado.  
  
21. La página Listo para instalar muestra una vista de árbol de las opciones de instalación que se especificaron durante la instalación. Para continuar, haga clic en **Instalar**. El programa de instalación instalará primero los requisitos previos necesarios para las características seleccionadas y a continuación realizará la instalación de características.  
  
     La página Progreso de la instalación muestra el estado para que pueda supervisar el progreso de la instalación durante la ejecución del programa de instalación. Después de la instalación, en la página **Operación completada** se proporciona un vínculo al archivo de registro de resumen de la instalación y a otras notas importantes.  
  
22. Para completar el proceso de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , haga clic en **Cerrar**.  
  
23. Si el programa indica que se reinicie el equipo, hágalo ahora. Es importante leer el mensaje del Asistente para la instalación tras finalizar el programa de instalación. Para obtener más información sobre los archivos de registro de instalación, vea [Ver y leer los archivos de registro de instalación de SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  
  
24. Repita los pasos anteriores para preparar los demás nodos para la instancia de clúster de conmutación por error. También puede utilizar el archivo de configuración autogenerado para ejecutar la preparación en los demás nodos. Para obtener más información, vea [Instalar SQL Server 2016 mediante un archivo de configuración](../../../database-engine/install-windows/install-sql-server-using-a-configuration-file.md).  
  
## <a name="complete"></a>Operación completada  
  
#### <a name="advancedenterprise-failover-cluster-instance-install-step-2-complete"></a>Paso 2 de la instalación de instancias de clúster de conmutación por error de Advanced/Enterprise: Operación completada  
  
1.  Después de preparar todos los nodos como se describe en el [paso de preparación](#prepare), ejecute el programa de instalación en uno de los nodos preparados, preferiblemente el que posee el disco compartido. En la página **Opciones avanzadas** del Centro de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , haga clic en **Finalización avanzada de clúster**.  
  
2.  El Comprobador de configuración del sistema ejecuta una operación de detección en el equipo. Para continuar, [!INCLUDE[clickOK](../../../includes/clickok-md.md)]. Puede ver los detalles en la pantalla haciendo clic en **Mostrar detalles** o, como un informe HTML, haciendo clic en **Ver informe detallado**.  
  
3.  En la página Archivos auxiliares del programa de instalación, haga clic en **Instalar** para instalar los archivos auxiliares del programa de instalación.  
  
4.  El Comprobador de configuración del sistema comprueba el estado del sistema del equipo antes de seguir con la instalación. Cuando se haya completado la comprobación, haga clic en **Siguiente** para continuar. Puede ver los detalles en la pantalla haciendo clic en **Mostrar detalles** o, como un informe HTML, haciendo clic en **Ver informe detallado**.  
  
5.  En la página Selección de idioma, puede especificar el idioma para su instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] si va a realizar la instalación en un sistema operativo localizado y el medio de instalación incluye paquetes de idioma en inglés y en el idioma correspondiente al sistema operativo. Para obtener más información sobre la compatibilidad entre idiomas y las consideraciones sobre la instalación, vea [Versiones de idioma local en SQL Server](../../../sql-server/install/local-language-versions-in-sql-server.md).  
  
     Para continuar, haga clic en **Siguiente**.  
  
6.  Utilice la página Configuración de nodo de clúster para seleccionar el nombre de instancia preparado para la agrupación en clústeres y especifique el nombre de red para la nueva instancia de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Este es el nombre que se usa para identificar la instancia de clúster de conmutación por error en la red.  
  
    > [!NOTE]  
    >  Se conoce como el nombre de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] virtual en versiones anteriores de instancias de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
7.  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ejecuta uno o varios conjuntos de reglas que se basan en las características que seleccionó para validar la configuración.  
  
8.  Use la página Grupo de recursos de clúster para especificar el nombre del grupo de recursos de clúster donde se ubicarán los recursos de servidor virtual de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Para especificar el nombre del grupo de recursos de clúster de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , Tiene dos opciones:  
  
    -   Usar la lista para especificar un grupo existente.  
  
    -   Escriba el nombre del grupo que desea crear. Tenga en cuenta que el nombre "Available storage" no es un nombre de grupo válido.  
  
9. En la página Selección de disco de clúster, seleccione el recurso de disco de clúster compartido para la instancia de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. El disco de clúster es donde se colocarán los datos de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Se puede especificar más de un disco. La cuadrícula Discos compartidos disponibles muestra la lista de discos disponibles, si están calificados todos ellos como discos compartidos, y una descripción de cada recurso de disco. Haga clic en **Siguiente** para continuar.  
  
    > [!NOTE]  
    >  La primera unidad se utiliza como unidad predeterminada para todas las bases de datos, pero se puede cambiar en las páginas de configuración del [!INCLUDE[ssDE](../../../includes/ssde-md.md)] o de [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] .  
  
10. En la página Configuración de red en clúster, especifique los recursos de red para la instancia de clústeres de conmutación por error:  
  
    -   **Configuración de red**: especifique el tipo de IP y la dirección IP para todos los nodos y subredes de la instancia de clústeres de conmutación por error. Puede especificar varias direcciones IP para una instancia de clúster de conmutación por error de varias subredes, pero solo se admite una dirección IP por subred. Cada nodo preparado debe ser propietario de al menos una dirección IP. Si tiene varias subredes en su instancia de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], se le solicitará que establezca la dependencia de recursos de dirección IP en OR.  
  
     Haga clic en **Siguiente** para continuar.  
  
11. El flujo de trabajo para el resto del tema depende de las características que haya especificado para la instalación. Dependiendo de las selecciones, es posible que no vea todas las páginas.  
  
12. Use la página Configuración de [!INCLUDE[ssDE](../../../includes/ssde-md.md)] - Aprovisionamiento de cuentas para especificar lo siguiente:  
  
    -   Modo de Seguridad: seleccione la autenticación de Windows o la autenticación de modo mixto para la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Si selecciona la autenticación de modo mixto, debe proporcionar una contraseña segura para la cuenta de administrador del sistema de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] integrada.  
  
         Una vez que un dispositivo establece una conexión correcta con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], el mecanismo de seguridad es el mismo para la autenticación de Windows y para el modo mixto.  
  
    -   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Administradores: debe especificar al menos un administrador del sistema para la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Para agregar la cuenta en la que se ejecuta el programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , haga clic en **Agregar usuario actual**. Para agregar o quitar cuentas de la lista de administradores del sistema, haga clic en **Agregar** o en **Quitar** y, a continuación, modifique la lista de usuarios, grupos o equipos que tendrán privilegios de administrador para la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
     Cuando haya terminado de modificar la lista, haga clic en [!INCLUDE[clickOK](../../../includes/clickok-md.md)]. Compruebe la lista de administradores en el cuadro de diálogo de configuración. Cuando la lista esté completa, haga clic en **Siguiente**.  
  
13. Use la página Configuración de [!INCLUDE[ssDE](../../../includes/ssde-md.md)] - Directorios de datos para especificar los directorios de instalación no predeterminados. Para instalar en los directorios predeterminados, haga clic en **Siguiente**.  
  
    > [!IMPORTANT]  
    >  Si especifica directorios de instalación no predeterminados, asegúrese de que las carpetas de instalación sean únicas para esta instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Ninguno de los directorios de este cuadro de diálogo se debe compartir con los de otras instancias de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Los directorios de datos se deben encontrar en el disco de clúster compartido para la instancia de clúster de conmutación por error.  
  
  
14. Use la página Configuración de [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] - Aprovisionamiento de cuentas para especificar los usuarios o las cuentas que tendrán permisos de administrador para [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]. Debe especificar al menos un administrador del sistema para [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]. Para agregar la cuenta en la que se ejecuta el programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , haga clic en **Agregar usuario actual**. Para agregar o quitar cuentas de la lista de administradores del sistema, haga clic en **Agregar** o **Quitar** y, a continuación, modifique la lista de usuarios, grupos o equipos que tendrán privilegios de administrador para [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].  
  
     Cuando haya terminado de modificar la lista, haga clic en [!INCLUDE[clickOK](../../../includes/clickok-md.md)]. Compruebe la lista de administradores en el cuadro de diálogo de configuración. Cuando la lista esté completa, haga clic en **Siguiente**.  
  
15. Use la página Configuración de [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] - Directorios de datos para especificar los directorios de instalación no predeterminados. Para instalar en los directorios predeterminados, haga clic en **Siguiente**.  
  
    > [!IMPORTANT]  
    >  Si especifica directorios de instalación no predeterminados, asegúrese de que las carpetas de instalación sean únicas para esta instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Ninguno de los directorios de este cuadro de diálogo se debe compartir con los de otras instancias de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Los directorios de datos se deben encontrar en el disco de clúster compartido para los clústeres de conmutación por error.  
  
  
16. El Comprobador de configuración del sistema ejecuta uno o varios conjuntos de reglas para validar la configuración con las características de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que se han especificado.  
  
17. La página Listo para instalar muestra una vista de árbol de las opciones de instalación que se especificaron durante la instalación. Para continuar, haga clic en **Instalar**. El programa de instalación instalará primero los requisitos previos necesarios para las características seleccionadas y a continuación realizará la instalación de características.  
  
18. La página Progreso de la instalación muestra el estado para que pueda supervisar el progreso de la instalación durante la ejecución del programa de instalación.  
  
19. Después de la instalación, en la página **Operación completada** se proporciona un vínculo al archivo de registro de resumen de la instalación y a otras notas importantes. Para completar el proceso de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , haga clic en **Cerrar**. Con este paso, todos los nodos preparados para la misma instancia de clúster de conmutación por error forman parte de la instancia de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] completada.  
  
## <a name="next-steps"></a>Pasos siguientes  
 **Configurar la nueva instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]** : para reducir el área expuesta del sistema susceptible de recibir ataques, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] instala y habilita los servicios y características clave de forma selectiva. Para obtener más información, vea [Surface Area Configuration](../../../relational-databases/security/surface-area-configuration.md).  
  
 Para obtener más información sobre las ubicaciones de los archivos de registro, vea [Ver y leer los archivos de registro de instalación de SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  
  
## <a name="see-also"></a>Consulte también  
 [Instalar SQL Server 2016 desde el símbolo del sistema](../../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)  
  
