---
title: Actualización de una instancia del clúster de conmutación por error
description: Pasos para actualizar una instancia de clúster de conmutación por error Always On de SQL Server mediante un soporte de instalación. Obtenga información sobre las actualizaciones graduales y la actualización de un clúster de varias subredes.
ms.custom: seo-lt-2019
ms.date: 11/21/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: conceptual
helpviewer_keywords:
- upgrading failover cluster instances
- clusters [SQL Server], upgrading
- failover clustering [SQL Server], upgrading
ms.assetid: daac41fe-7d0b-4f14-84c2-62952ad8cbfa
author: cawrites
ms.author: chadam
ms.openlocfilehash: 358006fa8eb00cdfc52a6d5a630f559ff86553f7
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100350028"
---
# <a name="upgrade-a-failover-cluster-instance"></a>Actualización de una instancia del clúster de conmutación por error 
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] admite la actualización de un clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] a una nueva versión de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], a una nueva actualización acumulativa o un nuevo Service Pack de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] o, al instalar, a una nueva actualización acumulativa o un nuevo Service Pack de Windows por separado en todos los nodos del clúster de conmutación por error, con tiempo de inactividad limitado a una sola conmutación por error manual (o dos conmutaciones por error manuales si conmuta por recuperación a la base de datos primaria original).  

  
 La actualización del sistema operativo Windows Server de un nodo que contiene una instancia de clúster de conmutación por error no se admite para los sistemas operativos anteriores a [!INCLUDE[winblue-server-2-md](../../../includes/winblue-server-2-md.md)]. Para actualizar un nodo de clúster de conmutación por error de Windows Server en [!INCLUDE[winblue-server-2-md](../../../includes/winblue-server-2-md.md)] o superior, vea [Realizar una actualización gradual](#perform-a-rolling-upgrade-or-update).  
  
 Los detalles de compatibilidad son los siguientes:  
  
-   La actualización de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]se puede realizar tanto a través de la interfaz de usuario como desde el símbolo del sistema. Puede ejecutar la actualización desde el símbolo del sistema en cada nodo de clúster de conmutación por error, o usando la IU del programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] para actualizar cada nodo de clúster. Para obtener más información, consulte:

   - Instalación de una nueva instancia de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]
   - [Instalar SQL Server desde el símbolo del sistema](../../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)

-   Los escenarios siguientes no se admiten como parte de una actualización de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] :  
  
    -   No puede actualizar desde una instancia independiente de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] a una instancia de clúster de conmutación por error.  
  
    -   No puede agregar características a una instancia de clúster de conmutación por error. Por ejemplo, no se puede agregar el [!INCLUDE[ssDE](../../../includes/ssde-md.md)] a una instancia de clúster de conmutación por error de solo [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] existente.  
  
    -   No se puede degradar una instancia de clúster de conmutación por error a una instancia independiente en ningún nodo del clúster de conmutación por error de Windows Server.  
  
    -   El cambio de edición de la instancia de clúster de conmutación por error está limitado a determinados escenarios. Para obtener información detallada, vea [Actualizaciones de ediciones y versiones admitidas](../../../database-engine/install-windows/supported-version-and-edition-upgrades.md).  
  
-   Durante el proceso de actualización de instancia de clúster de conmutación por error, el tiempo de inactividad se limita al tiempo de conmutación por error y al tiempo necesario para ejecutar los scripts de actualización. Si sigue el proceso de actualización gradual de instancia de clúster de conmutación por error que se muestra a continuación y cumple todos los requisitos previos en todos los nodos antes de comenzar dicho proceso, el tiempo de inactividad es mínimo. Se necesitará un poco más de tiempo a la hora de actualizar [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] si se usan tablas optimizadas para memoria. Para obtener más información, consulte [Planeación y prueba del plan de actualización del motor de base de datos](../../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md).  
  
## <a name="prerequisites"></a>Requisitos previos  
 Antes de empezar, revise la siguiente información importante:  
  
