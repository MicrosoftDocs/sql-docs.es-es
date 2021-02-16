---
title: Información general del Monitor de creación de reflejo de la base de datos | Microsoft Docs
description: Obtenga información sobre cómo usar el Monitor de creación de reflejos de bases de datos a fin de supervisar bases de datos reflejadas, comprobar el flujo de datos en la sesión de creación de reflejos de las bases de datos y solucionar problemas.
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.dbmmonitor.main.f1
helpviewer_keywords:
- Database Mirroring Monitor [SQL Server], interface
ms.assetid: 8ebbdcd6-565a-498f-b674-289c84b985eb
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 1bbe6002daa25cc29a91194abad022fce5c5ff92
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353125"
---
# <a name="database-mirroring-monitor-overview"></a>Información general del Monitor de creación de reflejo de la base de datos
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Si dispone de los permisos correctos, puede utilizar el Monitor de creación de reflejo de la base de datos para supervisar cualquier subconjunto de las bases de datos reflejadas de una instancia del servidor. La supervisión le permite comprobar si los datos fluyen en la sesión de creación de reflejo de la base de datos. Si hay flujo de datos, supervisa la calidad del mismo. Asimismo, el Monitor de creación de reflejo de la base de datos resulta útil para solucionar la causa de un flujo de datos reducido.  
  
 Puede registrar cualquiera de las bases de datos reflejadas para supervisar cada uno de los asociados de conmutación por error manualmente. Al registrar una base de datos, el Monitor de creación de reflejo de la base de datos almacena en caché la siguiente información acerca de dicha base de datos:  
  
-   Nombre de la base de datos  
  
-   Nombres de las dos instancias de servidor asociado  
  
-   Últimos roles conocidos de cada asociado (entidad de seguridad o reflejado)  
  
## <a name="permissions"></a>Permisos  
 Para supervisar la creación de reflejo de la base de datos, debe ser miembro del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **dbm_monitor** en la base de datos **msdb** de la instancia del servidor. Si solo es miembro de **sysadmin** o **dbm_monitor** en una de las instancias del servidor asociado, el monitor únicamente puede conectarse a dicho asociado; no puede recuperar información del otro asociado.  
  
 Si solo es miembro de **dbm_monitor** en una instancia del servidor, tendrá permisos limitados en esa instancia. Únicamente podrá ver la fila de estado más reciente. Si se conecta a una instancia del servidor mediante los permisos **dbm_monitor** , el Monitor de creación de reflejo de la base de datos le informa de que tiene permisos limitados.  
  
> [!IMPORTANT]  
>  El rol fijo de base de datos **dbm_monitor** se crea en la base de datos **msdb** cuando se registra la primera base de datos en el Monitor de creación de reflejo de la base de datos. El nuevo rol **dbm_monitor** no tiene miembros hasta que un administrador del sistema asigne usuarios al rol.  
  
## <a name="navigation-tree"></a>Árbol de navegación  
 Si las bases de datos se han registrado para supervisión con el Monitor de creación de reflejo de la base de datos, se mostrará una lista de bases de datos registradas en el árbol de navegación. El árbol se actualiza automáticamente cada 30 segundos. Para ver el estado de una base de datos registrada, selecciónela. Para obtener más información, vea "Panel de detalles" más adelante en este tema.  
  
 En cada base de datos registrada, se muestra la siguiente información:  
  
 _<Database_name>_ **(** _\<Status>_ **,** _<PRINCIPAL_SERVER>_ **->** _<MIRROR_SERVER>_ **)**  
  
 *<Database_name>*  
 Nombre de una base de datos reflejada que se registra con el Monitor de creación de reflejo de la base de datos.  
  
 *\<Status>*  
 Los estados posibles y sus iconos asociados son los siguientes:  
  
|Icono|Estado|Descripción|  
|----------|------------|-----------------|  
|Icono de advertencia|**Unknown**|El monitor no está conectado a ningún asociado. La única información disponible es lo que el monitor almacena en caché.|  
|Icono de advertencia|**Sincronizando**|El contenido de la base de datos reflejada está atrasado con respecto al contenido de la base de datos principal. La instancia de servidor principal envía las entradas de registro a la instancia del servidor reflejado, que aplica los cambios en la base de datos reflejada para confirmarla.<br /><br /> Al inicio de una sesión de creación de reflejo de la base de datos, las bases de datos principal y reflejada se encuentran en este estado.|  
|Cilindro de base de datos estándar|**Sincronizada**|El estado de la base de datos cambia a **Sincronizado** cuando el servidor reflejado está suficientemente al día con respecto al servidor principal. La base de datos permanece en este estado mientras el servidor principal continúa con el envío de cambios al servidor reflejado, y el servidor reflejado continúa con la aplicación de los cambios en la base de datos reflejada.<br /><br /> En el modo de seguridad alta, son posibles las conmutaciones manual y automática por error sin pérdida de datos.<br /><br /> En el modo de rendimiento alto, siempre es posible que se pierdan datos, incluso en el estado **Sincronizado** .|  
|Icono de advertencia|**Suspendido**|La base de datos principal está disponible, pero no envía ningún registro al servidor reflejado.|  
|Icono de error|**Desconectada**|La instancia del servidor no se puede conectar a su asociado.|  
  
 *<PRINCIPAL_SERVER>*  
 Nombre del asociado que es actualmente la instancia del servidor principal. El nombre adopta el siguiente formato:  
  
 *<SYSTEM_NAME>* [ **\\** _<instance_name>_ ]  
  
 donde *<SYSTEM_NAME>* es el nombre del sistema en el que se encuentra la instancia del servidor. En una instancia del servidor no predeterminada, también se muestra el nombre de la instancia: _<SYSTEM_NAME>_ **\\** _<nombre_instancia>_ .  
  
 *<MIRROR_SERVER>*  
 Nombre del asociado que es actualmente la instancia del servidor reflejado. El formato es el mismo que el del servidor principal.  
  
