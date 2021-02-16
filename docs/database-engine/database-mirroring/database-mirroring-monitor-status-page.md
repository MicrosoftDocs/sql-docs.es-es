---
title: Monitor de creación de reflejo de la base de datos (página Estado) | Microsoft Docs
description: Use esta página de Monitor de creación de reflejo de la base de datos de solo lectura para ver el estado de la creación de reflejo de las instancias del servidor principal y reflejado de una base de datos seleccionada.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.dbmmonitor.status.f1
ms.assetid: 4f64b4e1-89e9-4827-98fa-b92c3dc73b48
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 2d912623497cc6dc0baac1c4fbb4c8e794363ce8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353114"
---
# <a name="database-mirroring-monitor-status-page"></a>Monitor de creación de reflejo de la base de datos (página Estado)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En esta página de solo lectura se muestra el estado más reciente de la creación de reflejo de las instancias del servidor principal y reflejado seleccionadas actualmente en el árbol de navegación. Si la información sobre una instancia no se encuentra disponible, algunas celdas de la cuadrícula **Estado** correspondientes a dicha instancia aparecerán atenuadas y mostrarán **Desconocido**.  
  
 **Para utilizar SQL Server Management Studio a fin de supervisar la creación de reflejo de la base de datos**  
  
-   [Iniciar el Monitor de creación de reflejo de la base de datos &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
## <a name="options"></a>Opciones  
 **Estado**  
 Muestra una cuadrícula que contiene el estado de creación de reflejo de alto nivel más reciente de cada una de las instancias del servidor principal y reflejado. El orden de las filas de la cuadrícula **Estado** es el siguiente:  
  
-   Instancia de servidor principal  
  
-   Instancia del servidor reflejado  
  
 Estas son sus columnas:  
  
|Nombre de la columna|Descripción|  
|-----------------|-----------------|  
|**Instancia del servidor**|Nombre de la instancia de servidor cuyo estado se muestra en la fila **Estado** .|  
|**Rol actual**|Rol actual de la instancia del servidor, ya sea **Principal** o **Reflejado**.|  
|**Estado de creación de reflejo**|El estado de creación de reflejo que proporciona la instancia del servidor y un icono que indica la gravedad del estado. Los estados posibles y sus iconos asociados son los siguientes:<br /><br /> Icono: -, estado **Desconocido**. El monitor no está conectado a ningún asociado. La única información disponible es lo que el monitor almacena en caché.<br /><br /> Icono: icono de advertencia, estado **Sincronizando**. El contenido de la base de datos reflejada está atrasado con respecto al contenido de la base de datos principal. La instancia de servidor principal envía las entradas de registro a la instancia del servidor reflejado, que aplica los cambios en la base de datos reflejada para confirmarla. Al inicio de una sesión de creación de reflejo de la base de datos, las bases de datos principal y reflejada se encuentran en este estado.<br /><br /> Icono: cilindro de base de datos estándar, estado **Sincronizado**. El estado de la base de datos cambia a **Sincronizado** cuando el servidor reflejado está suficientemente al día con respecto al servidor principal. La base de datos permanece en este estado mientras el servidor principal envía cambios al servidor reflejado, y el servidor reflejado aplica los cambios en la base de datos reflejada.  En el modo de seguridad alta, son posibles las conmutaciones manual y automática por error sin pérdida de datos.  En el modo de rendimiento alto, siempre es posible que se pierdan datos, incluso en el estado **Sincronizado** .<br /><br /> Icono: icono de advertencia, estado **Suspendido**. <br />                            La base de datos principal está disponible, pero no envía ningún registro al servidor reflejado.<br /><br /> Icono: icono de error, estado **Desconectado**. La instancia del servidor no se puede conectar a su asociado.|  
|**Conexión del testigo**|El estado de la conexión del testigo, precedida de un icono de estado: **Desconocido**, **Conectado** o **Desconectado**.|  
|**Historial**|Haga clic en este botón para mostrar el historial de creación de reflejo en la instancia del servidor. Se abrirá el cuadro de diálogo **Historial de creación de reflejo de la base de datos** , que muestra el historial del estado de creación de reflejo y las estadísticas de una base de datos reflejada en una instancia del servidor determinada.<br /><br /> El botón **Historial** se atenúa si el monitor no está conectado a la instancia del servidor.|  
  
 **Registro del servidor principal (** *\<time>* **)**  
 Estado del registro en la instancia del servidor principal según la hora local de la instancia del servidor, indicado mediante *\<time>* . Aparecen los siguientes parámetros:  
  
 **Registro sin enviar**  
 Cantidad de registro a la espera en la cola de envío (expresada en kilobytes).  
  
 **Transacción sin enviar más antigua**  
 Antigüedad de la transacción sin enviar más antigua de la cola de envío. La antigüedad de esta transacción indica la cantidad de minutos de transacciones que no se han enviado aún a la instancia del servidor reflejado. Este valor ayuda a medir la posibilidad de pérdida de datos con respecto a la hora.  
  
 **Hora de envío de registro (estimada)**  
 Cantidad aproximada de tiempo que la instancia del servidor principal necesita para enviar el registro (actualmente en la cola de envío) a la instancia del servidor reflejado ( *tasa de envío*). Puesto que la velocidad de las transacciones entrantes puede variar de forma significativa, el registro de la hora de envío es estimativo. Sin embargo, la tasa de envío puede resultar útil para estimar de forma aproximativa el tiempo requerido para una conmutación por error manual.  
  
 **Tasa actual de envío**  
 Velocidad con la que las transacciones se envían a la instancia del servidor reflejado, expresada en kilobytes (KB) por segundo.  
  
 **Tasa actual de nuevas transacciones**  
 Tasa a la que se escriben las transacciones entrantes en el registro del servidor principal en KB por segundo. Para determinar si la creación de reflejo se retrasa, se mantiene o mejora, compare este valor con **Hora de envío de registro (estimada)** .  
  
 **Registro del servidor reflejado (** *\<time>* **)**  
 Estado del registro en la instancia del servidor reflejado según la hora local de la instancia del servidor, indicado mediante *\<time>* . Aparecen los siguientes parámetros:  
  
 **Registro sin restaurar**  
 Cantidad de registro a la espera en la cola rehecha (expresada en KB).  
  
 **Tiempo para restaurar registro (estimado)**  
 Número aproximado de minutos necesarios para que el registro que se encuentra actualmente en la cola de puesta al día se aplique a la base de datos reflejada.  
  
 **Tasa actual de restauración**  
 Tasa a la que se restauran las transacciones en la base de datos reflejada (en KB por segundo).  
  
 **Sobrecarga de confirmación del servidor reflejado**  
 Número de milisegundos de retraso medio por transacción tolerado antes de que se genere una advertencia en el servidor principal. Este retardo es la cantidad de sobrecarga en la que se incurre mientras la instancia del servidor principal espera a la instancia del servidor reflejado para escribir la entrada de registro de la transacción en la cola de puesta al día. Este valor solo es relevante en el modo de alta seguridad.  
  
 **Hora de envío y restauración de todos los registros actuales (estimada)**  
 Tiempo necesario para enviar y restaurar todos los registros confirmados en el servidor principal según la hora actual. El tiempo puede ser inferior a la suma de los valores de los campos **Hora de envío de registro (estimada)** y **Tiempo para restaurar registro (estimado)** , ya que el envío y la restauración pueden producirse de forma paralela. Esta estimación no predice el tiempo necesario para enviar y restaurar nuevas transacciones confirmadas en el servidor principal mientras se tratan los registros en la cola de envío.  
  
 **Dirección del testigo**  
 Dirección de red de la instancia de servidor testigo. Para obtener más información sobre el formato de esta dirección, vea [Especificar una dirección de red de servidor &#40;creación de reflejo de la base de datos&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).  
  
 **Modo de funcionamiento**  
 El modo de funcionamiento de la sesión de creación de reflejo de la base de datos:  
  
-   **Rendimiento alto (asincrónico)**  
  
-   **Seguridad alta sin conmutación automática por error (sincrónico)**  
  
-   **Seguridad alta con conmutación automática por error (sincrónico)**  
  
## <a name="remarks"></a>Observaciones  
 Los miembros del rol fijo de base de datos **dbm_monitor** pueden ver el estado actual de la creación de reflejo mediante el Monitor de creación de reflejo de la base de datos o el procedimiento almacenado **sp_dbmmonitorresults** . No obstante, estos usuarios no pueden actualizar la tabla de estado. Dependen del **Trabajo del Monitor de creación de reflejo de la base de datos** para actualizar la tabla de estado a intervalos regulares. Para conocer la antigüedad del estado mostrado, el usuario puede ver las horas en las etiquetas **Registro del servidor principal (** _\<time>_ **)** y **Registro del servidor reflejado (** _\<time>_ **)** .  
  
 Si este trabajo no existe o el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se detiene, el estado quedará cada vez más desusado y puede que deje de reflejar la configuración de la sesión de creación de reflejo. Por ejemplo, después de una conmutación por error, es posible que parezca que los asociados comparten el mismo rol (de servidor principal o reflejado) o que el servidor principal actual se muestre como reflejado, a la vez que el servidor reflejado actual se muestra como principal.  
  
## <a name="see-also"></a>Consulte también  
 [Iniciar el Monitor de creación de reflejo de la base de datos &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)   
 [Supervisar la creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [Iniciar el Asistente para la configuración de seguridad de la creación de reflejo de la base de datos &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
  