-   [Actualizaciones de ediciones y versiones admitidas](../../../database-engine/install-windows/supported-version-and-edition-upgrades.md): compruebe que puede realizar la actualización a la versión que quiere de [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] desde su versión del sistema operativo Windows y su versión de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Por ejemplo, no puede actualizar directamente desde una instancia de clúster de conmutación por error de SQL Server 2005 a [!INCLUDE [sssql14-md](../../../includes/sssql14-md.md)] ni actualizar una instancia de clúster de conmutación por error que se ejecuta en [!INCLUDE[winxpsvr-md](../../../includes/winxpsvr-md.md)].  
  
-   [Elegir un método de actualización del motor de base de datos](../../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md): seleccione el método y los pasos de actualización adecuados en función de la revisión de versiones admitidas y actualizaciones de ediciones, y también teniendo en cuenta otros componentes instalados en el entorno con el fin de actualizar los componentes en el orden correcto.  
  
-   [Planeación y prueba del plan de actualización del motor de base de datos](../../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md): revise las notas de la versión y los problemas conocidos de actualización, así como la lista de comprobación previa a la actualización, y desarrolle y pruebe el plan de actualización.  
  
-   [Requisitos de hardware y software para instalar SQL Server](../../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md):  revise los requisitos de software para instalar [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]. Si se requiere software adicional, puede instalarlo en cada nodo antes de comenzar el proceso de actualización para reducir los posibles tiempos de inactividad.  
  
## <a name="perform-a-rolling-upgrade-or-update"></a>Realizar una actualización gradual  
 Para actualizar una instancia de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], use el programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] para actualizar cada nodo que participa en la instancia de clúster de conmutación por error, uno por uno, comenzando por los nodos pasivos. A medida que se actualiza cada nodo, se excluye de los posibles propietarios de la instancia de clúster de conmutación por error. Si se produce una conmutación por error inesperada, los nodos actualizados no participan en ella hasta que el programa de instalación mueve la propiedad de rol del clúster de conmutación por error de Windows Server.  
  
 De forma predeterminada, el programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] determina automáticamente cuándo debe realizarse la conmutación por error a un nodo actualizado. Para ello se basa en el número total de nodos de la instancia de los clústeres de conmutación por error y en el número de nodos que ya se han actualizado. Cuando se ha actualizado la mitad de los nodos o más, el programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] realiza una conmutación por error a un nodo actualizado en el momento en que se realiza la actualización en el siguiente nodo. Tras la conmutación por error a un nodo actualizado, el grupo de clústeres se mueve a un nodo actualizado. Todos los nodos actualizados se colocan en la lista de propietarios posibles y todos los nodos que aún no se han actualizado se quitan de la lista. A medida que se actualiza cada uno de los nodos restantes, se agrega a los posibles propietarios de la instancia de clúster de conmutación por error.  
  
 Este proceso da lugar a un tiempo de inactividad limitado a un tiempo de conmutación por error y un tiempo de ejecución de script de actualización de bases de datos durante la actualización completa de los clústeres de conmutación por error.  
  
 Para controlar el comportamiento de la conmutación por error de los nodos en clúster durante el proceso de actualización, ejecute la operación de actualización en el símbolo del sistema y use el parámetro /FAILOVERCLUSTERROLLOWNERSHIP. Para más información, consulte [Instalar SQL Server 2016 desde el símbolo del sistema](../../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md).  

 ## <a name="upgrade-with-installation-media"></a>Actualización con medios de instalación 
  
1.  Desde el medio de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] para la edición que coincide con la edición que actualiza, haga doble clic en setup.exe en la carpeta raíz. Puede que se le solicite que instale los requisitos previos si no se han instalado previamente.  
  
2.  Una vez instalados los requisitos previos, el Asistente para instalación inicia el Centro de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Para actualizar una instancia existente de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], seleccione la instancia.  
  
3.  Si son necesarios los archivos auxiliares del programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , el citado programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se encarga de instalarlos. Si se le solicita que reinicie el equipo, hágalo antes de continuar.  
  
4.  El Comprobador de configuración del sistema ejecuta una operación de detección en el equipo. Para continuar, [!INCLUDE[clickOK](../../../includes/clickok-md.md)].  
  
