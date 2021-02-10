---
title: Capturar un seguimiento para las actualizaciones de SQL Server
description: Utilice Asistente para experimentación con bases de datos (DEA) para crear un archivo de seguimiento con un registro de eventos de servidor capturados.
ms.custom: seo-lt-2019
ms.date: 12/12/2019
ms.prod: sql
ms.prod_service: dea
ms.suite: sql
ms.technology: dea
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: pochiraju
ms.author: rajpo
ms.reviewer: mathoma
ms.openlocfilehash: 9bc1dafd5242dc4be8a9a2947465ed48a01fcb8d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042425"
---
# <a name="capture-a-trace-in-database-experimentation-assistant"></a>Capturar un seguimiento en Asistente para experimentación con bases de datos

Puede usar Asistente para experimentación con bases de datos (DEA) para crear un archivo de seguimiento con un registro de eventos de servidor capturados. Un evento de servidor capturado es un evento que se produce en un servidor específico durante un período de tiempo específico. Una captura de seguimiento se debe ejecutar una vez por servidor.

Antes de iniciar una captura de seguimiento, asegúrese de hacer una copia de seguridad de todas las bases de datos de destino.

El almacenamiento en caché de consultas en SQL Server puede afectar a los resultados de la evaluación. Se recomienda reiniciar el servicio de SQL Server (MSSQLSERVER) en la aplicación de servicios para mejorar la coherencia de los resultados de la evaluación.

## <a name="configure-a-trace-capture"></a>Configurar una captura de seguimiento

1. En DEA, en la barra de navegación de la izquierda, seleccione el icono de la cámara y, a continuación, en la página **todas las capturas** , seleccione **nueva captura**.

    ![Creación de una captura en DEA](./media/database-experimentation-assistant-capture-trace/dea-initiate-capture.png)

2. En la página **nueva captura** , en **detalles** de la captura, escriba o seleccione la siguiente información:

    - **Nombre** de la captura: escriba un nombre para el archivo de seguimiento de la captura.
    - **Formato**: especifique el formato (Trace o XEvents) para la captura.
    - **Duración**: seleccione el período de tiempo (en minutos) durante el que desea que se ejecute la captura de seguimiento.
    - **Ubicación** de la captura: seleccione la ruta de acceso de destino del archivo de seguimiento.

        > [!NOTE]
        > La ruta de acceso al archivo de seguimiento debe estar en el equipo que ejecuta SQL Server. Si el servicio SQL Server no está establecido para una cuenta específica, es posible que el servicio necesite permisos de escritura en la carpeta especificada para que se escriba el archivo de seguimiento.

3. Compruebe que ha realizado una copia de seguridad seleccionando la opción **sí, he realizado manualmente la copia de seguridad...** .

4. En **detalles** de la captura, escriba o seleccione la siguiente información:

    - **Tipo de servidor**: especifique el tipo de servidor SQL Server **(SQLServer**, **AzureSqlDb**, **AzureSqlManagedInstance**).
    - **Nombre del servidor**: especifique el nombre del servidor o la dirección IP de su SQL Server.
    - **Tipo de autenticación**: para el tipo de autenticación, seleccione **Windows**.
    - **Nombre** de la base de datos: escriba un nombre para la base de datos en la que se va a iniciar un seguimiento de base de datos. Si no se especifica una base de datos, el seguimiento se captura en todas las bases de datos del servidor.

5. Active o desactive las casillas **cifrar conexión** y **confiar en certificado de servidor** según corresponda para su escenario.

    ![Nueva página de captura](./media/database-experimentation-assistant-capture-trace/dea-new-capture.png)

## <a name="start-the-trace-capture"></a>Iniciar la captura de seguimiento

1. Después de escribir o seleccionar la información necesaria, seleccione **iniciar** para iniciar la captura de seguimiento.

    Si la información introducida es válida, comienza el proceso de captura de seguimiento. De lo contrario, los cuadros de texto con entradas no válidas se resaltan en rojo. Si se producen errores, corrija las entradas necesarias y, a continuación, seleccione **iniciar** de nuevo.

    Mientras se ejecuta la captura de seguimiento, en detalles de la **captura**, se muestra el estado y el progreso del proceso de captura de seguimiento.

    ![Supervisar el progreso de la captura](./media/database-experimentation-assistant-capture-trace/dea-capture-running.png)

