---
description: Agente SQL Server
title: Agente SQL Server
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, about SQL Server Agent
- automatic administration steps
ms.assetid: 8d1dc600-aabb-416f-b3af-fbc9fccfd0ec
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 543491e19d1182d611508797bab2354d483302af
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344253"
---
# <a name="sql-server-agent"></a>Agente SQL Server

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es un servicio de Microsoft Windows que ejecuta tareas administrativas programadas, denominadas *trabajos* en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  

## <a name="benefits-of-sql-server-agent"></a><a name="Benefits"></a>Ventajas del Agente SQL Server 

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para almacenar información de los trabajos. Los trabajos contienen uno o más pasos. Cada paso contiene su propia tarea; por ejemplo, realizar una copia de seguridad de una base de datos.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede ejecutar un trabajo según una programación, como respuesta a un evento específico o a petición. Por ejemplo, si desea realizar una copia de seguridad de todos los servidores de la organización todos los días entre semana después del horario de trabajo, puede automatizar esta tarea. Programe la copia de seguridad para que se ejecute después de las 22:00 h de lunes a viernes; si la copia de seguridad encuentra un problema, el Agente SQL Server puede registrar el evento y notificárselo.  
  
> [!NOTE]  
> El servicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] está deshabilitado de manera predeterminada cuando se instala [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] , a menos que el usuario elija explícitamente iniciar automáticamente el servicio.  
  
## <a name="sql-server-agent-components"></a><a name="Components"></a>Componentes del Agente SQL Server  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] emplea los siguientes componentes para definir las tareas que se van a realizar, cuándo se van a llevar a cabo y cómo se va a informar de si se han realizado correctamente o no.  
  
### <a name="jobs"></a>Trabajos  
Un *trabajo* es una serie especificada de acciones que realiza el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Use trabajos para definir una tarea administrativa que se puede ejecutar una o varias veces y supervisar para detectar si lo hace correctamente o con errores. Un trabajo se puede ejecutar en un servidor local o en varios servidores remotos.  
  
> [!IMPORTANT]  
> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se están ejecutando en el momento de un evento de conmutación por error en una instancia de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no se reanudan después de la conmutación por error a otro nodo de clúster de conmutación por error. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Los trabajos del Agente que se están ejecutando en el momento que se pausa un nodo de Hyper-V no se reanudan si la pausa origina una conmutación por error a otro nodo. Los trabajos que empiezan pero no se finalizan como consecuencia de un evento de conmutación por error se registran como iniciados, pero no muestran entradas de registro adicionales para que indiquen finalización o error. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se muestran como nunca finalizados.  
  
Existen varias maneras de ejecutar trabajos:  
  
-   Conforme a una o más programaciones.  
  
-   Como respuesta a una o varias alertas.  
  
-   Ejecutando el procedimiento almacenado sp_start_job.  
  
Cada acción de un trabajo es un *paso de trabajo*. Por ejemplo, un paso de trabajo puede consistir en la ejecución de una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] , la ejecución de un paquete [!INCLUDE[ssIS](../../includes/ssis_md.md)] o la emisión de un comando en un servidor de Analysis Services. Los pasos de trabajo se administran como parte de un trabajo.  
  
Cada paso se ejecuta en un contexto de seguridad específico. En el caso de los pasos de trabajo que utilizan [!INCLUDE[tsql](../../includes/tsql-md.md)], use la instrucción EXECUTE AS para establecer el contexto de seguridad para éstos. Para los demás tipos de pasos de trabajo, utilice una cuenta de proxy para establecer el contexto de seguridad.  
  
### <a name="schedules"></a>Programaciones  
Una *programación* especifica cuándo se ejecuta un trabajo. Se puede ejecutar más de un trabajo en la misma programación y se puede aplicar más de una programación al mismo trabajo. Una programación puede definir las condiciones siguientes para el momento en que se ejecuta un trabajo:  
  
-   Cuando se inicia el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Cuando el uso de la CPU del equipo se encuentre en un nivel que se haya definido como inactivo.  
  
-   Una vez, a una hora y una fecha específicas.  
  
-   Según una programación periódica.  
  
Para obtener más información, vea [Crear y adjuntar programaciones a trabajos](../../ssms/agent/create-and-attach-schedules-to-jobs.md).  
  
### <a name="alerts"></a>Alertas  
Una *alerta* es una respuesta automática a un evento específico. Por ejemplo, un evento puede ser el inicio de un trabajo o que los recursos del sistema alcancen un umbral específico. Debe definir las condiciones en las que se genera una alerta.  
  
Una alerta puede responder a una de las condiciones siguientes:  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eventos  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] condiciones de rendimiento  
  
-   Eventos del Instrumental de administración de Windows (WMI) en el equipo en el que se ejecuta el Agente SQL Server  
  
Una alerta puede realizar las acciones siguientes:  
  
