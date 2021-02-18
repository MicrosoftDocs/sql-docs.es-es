---
description: Administrar pasos de trabajo
title: Administrar pasos de trabajo
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- job steps [SQL Server replication]
- job steps [SQL Server Agent]
- jobs [SQL Server Agent], Integration Services package step
- executable programs as job steps
- operating systems [SQL Server], job steps
- Transact-SQL job step
- job steps [Transact-SQL]
- Integration Services packages, job steps
- replication job steps [SQL Server]
- logs [SQL Server], jobs
- SQL Server Agent jobs, job steps
- ActiveX scripting jobs [SQL Server]
- job steps [Analysis Services]
ms.assetid: 51352afc-a0a4-428b-8985-f9e58bb57c31
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 8892ff46f9c6c9fd382b39284cbc4bc81449a5d9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355181"
---
# <a name="manage-job-steps"></a>Administrar pasos de trabajo
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

Los pasos de trabajo son acciones que el trabajo realiza en una base de datos o en un servidor. Cada trabajo debe estar formado por un paso, como mínimo. Los pasos de trabajo pueden ser:  
  
-   Programas ejecutables y comandos del sistema operativo.  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] instrucciones, incluidos los procedimientos almacenados y los procedimientos almacenados extendidos.  
  
-   Scripts de PowerShell.  
  
-   [!INCLUDE[msCoName](../../includes/msconame_md.md)] Scripts de ActiveX.  
  
-   Tareas de replicación.  
  
-   [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] tareas.  
  
-   [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] paquetes.  
  
Todos los pasos de trabajo se ejecutan en un contexto de seguridad determinado. Si en el paso de trabajo se especifica un proxy, se ejecuta en el contexto de seguridad de la credencial del proxy. Si en el paso de trabajo no se especifica un proxy, se ejecuta en el contexto de la cuenta de servicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Solo los miembros del rol fijo de servidor sysadmin pueden crear trabajos en los que no se especifique un proxy de forma explícita.  
  
Puesto que los pasos de trabajo se ejecutan en el contexto de un usuario específico de [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows, dicho usuario debe disponer de los permisos y la configuración necesarios para que se ejecute el paso de trabajo. Por ejemplo, si crea un trabajo que requiere una letra de unidad o una ruta de acceso UNC (Convención de nomenclatura universal), los pasos de trabajo se pueden ejecutar con la cuenta de usuario de Windows durante la comprobación de las tareas. Sin embargo, el usuario de Windows para el paso de trabajo debe tener también los permisos y configuraciones de letra de unidad necesarios, o acceso a la unidad requerida. De lo contrario, se producirá un error en el paso de trabajo. Para evitar este problema, asegúrese de que el proxy para cada paso de trabajo dispone de los permisos necesarios para la tarea que realiza dicho paso. Para más información, consulte [Seguridad y protección (motor de base de datos)](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md).  
  
## <a name="job-step-logs"></a>Registros de pasos de trabajo  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] El Agente puede escribir el resultado de algunos pasos de trabajo en un archivo del sistema operativo o en la tabla sysjobstepslogs de la base de datos msdb. Los siguientes tipos de pasos de trabajo pueden escribir la salida en los siguientes destinos:  
  
-   Programas ejecutables y comandos del sistema operativo.  
  
-   Instrucciones[!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
-   [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] tareas.  
  
Solo los pasos de trabajo que ejecutan los usuarios que son miembros del rol fijo de servidor sysadmin pueden escribir la salida en archivos del sistema operativo. Si los pasos de trabajo son ejecutados por usuarios que son miembros de los roles fijos de base de datos SQLAgentUserRole, SQLAgentReaderRole o SQLAgentOperatorRole de la base de datos msdb, la salida de dichos pasos solo se puede escribir en la tabla sysjobstepslogs.  
  
Los registros de pasos de trabajo se eliminan automáticamente al eliminar los trabajos o pasos de trabajo.  
  
> [!NOTE]  
> El registro de pasos de trabajo de tareas de replicación y paquetes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] lo controla el subsistema respectivo. No se puede utilizar el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para configurar el registro de pasos de trabajo para estos tipos de pasos.  
  
## <a name="executable-programs-and-operating-system-commands-as-job-steps"></a>Programas ejecutables y comandos del sistema operativo como pasos de trabajo  
Los programas ejecutables y comandos del sistema operativo se pueden utilizar como pasos de trabajo. Los archivos pueden tener las extensiones .bat, .cmd, .com o .exe.  
  
Si utiliza un programa ejecutable o un comando del sistema operativo como paso de trabajo, debe especificar:  
  
-   El código de salida del proceso que se devuelve si el comando se ha ejecutado correctamente.  
  
-   El comando que se debe ejecutar. Para ejecutar un comando del sistema operativo, se trata simplemente del propio comando. En un programa externo, es el nombre del programa y los argumentos para el programa, por ejemplo: **C:\Archivos de programa\Microsoft SQL Server\100\Tools\Binn\sqlcmd.exe -e -q "sp_who"**  
  
    > [!NOTE]  
    > Debe proporcionar la ruta de acceso completa del archivo ejecutable si éste no se encuentra en un directorio especificado en la ruta de acceso del sistema o la ruta de acceso del usuario con el que se ejecuta el paso de trabajo.  
  
## <a name="transact-sql-job-steps"></a>Pasos de trabajo Transact-SQL  
Al crear un paso de trabajo de [!INCLUDE[tsql](../../includes/tsql-md.md)] , debe:  
  
-   Identificar la base de datos en la que se ejecutará el trabajo.  
  
-   Escribir la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] que se debe ejecutar. La instrucción puede llamar a un procedimiento almacenado o un procedimiento almacenado extendido.  
  
