---
description: Utilidad de ejecución de paquetes (dtexecui)
title: Utilidad de ejecución de paquetes (dtexecui) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.dtexecui.setvalues.f1
- sql13.dts.dtexecui.reporting.f1
- sql13.dts.dtexecui.datasources.f1
- sql13.dts.dtexecui.commandfiles.f1
- sql13.dts.dtexecui.logging.f1
- sql13.dts.dtexecui.general.f1
- sql13.dts.dtexecui.verification.f1
- sql13.dts.dtexecui.executionoptions.f1
- sql13.dts.dtexecui.commandline.f1
- sql13.dts.dtexecui.configuration.f1
helpviewer_keywords:
- DTExecUI utility
ms.assetid: 3d71df39-126b-4c8e-bd77-128bbd5b0887
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 14392191ab3d11165f75f1637c2fb3330aa886ad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346908"
---
# <a name="execute-package-utility-dtexecui"></a>Utilidad de ejecución de paquetes (dtexecui)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Use la **Utilidad de ejecución de paquetes** para ejecutar paquetes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . La utilidad ejecuta los paquetes almacenados en una de estas tres ubicaciones: la base de datos de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el almacén de paquetes de [!INCLUDE[ssIS](../../includes/ssis-md.md)] y el sistema de archivos. Esta interfaz de usuario, que se puede iniciar desde [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o escribiendo **dtexecui** en el símbolo del sistema, es una alternativa a ejecutar paquetes con la herramienta del símbolo del sistema **DTExec** .  
  
 Los paquetes se ejecutan en el mismo proceso que la utilidad **dtexecui.exe** . Como esta utilidad es una herramienta de 32 bits, los paquetes que se ejecutan con **dtexecui.exe** en un entorno de 64 bits se ejecutan en Windows en Win32 (WOW). Al desarrollar y probar comandos mediante la utilidad dtexecui.exe en un equipo de 64 bits, debe probar los comandos en modo de 64 bits mediante la versión de 64 bits de **dtexecui.exe** antes de implementar o programar los comandos en un servidor de producción.  
  
 La **Utilidad de ejecución de paquetes** es una interfaz gráfica de usuario para la herramienta del símbolo del sistema **DTExec** . La interfaz de usuario permite configurar opciones de forma sencilla y reúne automáticamente la línea de comandos que se pasa a la herramienta del símbolo del sistema **DTExec** al ejecutar el paquete desde las opciones especificadas.  
  
 La **Utilidad de ejecución de paquetes** también se puede usar para reunir líneas de comandos que se usan al ejecutar **DTExec** directamente.  
  
### <a name="to-open-execute-package-utility-in-sql-server-management-studio"></a>Para abrir la Utilidad de ejecución de paquetes en SQL Server Management Studio  
  
1.  En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], en el menú **Ver** , haga clic en **Explorador de objetos**.  
  
2.  En el Explorador de objetos, haga clic en **Conectar** y, después, haga clic en **Integration Services**.  
  
3.  En el cuadro de diálogo **Conectar al servidor** , escriba el nombre de servidor en la lista **Nombre del servidor** y, después, haga clic en **Conectar**.  
  
4.  Expanda la carpeta **Paquetes almacenados** y sus subcarpetas, haga clic con el botón derecho en el paquete que quiera ejecutar y, después, haga clic en **Ejecutar paquete**.  
  
### <a name="to-open-the-execute-package-utility-at-the-command-prompt"></a>Para abrir la Utilidad de ejecución de paquetes en el símbolo del sistema  
  
-   En una ventana del símbolo del sistema, ejecute **dtexecui**.  
  
 En las siguientes secciones se describen las páginas del cuadro de diálogo **Utilidad de ejecución de paquetes** .  
  
## <a name="general-page"></a>Página General  
 Use la página **General** del cuadro de diálogo **Utilidad de ejecución de paquetes** para especificar un nombre y una ubicación del paquete.  
  
 La Utilidad de ejecución de paquetes (dtexecui.exe) siempre ejecuta un paquete en el equipo local, aunque el paquete se guarde en un servidor remoto. Si el paquete remoto usa archivos de configuración que también se guardan en el servidor remoto, es probable que la Utilidad de ejecución de paquetes no encuentre las configuraciones y que el paquete genere un error. Para impedir que esto suceda, se deben crear referencias a las configuraciones mediante el uso de un nombre compartido de la convención de nomenclatura universal (UNC) como \\\miservidor\miarchivo.  
  
