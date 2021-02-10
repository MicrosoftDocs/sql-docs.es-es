---
title: Reproducir un seguimiento para las actualizaciones de SQL Server
description: Obtenga información acerca de cómo reproducir un seguimiento capturado con Asistente para experimentación con bases de datos para las actualizaciones de SQL Server.
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
ms.openlocfilehash: 105f471afc10e03c8d6371aa22cce1f6e74ca8c1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065920"
---
# <a name="replay-a-trace-in-database-experimentation-assistant"></a>Reproducir un seguimiento en Asistente para experimentación con bases de datos

En Asistente para experimentación con bases de datos (DEA), puede reproducir un archivo de seguimiento capturado en un entorno de prueba actualizado. Por ejemplo, considere una carga de trabajo de producción que se ejecuta en SQL Server 2008 R2. El archivo de seguimiento de la carga de trabajo se debe reproducir dos veces: una vez en un entorno con la misma versión de SQL Server que se ejecuta en producción y una segunda vez en un entorno que tiene el destino de actualización SQL Server versión, como SQL Server 2016.

> [!NOTE]
> La reproducción de un seguimiento requiere la configuración manual de máquinas virtuales o equipos físicos para ejecutar Distributed Replay seguimientos. Para obtener más información, consulte [configuración de Distributed Replay para Asistente para experimentación con bases de datos](database-experimentation-assistant-configure-replay.md).
>

## <a name="configure-a-trace-replay-for-target-1"></a>Configurar una reproducción de seguimiento para el destino 1

En primer lugar, debe realizar una reproducción de seguimiento en el destino 1, que representa el entorno de producción existente.

1. En DEA, en la barra de navegación de la izquierda, seleccione el icono de flecha y, a continuación, en la página **todas las reproducciones** , seleccione **nueva reproducción**.

    ![Crear una reproducción en DEA](./media/database-experimentation-assistant-replay-trace/dea-create-replay.png)

    > [!NOTE]
    > El equipo del controlador de Distributed Replay requiere permisos para la cuenta de usuario que utiliza para conectarse de forma remota.

2. En la página **nueva reproducción** , en detalles de la **reproducción**, escriba o seleccione la siguiente información:

    - **Nombre de reproducción**: escriba un nombre para la reproducción de seguimiento.
    - **Formato de seguimiento de origen**: especifique el formato (Trace o XEvents) del archivo de seguimiento de origen.
    - **Ruta de acceso completa al archivo de código fuente**: especifique la ruta de acceso completa al archivo de seguimiento de origen. Si usa DReplay, el archivo debe existir en el equipo que actúa como controlador de DReplay y la cuenta de usuario requiere acceso al archivo y a la carpeta.
    - **Herramienta de reproducción**: especifique la herramienta de reproducción (DReplay o integrado).
    - **Nombre del equipo del controlador**: especifique el nombre del equipo que actúa como controlador de Distributed Replay.
    - **Reproducir la ubicación de seguimiento**: especifique la ruta de acceso para almacenar los archivos de seguimiento y XEvents asociados con la reproducción de seguimiento.

        > [!NOTE]
        > Para una Azure SQL Database o un Instancia administrada de Azure SQL, debe proporcionar el URI de SAS de la cuenta de almacenamiento de blobs de Azure.

3. Compruebe que ha restaurado las bases de datos. para ello, active la casilla **sí, he restaurado manualmente la base de datos** .

4. En **SQL Server Detalles de conexión**, escriba o seleccione la siguiente información:

    - **Tipo de servidor**: especifique el tipo de servidor SQL Server **(SQLServer**, **AzureSqlDb**, **AzureSqlManagedInstance**).
    - **Nombre del servidor**: especifique el nombre del servidor o la dirección IP de su SQL Server.
    - **Tipo de autenticación**: para el tipo de autenticación, seleccione **Windows**.
    - **Nombre** de la base de datos: escriba un nombre para la base de datos en la que se va a iniciar un seguimiento del lado servidor. Si no se especifica una base de datos, el seguimiento se captura en todas las bases de datos del servidor.

5. Active o desactive las casillas **cifrar conexión** y **confiar en certificado de servidor** según corresponda para su escenario.

    ![Nueva página de reproducción](./media/database-experimentation-assistant-replay-trace/dea-new-replay.png)

## <a name="start-the-trace-replay-on-target-1"></a>Iniciar la reproducción del seguimiento en el destino 1

