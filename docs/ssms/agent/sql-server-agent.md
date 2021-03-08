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
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/03/2021
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 751a627530b7d1017bfd8f0f89aa626ea84bc35c
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186546"
---
# <a name="sql-server-agent"></a>Agente SQL Server

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

El Agente SQL Server es un servicio de Microsoft Windows que ejecuta tareas administrativas programadas, denominadas *trabajos*, en SQL Server.

## <a name="benefits-of-sql-server-agent"></a><a name="Benefits"></a>Ventajas del Agente SQL Server

El Agente SQL Server usa SQL Server para almacenar información del trabajo. Los trabajos contienen uno o más pasos. Cada paso contiene su propia tarea; por ejemplo, realizar una copia de seguridad de una base de datos.

El Agente SQL Server puede ejecutar un trabajo según una programación como respuesta a un evento específico o a petición. Por ejemplo, si desea realizar una copia de seguridad de todos los servidores de la organización todos los días entre semana después del horario de trabajo, puede automatizar esta tarea. Programe la copia de seguridad para que se ejecute después de las 22:00 de lunes a viernes. Si la copia de seguridad encuentra un problema, el Agente SQL Server puede registrar el evento y notificárselo.

> [!NOTE]
> El servicio Agente SQL Server está deshabilitado de manera predeterminada cuando se instala SQL Server, a menos que el usuario elija explícitamente iniciar automáticamente el servicio.

## <a name="sql-server-agent-components"></a><a name="Components"></a>Componentes del Agente SQL Server

El Agente SQL Server emplea los componentes siguientes para definir las tareas que se van a realizar, cuándo se van a llevar a cabo y cómo se va a notificar si se han realizado correctamente o no.

### <a name="jobs"></a>Trabajos

Un *trabajo* es una serie especificada de acciones que realiza el Agente SQL Server. Use trabajos para definir una tarea administrativa que se puede ejecutar una o varias veces y supervisar para detectar si lo hace correctamente o con errores. Un trabajo se puede ejecutar en un servidor local o en varios servidores remotos.

> [!IMPORTANT]
> Los trabajos del Agente SQL Server que se están ejecutando en el momento de un evento de conmutación por error en una instancia de clúster de conmutación por error de SQL Server no se reanudan después de la conmutación por error a otro nodo de clúster de conmutación por error. Los trabajos del Agente SQL Server que se están ejecutando en el momento que se pausa un nodo de Hyper-V no se reanudan si la pausa origina una conmutación por error a otro nodo. Los trabajos que empiezan pero no se finalizan como consecuencia de un evento de conmutación por error se registran como iniciados, pero no muestran entradas de registro adicionales para que indiquen finalización o error. Los trabajos del Agente SQL Server en estos escenarios se muestran como nunca finalizados.

Existen varias maneras de ejecutar trabajos:

- Conforme a una o más programaciones.

- Como respuesta a una o varias alertas.

- Ejecutando el procedimiento almacenado sp_start_job.

Cada acción de un trabajo es un *paso de trabajo*. Por ejemplo, un paso de trabajo puede consistir en la ejecución de una instrucción Transact\-SQL, la ejecución de un paquete SSIS o la emisión de un comando en un servidor de Analysis Services. Los pasos de trabajo se administran como parte de un trabajo.

Cada paso se ejecuta en un contexto de seguridad específico. En el caso de los pasos de trabajo que utilizan Transact\-SQL, use la instrucción EXECUTE AS para establecer el contexto de seguridad para este paso. Para los demás tipos de pasos de trabajo, utilice una cuenta de proxy para establecer el contexto de seguridad.

### <a name="schedules"></a>Programaciones

Una *programación* especifica cuándo se ejecuta un trabajo. Se puede ejecutar más de un trabajo en la misma programación y se puede aplicar más de una programación al mismo trabajo. Una programación puede definir las condiciones siguientes para el momento en que se ejecuta un trabajo:

- Cada vez que se inicia el Agente SQL Server.

- Cuando el uso de la CPU del equipo se encuentre en un nivel que se haya definido como inactivo.

- Una vez, a una hora y una fecha específicas.

- Según una programación periódica.

Para obtener más información, vea [Crear y adjuntar programaciones a trabajos](../../ssms/agent/create-and-attach-schedules-to-jobs.md).

### <a name="alerts"></a>Alertas

Una *alerta* es una respuesta automática a un evento específico. Por ejemplo, un evento puede ser el inicio de un trabajo o que los recursos del sistema alcancen un umbral específico. Debe definir las condiciones en las que se genera una alerta.

Una alerta puede responder a una de las condiciones siguientes:

- Eventos de SQL Server

- Condiciones de rendimiento de SQL Server

- Eventos del Instrumental de administración de Windows (WMI) en el equipo en el que se ejecuta el Agente SQL Server

Una alerta puede realizar las acciones siguientes:

- Notificar a uno o varios operadores

- Ejecución de un trabajo