### <a name="static-options"></a>Opciones estáticas  
 **Origen del paquete**  
 Especifique la ubicación del paquete que desea ejecutar, mediante las siguientes opciones:  
  
|Value|Descripción|  
|-|-|  
|**SQL Server**|Seleccione esta opción si el paquete reside en [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Especifique una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y proporcione un nombre de usuario y una contraseña para llevar a cabo la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cada nombre de usuario y contraseña agrega las opciones **/USER** _username_ y **/PASSWORD** _password_ al símbolo del sistema.|  
|**Sistema de archivos**|Seleccione esta opción si el paquete reside en el sistema de archivos.|  
|**Almacén de paquetes SSIS**|Seleccione esta opción si el paquete reside en el Almacén de paquetes [!INCLUDE[ssIS](../../includes/ssis-md.md)] .|  
  
 Cada una de estas selecciones tiene el siguiente conjunto de opciones.  
  
 **Ejecutar**  
 Haga clic para ejecutar el paquete.  
  
 **Cerrar**  
 Haga clic en esta opción para cerrar el cuadro de diálogo **Utilidad de ejecución de paquetes** .  
  
### <a name="dynamic-options"></a>Opciones dinámicas  
  
#### <a name="package-source--sql-server"></a>Origen del paquete = SQL Server  
 **Server**  
 Escriba el nombre del servidor en el que reside el paquete o seleccione un servidor de la lista.  
  
 **Iniciar sesión en el servidor**  
 Especifique si el paquete debe usar autenticación de Windows o autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener una mayor seguridad, es recomendable utilizar la autenticación de Windows. Si se utiliza la autenticación de Windows, no será necesario especificar un nombre de usuario y una contraseña.  
  
 **Utilizar autenticación de Windows**  
 Seleccione esta opción para usar la autenticación de Windows e iniciar sesión con una cuenta de usuario de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.  
  
 **Utilizar autenticación de SQL Server**  
 Elija esta opción para usar la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cuando un usuario se conecta con un nombre y una contraseña de inicio de sesión determinados desde una conexión no de confianza, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] realiza la autenticación y comprueba si se ha configurado una cuenta de inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y si la contraseña especificada coincide con la almacenada anteriormente. Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no encuentra la cuenta de inicio de sesión, la autenticación no se realizará correctamente y el usuario recibirá un mensaje de error.  
  
> [!IMPORTANT]  
>  Siempre que sea posible, utilice la autenticación de Windows.  
  
 **Paquete**  
 Escriba el nombre del paquete, o bien haga clic en el botón de puntos suspensivos **(…)** para buscar un paquete mediante el cuadro de diálogo **Seleccionar un paquete SSIS**.  
  
#### <a name="package-source--file-system"></a>Origen del paquete = Sistema de archivos  
 **Paquete**  
 Escriba el nombre del paquete, o bien haga clic en el botón de puntos suspensivos **(…)** para buscar un paquete mediante el cuadro de diálogo Abrir. De forma predeterminada, el cuadro de diálogo solamente muestra archivos con la extensión .dtsx.  
  
#### <a name="package-source--ssis-package-store"></a>Origen del paquete = Almacén de paquetes SSIS  
 **Server**  
 Escriba el nombre del equipo en el que reside el paquete o seleccione un equipo de la lista.  
  
 **Iniciar sesión en el servidor**  
 Especifique si el paquete debe utilizar la autenticación de Microsoft Windows para conectarse al origen del paquete. Para obtener una mayor seguridad, es recomendable utilizar la autenticación de Windows. Si se utiliza la autenticación de Windows, no será necesario especificar un nombre de usuario y una contraseña.  
  
 **Utilizar autenticación de Windows**  
 Seleccione esta opción para utilizar la autenticación de Windows e iniciar sesión con una cuenta de usuario de Microsoft Windows.  
  
 **Utilizar autenticación de SQL Server**  
 Esta opción no está disponible si ejecuta un paquete almacenado en el **Almacén de paquetes SSIS**.  
  
 **Paquete**  
 Escriba el nombre del paquete, o bien haga clic en el botón de puntos suspensivos **(…)** para buscar un paquete mediante el cuadro de diálogo **Seleccionar un paquete SSIS**.  
  
