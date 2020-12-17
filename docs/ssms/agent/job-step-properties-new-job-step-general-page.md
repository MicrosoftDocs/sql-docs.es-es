---
description: Propiedades de paso de trabajo - Nuevo paso de trabajo (página General)
title: 'Propiedades de nuevo paso de trabajo: (página general)'
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.job.stepgeneral.f1
ms.assetid: 8d1885ba-4386-4528-8f2b-68c16852720c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: e13935b59d22a6184de14a29910117b9611ad4cd
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466476"
---
# <a name="job-step-properties---new-job-step-general-page"></a>Propiedades de paso de trabajo - Nuevo paso de trabajo (página General)

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

Use esta página para ver y cambiar las propiedades de un paso de trabajo del Agente [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], o bien para definir un nuevo paso de trabajo.  
  
Para navegar a esta página, en el Explorador de objetos de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , expanda el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , haga clic con el botón derecho en **Trabajos**, haga clic en **Nuevo trabajo**, seleccione la página **Pasos** y haga clic en **Nuevo**. También puede navegar a esta página si hace clic con el botón derecho en un trabajo en el Explorador de objetos, hace clic en **Propiedades**, selecciona la página **Pasos** y hace clic en **Nuevo**, **Insertar** o **Editar**.  
  
## <a name="options"></a>Opciones  
**Nombre del paso**  
Establece el nombre del paso de trabajo.  
  
**Tipo**  
Establece el subsistema que utiliza el paso de trabajo. En función del subsistema elegido, las opciones que se muestran para definir el paso de trabajo son diferentes.  
  
**Ejecutar como**  
Establece la cuenta de proxy del paso de trabajo. Los miembros del rol fijo de servidor sysadmin también pueden especificar la **cuenta de servicio del Agente SQL**.  
  
**Base de datos**  
Establece la base de datos en la que se ejecuta el paso de trabajo. Esta opción no está disponible para todos los tipos de pasos de trabajo.  
  
**Comando**  
Establece el comando que ejecuta el paso de trabajo.  
  
## <a name="options-for-transact-sql-job-steps"></a>Opciones de pasos de trabajo Transact-SQL  
**Abrir**  
Carga el comando desde un archivo.  
  
**Seleccionar todo**  
Selecciona el texto del comando.  
  
**Copiar**  
Copia el texto seleccionado al Portapapeles.  
  
**Pegar**  
Pega el contenido del Portapapeles.  
  
**Parse**  
Comprueba la sintaxis del comando.  
  
## <a name="options-for-activex-script-job-steps"></a>Opciones de pasos de trabajo de scripts ActiveX  
  