Para más información, consulte [Alertas](../../ssms/agent/alerts.md).  

### <a name="operators"></a>Operadores

Un *operador* define la información de contacto de las personas responsables del mantenimiento de una o varias instancias de SQL Server. En algunas compañías, las responsabilidades de operador están asignadas a una sola persona. En compañías con varios servidores, muchas personas comparten las responsabilidades de operador. Un operador no contiene información de seguridad y no define una entidad de seguridad.  

SQL Server puede notificar a los operadores de alertas a través de...

- Correo electrónico

- Buscapersonas (por correo electrónico)

- **net send**

> [!NOTE]
> Para enviar notificaciones mediante **net send**,se debe iniciar el servicio Windows Messenger en el equipo en el que reside el Agente SQL Server.

> [!IMPORTANT]
> Las opciones Buscapersonas y **net send** se quitarán del Agente SQL Server en una versión futura de SQL Server. Evite utilizar estas características en los nuevos trabajos de programación y planee modificar las aplicaciones que actualmente las utilizan.  

Para enviar a los operadores notificaciones por correo electrónico o buscapersonas, deberá configurar el Agente SQL Server para usar Correo electrónico de base de datos. Para obtener más información, consulte [Correo electrónico de base de datos](../../relational-databases/database-mail/database-mail.md).