## <a name="configurations-page"></a>Página Configuraciones  
 Use la página **Configuraciones** del cuadro de diálogo **Utilidad de ejecución de paquetes** para seleccionar los archivos de configuración que se van a cargar en tiempo de ejecución y especificar el orden de carga.  
  
### <a name="options"></a>Opciones  
 **Archivos de configuración**  
 Muestra una lista de las configuraciones que utiliza el paquete. Cada uno de los archivos de configuración agrega una opción **/CONFIGFILE nombreDeArchivo** al símbolo del sistema.  
  
 **Teclas de dirección**  
 Seleccione un archivo de configuración de la lista y utilice las flechas de dirección situadas a la derecha para cambiar el orden de carga. Las configuraciones se cargan en orden comenzando desde la parte superior de la lista.  
  
> [!NOTE]  
>  Si varias configuraciones modifican la misma propiedad, se utiliza la configuración que se carga en último lugar.  
  
 **Add (Agregar)**  
 Haga clic para agregar configuraciones mediante el cuadro de diálogo **Abrir** . De manera predeterminada, el cuadro de diálogo muestra una lista de los archivos que tienen la extensión .dtsconfig.  
  
 **Remove**  
 Seleccione un archivo de configuración de la lista y, después, haga clic en **Quitar**.  
  
 **Ejecutar**  
 Haga clic para ejecutar el paquete.  
  
 **Cerrar**  
 Haga clic en esta opción para cerrar el cuadro de diálogo **Utilidad de ejecución de paquetes** .  
  
## <a name="command-files-page"></a>Página Archivos de comandos  
 Use la página **Archivos de comandos** del cuadro de diálogo **Utilidad de ejecución de paquetes** para seleccionar los archivos de comandos que quiere cargar en tiempo de ejecución.  
  
### <a name="options"></a>Opciones  
 **Archivos de comandos**  
 Muestra los archivos de comandos que utiliza el paquete. Un paquete puede utilizar varios archivos para establecer las opciones de línea de comandos.  
  
 **Teclas de dirección**  
 Seleccione un archivo de comandos de la lista y utilice las teclas de dirección de la derecha para cambiar el orden de carga. Los archivos de comandos se cargarán en orden, comenzando por la parte superior de la lista.  
  
 **Add (Agregar)**  
 Haga clic para agregar un archivo de comandos mediante el cuadro de diálogo **Abrir** .  
  
 **Remove**  
 Para quitar un archivo de comandos, selecciónelo en el cuadro de texto y haga clic en el botón **Quitar** .  
  
 **Ejecutar**  
 Haga clic para ejecutar el paquete.  
  
 **Cerrar**  
 Haga clic en esta opción para cerrar el cuadro de diálogo **Utilidad de ejecución de paquetes** .  
  
## <a name="connection-managers-page"></a>Página Administradores de conexión  
 Use la página **Administradores de conexiones** del cuadro de diálogo **Utilidad de ejecución de paquetes** para editar las cadenas de conexión de los administradores de conexión que el paquete usa.  
  
### <a name="options"></a>Opciones  
 **Connection Manager**  
 Active esta casilla para que la columna **Cadena de conexión** se pueda editar.  
  
 **Descripción**  
 Vea una descripción de cada administrador de conexiones. Las descripciones no se pueden editar.  
  
 **Cadena de conexión**  
 Edite la cadena de conexión para un administrador de conexiones. Este campo solamente se puede editar si la casilla **Administrador de conexiones** está activada.  
  
 **Ejecutar**  
 Haga clic para ejecutar el paquete.  
  
 **Cerrar**  
 Haga clic en esta opción para cerrar el cuadro de diálogo **Utilidad de ejecución de paquetes** .  
  
## <a name="execution-options-page"></a>Página Opciones de ejecución  
 Use la página **Opciones de ejecución** del cuadro de diálogo **Utilidad de ejecución de paquetes** para especificar opciones de tiempo de ejecución para el paquete.  
  