-   Notificar a uno o varios operadores  
  
-   Ejecución de un trabajo  
  
Para más información, consulte [Alertas](../../ssms/agent/alerts.md).  
  
### <a name="operators"></a>Operadores  
Un *operador* define información de contacto para las personas responsables del mantenimiento de una o varias instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En algunas compañías, las responsabilidades de operador están asignadas a una sola persona. En compañías con varios servidores, muchas personas comparten las responsabilidades de operador. Un operador no contiene información de seguridad y no define una entidad de seguridad.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede notificar a los operadores de alertas mediante una o varias de las opciones siguientes:  
  
-   Correo electrónico  
  
-   Buscapersonas (por correo electrónico)  
  
-   **net send**  
  
> [!NOTE]  
> Para enviar notificaciones mediante **net send,** se debe iniciar el servicio Windows Messenger en el equipo en el que reside el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!IMPORTANT]  
> Las opciones Buscapersonas y **net send** se quitarán del Agente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en una versión futura de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evite utilizar estas características en los nuevos trabajos de programación y planee modificar las aplicaciones que actualmente las utilizan.  
  
Para enviar a los operadores notificaciones por correo electrónico o buscapersonas, deberá configurar el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para usar Correo electrónico de base de datos. Para obtener más información, consulte [Correo electrónico de base de datos](../../relational-databases/database-mail/database-mail.md).  
  
Puede definir un operador como alias de un grupo de personas. De esta manera, todos los miembros de este alias pueden recibir notificaciones al mismo tiempo. Para obtener más información, vea [Operadores (Guía de programación de C#)](../../ssms/agent/operators.md).  
  
## <a name="security-for-sql-server-agent-administration"></a><a name="Security"></a>Seguridad en la administración del Agente SQL Server  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa los roles fijos de base de datos **SQLAgentUserRole**, **SQLAgentReaderRole** y **SQLAgentOperatorRole** en la base de datos **msdb** para controlar el acceso al Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para aquellos usuarios que no son miembros del rol fijo de servidor **sysadmin** . Además de estos roles fijos de base de datos, los subsistemas y los servidores proxy ayudan a los administradores de bases de datos a garantizar que cada paso de trabajo se ejecuta con los permisos mínimos necesarios para realizar la tarea.  
  
### <a name="roles"></a>Roles  
Los miembros de los roles fijos de base de datos **SQLAgentUserRole**, **SQLAgentReaderRole** y **SQLAgentOperatorRole** de **msdb** y los miembros del rol fijo de servidor **sysadmin** tienen acceso al Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Un usuario que no pertenezca a ninguno de estos roles no puede utilizar el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para más información sobre los roles que usa el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consulte [Implementar la seguridad del Agente SQL Server](../../ssms/agent/implement-sql-server-agent-security.md).  
  
### <a name="subsystems"></a>Subsistemas  
Un subsistema es un objeto predefinido que representa las funciones disponibles para un paso de trabajo. Cada proxy tiene acceso a uno o varios subsistemas. Los subsistemas proporcionan seguridad, ya que delimitan el acceso a las que funciones que están disponibles para el proxy. Cada paso de trabajo se ejecuta en el contexto de un proxy, con la excepción de los pasos de trabajo de [!INCLUDE[tsql](../../includes/tsql-md.md)] . El trabajo [!INCLUDE[tsql](../../includes/tsql-md.md)] usa el comando EXECUTE AS para establecer el contexto de seguridad como su propietario.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] define los subsistemas incluidos en la tabla siguiente:  
  
|Nombre del subsistema|Descripción|  
|--------------|-----------|  
|Scripts Microsoft ActiveX|Ejecuta un paso de trabajo de scripts ActiveX.<br /><br />**Advertencia** El subsistema de scripts ActiveX se quitará del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en una versión futura de [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan.|  
|Sistema operativo (**CmdExec**)|Ejecuta un programa ejecutable.|  
|PowerShell|Ejecuta un paso de trabajo de scripts de PowerShell.|  
|Distribuidor de replicación|Ejecuta un paso de trabajo que activa el Agente de distribución de replicación.|  
|Mezcla de replicación|Ejecuta un paso de trabajo que activa el Agente de mezcla.|  
|Lector de cola de replicación|Ejecuta un paso de trabajo que activa el Agente de lectura de cola de replicación.|  
|Instantánea de replicación|Ejecuta un paso de trabajo que activa el Agente de instantáneas.|  
|Registro del LOG de transacciones de replicación|Ejecuta un paso de trabajo que activa el Agente de registro del LOG.|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] Command|Ejecuta un comando de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] .|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] Query|Ejecuta una consulta de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] .|  
|[!INCLUDE[ssIS](../../includes/ssis_md.md)] ejecución de paquetes|Ejecuta un paquete de [!INCLUDE[ssIS](../../includes/ssis_md.md)] .|  
  