- Después de escribir o seleccionar la información necesaria, seleccione **iniciar** para iniciar la reproducción de seguimiento.

  Si la información introducida es válida, se inicia el proceso de Distributed Replay. De lo contrario, los cuadros de texto que tienen información incorrecta se resaltan en rojo. Asegúrese de que los valores especificados son correctos y, a continuación, seleccione **iniciar**.

  ![Reproducir el progreso en el destino 1](./media/database-experimentation-assistant-replay-trace/dea-run-replay-target1.png)

  Puede supervisar el proceso según sea necesario. Una vez finalizada la ejecución de la reproducción, DEA almacenará los resultados en un archivo en la ubicación especificada.

  ![Reproducir en destino 1 completado](./media/database-experimentation-assistant-replay-trace/dea-replay-target1-complete.png)

## <a name="perform-the-trace-replay-against-target-2"></a>Realizar la reproducción del seguimiento en el destino 2

Cuando termine de realizar la reproducción del seguimiento en el destino 1, deberá hacer lo mismo con el segundo destino, que representa el entorno de actualización previsto.

1. Configurar una reproducción de seguimiento, esta vez con los detalles asociados al entorno de destino 2.
2. Inicie la reproducción del seguimiento en el destino 2.

   Puede supervisar el proceso según sea necesario. Una vez finalizada la ejecución de la reproducción, DEA almacenará los resultados en un archivo en la ubicación especificada.

## <a name="frequently-asked-questions-about-trace-replay"></a>Preguntas más frecuentes sobre la reproducción de seguimiento

**P: ¿Qué permisos de seguridad necesito para iniciar una captura de reproducción en el servidor de destino?**

- El usuario de Windows que ejecuta la operación de seguimiento en la aplicación de DEA debe tener derechos de administrador del sistema en el equipo de destino que ejecuta SQL Server. Estos derechos de usuario son necesarios para iniciar un seguimiento.
- La cuenta de servicio en la que se ejecuta el equipo de destino que ejecuta SQL Server debe tener acceso de escritura a la ruta de acceso del archivo de seguimiento especificada.
- La cuenta de servicio en la que se ejecutan los servicios de cliente de Distributed Replay debe tener derechos de usuario para conectarse al equipo de destino que ejecuta SQL Server y ejecutar consultas.

**P: ¿se puede iniciar más de una reproducción en la misma sesión?**

Sí, puede iniciar varias reproducciones y realizar un seguimiento de ellas hasta su finalización en la misma sesión.

**P: ¿se puede iniciar más de una reproducción en paralelo?**

Sí, pero no con el mismo conjunto de equipos seleccionados en **controlador más clientes**. El controlador y los clientes estarán ocupados. Configure un conjunto independiente de equipos en **controlador más cliente** para iniciar una reproducción paralela.

**P: ¿Cuánto tiempo tarda en completarse una reproducción?**

Una reproducción suele tardar la misma cantidad de tiempo que el seguimiento de origen más la cantidad de tiempo que se tarda en preprocesar el seguimiento de origen. Sin embargo, si los equipos cliente que están registrados con el controlador no son suficientes para administrar la carga que se produce a partir de la reproducción, la reproducción puede tardar más tiempo en completarse. Puede registrar hasta 16 equipos cliente con el controlador.

**P: ¿Qué tamaño tienen los archivos de seguimiento de destino?**

Los archivos de seguimiento de destino pueden estar entre 5 y 15 veces el tamaño del seguimiento de origen. El tamaño del archivo se basa en el número de consultas que se ejecutan. Por ejemplo, los blobs del plan de consulta pueden ser grandes. Si las estadísticas de estas consultas cambian con frecuencia, se capturan más eventos.

**P: ¿por qué es necesario restaurar las bases de datos?**

SQL Server es un sistema de administración de bases de datos relacionales con estado. Para ejecutar correctamente una prueba A/B, el estado de la base de datos se debe conservar en todo momento. De lo contrario, es posible que vea errores en las consultas durante la reproducción que no aparecerán en producción. Para evitar estos errores, se recomienda que realice una copia de seguridad justo antes de la captura de origen. Del mismo modo, es necesario restaurar la copia de seguridad en el equipo de destino que ejecuta SQL Server para evitar errores durante la reproducción.

**P: ¿Qué significa "pass%" en la página de reproducción?**

El **paso%** significa que solo se pasó un porcentaje de consultas. Puede diagnosticar si se espera el número de errores. Es posible que se produzcan errores o que se produzcan errores porque la base de datos ha perdido su integridad. Si el valor de **Pass%** no es el esperado, puede detener el seguimiento y ver el archivo de seguimiento en SQL Profiler para ver qué consultas no se han realizado correctamente.

**P: ¿Cómo puedo ver los eventos de seguimiento recopilados durante la reproducción?**