Puede definir un operador como alias de un grupo de personas. De esta manera, todos los miembros de ese alias no se verifican al mismo tiempo. Para obtener más información, vea [Operadores (Guía de programación de C#)](../../ssms/agent/operators.md).

## <a name="security-for-sql-server-agent-administration"></a><a name="Security"></a>Seguridad en la administración del Agente SQL Server

El Agente SQL Server usa los roles fijos de base de datos **SQLAgentUserRole**, **SQLAgentReaderRole** y **SQLAgentOperatorRole** en la base de datos **msdb** a fin de controlar el acceso al Agente SQL Server para aquellos usuarios que no son miembros del rol fijo de servidor **sysadmin**. Además de estos roles fijos de base de datos, los subsistemas y los servidores proxy ayudan a los administradores de bases de datos a garantizar que cada paso de trabajo se ejecuta con los permisos mínimos necesarios para realizar la tarea.  

### <a name="roles"></a>Roles

Los miembros de los roles fijos de base de datos **SQLAgentUserRole**, **SQLAgentReaderRole** y **SQLAgentOperatorRole** de **msdb**, y los miembros del rol fijo de servidor **sysadmin** tienen acceso al Agente SQL Server. Un usuario que no pertenezca a ninguno de estos roles no puede utilizar el Agente SQL Server. Para obtener más información sobre los roles que usa el Agente SQL Server, vea [Implementar la seguridad del Agente SQL Server](../../ssms/agent/implement-sql-server-agent-security.md).

### <a name="subsystems"></a>Subsistemas

Un subsistema es un objeto predefinido que representa las funciones disponibles para un paso de trabajo. Cada proxy tiene acceso a uno o varios subsistemas. Los subsistemas proporcionan seguridad, ya que delimitan el acceso a las que funciones que están disponibles para el proxy. Cada paso de trabajo se ejecuta en el contexto de un proxy, con la excepción de los pasos de trabajo de Transact\-SQL. Los pasos de trabajo de Transact\-SQL usan el comando EXECUTE AS para establecer el contexto de seguridad en el propietario del trabajo.  

SQL Server define los subsistemas mostrados en la tabla siguiente:  

|Nombre del subsistema|Descripción|
|--------------|-----------|
|Scripts Microsoft ActiveX|Ejecuta un paso de trabajo de scripts ActiveX.<br /><br />**Advertencia** El subsistema de scripting ActiveX se quitará del Agente SQL Server en una versión futura de Microsoft SQL Server. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan.|  
|Sistema operativo (**CmdExec**)|Ejecuta un programa ejecutable.|
|PowerShell|Ejecuta un paso de trabajo de scripts de PowerShell.|
|Distribuidor de replicación|Ejecuta un paso de trabajo que activa el Agente de distribución de replicación.|
|Mezcla de replicación|Ejecuta un paso de trabajo que activa el Agente de mezcla.|
|Lector de cola de replicación|Ejecuta un paso de trabajo que activa el Agente de lectura de cola de replicación.|
|Instantánea de replicación|Ejecuta un paso de trabajo que activa el Agente de instantáneas.|
|Registro del LOG de transacciones de replicación|Ejecuta un paso de trabajo que activa el Agente de registro del LOG.|
|Comando de Analysis Services|Ejecute un comando de Analysis Services.|
|Consulta de Analysis Services|Ejecute una consulta de Analysis Services.|
|Ejecución de paquetes SSIS|Ejecute un paquete SSIS.|

> [!NOTE]
> Puesto que los pasos de trabajo de Transact\-SQL no usan servidores proxy, no hay ningún subsistema del Agente SQL Server para los pasos de trabajo de Transact\-SQL.  

El Agente SQL Server aplica las restricciones del subsistema incluso si la entidad de seguridad del proxy tiene permiso para ejecutar la tarea del paso de trabajo. Por ejemplo, un proxy para un usuario que es miembro del rol fijo de servidor sysadmin no puede ejecutar un paso de trabajo de SSIS, salvo en el caso de que el proxy tenga acceso al subsistema de SSIS, aunque el usuario pueda ejecutar paquetes SSIS.  

### <a name="proxies"></a>Servidores proxy

El Agente SQL Server usa servidores proxy para administrar contextos de seguridad. Se puede utilizar un servidor proxy en más de un paso de trabajo. Los miembros del rol fijo de servidor **sysadmin** pueden crear servidores proxy.  

Cada proxy se corresponde con unas credenciales de seguridad. Cada proxy puede asociarse a un conjunto de subsistemas y un conjunto de inicios de sesión. El proxy solo se puede utilizar con pasos de trabajo que utilizan un subsistema asociado al proxy. Para crear un paso de trabajo que utilice un proxy determinado, el propietario del trabajo debe utilizar un inicio de sesión asociado al proxy o un miembro de un rol con acceso sin restricciones a los servidores proxy. Los miembros del rol fijo de servidor **sysadmin** tienen acceso ilimitado a los servidores proxy. Los miembros de **SQLAgentUserRole**, **SQLAgentReaderRole** o **SQLAgentOperatorRole** solo pueden utilizar servidores proxy para los que dispongan de acceso específico. Cada usuario que sea miembro de alguno de estos roles fijos de base de datos del Agente SQL Server debe tener acceso a servidores proxy específicos para poder crear pasos de trabajo que usen esos servidores proxy.  

## <a name="related-tasks"></a>Related Tasks

Siga estos pasos para configurar el Agente SQL Server a fin de automatizar la administración de SQL Server:

1. Establezca las tareas administrativas o eventos del servidor que se realizan con regularidad y si estas tareas o eventos se pueden administrar mediante programación. Una tarea es una buena candidata a la automatización si consta de una secuencia de pasos predecible y se produce en un momento específico o en respuesta a un evento concreto.

2. Defina un conjunto de trabajos, programaciones, alertas y operadores mediante SQL Server Management Studio, scripts de Transact\-SQL u Objetos de administración de SQL Server (SMO). Para más información, consulte [Crear trabajos](../../ssms/agent/create-jobs.md).  

3. Ejecute los trabajos del Agente SQL Server que se han definido.

> [!NOTE]
> En el caso de la instancia predeterminada de SQL Server, el servicio de SQL Server se denomina SQLSERVERAGENT. En el caso de las instancias con nombre, el servicio Agente SQL Server se denomina SQLAgent$*instancename*.

Si ejecuta varias instancias de SQL Server, use la administración multiservidor para automatizar las tareas comunes a todas las instancias. Para más información, consulte [Administración automatizada en una empresa](../../ssms/agent/automated-administration-across-an-enterprise.md).

Use las tareas siguientes para comenzar a trabajar con el Agente SQL Server:

|Descripción|Tema|
|-----------|-----|
|Describe cómo configurar el Agente SQL Server.|[Configurar el Agente SQL Server](../../ssms/agent/configure-sql-server-agent.md)|  
|Describe cómo iniciar, detener y pausar el servicio del Agente SQL Server.|[Iniciar, detener o pausar el servicio del Agente SQL Server](../../ssms/agent/start-stop-or-pause-the-sql-server-agent-service.md)|
|Describe las consideraciones para especificar una cuenta para el servicio del Agente SQL Server.|[Seleccionar una cuenta para el servicio Agente SQL Server](../../ssms/agent/select-an-account-for-the-sql-server-agent-service.md)|
|Describe cómo usar el registro de errores del Agente SQL Server.|[Registro de errores del Agente SQL Server](../../ssms/agent/sql-server-agent-error-log.md)|  
|Describe cómo usar los objetos de rendimiento.|[Usar objetos de rendimiento](../../ssms/agent/use-performance-objects.md)|
|Describe el Asistente para planes de mantenimiento, que es una utilidad que se usa para crear trabajos, alertas y operadores a fin de automatizar la administración de una instancia de SQL Server.|[Usar el Asistente para planes de mantenimiento](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md)|
|Describe cómo automatizar tareas administrativas mediante el Agente SQL Server.|[Tareas administrativas automatizadas &#40;Agente SQL Server&#41;](../../ssms/agent/automated-administration-tasks-sql-server-agent.md)|

## <a name="nosqlps"></a>NOSQLPS

[!INCLUDE [sql-server-powershell-no-sqlps](../../includes/sql-server-powershell-no-sqlps.md)]

## <a name="see-also"></a>Consulte también

- [Configuración de Área expuesta](../../relational-databases/security/surface-area-configuration.md)
- [Agente SQL Server con PowerShell](../../powershell/sql-server-powershell.md#sql-server-agent)
