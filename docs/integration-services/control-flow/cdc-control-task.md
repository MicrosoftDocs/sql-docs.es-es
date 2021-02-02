---
description: Tarea Control CDC
title: Tarea Control CDC | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.designer.cdccontroltask.f1
- sql13.ssis.designer.cdccontroltask.config.f1
ms.assetid: 6404dc7f-550c-47cc-b901-c072742f430a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 492a0cd4b510b91d1f5f17ad04de54b005502bba
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049120"
---
# <a name="cdc-control-task"></a>Tarea Control CDC

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La tarea Control CDC se usa para controlar el ciclo de vida de los paquetes de captura de datos modificados (CDC). Controla la sincronización de paquetes CDC con el paquete de carga inicial, la administración de los intervalos de número de secuencia de registro (LSN) que se procesan en una ejecución de un paquete CDC. Además, la tarea Control CDC se ocupa de los escenarios de error y de la recuperación.  
  
 La tarea Control CDC mantiene el estado del paquete CDC en una variable de paquete SSIS y también puede conservarlo en una tabla de base de datos para mantener el estado a través de las activaciones de paquete y entre varios paquetes que realizan conjuntamente un proceso CDC común (por ejemplo, una tarea puede ser responsable de la carga inicial y otro de las actualizaciones de fuente de generación).  
  
 La tarea Control CDC admite dos grupos de operaciones. Un grupo controla la sincronización del procesamiento de los cambios y la carga inicial y el otro administra el intervalo de procesamiento de los cambios de los LSN para una ejecución de un paquete CDC y realiza el seguimiento de lo que se procesó correctamente.  
  
 Las siguientes operaciones controlan la sincronización de la carga inicial y el procesamiento de cambios:  
  
|Operación|Descripción|  
|---------------|-----------------|  
|ResetCdcState|Esta operación se usa para restablecer el estado CDC persistente asociado al contexto CDC actual. Después de ejecutar esta operación, el LSN máximo actual de la tabla de `sys.fn_cdc_get_max_lsn` de la marca de tiempo de LSN se convierte en el inicio del intervalo para el siguiente intervalo de procesamiento. Esta operación requiere una conexión a la base de datos de origen.|  
|MarkInitialLoadStart|Esta operación se usa al comienzo de un paquete de carga inicial para registrar el LSN actual en la base de datos de origen antes de que el paquete de carga inicial comience a leer las tablas de origen. Esto requiere una conexión a la base de datos de origen para llamar a `sys.fn_cdc_get_max_lsn`.<br /><br /> Si selecciona MarkInitialLoadStart cuando trabaja en CDC de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] (es decir, no en Oracle), el usuario especificado en el administrador de conexiones debe ser db_owner o sysadmin.|  
|MarkInitialLoadEnd|Esta operación se usa al final de un paquete de carga inicial para registrar el LSN actual en la base de datos de origen después de que el paquete de carga inicial termine de leer las tablas de origen. Este LSN se determina mediante el registro de la hora en el momento en que se produjo esta operación y, posteriormente, mediante consulta a la tabla de asignación de `cdc.lsn_time_`de la base de datos CDC en busca de un cambio que hubiera tenido lugar después de dicha hora.<br /><br /> Si selecciona MarkInitialLoadEnd cuando trabaja en CDC de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] (es decir, no en Oracle), el usuario especificado en el administrador de conexiones debe ser db_owner o sysadmin.|  
|MarkCdcStart|Esta operación se usa cuando la carga inicial se realiza desde una base de datos de instantáneas. En este caso, el procesamiento de cambios debe iniciarse inmediatamente después del LSN de la instantánea. Puede especificar el nombre de la base de datos de instantáneas que se usará y la tarea Control CDC consulta a [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] el LSN de la instantánea. También tiene la opción de especificar directamente el LSN de la instantánea<br /><br /> Si selecciona MarkCdcStart cuando trabaja en CDC de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] (es decir, no en Oracle), el usuario especificado en el administrador de conexiones debe ser db_owner o sysadmin.|  
  
 Las operaciones siguientes se usan para administrar el intervalo de procesamiento:  
  