Opcionalmente, puede abrir un archivo [!INCLUDE[tsql](../../includes/tsql-md.md)] existente que actúe como comando para el paso de trabajo.  
  
[!INCLUDE[tsql](../../includes/tsql-md.md)] los pasos de trabajo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no usan cuentas de proxy del Agente. En lugar de ello, el paso de trabajo se ejecuta como el propietario del paso de trabajo, o como la cuenta de servicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si el propietario del paso de trabajo es miembro del rol fijo de servidor sysadmin. Los miembros del rol fijo de servidor sysadmin también pueden especificar que los pasos de trabajo de [!INCLUDE[tsql](../../includes/tsql-md.md)] se ejecuten en el contexto de otro usuario mediante el parámetro *database_user_name* del procedimiento almacenado sp_add_jobstep. Para más información, consulte [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md).  
  
> [!NOTE]  
> Un solo paso de trabajo de [!INCLUDE[tsql](../../includes/tsql-md.md)] puede contener varios lotes. [!INCLUDE[tsql](../../includes/tsql-md.md)] los pasos de trabajo pueden contener comandos GO incrustados.  
  
## <a name="powershell-scripting-job-steps"></a>Pasos de trabajo de scripts de PowerShell  
Al crear un paso de trabajo de script de PowerShell, es preciso especificar una de estas dos cosas como comando para dicho paso:  
  
-   El texto de un script de PowerShell.  
  
-   El archivo de script de PowerShell que se desea abrir.  
  
El subsistema PowerShell del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] abre una sesión PowerShell y carga los complementos PowerShell de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . El script de PowerShell usado como comando del paso de trabajo puede hacer referencia los cmdlets y al proveedor de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell. Para más información sobre cómo escribir scripts de PowerShell mediante los complementos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell, consulte [SQL Server PowerShell](../../powershell/sql-server-powershell.md).  
  
## <a name="activex-scripting-job-steps"></a>Pasos de trabajo de scripts ActiveX  
  
