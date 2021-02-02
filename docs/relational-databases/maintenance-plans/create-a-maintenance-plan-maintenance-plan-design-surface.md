---
title: Creación de un plan de mantenimiento mediante la superficie de diseño
description: Obtenga información sobre cómo crear un plan de mantenimiento de varios servidores o de un único servidor mediante la Superficie de diseño del plan de mantenimiento de SQL Server.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- Maintenance Plan Design Surface
ms.assetid: 2ef803ee-a9f8-454a-ad63-fedcbe6838d1
author: MikeRayMSFT
ms.author: mikeray
ms.custom: seo-lt-2019
ms.openlocfilehash: 931aa85fbb37d8dba1c9bb3e06e00760d3f16caf
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049170"
---
# <a name="create-a-maintenance-plan-maintenance-plan-design-surface"></a>Crear un plan de mantenimiento (superficie de diseño del plan de mantenimiento)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo crear un plan de mantenimiento de varios servidores o de uno mediante la superficie de diseño del plan de mantenimiento de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Aunque el **Asistente para plan de mantenimiento** es mejor para crear planes de mantenimiento básicos, crear un plan con la superficie de diseño le permite usar un flujo de trabajo mejorado.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Seguridad](#Security)  
  
-   [Crear un plan de mantenimiento mediante la superficie de diseño del plan de mantenimiento](#SSMSProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   Para crear un plan de mantenimiento multiservidor, se debe configurar un entorno multiservidor que contenga un servidor maestro y uno o varios servidores de destino. Los planes de mantenimiento multiservidor se deben crear y mantener en el servidor maestro. Estos planes se pueden ver, pero no mantener, en servidores de destino.  
  
-   Los miembros de los roles **db_ssisadmin** y **dc_admin** quizá puedan elevar sus privilegios a **sysadmin**. Esta elevación de privilegio se puede producir porque estos roles pueden modificar los paquetes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ; estos paquetes los puede ejecutar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizando el contexto de seguridad de **sysadmin** del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para protegerse contra esta elevación de privilegio al ejecutar planes de mantenimiento, conjuntos de recopilación de datos y otros paquetes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , configure los trabajos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que ejecutan paquetes para usar una cuenta de proxy con privilegios limitados o agregar solo los miembros de **sysadmin** a los roles **db_ssisadmin** y **dc_admin** .  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Para crear o administrar planes de mantenimiento, debe ser miembro del rol fijo de servidor **sysadmin** . El Explorador de objetos solo muestra el nodo **Planes de mantenimiento** para los usuarios que son miembros del rol fijo de servidor **sysadmin** .  
  
##  <a name="using-maintenance-plan-design-surface"></a><a name="SSMSProcedure"></a> Usar la Superficie de diseño del plan de mantenimiento  
  
#### <a name="to-create-a-maintenance-plan"></a>Para crear un plan de mantenimiento  
  
1.  En el Explorador de objetos, haga clic en el signo más para expandir el servidor donde desea crear un plan de mantenimiento.  
  
2.  Haga clic en el signo más para expandir la carpeta **Administración** .  
  
3.  Haga clic con el botón derecho en la carpeta **Planes de mantenimiento** y seleccione **Nuevo plan de mantenimiento**.  
  
4.  En el cuadro de diálogo **Nuevo plan de mantenimiento** , en el cuadro **Nombre** , escriba un nombre para el plan y, a continuación, haga clic en **Aceptar**. De este modo se abre el cuadro de herramientas y la superficie *maintenance_plan_name* **[Design]** con el subplán **Subplan_1** creado en la cuadrícula principal.  
  
     Las siguientes opciones están disponibles en el encabezado del espacio de diseño.  
  
     **Agregar subplán**  
     Agrega un subplán que puede configurar.  
  
     **Propiedades del subplán**  
     Muestra el cuadro de diálogo **Propiedades del subplán** para el subplán seleccionado en la cuadrícula principal. O bien, haga doble clic en un subplán de la cuadrícula para mostrar el cuadro de diálogo **Propiedades del subplán** . Vea a continuación para obtener más información sobre este cuadro de diálogo.  
  
     **Eliminar subplán seleccionado**  
     Elimina el subplán seleccionado.  
  
     **Programación del subplán**  
     Muestra el cuadro de diálogo **Nueva programación de trabajo** para el subplán seleccionado.  
  
     **Quitar programación**  
     Quita una programación del subplán seleccionado.  
  
     **Administrar conexiones**  
     Muestra el cuadro de diálogo **Administrar conexiones** . Se utiliza para agregar conexiones adicionales de instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al plan de mantenimiento. Vea a continuación para obtener más información sobre este cuadro de diálogo.  
  
     **Informes y registro**  
     Muestra el cuadro de diálogo **Informes y registro** . Vea a continuación para obtener más información sobre este cuadro de diálogo.  
  
     **Servidores**  
     Muestra el cuadro de diálogo **Servidores** , que se usa para seleccionar los servidores en los que se ejecutarán las tareas del subplan. Esta opción está habilitada solo en servidores maestros en entornos multiservidor. Para obtener más información, vea [Crear un entorno multiservidor](../../ssms/agent/create-a-multiserver-environment.md) y [Maintenance Plan &#40;Servers&#41; (Plan de mantenimiento &#40;servidores&#41;)](../../relational-databases/maintenance-plans/maintenance-plan-servers.md).  
  
     **Nombre**  
     Muestra el nombre del plan de mantenimiento. En los nuevos planes de mantenimiento, el nombre se especifica en un cuadro de diálogo antes de que se abra el diseñador de planes de mantenimiento. Para cambiar el nombre de un plan de mantenimiento, haga clic con el botón derecho en el plan en el Explorador de objetos y, luego, haga clic en **Cambiar nombre**.  
  
     **Descripción**  
     Vea o especifique una descripción para el plan de mantenimiento. La longitud máxima de una descripción es de 512 caracteres.  
  
     **Superficie del diseñador**  
     Diseña y mantiene los planes de mantenimiento. Utilice la superficie del diseñador para agregar o eliminar tareas de mantenimiento a un plan, especificar los vínculos de precedencia entre las tareas e indicar la bifurcación y el paralelismo de tareas.  
  
     Un vínculo de precedencia entre dos tareas establece una relación entre ellas. La segunda tarea (la *tarea dependiente*) solo se ejecuta si el resultado de la ejecución de la primera tarea (la *tarea precedente*) coincide con el criterio especificado. Por lo general, el resultado de la ejecución es **Correcto**, **Error** o **Conclusión**. Para obtener más información, vea el paso **8** a continuación.  
  
5.  En el encabezado del espacio de diseño, haga doble clic en **Subplan_1** y escriba un nombre y una descripción para el subplán en el cuadro de diálogo **Propiedades del subplán** .  
  
     Las siguientes opciones están disponibles en el cuadro de diálogo **Propiedades del subplán** .  
  
     **Nombre**  
     Nombre del subplán.  
  
     **Descripción**  
     Breve descripción del subplán.  
  
     **Programación**  
     Indica en qué programación se ejecutó el subplán. Haga clic en **Programación del subplán** para abrir el cuadro de diálogo **Nueva programación de trabajo** . Haga clic en **Quitar programación** para eliminar la programación del subplán.  
  
     Lista de **Ejecutar como**  
     Seleccione la cuenta que se utilizará para ejecutar esta subtarea.  
  
6.  Haga clic en **Programación del subplán** para escribir detalles de programación en el cuadro de diálogo **Nueva programación del trabajo** .  
  
7.  Para generar el subplán, arrastre y coloque los elementos del flujo de tareas del **Cuadro de herramientas** a la superficie de diseño del plan. Haga doble clic en las tareas para abrir los cuadros de diálogo que le permitirán configurar las opciones de las tareas.  
  
     Las siguientes tareas del plan de mantenimiento están disponibles en **Cuadro de herramientas**:  
  
    -   **Tarea Copia de seguridad de la base de datos**  
  
    -   **Tarea Comprobar la integridad de la base de datos**  
  
    -   **Tarea Ejecutar trabajo del Agente SQL Server**  
  
    -   **Tarea Ejecutar instrucción T-SQL**  
  
    -   **Tarea Limpieza de historial**  
  
    -   **Tarea Limpieza de mantenimiento**  
  
    -   **Tarea Notificar al operador**  
  
    -   **Tarea Volver a generar índice**  
  
    -   **Tarea Reorganizar índice**  
  
    -   **Tarea Reducir base de datos**  
  
    -   **Tarea Actualizar estadísticas**  
  
     Para agregar tareas al **Cuadro de herramientas**:  
  
    1.  En el menú **Herramientas** , haga clic en **Elegir elementos del cuadro de herramientas**.  
  
    2.  Seleccione las herramientas que desee que aparezcan en el **Cuadro de herramientas** y, a continuación, haga clic en **Aceptar**.  
  
     Agregar tareas del plan de mantenimiento al **Cuadro de herramientas** también hace que estén disponibles en el **Asistente para planes de mantenimiento**. Para obtener más información sobre las tareas individuales anteriores, vea [Using Maintenance Plan Wizard (Usar el Asistente para planes de mantenimiento)](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md#SSMSProcedure) en **Start the Maintenance Plan Wizard (Iniciar el Asistente para planes de mantenimiento)** .  
  
8.  Para definir un flujo de trabajo entre tareas:  
  
    1.  Haga clic con el botón derecho en la tarea anterior y seleccione **Agregar restricción de precedencia**.  
  
    2.  En el cuadro de diálogo **Flujo de control** , en la lista **Para** , seleccione la tarea dependiente y haga clic en **Aceptar**.  
  
    3.  Haga doble clic en el conector entre las dos tareas para abrir el cuadro de diálogo **Editor de restricciones de precedencia** .  
  
         Las siguientes opciones están disponibles en el cuadro de diálogo **Editor de restricciones de precedencia** .  
  
         **Opción de restricción**  
         Define cómo funciona una restricción entre dos tareas.  
  
         Lista **Operación de evaluación**  
         Permite especificar la operación de evaluación que utiliza la restricción de precedencia. Las operaciones son: **Restricción**, **Expresión**, **Expresión y restricción** y **Expresión o restricción**.  
  
         Lista **Valor**  
         Especifique el valor de restricción: **Correcto**, **Error** o **Finalización**. **Correcto** es el valor predeterminado.  
  
        > [!NOTE]  
        >  La línea de restricción de precedencia es verde para **Correcto**, roja para **Error** y azul para **Conclusión**.  
  
         **Expression**  
         Si usa las operaciones **Expresión**, **Expresión y restricción** o **Expresión o restricción**, escriba una expresión. La expresión debe evaluarse como un valor booleano.  
  
         **Test**  
         Permite validar la expresión.  
  
         **Varias restricciones**  
         Define el modo en que varias restricciones interoperan para controlar la ejecución de la tarea restringida.  
  
         **Y lógico**  
         Seleccione esta opción para indicar que varias restricciones de precedencia en el mismo archivo ejecutable deben evaluarse de forma conjunta. Todas las restricciones deben evaluarse como True. Es la opción predeterminada.  
  
        > [!NOTE]  
        >  Este tipo de restricción de precedencia se muestra como una línea sólida verde, roja o azul.  
  
         **O lógico**  
         Seleccione esta opción para indicar que varias restricciones de precedencia en el mismo archivo ejecutable deben evaluarse de forma conjunta. Al menos una restricción debe evaluarse como True.  
  
        > [!NOTE]  
        >  Este tipo de restricción de precedencia se muestra como una línea de puntos verde, roja o azul.  
  
9. Para agregar otro subplán que contiene tareas que se ejecutan en otra programación, haga clic en **Agregar subplán** en la barra de herramientas para abrir el cuadro de diálogo **Propiedades del subplán** .  
  
10. Para agregar conexiones a servidores diferentes:  
  
    1.  En la barra de herramientas del espacio de diseño, haga clic en **Administrar conexiones**.  
  
    2.  En el cuadro de diálogo **Administrar conexiones** , haga clic en **Agregar**.  
  
    3.  En el cuadro de diálogo **Propiedades de conexión** , en el cuadro **Nombre de la conexión** , escriba el nombre de la conexión que va a crear.  
  
    4.  En **Especificar lo siguiente para conectarse a los datos de SQL Server**, en el cuadro **Seleccionar o especificar un nombre de servidor**, escriba el nombre del servidor SQL Server que quiere usar o haga clic en los puntos suspensivos **(...)** y seleccione un servidor en el cuadro de diálogo **SQL Server**. Si selecciona un servidor en el cuadro de diálogo **SQL Server** , haga clic en **Aceptar**.  
  
    5.  En **Especificar información para iniciar sesión en el servidor**, seleccione **Usar seguridad integrada de Windows NT** o **Usar un nombre de usuario y contraseña específicos**. Si elige usar un nombre de usuario y una contraseña específicos, escriba esa información en los cuadros **Nombre de usuario** y **Contraseña** , respectivamente.  
  
    6.  En el cuadro de diálogo **Propiedades de conexión** , haga clic en **Aceptar**.  
  
    7.  En el cuadro de diálogo **Administrar las conexiones** , haga clic en **Cerrar**.  
  
11. Para especificar las opciones de informes:  
  
    1.  En la barra de herramientas del espacio de diseño, haga clic en **Informes y registro**.  
  
    2.  En el cuadro de diálogo **Informes y registro** , debajo de **Informes**, seleccione **Generar un informe de archivo de texto** o **Enviar informe a un destinatario de correo electrónico** o ambos.  
  
        1.  Si selecciona **Generar un informe de archivo de texto**, seleccione **Crear un nuevo archivo** o **Anexar a archivo**.  
  
        2.  Según la selección anterior, escriba el nombre y la ruta de acceso completa del nuevo archivo o del archivo que se va a anexar especificando la información en los cuadros **Carpeta** o **Nombre de archivo** . Como alternativa, haga clic en los puntos suspensivos **(...)** y seleccione la ruta de acceso a la carpeta o el nombre de archivo en los cuadros de diálogo **Buscar carpeta -** _nombre\_servidor_ o **Buscar archivos de base de datos -** _nombre\_servidor_.  
  
        3.  Si selecciona **Enviar informe a un destinatario de correo electrónico**, en la lista de **Operador del agente** , seleccione el destinatario del informe enviado por correo electrónico.  
  
            > [!NOTE]  
            >  El Agente SQL Server debe configurarse para que utilice el Correo electrónico de base de datos para enviar correo. Para obtener más información, consulte [Configure SQL Server Agent Mail to Use Database Mail](../../relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail.md).  
  
    3.  Para guardar más información detallada, en **Registro**, seleccione **Registrar información adicional**.  
  
    4.  Para escribir la información de los resultados del plan de mantenimiento para otro servidor, seleccione **Registrar en el servidor remoto** y después seleccione una conexión al servidor en la lista **Conexión** o haga clic en **Nueva** y escriba la información de conexión en el cuadro de diálogo **Propiedades de conexión** .  
  
    5.  En el cuadro de diálogo **Informes y registro** , haga clic en **Aceptar**.  
  
12. Para ver los resultados en el visor del archivo de registro, en el **Explorador de objetos**, haga clic con el botón derecho en la carpeta **Planes de mantenimiento** o en el plan de mantenimiento específico y, luego, selecciones **Ver historial**.  

     Las opciones siguientes están disponibles en el cuadro de diálogo **Visor de archivos de registro -** _nombre\_servidor_.  
  
     **Cargar registro**  
     Abre un cuadro de diálogo donde puede especificar un archivo de registro para cargar.  
  
     **Exportarar**  
     Abre un cuadro de diálogo que permite exportar la información que se muestra en la cuadrícula **Resumen de archivos de registro** a un archivo de texto.  
  
     **Actualizar**  
     Actualice la vista de los registros seleccionados. El botón **Actualizar** hace que se vuelvan a leer los registros seleccionados del servidor de destino al tiempo que se aplica una configuración de filtro.  
  
     **Filter**  
     Abre un cuadro de diálogo que permite especificar la configuración que se usa para filtrar el archivo de registro, como **Conexión**, **Fecha** u otros criterios de filtro de **General** .  
  
     **Búsqueda**  
     Permite buscar texto específico en el archivo de registro. No se admite la búsqueda con caracteres comodín.  
  
     **Detención**  
     Detiene la carga de las entradas del archivo de registro. Por ejemplo, puede utilizar esta opción si la carga de un archivo de registro remoto o sin conexión tarda mucho tiempo y solo desea ver las entradas más recientes.  
  
     **Resumen de archivos de registro**  
     Este panel de información muestra un resumen del filtro del archivo de registro. Si no se ha filtrado el archivo, se mostrará el siguiente texto: **No se aplicó ningún filtro**. Si se aplicó un filtro al registro, se mostrará el texto **Filtrar entradas del registro en:** \<filter criteria>.  
  
     **Date**  
     Muestra la fecha del evento.  
  
     **Origen**  
     Muestra la característica de origen desde la que se crea el evento, como el nombre del servicio (MSSQLSERVER, por ejemplo). No aparece para todos los tipos de registro.  
  
     **Mensaje**  
     Muestra todos los mensajes asociados al evento.  
  
     **Tipo de registro**  
     Muestra el tipo de registro al que pertenece el evento. Todos los registros seleccionados aparecen en la ventana de resumen del archivo de registro.  
  
     **Origen del registro**  
     Muestra una descripción del registro de origen en el que se captura el evento.  
  
     **Detalles de las filas seleccionadas**  
     Seleccione una fila para mostrar detalles adicionales sobre la fila de evento seleccionada en la parte inferior de la página. Puede ordenar de nuevo las columnas arrastrándolas a nuevas ubicaciones de la cuadrícula. Puede modificar el tamaño de las columnas arrastrando las barras de separación de las columnas del encabezado de la cuadrícula hacia la izquierda o hacia la derecha. Haga doble clic en las barras de separación de las columnas del encabezado de la cuadrícula para ajustar automáticamente el tamaño de la columna al ancho del contenido.  
  
     **Instancia**  
     Nombre de la instancia en que se produjo el evento. Esto se muestra como *nombre de equipo*\\*nombre de instancia*.  
  
  