|Operación|Descripción|  
|---------------|-----------------|  
|GetProcessingRange|Esta operación se usa antes de invocar el flujo de datos que usa el flujo de datos origen de CDC. Establece un intervalo de LSN que lee el flujo de datos de origen de CDC cuando se invoca. El intervalo se almacena en una variable de paquete SSIS que usa el origen de CDC durante el procesamiento del flujo de datos.<br /><br /> Para obtener más información sobre los estados que se almacenan, vea [Definir una variable de estado](../../integration-services/data-flow/define-a-state-variable.md).|  
|MarkProcessedRange|: esta operación se ejecuta después de cada ejecución de CDC (después de que el flujo de datos de CDC se completa correctamente) para registrar el último LSN que se ha procesado totalmente en la ejecución de CDC. La próxima vez que se ejecute GetProcessingRange, esta posición será el inicio del intervalo de procesamiento.|  
  
## <a name="handling-cdc-state-persistency"></a>Controlar la persistencia de estado CDC  
 La tarea Control CDC mantiene un estado persistente entre las activaciones. La información almacenada en el estado CDC se usa para determinar y mantener el intervalo de procesamiento del paquete CDC y para detectar condiciones de error. El estado persistente se almacena como una cadena. Para obtener más información, consulte [Definir una variable de estado](../../integration-services/data-flow/define-a-state-variable.md).  
  
 La tarea Control CDC admite dos tipos de persistencia de estado  
  
-   Persistencia de estado manual: en este caso, la tarea Control CDC administra el estado almacenado en una variable de paquete pero el desarrollador del paquete debe leer la variable desde un almacén persistente antes de llamar al Control CDC y después escribirla de nuevo en el almacén persistente cuando el Control CDC se llame por última vez y la ejecución del CDC se complete.  
  
-   Persistencia de estado automática: el estado del CDC se almacena en una tabla de una base de datos. El estado se almacena con un nombre proporcionado en la propiedad **StateName** en una tabla denominada en la propiedad **Tabla para Storing State** , que se encuentra en un administrador de conexiones seleccionado para almacenar el estado. El valor predeterminado es el administrador de conexiones de origen pero la práctica común es que sea el administrador de conexiones de destino. La tarea Control CDC actualiza el valor de estado en la tabla de estados y esta se confirma como parte de la transacción ambiente.  
  
## <a name="error-handling"></a>Tratamiento de errores  
 La tarea Control CDC puede notificar un error cuando:  
  
-   No se puede leer el estado CDC persistente o cuando la actualización del estado persistente da error.  
  
-   No puede leer la información del LSN actual desde la base de datos de origen.  
  
-   Lectura del estado CDC no es coherente.  
  
 En todos estos casos, la tarea Control CDC notifica un error que se puede controlar igual que SSIS controla los errores de flujo de control.  
  
 La tarea Control CDC también puede notificar una advertencia cuando la operación del intervalo de procesamiento Get se invoca directamente después de otra operación de intervalo de procesamiento Get sin llamar a Marcar intervalo de procesamiento. Esta es una indicación de que la ejecución anterior dio error o de que otro paquete CDC puede estar ejecutándose con el mismo nombre del estado CDC.  
  
## <a name="configuring-the-cdc-control-task"></a>Configurar la tarea Control CDC  
 Puede establecer propiedades a través del Diseñador de SSIS o mediante programación.  
  
## <a name="in-this-section"></a>En esta sección  
  
-   [Propiedades personalizadas de la tarea de control CDC](../../integration-services/control-flow/cdc-control-task-custom-properties.md)  
  
## <a name="related-tasks"></a>Related Tasks  
 [Definir una variable de estado](../../integration-services/data-flow/define-a-state-variable.md)  
  
## <a name="related-content"></a>Contenido relacionado  
  