2. Cuando finaliza la ejecución de la captura de seguimiento, el nuevo archivo de seguimiento (. TRC) se guarda en la **Ubicación de captura** que se especifica durante la configuración inicial.

    ![Captura de seguimiento completada](./media/database-experimentation-assistant-capture-trace/dea-capture-complete.png)

    El archivo de seguimiento incluye los resultados de seguimiento de la actividad de una base de datos de SQL Server. los archivos. TRC están diseñados para proporcionar más información acerca de los errores detectados e indicados por SQL Server.

## <a name="frequently-asked-questions-about-trace-capture"></a>Preguntas más frecuentes sobre la captura de seguimiento

A continuación se muestran algunas de las preguntas más frecuentes sobre la captura de seguimiento en DEA.

**P: ¿Qué eventos se capturan cuando ejecuto una captura de seguimiento en una base de datos de producción?**

En la tabla siguiente se enumeran los eventos y los datos de columna correspondientes que DEA recopila para seguimientos:
  
|Nombre del evento|Datos de texto (1)|Datos binarios (2)|IDENTIFICADOR de base de datos (3)|Nombre de host (8)|Nombre de la aplicación (10)|Nombre de inicio de sesión (11)|SPID (12)|Hora de inicio (14)|Hora de finalización (15)|Nombre de la base de datos (35)|Secuencia de eventos (51)|IsSystem (60)|  
|---|---|---|---|---|---|---|---|---|---|---|---|---|  
|**RPC: completado (10)**||*|*|*|*|*|*|*|*|*|*|*|  
|**RPC: iniciando (11)**||*|*|*|*|*|*|*||*|*|*|  
|**Parámetro de salida RPC (100)**|*||*|*|*|*|*|*||*|*|*|  
|**SQL: BatchCompleted (12)**|*||*|*|*|*|*|*|*|*|*|*|  
|**SQL: BatchStarting (13)**|*||*|*|*|*|*|*||*|*|*|  
|**Auditar inicio de sesión (14)**|*|*|*|*|*|*|*|*||*|*|*|  
|**Fin de sesión de auditoría (15)**|*||*|*|*|*|*|*|*|*|*|*|  
|**ExistingConnection (17)**|*|*|*|*|*|*|*|*||*|*|*|  
|**CursorOpen (53)**|*||*|*|*|*|*|*||*|*|*|  
|**CursorPrepare (70)**|*||*|*|*|*|*|*||*|*|*|  
|**Preparar SQL (71)**|||*|*|*|*|*|*||*|*|*|  
|**Exec Prepared SQL (72)**|||*|*|*|*|*|*||*|*|*|  
|**CursorExecute (74)**|*||*|*|*|*|*|*||*|*|*|  
|**CursorUnprepare (77)**|*||*|*|*|*|*|*||*|*|*|  
|**CursorClose (78)**|*||*|*|*|*|*|*||*|*|*|  

**P: ¿hay algún efecto de rendimiento en el servidor de producción cuando se ejecuta la captura de seguimiento?**

Sí, hay un efecto mínimo en el rendimiento durante la recopilación de seguimiento. En nuestras pruebas, encontramos aproximadamente una presión de memoria del 3%.

**P: ¿Qué tipo de permisos son necesarios para capturar seguimientos en una carga de trabajo de producción?**

- El usuario de Windows que ejecuta la operación de seguimiento en la aplicación de DEA debe tener derechos de administrador del sistema en el equipo que ejecuta SQL Server.
- La cuenta de servicio usada en el equipo que ejecuta SQL Server debe tener acceso de escritura a la ruta de acceso del archivo de seguimiento especificada.

**P: ¿se pueden capturar seguimientos para todo el servidor o solo en una base de datos única?**

Puede usar DEA para capturar seguimientos para todas las bases de datos del servidor o para una sola base de datos.

**P: tengo un servidor vinculado configurado en el entorno de producción. ¿Las consultas se muestran en los seguimientos?**