5.  En la página Clave del producto, escriba la clave de PID de la nueva edición de la versión, que debe ser la misma edición que la de la versión anterior del producto. Por ejemplo, para actualizar un clústeres de conmutación por error de Enterprise, debe especificar la clave de PID de [!INCLUDE[ssEnterprise](../../../includes/ssenterprise-md.md)]. Haga clic en **Siguiente** para continuar. Tenga en cuenta que la clave de PID que use para actualizar los clústeres de conmutación por error debe ser coherente en todos los nodos de clúster de conmutación por error de la misma instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
6.  En la página Términos de licencia, lea el contrato de licencia y active la casilla para aceptar los términos y condiciones de la licencia. Para ayudar a mejorar [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], también puede habilitar la opción de uso de características y enviar informes a [!INCLUDE[msCoName](../../../includes/msconame-md.md)]. Para continuar, haga clic en **Siguiente**. Para salir del programa de instalación, haga clic en **Cancelar**.  
  
7.  En la página Seleccionar instancia, especifique la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que quiere actualizar. Para continuar, haga clic en **Siguiente**.  
  
8.  En la página Selección de características aparecen seleccionadas las características que van a actualizarse. Después de seleccionar el nombre de la característica se muestra una descripción de cada grupo de componentes en el panel derecho. Tenga en cuenta que no puede cambiar las características que se van a actualizar, y no puede agregar características durante la operación de actualización. Para agregar características a una instancia actualizada de [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] una vez completada la operación de actualización, vea [Agregar características a una instancia de SQL Server 2016 &#40;programa de instalación&#41;](../../../database-engine/install-windows/add-features-to-an-instance-of-sql-server-setup.md).  
  
     Los requisitos previos para las características seleccionadas se muestran en el recuadro del lado derecho. La instalación de SQL Server instalará los requisitos previos que no se hayan instalado todavía durante el paso de instalación que se describe más adelante en este procedimiento. Para ahorrar tiempo, debe instalar previamente estos requisitos previos en cada nodo.  
  
9. En la página Configuración de instancia, los campos se rellenan automáticamente con los valores de la instancia anterior. Si lo desea, puede especificar el nuevo valor de InstanceID.  
  
     **Id. de instancia** : de manera predeterminada, el nombre de instancia se usa como identificador de la instancia. Se usa para identificar los directorios de instalación y las claves del Registro para la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Es así en las instancias predeterminadas y en las instancias con nombre. Con una instancia predeterminada, el nombre y el identificador serían MSSQLSERVER. Para utilizar un identificador de instancia no predeterminado, active la casilla **Id. de instancia** y proporcione un valor. Si invalida el valor predeterminado, deberá especificar el mismo identificador de instancia para la instancia que se está actualizando en todos los nodos de clúster de conmutación por error. El identificador de la instancia actualizada debe coincidir en todos los nodos.  
  
     **Características e instancias detectadas:** la cuadrícula muestra las instancias de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que hay en el equipo en el que se ejecuta el programa de instalación. Para continuar, haga clic en **Siguiente**.  
  
10. La página Requisitos de espacio en disco calcula el espacio en disco necesario para las características especificadas y compara los requisitos con el espacio en disco disponible en el equipo donde se ejecuta el programa de instalación.  
  
11. En la página Actualización de la búsqueda de texto completo, especifique las opciones de actualización para las bases de datos que van a actualizarse. Para obtener más información, vea [Opciones de actualización de búsqueda de texto completo](../../../database-engine/install-windows/install-sql-server.md).  
  
12. En la página **Informes de errores** , especifique la información que desee enviar a [!INCLUDE[msCoName](../../../includes/msconame-md.md)] y que ayudará a mejorar [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. De forma predeterminada, se habilitan las opciones de informes de errores.  
  
13. El Comprobador de configuración del sistema ejecuta uno o varios conjuntos de reglas para validar la configuración del equipo con las características de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que ha especificado antes de que comience la operación de actualización.  
  
14. La página Informe de actualización de clúster muestra la lista de nodos de la instancia de los clústeres de conmutación por error y la información de versión de la instancia para los componentes de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] de cada nodo. También muestra el estado del script de base de datos y el estado del script de replicación. Además, muestra mensajes informativos sobre lo que ocurrirá al hacer clic en **Siguiente**. En función del número de nodos de clúster de conmutación por error que se han actualizado y del número total de nodos, el programa de instalación muestra el comportamiento de conmutación por error que tiene lugar cuando se hace clic en **Siguiente**. También le advierte del posible tiempo de inactividad innecesario que puede producirse si aún no ha instalado los requisitos previos.   
  
15. La página Listo para actualizar muestra una vista de árbol de las opciones de instalación que se especificaron durante la instalación. Para continuar, haga clic en **Actualizar**. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] El programa de instalación instalará primero los requisitos previos necesarios para las características seleccionadas y a continuación realizará la instalación de características.  
  