> [!IMPORTANT]
> El paso de trabajo de scripts ActiveX se quitará del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en una versión futura de [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan.  
  
Al crear un paso de trabajo de scripts ActiveX, debe realizar las siguientes acciones:  
  
-   Identificar el lenguaje de scripts en el que se ha escrito el paso de trabajo  
  
-   Escribir el script ActiveX  
  
También puede abrir un archivo de scripts ActiveX existente y utilizarlo como comando para el paso de trabajo. Opcionalmente, los comandos de los scripts ActiveX se pueden compilar externamente (por ejemplo, mediante Microsoft Visual Basic) y, después, ejecutarse como programas ejecutables.  
  
Si el comando del paso de trabajo es un script ActiveX, puede utilizar el objeto SQLActiveScriptHost para imprimir la salida en el registro de historial del paso de trabajo o para crear objetos COM. SQLActiveScriptHost es un objeto global que el sistema host del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] introduce en el espacio de nombres de scripts. El objeto tiene dos métodos (Print y CreateObject). En el siguiente ejemplo se muestra cómo funcionan los scripts ActiveX en Visual Basic Scripting Edition (VBScript).  
  
```  
' VBScript example for ActiveX Scripting job step  
' Create a Dmo.Server object. The object connects to the  
' server on which the script is running.  
  
Set oServer = CreateObject("SQLDmo.SqlServer")  
oServer.LoginSecure = True  
oServer.Connect "(local)"  
'Disconnect and destroy the server object  
oServer.DisConnect  
Set oServer = nothing  
```  
  
## <a name="replication-job-steps"></a>Pasos de trabajos de replicación  
Si crea publicaciones y suscripciones con replicación, se crean trabajos de replicación de forma predeterminada. El tipo de replicación (de instantáneas, transaccional o de mezcla) y las opciones utilizadas determinan el tipo de trabajo que se crea.  
  
Los pasos de trabajo de replicación activan uno de los siguientes agentes de réplica:  
  
-   Agente de instantáneas (trabajo Instantánea)  
  
-   Agente de registro del LOG  (trabajo Lector del registro)  
  
-   Agente de distribución (trabajo Distribución)  
  
-   Agente de mezcla (trabajo Mezcla)  
  
-   Agente de lectura de cola (trabajo Lectura de cola)  
  
Cuando esté configurada la replicación, puede especificar que los agentes de replicación se ejecuten de una de estas tres maneras: continuamente tras iniciar el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , a petición o según una programación. Para más información sobre los agentes de replicación, consulte [Información general sobre los agentes de replicación](../../relational-databases/replication/agents/replication-agents-overview.md).  
  
## <a name="analysis-services-job-steps"></a>Pasos de trabajo de Analysis Services  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] El Agente admite dos tipos distintos de pasos de trabajo de Analysis Services: pasos de trabajo de comando y pasos de trabajo de consulta.  
  
### <a name="analysis-services-command-job-steps"></a>Pasos de trabajo de comando de Analysis Services  
Al crear un paso de trabajo de comando de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] , debe:  
  
-   Identificar el servidor OLAP de la base de datos en el que se ejecutará el paso de trabajo.  
  
-   Escribir la instrucción que se debe ejecutar. Esta instrucción debe ser XML para el método [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] **Execute**. Puede que la instrucción no contenga un elemento SOAP completo o un XML para el método [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] **Discover**. Observe que, mientras que [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] es compatible con los sobres SOAP completos y con el método **Discover** , los pasos de trabajo del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no lo son.  
  
### <a name="analysis-services-query-job-steps"></a>Pasos de trabajo de consulta de Analysis Services  
Al crear un paso de trabajo de consulta de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] , debe:  
  
-   Identificar el servidor OLAP de la base de datos en el que se ejecutará el paso de trabajo.  
  
-   Escribir la instrucción que se debe ejecutar. La instrucción debe ser una consulta de expresiones multidimensionales (MDX).  
  