Si está ejecutando una captura de seguimiento para todo el servidor, el seguimiento captura todas las consultas, incluidas las consultas de servidor vinculado. Para ejecutar una captura de seguimiento para todo el servidor, deje el cuadro Nombre de la **base de datos** en **nueva captura** vacía.

**P: ¿Cuál es el tiempo mínimo recomendado para los seguimientos de carga de trabajo de producción?**

Se recomienda elegir la hora que mejor represente la totalidad de la carga de trabajo. De este modo, el análisis se ejecuta en todas las consultas de la carga de trabajo.

**P: ¿es importante tomar una copia de seguridad de base de datos antes de iniciar una captura de seguimiento?**

Antes de iniciar una captura de seguimiento, asegúrese de hacer una copia de seguridad de todas las bases de datos de destino. Se reproduce el seguimiento capturado en destino 1 y destino 2. Si el estado de la base de datos no es el mismo, se sesgan los resultados de la experimentación.

**P: ¿puedo recopilar XEvents en lugar de seguimientos y puedo reproducir XEvents?**

Sí. DEA es compatible con XEvents. Descargue la versión más reciente de DEA y pruébela.

## <a name="troubleshoot-trace-captures"></a>Solucionar problemas de capturas de seguimiento

Si ve un error al ejecutar una captura de seguimiento, confirme lo siguiente:

- El nombre del equipo que ejecuta SQL Server es válido. Para confirmar, intente conectarse al equipo que ejecuta SQL Server mediante SQL Server Management Studio (SSMS).
- La configuración del firewall no bloquea las conexiones con el equipo que ejecuta SQL Server.
- El usuario tiene los permisos que se enumeran en las [preguntas más frecuentes sobre la reproducción](./database-experimentation-assistant-replay-trace.md#frequently-asked-questions-about-trace-replay).
- El nombre de seguimiento no sigue la Convención estándar de sustitución incremental (Capture \_ 1). En su lugar, pruebe nombres de seguimiento como Capture \_ 1A o Capture1.

A continuación se muestran algunos posibles errores que podrían aparecer y soluciones para resolverlos:

|Posibles errores|Solución|  
|---|---|  
|No se puede iniciar el seguimiento en el SQL Server de destino, compruebe si tiene los permisos necesarios y si la cuenta de SQL Server tiene acceso de escritura a la ruta del archivo de seguimiento especificada código de error de SQL (53)|El usuario que ejecuta la herramienta DEA debe tener acceso al equipo que ejecuta SQL Server. El usuario debe tener asignado el rol sysadmin.|  
|No se puede iniciar el seguimiento en el SQL Server de destino, compruebe si tiene los permisos necesarios y si la cuenta de SQL Server tiene acceso de escritura a la ruta del archivo de seguimiento especificada código de error de SQL (19062)|Es posible que la ruta de acceso especificada no exista o que la carpeta no tenga permisos de escritura para la cuenta en la que se están ejecutando los servicios de SQL Server (por ejemplo, servicio de red). La ruta de acceso debe existir y debe tener los permisos necesarios para que se inicie el seguimiento.|  
|Actualmente se está ejecutando un seguimiento de DEA en el servidor de destino.|Ya se está ejecutando un seguimiento activo en el servidor de destino. No se puede iniciar un nuevo seguimiento cuando un seguimiento de todo el servidor ya se está ejecutando.|  
|No se puede abrir la base de datos solicitada para capturar el seguimiento. Este error puede deberse a un nombre de base de datos incorrecto.|La base de datos especificada no existe o no es accesible para el usuario actual. Use el nombre de base de datos correcto.|  

Si ve algún otro error con la etiqueta *código de error de SQL*, consulte [motor de base de datos errores](../relational-databases/errors-events/database-engine-events-and-errors.md) para obtener descripciones detalladas.

## <a name="see-also"></a>Consulte también

- Para obtener información sobre cómo configurar las herramientas de Distributed Replay en SQL Server antes de reproducir un seguimiento capturado, consulte [configuración de Distributed Replay para Asistente para experimentación con bases de datos](database-experimentation-assistant-configure-replay.md).