-   Artículo técnico [Instalar la captura de datos modificados Microsoft SQL Server 2012 para Oracle de Attunity](https://go.microsoft.com/fwlink/?LinkId=252958), en social.technet.microsoft.com.  
  
-   Artículo técnico [Solucionar problemas de configuración de la captura de datos modificados de Microsoft para Oracle de Attunity](https://go.microsoft.com/fwlink/?LinkId=252960), en social.technet.microsoft.com.  
  
-   Artículo técnico [Solucionar problemas de errores de instancias de CDC de la captura de datos modificados de Microsoft para Oracle de Attunity](https://go.microsoft.com/fwlink/?LinkId=252961), en social.technet.microsoft.com.  
  
## <a name="cdc-control-task-editor"></a>Editor de la tarea Control de CDC
  Use el cuadro de diálogo **Editor de la tarea Control CDC** para configurar la tarea Control CDC. La configuración de la tarea Control CDC incluye la definición de una conexión a la base de datos CDC, la operación de tarea CDC y la información sobre la administración de estados.  
  
 Para obtener más información acerca de la tarea Control CDC, vea [CDC Control Task](../../integration-services/control-flow/cdc-control-task.md).  
  
 **Para abrir el Editor de la tarea Control CDC**  
  
1.  En [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], abra el paquete de [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] que tiene la tarea Control CDC.  
  
2.  En la pestaña **Flujo de control** , haga doble clic en la tarea Control CDC.  
  
### <a name="options"></a>Opciones  
 **Administrador de conexiones de ADO.NET para la base de datos CDC de SQL Server**  
 Seleccione un administrador de conexiones existente de la lista o haga clic en **Nueva** para crear una nueva conexión. Es preciso realizar la conexión a una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] habilitada para CDC y donde se encuentre la tabla de cambios seleccionada.  
  
 **Operación de Control CDC**  
 Seleccione la operación que vaya a ejecutar para esta tarea. Todas las operaciones usan la variable de estado que se almacena en una variable de paquete SSIS donde se almacena el estado y lo pasa entre los distintos componentes del paquete.  
  
-   **Marcar comienzo de carga inicial**: esta operación se usa cuando se ejecuta una carga inicial desde una base de datos activa sin una instantánea. Se invoca al comienzo de un paquete de carga inicial para registrar el LSN actual en la base de datos de origen antes de que el paquete de carga inicial comience a leer las tablas de origen. Esto requiere una conexión a la base de datos de origen.  
  
     Si selecciona **Mark Initial Load Start** (Marcar comienzo de carga inicial) cuando trabaja en CDC de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] (es decir, no en Oracle), el usuario especificado en el administrador de conexiones debe ser  **db_owner** o **sysadmin**.  
  
-   **Marcar final de carga inicial**: esta operación se usa al ejecutar una carga inicial desde una base de datos activa sin una instantánea. Se invoca al final de un paquete de carga inicial para registrar el LSN actual en la base de datos de origen después de que el paquete de carga inicial termine de leer las tablas de origen. Este LSN se determina mediante el registro de la hora en el momento en que se produjo esta operación y, posteriormente, mediante consulta a la tabla de asignación de `cdc.lsn_time_`de la base de datos CDC en busca de un cambio que hubiera tenido lugar después de dicha hora.  
  
     Si selecciona **Mark Initial Load End** (Marcar final de carga inicial) cuando trabaja en CDC de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] (es decir, no en Oracle), el usuario especificado en el administrador de conexiones debe ser  **db_owner** o **sysadmin**.  
  
-   **Marcar comienzo de CDC**: esta operación se utiliza cuando la carga inicial se realiza desde una base de datos de base de datos de instantánea o desde una base de datos de inactividad. Se invoca en cualquier punto del paquete de carga inicial. La operación acepta un parámetro que puede ser un LSN de instantánea, un nombre de una base de datos de instantánea (desde la que el LSN de instantánea se deriva automáticamente) o se puede dejar vacío, en cuyo caso el LSN de la base de datos actual se usa como el LSN de inicio para el paquete de procesamiento de cambios.  
  
     Esta operación se usa en lugar de las operaciones Marcar comienzo/final de carga inicial.  
  
     Si selecciona **Mark CDC Start** (Marcar comienzo de CDC) cuando trabaja en CDC de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] (es decir, no en Oracle), el usuario especificado en el administrador de conexiones debe ser  **db_owner** o **sysadmin**.  
  