## <a name="detail-pane"></a>Panel de detalles  
 El aspecto del monitor depende de si está seleccionada una base de datos. Al abrir el monitor, el panel de detalles muestra un vínculo **Registrar base de datos reflejada** . Haga clic en el vínculo para registrar una base de datos. Las bases de datos registradas aparecen debajo del nodo **Monitor de creación de reflejo de la base de datos** del árbol de navegación. El Monitor de creación de reflejo de la base de datos intenta conectarse a todas las instancias de servidor para las que haya almacenado credenciales.  
  
 Cuando seleccione una base de datos, su estado se mostrará en la página con pestañas **Estado** del panel de detalles. El contenido de esta página procede de las instancias de servidor principal y reflejado. La página se actualiza de forma asincrónica, a medida que se recopila la información del estado a través de conexiones independientes en las instancias de servidor principal y reflejado. El estado se actualiza automáticamente a intervalos de 30 segundos.  
  
> [!NOTE]  
>  No puede cambiar la frecuencia de actualización del monitor, pero sí puede actualizar la tabla de estado del cuadro de diálogo **Historial de creación de reflejo de la base de datos** .  
  
 Un administrador del sistema puede ver la configuración actual de las advertencias para la base de datos; para ello, debe seleccionar la página con pestañas **Advertencias** . Desde dicha página, el administrador puede iniciar el cuadro de diálogo **Establecer umbrales de advertencia** para habilitar y configurar uno o varios umbrales de advertencia.  
  
 En el titular situado encima de las pestañas, el panel de detalles muestra la última hora a la que el monitor ha actualizado la información de estado, como **Última actualización:** _\<date>\<time>_ . Normalmente, el Monitor de creación de reflejo de la base de datos recupera información de estado de las instancias del servidor principal y reflejado a horas diferentes. Se muestran las dos horas de actualización más antiguas.  
  
## <a name="action-menu"></a>Menú Acción  
 El menú **Acción** siempre contiene los siguientes comandos:  
  
|Get-Help|Descripción|  
|-------------|-----------------|  
|**Registrar base de datos reflejada...**|Abre el cuadro de diálogo **Registrar base de datos reflejada** . Utilice este cuadro de diálogo para registrar una o varias bases de datos reflejadas en una instancia del servidor determinada, mediante la adición de las bases de datos al Monitor de creación de reflejo de la base de datos. Cuando se agrega una base de datos, el Monitor de creación de reflejo de la base de datos almacena localmente en caché información acerca de la base de datos, sus asociados y el método de conexión a éstos.|  
|**Administrar conexiones de instancia del servidor...**|Si selecciona este comando, se abre el cuadro de diálogo **Administrar conexiones de instancia del servidor** . En dicho diálogo, puede elegir una instancia del servidor para la que desee especificar credenciales del monitor, que usará al conectarse a un asociado determinado.<br /><br /> Para editar credenciales para un asociado, localice su entrada en la cuadrícula **Instancias del servidor** y haga clic en **Editar** en la fila. Se abrirá el cuadro de diálogo **Conectar al servidor** para ese nombre de instancia del servidor, con los controles de credencial inicializados en el valor actual almacenado en caché. Cambie la información de autenticación según corresponda y haga clic en **Conectar**. Si las credenciales tienen privilegios suficientes, la columna **Conectar utilizando** se actualizará con las nuevas credenciales.|  
  
 Si selecciona una base de datos, el menú **Acción** también contiene los siguientes comandos.  
  
|Get-Help|Descripción|  
|-------------|-----------------|  
|**Eliminar esta base de datos del Registro**|Quita la base de datos seleccionada del Monitor de creación de reflejo de la base de datos.|  
|**Establecer umbrales de advertencia...**|Abre el cuadro de diálogo **Establecer umbrales de advertencia** . En dicho diálogo, un administrador del sistema puede habilitar o deshabilitar las advertencias para la base de datos en cada uno de los asociados y cambiar el umbral de las advertencias. Es recomendable establecer un umbral para una advertencia específica en los dos asociados, con lo que se puede garantizar que la advertencia persiste si se produce una conmutación por error en la base de datos. El umbral adecuado para cada asociado depende de la capacidad de rendimiento del sistema de dicho asociado.<br /><br /> Un evento solo se escribe en el registro de eventos para un rendimiento si su valor se encuentra en el umbral, o por encima de éste, cuando se actualiza la tabla de estado. Si un valor máximo alcanza el umbral momentáneamente entre las actualizaciones de estado, se pierde dicho máximo.|  
  
 **Para supervisar la creación de reflejo de la base de datos mediante SQL Server Management Studio**  
  
-   [Iniciar el Monitor de creación de reflejo de la base de datos &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
## <a name="see-also"></a>Consulte también  
 [Supervisar la creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [Iniciar el Asistente para la configuración de seguridad de la creación de reflejo de la base de datos &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
  