### <a name="options"></a>Opciones  
 **Rechazar el paquete cuando haya advertencias de validación**  
 Indique si debe rechazarse el paquete cuando se produce una advertencia de validación.  
  
 **Validar el paquete sin ejecutarlo**  
 Indique si el paquete solo debe validarse.  
  
 **Número máximo de ejecutables simultáneos**  
 Indica si desea especificar el número máximo de ejecutables que pueden ejecutarse en el paquete al mismo tiempo. Después de activar esta casilla, utilice el cuadro de número para especificar el número máximo de ejecutables.  
  
 **Habilitar puntos de comprobación de paquetes**  
 Indique si deben habilitarse los puntos de comprobación de los paquetes.  
  
 **Archivo de punto de comprobación**  
 Indique el archivo de punto de comprobación que utiliza el paquete si se han habilitado los puntos de comprobación de los paquetes.  
  
 **Browse**  
 Haga clic en el botón Examinar **(…)** para buscar el archivo de punto de control mediante el cuadro de diálogo **Abrir** si ha habilitado los puntos de control de los paquetes. Si ya se ha especificado un punto de comprobación, se sustituye por el archivo seleccionado.  
  
 **Omitir opciones de reinicio**  
 Indique si deben reemplazarse las opciones de reinicio si se habilitan los puntos de comprobación de los paquetes.  
  
 **Opción de reinicio**  
 Seleccione cómo deben utilizarse los puntos de comprobación si se reemplazan las opciones de reinicio.  
  
 **Ejecutar**  
 Haga clic para ejecutar el paquete.  
  
 **Cerrar**  
 Haga clic en esta opción para cerrar el cuadro de diálogo **Utilidad de ejecución de paquetes** .  
  
## <a name="reporting-page"></a>Página de informe  
 Use la página **Informes** del cuadro de diálogo **Utilidad de ejecución de paquetes** para especificar los eventos y la información sobre el paquete que se registrará en la consola cuando se ejecute el paquete.  
  
### <a name="options"></a>Opciones  
 **Eventos de consola**  
 Indica los eventos y los tipos de mensajes que se notificarán.  
  
 **None**  
 Seleccione esta opción para no obtener elaborar ningún informe.  
  
 **Errores**  
 Seleccione esta opción para notificar mensajes de error.  
  
 **Advertencias**  
 Seleccione esta opción para notificar mensajes de advertencia.  
  
 **Eventos personalizados**  
 Seleccione esta opción para notificar mensajes de eventos personalizados.  
  
 **Eventos de canalización**  
 Seleccione esta opción notificar mensajes de eventos de flujo de datos.  
  
 **Información**  
 Seleccione esta opción para notificar mensajes informativos.  
  
 **Detallado**  
 Seleccione esta opción para utilizar informes detallados.  
  
 **Registro de consolas**  
 Especifique la información que desea escribir en el registro cuando se produzca el evento seleccionado.  
  
 **Nombre**  
 Seleccione esta opción para notificar el nombre de la persona que creó el paquete.  
  
 **Equipo**  
 Seleccione esta opción para notificar el nombre del equipo donde se está ejecutando el paquete.  
  
 **Operador**  
 Seleccione esta opción para notificar el nombre de la persona que inició el paquete.  
  
 **Nombre de origen**  
 Seleccione esta opción para notificar el nombre del paquete.  
  
 **GUID de origen**  
 Seleccione esta opción para notificar el GUID del paquete.  
  
 **GUID de ejecución**  
 Seleccione esta opción para notificar el GUID de la instancia de ejecución del paquete.  
  
 **Message**  
 Seleccione esta opción para notificar mensajes.  
  
 **Hora de inicio y finalización**  
 Seleccione esta opción para notificar cuándo el paquete comienza y termina.  
  
 **Ejecutar**  
 Haga clic para ejecutar el paquete.  
  
 **Cerrar**  
 Haga clic en esta opción para cerrar el cuadro de diálogo **Utilidad de ejecución de paquetes** .  
  
## <a name="logging-page"></a>Página Registro  
 Use la página **Registro** del cuadro de diálogo **Utilidad de ejecución de paquetes** para que los proveedores de registro estén disponibles para el paquete en tiempo de ejecución. Seleccione el tipo de proveedor de registro del paquete y la cadena de conexión para la conexión al registro. Cada entrada de los proveedores de registro agrega una opción **/LOGGER**_classid_ al símbolo del sistema.  
  