Para más información sobre MDX, consulte [Aspectos básicos de la instrucción MDX](/analysis-services/multidimensional-models/mdx/mdx-query-fundamentals-analysis-services?viewFallbackFrom=sql-server-ver15).  
  
## <a name="integration-services-packages"></a>Paquetes de Integration Services  
Al crear un paso de trabajo de paquete de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , debe realizar las operaciones siguientes:  
  
-   Identificar el origen del paquete.  
  
-   Identificar la ubicación del paquete.  
  
-   Identificar los archivos de configuración si son necesarios para el paquete.  
  
-   Identificar los archivos de comandos si son necesarios para el paquete.  
  
-   Identificar la comprobación que se debe utilizar para el paquete. Por ejemplo, puede especificar que el paquete debe estar firmado o que debe tener un Id. de paquete específico.  
  
-   Identificar los orígenes de datos del paquete.  
  
-   Identificar los proveedores de registro del paquete.  
  
-   Especificar las variables y los valores que se deben establecer para ejecutar el paquete.  
  
-   Identificar las opciones de ejecución.  
  
-   Agregar o modificar las opciones de línea de comandos.  
  
Tenga en cuenta que si implementó el paquete en el catálogo de SSIS y especifica **Catálogo de SSIS** como origen del paquete, gran parte de esta información de configuración se obtiene automáticamente del paquete. En la pestaña **Configuración** puede especificar el entorno, los valores de parámetro, los valores del administrador de conexiones, las invalidaciones de propiedad y si el paquete se ejecuta en un entorno en tiempo de ejecución de 32 bits.  
  
Para más información sobre cómo crear pasos de trabajo que ejecutan paquetes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , consulte [Trabajos del Agente SQL Server para paquetes](../../integration-services/packages/sql-server-agent-jobs-for-packages.md).  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Descripción|Tema|  
|-|-|  
|Describe cómo crear un paso de trabajo con un programa ejecutable.|[Crear un paso de trabajo CmdExec](../../ssms/agent/create-a-cmdexec-job-step.md)|  
|Describe cómo restablecer los permisos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|[Configurar un usuario para crear y administrar trabajos del Agente SQL Server](../../ssms/agent/configure-a-user-to-create-and-manage-sql-server-agent-jobs.md)|  
|Describe cómo crear un paso de trabajo de [!INCLUDE[tsql](../../includes/tsql-md.md)] .|[Create a Transact-SQL Job Step](../../ssms/agent/create-a-transact-sql-job-step.md)|  
|Describe cómo definir opciones para los pasos de trabajo Transact-SQL del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de Microsoft.|[Define Transact-SQL Job Step Options](../../ssms/agent/define-transact-sql-job-step-options.md)|  
|Describe cómo crear un paso de trabajo de script ActiveX.|[Create an ActiveX Script Job Step](../../ssms/agent/create-an-activex-script-job-step.md)|  
|Describe cómo crear y definir pasos de trabajo del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que ejecutan comandos y consultas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Analysis Services.|[Create an Analysis Services Job Step](../../ssms/agent/create-an-analysis-services-job-step.md)|  
|Describe la acción que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe realizar si se produce un error durante la ejecución del trabajo.|[Set Job Step Success or Failure Flow](../../ssms/agent/set-job-step-success-or-failure-flow.md)|  
|Describe cómo ver detalles de pasos de trabajo en el cuadro de diálogo Propiedades de paso de trabajo.|[View Job Step Information](../../ssms/agent/view-job-step-information.md)|  
|Describe cómo eliminar un registro de pasos de trabajo del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|[Delete a Job Step Log](../../ssms/agent/delete-a-job-step-log.md)|  
  
## <a name="see-also"></a>Consulte también  
[sysjobstepslogs (Transact-SQL)](../../relational-databases/system-tables/dbo-sysjobstepslogs-transact-sql.md)  
[Crear trabajos](../../ssms/agent/create-jobs.md)  
[sp_add_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)  