> [!NOTE]  
> Puesto que los pasos de trabajo [!INCLUDE[tsql](../../includes/tsql-md.md)] no utilizan proxy, no hay ningún subsistema del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para los pasos de trabajo [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aplica las restricciones del subsistema incluso si la entidad de seguridad del proxy tuviera permiso para ejecutar la tarea del paso de trabajo. Por ejemplo, un proxy para un usuario que es miembro del rol fijo de servidor sysadmin no puede ejecutar un paso de trabajo [!INCLUDE[ssIS](../../includes/ssis_md.md)] salvo que el proxy tenga acceso al subsistema de [!INCLUDE[ssIS](../../includes/ssis_md.md)] , aunque el usuario pueda ejecutar paquetes de [!INCLUDE[ssIS](../../includes/ssis_md.md)] .  
  
### <a name="proxies"></a>Servidores proxy  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa servidores proxy para administrar contextos de seguridad. Se puede utilizar un servidor proxy en más de un paso de trabajo. Los miembros del rol fijo de servidor **sysadmin** pueden crear servidores proxy.  
  
Cada proxy se corresponde con unas credenciales de seguridad. Cada proxy puede asociarse a un conjunto de subsistemas y un conjunto de inicios de sesión. El proxy solo se puede utilizar con pasos de trabajo que utilizan un subsistema asociado al proxy. Para crear un paso de trabajo que utilice un proxy determinado, el propietario del trabajo debe utilizar un inicio de sesión asociado al proxy o debe ser miembro de un rol con acceso ilimitado a los servidores proxy. Los miembros del rol fijo de servidor **sysadmin** tienen acceso ilimitado a los servidores proxy. Los miembros de **SQLAgentUserRole**, **SQLAgentReaderRole** o **SQLAgentOperatorRole** solo pueden utilizar servidores proxy para los que dispongan de acceso específico. Cada usuario que sea miembro de alguno de estos roles fijos de base de datos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe tener acceso a servidores proxy específicos para poder crear pasos de trabajo que usen esos proxy.  
  
## <a name="related-tasks"></a>Related Tasks  
Use los pasos siguientes para configurar el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de manera que se automatice la administración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
1.  Establezca las tareas administrativas o eventos del servidor que se realizan con regularidad y si estas tareas o eventos se pueden administrar mediante programación. Una tarea es una buena candidata a la automatización si consta de una secuencia de pasos predecible y se produce en un momento específico o en respuesta a un evento concreto.  
  
2.  Defina un conjunto de trabajos, programaciones, alertas y operadores mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] u objetos de administración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SMO). Para más información, consulte [Crear trabajos](../../ssms/agent/create-jobs.md).  
  
3.  Ejecute los trabajos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que haya definido.  
  
> [!NOTE]  
> Para la instancia predeterminada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se denomina SQLSERVERAGENT. Para las instancias con nombre, el servicio Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se denomina SQLAgent$*nombreDeInstancia*.  
  
Si ejecuta varias instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], use la administración multiservidor para automatizar las tareas comunes a todas las instancias. Para más información, consulte [Administración automatizada en una empresa](../../ssms/agent/automated-administration-across-an-enterprise.md).  
  
Use las siguientes tareas para comenzar a trabajar con el agente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
|Descripción|Tema|  
|-----------|-----|  
|Describe cómo configurar el Agente SQL Server.|[Configurar el Agente SQL Server](../../ssms/agent/configure-sql-server-agent.md)|  
|Describe cómo iniciar, detener y pausar el servicio del Agente SQL Server.|[Iniciar, detener o pausar el servicio del Agente SQL Server](../../ssms/agent/start-stop-or-pause-the-sql-server-agent-service.md)|  
|Describe las consideraciones para especificar una cuenta para el servicio del Agente SQL Server.|[Seleccionar una cuenta para el servicio Agente SQL Server](../../ssms/agent/select-an-account-for-the-sql-server-agent-service.md)|  
|Describe cómo usar el registro de errores del Agente SQL Server.|[Registro de errores del Agente SQL Server](../../ssms/agent/sql-server-agent-error-log.md)|  
|Describe cómo usar los objetos de rendimiento.|[Usar objetos de rendimiento](../../ssms/agent/use-performance-objects.md)|  
|Describe el Asistente para planes de mantenimiento, que es una utilidad que puede ayudarle a crear trabajos, alertas y operadores para automatizar la administración de una instancia de SQL Server.|[Usar el Asistente para planes de mantenimiento](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md)|  
|Describe cómo automatizar tareas administrativas mediante el Agente SQL Server.|[Tareas administrativas automatizadas &#40;Agente SQL Server&#41;](../../ssms/agent/automated-administration-tasks-sql-server-agent.md)|  
  
## <a name="see-also"></a>Consulte también  
[Configuración de Área expuesta](../../relational-databases/security/surface-area-configuration.md)  