### <a name="options"></a>Opciones  
 **Proveedor de registro**  
 Seleccione un proveedor de registro de la lista.  
  
 **Cadena de configuración**  
 Seleccione el nombre del administrador de conexiones del paquete que señala la ubicación del registro o escriba la cadena de conexión para la conexión al proveedor de registro.  
  
 **Remove**  
 Seleccione un proveedor de registro y haga clic para quitarlo.  
  
 **Ejecutar**  
 Haga clic para ejecutar el paquete.  
  
 **Cerrar**  
 Haga clic en esta opción para cerrar el cuadro de diálogo **Utilidad de ejecución de paquetes** .  
  
## <a name="set-values-page"></a>Página Valores establecidos  
 Use la página **Valores establecidos** del cuadro de diálogo **Utilidad de ejecución de paquetes** para establecer los valores de las propiedades de paquetes, ejecutables, conexiones, variables y proveedores de registro escribiendo las rutas de acceso de las propiedades y los valores de las propiedades. Cada entrada de ruta agrega una opción **/SET**_propertypath;value_ al símbolo del sistema.  
  
### <a name="options"></a>Opciones  
 **Ruta de acceso de la propiedad**  
 Escriba la ruta de acceso de la propiedad. En la sintaxis de la ruta se usa una barra diagonal inversa (\\) para indicar que el elemento siguiente es un contenedor, un punto (.) para indicar que el elemento siguiente es una propiedad y corchetes para indicar que es un miembro de una colección. Es posible identificar al miembro por su índice o nombre. Por ejemplo, la ruta de acceso de la propiedad de una variable de un paquete es \Package.Variables[MyVariable].Value.  
  
 **Valor**  
 Escriba el valor de la propiedad.  
  
 **Remove**  
 Seleccione una ruta de acceso a una propiedad y haga clic para quitarla.  
  
 **Ejecutar**  
 Haga clic para ejecutar el paquete.  
  
 **Cerrar**  
 Haga clic en esta opción para cerrar el cuadro de diálogo **Utilidad de ejecución de paquetes** .  
  
## <a name="verification-page"></a>Página Comprobación  
 Use la página **Comprobación** del cuadro de diálogo **Ejecutar paquete** para establecer los criterios para comprobar el paquete.  
  
### <a name="options"></a>Opciones  
 **Ejecutar solo los paquetes firmados**  
 Seleccione esta opción para ejecutar solamente los paquetes que se han firmado.  
  
 **Comprobar la generación del paquete**  
 Seleccione esta opción para comprobar la generación del paquete.  
  
 Build  
 Especifique el número secuencial de versión asociado a la compilación.  
  
 **Comprobar el Id. del paquete**  
 Seleccione esta opción para comprobar el identificador del paquete.  
  
 Id. de paquete  
 Especifique el número de identificación del paquete.  
  
 **Comprobar el Id. de versión**  
 Seleccione esta opción para comprobar el identificador de la versión.  
  
 Id. de la versión  
 Especifique el número de identificación de la versión.  
  
 **Ejecutar**  
 Haga clic para ejecutar el paquete.  
  
 **Cerrar**  
 Haga clic en esta opción para cerrar el cuadro de diálogo **Utilidad de ejecución de paquetes** .  
  
## <a name="command-line-page"></a>Página Línea de comandos  
 Use el nodo **Línea de comandos** del cuadro de diálogo **Utilidad de ejecución de paquetes** para editar la línea de comandos generada con las opciones creadas en los diversos cuadros de diálogo.  
  
### <a name="options"></a>Opciones  
 **Restaurar las opciones originales**  
 Haga clic para restaurar el estado original de la línea de comandos. Use esta opción si ha realizado modificaciones con la opción **Editar la línea de comandos manualmente** y quiere restaurar las opciones originales de la línea de comandos.  
  
 **Editar la línea de comandos manualmente**  
 Haga clic para editar la línea de comandos en el cuadro de texto **Línea de comandos** .  
  
 **Línea de comandos**  
 Muestra la línea de comandos actual. Podrá editarla si ha seleccionado la opción para editar la línea de comandos manualmente.  
  
 **Ejecutar**  
 Haga clic para ejecutar el paquete.  
  
 **Cerrar**  
 Haga clic en esta opción para cerrar el cuadro de diálogo **Utilidad de ejecución de paquetes** .  
  
## <a name="see-also"></a>Consulte también  
 [dtexec (utilidad)](../../integration-services/packages/dtexec-utility.md)  
  
  