Abra un archivo de seguimiento de destino y verlo en SQL Server Profiler. O bien, si desea realizar modificaciones en la captura de reproducción, todos los scripts de SQL Server están disponibles en C: \\ archivos de programa (x86) \\ Microsoft Corporation \\ Asistente para experimentación con bases de datos \\ scripts \\ StartReplayCapture. SQL.

**P: ¿Qué eventos de seguimiento recopilan DEA durante la reproducción?**

DEA captura eventos de seguimiento que contienen información relacionada con el rendimiento. La configuración de captura está en el script StartReplayCaptureTrace. SQL. Estos eventos son típicos SQL Server eventos de seguimiento que se enumeran en la [documentación de referencia de sp_trace_setevent (Transact-SQL)](../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md).

## <a name="troubleshoot-trace-replay"></a>Solucionar problemas de reproducción de seguimiento

**P: ¿por qué no puedo conectarme al equipo que ejecuta SQL Server?**

- Confirme que el nombre del equipo que ejecuta SQL Server es válido. Para confirmar, intente conectarse al servidor mediante SQL Server Management Studio (SSMS).
- Confirme que la configuración del firewall no bloquea las conexiones con el equipo que ejecuta SQL Server.
- Confirme que el usuario tiene los derechos de usuario necesarios.
- Confirme que la cuenta de servicio del cliente de Distributed Replay tiene acceso al equipo que ejecuta SQL Server.

Puede obtener más detalles en los registros en% Temp% \\ DEA. Si el problema persiste, póngase en contacto con el equipo del producto.

**P: ¿por qué no puedo conectarme al controlador de Distributed Replay?**

- Compruebe que el servicio del controlador de Distributed Replay se está ejecutando en el equipo del controlador. Para comprobarlo, use las herramientas de administración de Distributed Replay (ejecute el comando `dreplay.exe status -f 1` ).
- Si la reproducción se inicia de forma remota:
  - Confirme que el equipo que ejecuta DEA puede hacer ping correctamente en el controlador. Confirme que la configuración del Firewall permite conexiones según las instrucciones de la página **configurar el entorno de reproducción** . Para obtener más información, vea el artículo [SQL Server Distributed Replay](../tools/distributed-replay/sql-server-distributed-replay.md).
  - Asegúrese de que se permite el inicio remoto DCOM y la activación remota para el usuario del controlador de Distributed Replay.
  - Asegúrese de que los derechos de usuario de acceso remoto DCOM están permitidos para el usuario del controlador de Distributed Replay.

**P: la ruta de acceso del archivo de seguimiento existe en el equipo. ¿Por qué no se encuentra Distributed Replay controlador?**

Distributed Replay solo puede tener acceso a los recursos del disco local. Debe copiar los archivos de seguimiento de origen en la máquina del controlador de Distributed Replay antes de iniciar la reproducción. Además, debe proporcionar la ruta de acceso en la página de **nueva reproducción** de DEA.

Las rutas UNC no son compatibles con Distributed Replay. Distributed Replay rutas de acceso deben ser rutas de acceso absolutas y locales al primer archivo de seguimiento de origen, incluida la extensión.

**P: ¿por qué no se pueden buscar archivos en la nueva página de reproducción?**

Dado que no se pueden examinar las carpetas en un equipo remoto, la exploración de archivos no es útil. Es más eficaz copiar y pegar las rutas de acceso absolutas.

**P: he iniciado la reproducción con un seguimiento pero Distributed Replay no ha reproducido ningún evento. ¿Por qué?**

Este problema puede deberse a que el archivo de seguimiento no tiene los eventos reproducibles o que contiene información sobre cómo reproducir eventos. Confirme si la ruta de acceso del archivo de seguimiento proporcionada apunta a un archivo de seguimiento de origen. El archivo de seguimiento de origen se crea con la configuración proporcionada en el script StartCaptureTrace. SQL.

**P: veo el mensaje "error inesperado!" al intentar preprocesar los archivos de seguimiento mediante el controlador SQL Server 2017 Distributed Replay. ¿Por qué?**

Este problema se conoce en la versión RTM de SQL Server 2017. Para obtener más información, vea [error inesperado al usar la característica de DReplay para reproducir un seguimiento capturado en SQL Server 2017](https://support.microsoft.com/help/4045678/fix-unexpected-error-when-you-use-the-dreplay-feature-to-replay-a).  
  
El problema se ha solucionado en la última actualización acumulativa 1 de SQL Server 2017. Descargue la versión más reciente de la [actualización acumulativa 1 para SQL Server 2017](https://support.microsoft.com/help/4038634/cumulative-update-1-for-sql-server-2017).

## <a name="see-also"></a>Consulte también

- Para crear un informe de análisis que le ayude a obtener información sobre los cambios propuestos, vea [crear informes](database-experimentation-assistant-create-report.md).