-   **Obtener intervalo de procesamiento**: esta operación se usa en un paquete de procesamiento de cambios antes de invocar el flujo de datos que usa el flujo de datos de origen de CDC. Establece un intervalo de LSN que lee el flujo de datos de origen de CDC cuando se invoca. El intervalo se almacena en una variable de paquete SSIS que usa el origen de CDC durante el procesamiento del flujo de datos.  
  
     Para obtener más información sobre los posibles estados CDC que se almacenan, vea [Definir una variable de estado](../../integration-services/data-flow/define-a-state-variable.md).  
  
-   **Marcar intervalo procesado**: esta operación se usa en un paquete de procesamiento de cambios al final de una ejecución CDC (después de que el flujo de datos CDC se complete correctamente) para registrar el último LSN que se haya procesado totalmente en la ejecución CDC. La próxima vez que se ejecute `GetProcessingRange` , esta posición determinará el inicio del siguiente intervalo de procesamiento.  
  
-   **Restablecer estado CDC**: esta operación se usa para restablecer el estado CDC persistente asociado al contexto CDC actual. Después de ejecutar esta operación, el LSN máximo actual de la tabla de `sys.fn_cdc_get_max_lsn` de la marca de tiempo de LSN se convierte en el inicio del intervalo para el siguiente intervalo de procesamiento. Esta operación requiere una conexión a la base de datos de origen.  
  
     Un ejemplo de uso de esta operación es cuando se desea procesar solo los registros de cambios creados recientemente y omitir todos los registros de cambios anteriores.  
  
 **Variable que contiene el estado CDC**  
 Seleccione la variable del paquete SSIS que almacena la información sobre el estado de la operación de la tarea. Debe definir una variable antes de empezar. Si selecciona **Persistencia de estado automática**, la variable de estado se carga y guarda automáticamente.  
  
 Para obtener más información sobre cómo se define la variable de estado, vea [Definir una variable de estado](../../integration-services/data-flow/define-a-state-variable.md).  
  
 **LSN de SQL Server para iniciar el nombre de CDC/instantánea:**  
 Especifique el LSN de la base de datos de origen actual o el nombre de la base de datos de instantánea de la carga inicial desde donde se realizará la carga inicial con el fin de determinar dónde se inicia CDC. Esto solo está disponible si se establece **Operación de Control CDC** en **Marcar comienzo de CDC**.  
  
 Para obtener más información acerca de estas operaciones, vea [CDC Control Task](../../integration-services/control-flow/cdc-control-task.md).  
  
 **Almacenar automáticamente el estado en una tabla de base de datos**  
 Active esta casilla para que la tarea Control CDC controle automáticamente la carga y el almacenamiento del estado de CDC en una tabla de estado que se encuentre en una base de datos especificada. Si no se ha activado, el desarrollador debe cargar el estado de CDC cuando el paquete se inicie y guardarlo cada vez que cambie el estado de CDC.  
  
 **Administrador de conexiones para la base de datos donde se almacena el estado**  
 Seleccione un administrador de conexiones de ADO.NET existente de la lista o haga clic en Nueva para crear una nueva conexión. Esta conexión es a una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que contenga la tabla de estado. La tabla de estado contiene la información sobre el estado.  
  
 Esto solo está disponible si se ha seleccionado **Persistencia de estado automática** y es un parámetro obligatorio.  
  
 **Tabla que se va a usar para almacenar el estado**  
 Escriba el nombre de la tabla de estado que se va a usar para almacenar el estado de CDC. La tabla especificada debe tener dos columnas denominadas **nombre** y **estado** y ambas columnas deben ser del tipo de datos **varchar (256)**.  
  
 Opcionalmente, puede seleccionar **Nueva** para obtener un script SQL que cree una nueva tabla de estado con las columnas necesarias. Cuando se selecciona **Persistencia de estado automática** , el desarrollador debe crear una tabla de estado según los requisitos descritos anteriormente.  
  
 Esto solo está disponible si se ha seleccionado **Persistencia de estado automática** y es un parámetro obligatorio.  
  
 **Nombre del estado**  
 Escriba un nombre para asociar al estado CDC persistente. Los paquetes de carga completa y CDC que funcionan con el mismo contexto CDC especificarán un nombre común para el estado. Este nombre se usa para buscar la fila de estado en la tabla de estado.  
  