16. Durante la actualización, la página Progreso muestra el estado para que pueda supervisar el progreso de la actualización en el nodo actual a medida que se ejecuta el programa de instalación.  
  
17. Tras la actualización del nodo actual, la página Informe de actualización de clúster muestra información sobre el estado de actualización de todos los nodos de clúster de conmutación por error, las características de cada uno de los nodos de clúster de conmutación por error y su información de versión. Confirme la información de versión que se muestra y prosiga con la actualización de los nodos restantes. En la página de estado también se indicará si se ha producido la conmutación por error a los nodos actualizados. También puede comprobarlo en la herramienta Administrador de clústeres de Windows de cara a una confirmación.  
  
18. Después de la actualización, la página Operación completada proporciona un vínculo al archivo de registro de resumen para la instalación y otras notas importantes. Para completar el proceso de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , haga clic en **Cerrar**.  
  
19. Si el programa indica que se reinicie el equipo, hágalo ahora. Es importante leer el mensaje del Asistente para la instalación tras finalizar el programa de instalación. Para obtener más información sobre los archivos de registro de instalación, vea [Ver y leer los archivos de registro de instalación de SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  
  
20. Para completar el proceso de actualización, repita estos pasos en todos los demás nodos de la instancia de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
## <a name="upgrade-a-multi-subnet-failover-cluster-instance"></a>Actualización de una instancia de clúster de conmutación por error de varias subredes  

Siga estos pasos para actualizar la instancia del clúster de conmutación por error Always On en un entorno de varias subredes. 
  
### <a name="to-upgrade-to-a-ssnoversion-multi-subnet-failover-cluster-instance-existing-ssnoversion-cluster-is-a-non-multi-subnet-cluster"></a>Para actualizar a una instancia de clúster de conmutación por error de varias subredes de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (el clúster de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] existente es un clúster que no es de varias subredes).  
  
1.  Siga los pasos anteriores para actualizar la instancia de clúster de conmutación por error.  
  
2.  Para agregar un nodo nuevo en otra subred utilizando la acción AddNode Setup y confirmar la dependencia de recurso de dirección IP a OR en la página **Configuración de red en clúster**. Para más información, vea [Agregar o quitar nodos en un clúster de conmutación por error de SQL Server (programa de instalación)](../install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md).  
  
### <a name="to-upgrade-a-multi-subnet-failover-cluster-instance-currently-using-stretch-vlan-to-use-multi-subnet"></a>Para actualizar una instancia de clúster de conmutación por error de varias subredes actualmente mediante Stretch VLAN para usar varias subredes.  
  
1.  Siga los pasos anteriores para actualizar el clúster.  
  
2.  Cambie la configuración de red para mover el nodo remoto a una subred diferente.  
  
3.  Con el administrador de clústeres de conmutación por error de Windows o PowerShell, agregue una nueva dirección IP para la nueva subred para establecer la dependencia de recurso de dirección IP en OR.  
  
## <a name="next-steps"></a>Pasos siguientes  
 Después de actualizar, lleve a cabo las siguientes tareas:  
  
-   [Completar la actualización motor de base de datos](../../../database-engine/install-windows/complete-the-database-engine-upgrade.md)  
  
-   [Cambiar el modo de compatibilidad de la base de datos y usar el almacén de consultas](../../../database-engine/install-windows/change-the-database-compatibility-mode-and-use-the-query-store.md)  
  
-   [Aprovechamiento de las nuevas características de SQL Server 2016](../../what-s-new-in-sql-server-2017.md)  
  

  