> [!IMPORTANT]
> El subsistema de scripts ActiveX se quitará del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en una versión futura de [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan.  
  
**VBScript**  
Especifica [!INCLUDE[msCoName](../../includes/msconame_md.md)] Visual Basic Scripting Edition como lenguaje de los pasos de trabajo.  
  
**JScript**  
Especifica JScript como lenguaje de los pasos de trabajo.  
  
**Otros**  
Escriba el nombre del lenguaje para los pasos de trabajo escritos en otro lenguaje de scripts.  
  
**Abrir**  
Carga el comando desde un archivo.  
  
**Seleccionar todo**  
Selecciona el texto del comando.  
  
**Copiar**  
Copia el texto seleccionado.  
  
**Pegar**  
Pega el contenido del Portapapeles.  
  
## <a name="options-for-operating-system-cmdexec-job-steps"></a>Opciones de pasos de trabajo del sistema operativo (CmdExec)  
**Procesar código de salida de un comando correcto**  
Escriba el código de salida que devuelve el comando para indicar un fin correcto.  
  
**Abrir**  
Carga el comando desde un archivo.  
  
**Seleccionar todo**  
Selecciona el texto del comando.  
  
**Copiar**  
Copia el texto seleccionado.  
  
**Pegar**  
Pega el contenido del Portapapeles.  
  
## <a name="options-for-powershell-job-steps"></a>Opciones de pasos de trabajo de PowerShell  
**Abrir**  
Carga el script desde un archivo.  
  
**Seleccionar todo**  
Selecciona el texto del script.  
  
**Copiar**  
Copia el texto seleccionado.  
  
**Pegar**  
Pega el contenido del Portapapeles.  
  
## <a name="options-for-replication-distributor-job-steps"></a>Opciones de pasos de trabajo del Distribuidor de replicación  
**Seleccionar todo**  
Selecciona el texto del comando.  
  
**Copiar**  
Copia el texto seleccionado.  
  
**Pegar**  
Pega el contenido del Portapapeles.  
  
## <a name="options-for-replication-merge-job-steps"></a>Opciones de pasos de trabajo de mezclas de replicación  
**Seleccionar todo**  
Selecciona el texto del comando.  
  
**Copiar**  
Copia el texto seleccionado.  
  
**Pegar**  
Pega el contenido del Portapapeles.  
  
## <a name="options-for-replication-queue-reader-job-steps"></a>Opciones de pasos de trabajo del Lector de cola de replicación  
**Base de datos**  
La base de datos que se utiliza en el paso de trabajo.  
  
**Seleccionar todo**  
Selecciona el texto del comando.  
  
**Copiar**  
Copia el texto seleccionado.  
  
**Pegar**  
Pega el contenido del Portapapeles.  
  
## <a name="options-for-replication-snapshot-job-steps"></a>Opciones de pasos de trabajo de instantáneas de replicación  
**Seleccionar todo**  
Selecciona el texto del comando.  
  
**Copiar**  
Copia el texto seleccionado.  
  
**Pegar**  
Pega el contenido del Portapapeles.  
  
## <a name="options-for-replication-transaction-log-reader-job-steps"></a>Opciones de pasos de trabajo del Registro del LOG de transacciones de replicación  
**Seleccionar todo**  
Selecciona el texto del comando.  
  
**Copiar**  
Copia el texto seleccionado.  
  
**Pegar**  
Pega el contenido del Portapapeles.  
  
## <a name="options-for-sql-server-analysis-services-command-job-steps"></a>Opciones de pasos de trabajo de Comando de SQL Server Analysis Services  
**Server**  
Establece el servidor donde se ejecuta el paso de trabajo.  
  
**Abrir**  
Carga el comando desde un archivo.  
  
**Seleccionar todo**  
Selecciona el texto del comando.  
  
**Copiar**  
Copia el texto seleccionado.  
  
**Pegar**  
Pega el contenido del Portapapeles.  
  
## <a name="options-for-sql-server-analysis-services-query-job-steps"></a>Opciones de pasos de trabajo de Consulta de SQL Server Analysis Services  
**Server**  
Establece el servidor donde se ejecuta el paso de trabajo.  
  
**Base de datos**  
La base de datos que se utiliza en el paso de trabajo.  
  
**Abrir**  
Carga el comando desde un archivo.  
  
**Seleccionar todo**  
Selecciona el texto del comando.  
  
**Copiar**  
Copia el texto seleccionado.  
  
**Pegar**  
Pega el contenido del Portapapeles.  
  
## <a name="options-for-integration-services-package-execution-job-steps"></a>Opciones de pasos de trabajo para la ejecución de paquetes de Integration Services  
  
### <a name="general-tab"></a>Pestaña General  
Especifique dónde se encuentra el paquete [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ([!INCLUDE[ssIS](../../includes/ssis_md.md)]) y qué método de autenticación se va a utilizar. Cuando seleccione esta pestaña, aparecerán las opciones siguientes.  
  
**Origen del paquete**  
Especifica dónde se almacena el paquete [!INCLUDE[ssIS](../../includes/ssis_md.md)] . Elija alguna de las acciones siguientes:  
  
-   **SQL Server**  
  
-   **Sistema de archivos**  
  
-   **Almacén de paquetes SSIS**  
  
**Server**  
Escriba el nombre del servidor en donde se almacena el paquete [!INCLUDE[ssIS](../../includes/ssis_md.md)] . Esta opción solo está disponible cuando se especifica **SQL Server** o **Almacén de paquetes SSIS** para **Origen del paquete**.  
  
**Utilizar autenticación de Windows**  
Para los inicios de sesión en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se utiliza la autenticación de [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows.  
  
**Utilizar autenticación de SQL Server**  
Para los inicios de sesión en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se utiliza la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si selecciona este método de autenticación, escriba el **nombre de usuario** y la **contraseña** adecuados.  
  
> [!IMPORTANT]  
> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se proporciona por motivos de compatibilidad con versiones anteriores. Para mejorar la seguridad, utilice la autenticación de Windows siempre que sea posible.  
  
**Paquete**  
Escriba la ubicación del paquete.  
  
> [!IMPORTANT]  
> Para los paquetes de [!INCLUDE[ssIS](../../includes/ssis_md.md)] protegidos mediante contraseña, haga clic en la pestaña **Configuraciones** para escribir la contraseña en el cuadro de diálogo **Contraseña del paquete** . En caso contrario, el trabajo del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que ejecuta el paquete protegido mediante contraseña generará un error.  
  
### <a name="configurations-tab"></a>Configuraciones (pestaña)  
Especifique las opciones de configuración del paquete [!INCLUDE[ssIS](../../includes/ssis_md.md)] . Cuando selecciona esta pestaña, dispone de las siguientes opciones.  
  
**Archivos de configuración**  
Presenta una lista de los archivos de configuración del paquete.  
  
**Add (Agregar)**  
Agrega un archivo de configuración para el paquete.  
  
**Remove**  
Quita un archivo de configuración del paquete.  
  
**Subir**  
Sube el archivo de configuración seleccionado.  
  
**Bajar**  
Baja el archivo de configuración seleccionado.  
  
### <a name="command-files-tab"></a>Archivos de comandos (pestaña)  
Seleccione los archivos de comandos del paquete. Estos archivos se procesan en el orden en que aparecen en la lista. Cuando seleccione esta pestaña, aparecerán las opciones siguientes.  
  
**Archivos de comandos**  
Presenta una lista de los archivos de comandos del paquete.  
  
**Add (Agregar)**  
Agrega un archivo de comandos.  
  
**Remove**  
Quita el archivo de comandos seleccionado.  
  
**Subir**  
Sube el archivo de comandos seleccionado.  
  
**Bajar**  
Baja el archivo de comandos seleccionado.  
  
### <a name="data-sources-tab"></a>Orígenes de datos (pestaña)  
En esta pestaña puede consultar los orígenes de datos especificados en el paquete.  
  
**Connection Manager**  
Muestra el nombre del origen de datos.  
  
**Descripción**  
Muestra la descripción del origen de datos.  
  
**Cadena de conexión**  
Muestra la cadena de conexión para el origen de datos.  
  
### <a name="execution-options-tab"></a>Opciones de ejecución (pestaña)  
En esta pestaña puede consultar o cambiar las opciones de configuración del paquete.  
  
**Rechazar el paquete cuando haya advertencias de validación**  
Active esta opción para rechazar la ejecución del paquete si se producen advertencias de validación.  
  
**Validar el paquete sin ejecutarlo**  
Active esta opción para que el paso de trabajo valide, pero no ejecute, el paquete.  
  
**Número máximo de ejecutables simultáneos**  
Número máximo de archivos ejecutables que se pueden ejecutar a la vez.  
  
**Habilitar puntos de comprobación de paquetes**  
Active esta opción para que el paso de trabajo utilice puntos de comprobación de paquetes.  
  
**Archivo de punto de comprobación**  
Escriba el nombre del archivo de punto de comprobación del paquete.  
  
**...**  
Busque el archivo de punto de comprobación del paquete.  
  
**Omitir opciones de reinicio**  
Active esta opción para especificar en este paso de trabajo opciones de reinicio diferentes de las opciones especificadas en el paquete.  
  
**Opción de reinicio**  
Seleccione la acción que debe realizarse cuando se reinicia el paquete.  
  
### <a name="logging-tab"></a>Registro (pestaña)  
En esta pestaña puede consultar o cambiar los proveedores de registro del paquete.  
  
**Proveedor de registro**  
Seleccione el valor de ClassID del proveedor de registro.  
  
**Cadena de configuración**  
Escriba la cadena de configuración del proveedor de registro.  
  
**Remove**  
Quita el proveedor de registro.  
  
### <a name="set-values-tab"></a>Valores establecidos (pestaña)  
En esta pestaña puede consultar o cambiar valores de propiedades del paquete.  
  
**Ruta de acceso de la propiedad**  
Permite ver o cambiar la ruta de acceso de la propiedad.  
  
**Valor**  
Permite ver o cambiar el valor de la propiedad.  
  
**Remove**  
Quita la propiedad.  
  
### <a name="verification-tab"></a>Comprobación (pestaña)  
En esta pestaña puede seleccionar las opciones de comprobación del paso de trabajo.  
  
**Ejecutar solo los paquetes firmados**  
Solo ejecuta paquetes con firma. Si se selecciona esta opción, el paso de trabajo se rechaza si es un paquete sin firma.  
  
**Comprobar la generación del paquete**  
Solo ejecuta paquetes con un número de generación específico. Si se selecciona esta opción, el paso de trabajo se rechaza si el paquete no tiene el número de generación especificado.  
  
**Compilar**  
Escriba el número de generación del paquete.  
  
**Comprobar el Id. del paquete**  
Ejecuta solo los paquetes que tienen un identificador concreto. Si se selecciona esta opción, el paso de trabajo se rechaza si el paquete no tiene el identificador especificado.  
  
**Id. de paquete**  
Escriba el Id. del paquete.  
  
**Comprobar el Id. de versión**  
Ejecuta solo los paquetes que tienen un identificador de versión concreto. Si se selecciona esta opción, el paso de trabajo se rechaza si el paquete no tiene el identificador de versión especificado.  
  
**Id. de la versión**  
Escriba el Id. de versión.  
  
### <a name="command-line-tab"></a>Línea de comandos (pestaña)  
En esta pestaña puede especificar las opciones de línea de comandos del paquete. Cuando selecciona esta pestaña, dispone de las siguientes opciones.  
  
**Restaurar las opciones originales**  
Utiliza las opciones de línea de comandos establecidas en este cuadro de diálogo.  
  
**Editar la línea de comandos manualmente**  
Debe especificar las opciones en la ventana de línea de comandos.  
  
**Línea de comandos**  
Escriba las opciones de línea de comandos que se van a utilizar en este paquete.  
  
## <a name="see-also"></a>Consulte también  
[Administrar pasos de trabajo](../../ssms/agent/manage-job-steps.md)  
[Trabajos del Agente SQL Server para paquetes](../../integration-services/packages/sql-server-agent-jobs-for-packages.md)  
[Administrar agentes de replicación](../../relational-databases/replication/agents/replication-agent-administration.md)  
