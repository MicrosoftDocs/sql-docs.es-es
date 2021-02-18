---
title: Configuración de Distributed Replay
titleSuffix: SQL Server Distributed Replay
description: En este artículo se describen los requisitos de producto que deben tenerse en cuenta para poder usar la característica Distributed Replay de SQL Server.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: aee11dde-daad-439b-b594-9f4aeac94335
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: ee8c7a67b1071f3b78b5fc18fb21be28f1d658c2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345912"
---
# <a name="configure-distributed-replay"></a>Configure Distributed Replay
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Los detalles de configuración de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay se especifican en archivos XML en Distributed Replay Controller, en los clientes y donde se encuentra instalada la herramienta de administración. Entre los archivos figuran los siguientes:  
  
-   [Archivo de configuración del controlador](#DReplayController)  
  
-   [Archivo de configuración del cliente](#DReplayClient)  
  
-   [Archivo de configuración de preproceso](#PreprocessConfig)  
  
-   [Archivo de configuración de reproducción](#ReplayConfig)  
  
##  <a name="controller-configuration-file-dreplaycontrollerconfig"></a><a name="DReplayController"></a> Archivo de configuración del controlador: DReplayController.config  
 Cuando el servicio de Distributed Replay Controller de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se inicia, carga el nivel de registro del archivo de configuración del controlador, `DReplayController.config`. Este archivo se encuentra en la carpeta donde instaló el servicio de Distributed Replay Controller:  
  
 **\<controller installation path>\DReplayController.config**  
  
 El nivel de registro especificado por el archivo de configuración del controlador incluye lo siguiente:  
  
|Configuración|Elemento XML|Descripción|Valores permitidos|Obligatorio|  
|-------------|-----------------|-----------------|--------------------|--------------|  
|Nivel de registro|`<LoggingLevel>`|Especifica el nivel de registro para el servicio del controlador.|`INFORMATION` &#124; `WARNING` &#124; `CRITICAL`|No. De forma predeterminada, el valor es `CRITICAL`.|  
  
### <a name="example"></a>Ejemplo  
 En este ejemplo se muestra un archivo de configuración del controlador modificado para suprimir las entradas de registro `INFORMATION` y `WARNING` .  
  
```  
<?xml version='1.0'?>  
<Options>  
<LoggingLevel>CRITICAL</LoggingLevel>  
</Options>  
```  
  
##  <a name="client-configuration-file-dreplayclientconfig"></a><a name="DReplayClient"></a> Archivo de configuración del cliente: DReplayClient.config  
 Cuando el servicio de Distributed Replay Client de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se inicia, carga los valores de configuración del archivo de configuración del cliente, `DReplayClient.config`. Este archivo se encuentra en cada cliente, en la carpeta donde instaló el servicio de Distributed Replay Client:  
  
 **\<client installation path>\DReplayClient.config**  
  
 La configuración especificada por el archivo de configuración del cliente incluye lo siguiente:  
  
|Configuración|Elemento XML|Descripción|Valores permitidos|Obligatorio|  
|-------------|-----------------|-----------------|--------------------|--------------|  
|Controller|`<Controller>`|Especifica el nombre del equipo que se va a controlar. El cliente intentará registrarse con el entorno de Distributed Replay poniéndose en contacto con el controlador.|Puede utilizar "`localhost`" o "`.`" para hacer referencia al equipo local.|No. De forma predeterminada, el cliente intenta registrarse con la instancia del controlador que se está ejecutando localmente ("`.`"), si existe.|  
|Directorio de trabajo del cliente|`<WorkingDirectory>`|Es la ruta de acceso local del cliente donde se guardan los archivos de distribución.<br /><br /> Los archivos de este directorio se sobrescriben en la siguiente reproducción.|Un nombre de directorio completo, empezando con la letra de unidad.|No. Si no se especifica ningún valor, los archivos de distribución se guardarán en la misma ubicación que el archivo de configuración del cliente predeterminado. Si se especifica un valor y esa carpeta no existe en el cliente, el servicio del cliente no se iniciará.|  
|Directorio de resultados del cliente|`<ResultDirectory>`|Es la ruta de acceso local en el cliente donde se guarda el archivo de seguimiento del resultado de la actividad de reproducción (para el cliente).<br /><br /> Los archivos de este directorio se sobrescriben en la siguiente reproducción.|Un nombre de directorio completo, empezando con la letra de unidad.|No. Si no se especifica ningún valor, el archivo de seguimiento del resultado se guardará en la misma ubicación que el archivo de configuración del cliente predeterminado. Si se especifica un valor y esa carpeta no existe en el cliente, el servicio del cliente no se iniciará.|  
|Nivel de registro|`<LoggingLevel>`|Es el nivel de registro para el servicio del cliente.|`INFORMATION` &#124; `WARNING` &#124; `CRITICAL`|No. De forma predeterminada, el valor es `CRITICAL`.|  
  
### <a name="example"></a>Ejemplo  
 Este ejemplo muestra un archivo de configuración del cliente modificado para especificar que el servicio del controlador se está ejecutando en un equipo diferente, denominado `Controller1`. Los elementos `WorkingDirectory` y `ResultDirectory` se han configurado de modo que utilicen las carpetas `c:\ClientWorkingDir` y `c:\ResultTraceDir`, respectivamente. El nivel de registro se ha cambiado de su valor predeterminado para suprimir las entradas de registro `INFORMATION` y `WARNING` .  
  
```  
<?xml version='1.0'?>  
<Options>  
    <Controller>Controller1</Controller>  
    <WorkingDirectory>c:\ClientWorkingDir</WorkingDirectory>  
    <ResultDirectory>c:\ResultTraceDir</ResultDirectory>  
    <LoggingLevel>CRITICAL</LoggingLevel>  
</Options>  
```  
  
##  <a name="preprocess-configuration-file-dreplayexepreprocessconfig"></a><a name="PreprocessConfig"></a> Archivo de configuración de preproceso: DReplay.exe.preprocess.config  
 Al utilizar la herramienta de administración para iniciar la fase de preproceso, la herramienta de administración carga los valores de configuración de preproceso desde el archivo de configuración de preproceso, `DReplay.exe.preprocess.config`.  
  
 Use el archivo de configuración predeterminado o el parámetro **-c** de la herramienta de administración para especificar la ubicación de un archivo de configuración de preproceso modificado. Para obtener más información sobre cómo usar la opción de preprocesamiento de la herramienta de administración, vea [Opción de preprocesamiento &#40;herramienta de administración Distributed Replay&#41;](../../tools/distributed-replay/preprocess-option-distributed-replay-administration-tool.md).  
  
 El archivo de configuración de preproceso predeterminado se encuentra en la carpeta donde instaló la herramienta de administración:  
  
 **\<administration tool installation path>\DReplayAdmin\DReplay.exe.preprocess.config**  
  
 Los valores de configuración de preproceso se especifican en elementos XML secundarios del elemento `<PreprocessModifiers>` en el archivo de configuración de preproceso. Entre los ajustes se incluyen los siguientes:  
  
|Configuración|Elemento XML|Descripción|Valores permitidos|Obligatorio|  
|-------------|-----------------|-----------------|--------------------|--------------|  
|Incluir las actividades de sesión de sistema|`<IncSystemSession>`|Indica si las actividades de sesión de sistema en la captura se incluirán durante la reproducción.|`Yes` &#124; `No`|No. De forma predeterminada, el valor es `No`.|  
|Tiempo de inactividad máximo|`<MaxIdleTime>`|Limita el tiempo de inactividad a un número absoluto (en segundos).|Un valor entero >= -1.<br /><br /> `-1` indica que no ha habido ningún cambio respecto del valor original en el archivo de seguimiento original.<br /><br /> `0` indica que existe alguna actividad en algún momento.|No. De forma predeterminada, el valor es `-1`.|  
  
### <a name="example"></a>Ejemplo  
 El archivo de configuración de preproceso predeterminado:  
  
```  
<?xml version='1.0'?>  
<Options>  
    <PreprocessModifiers>  
        <IncSystemSession>No</IncSystemSession>  
        <MaxIdleTime>-1</MaxIdleTime>  
    </PreprocessModifiers>  
</Options>  
```  
  
##  <a name="replay-configuration-file-dreplayexereplayconfig"></a><a name="ReplayConfig"></a> Archivo de configuración de reproducción: DReplay.exe.replay.config  
 Al utilizar la herramienta de administración para iniciar la fase de reproducción de eventos, la herramienta de administración carga los valores de configuración de reproducción desde el archivo de configuración de reproducción, `DReplay.exe.replay.config`.  
  
 Use el archivo de configuración predeterminado o el parámetro **-c** de la herramienta de administración para especificar la ubicación de un archivo de configuración de reproducción modificado. Para obtener más información sobre cómo usar la opción replay de la herramienta de administración, vea [Opción Replay &#40;herramienta de administración Distributed Replay&#41;](../../tools/distributed-replay/replay-option-distributed-replay-administration-tool.md).  
  
 El archivo de configuración de reproducción predeterminado se encuentra en la carpeta donde instaló la herramienta de administración:  
  
 **\<administration tool installation path>\DReplayAdmin\DReplay.exe.replay.config**  
  
 Los valores de configuración de reproducción se especifican en elementos XML secundarios de los elementos `<ReplayOptions>` y `<OutputOptions>` del archivo de configuración de reproducción.  
  
### <a name="replayoptions-element"></a>Elemento \<ReplayOptions>  
 Los valores especificados por el archivo de configuración de reproducción en el elemento `<ReplayOptions>` incluyen lo siguiente:  
  
|Configuración|Elemento XML|Descripción|Valores permitidos|Obligatorio|  
|-------------|-----------------|-----------------|--------------------|--------------|  
|Instancia de destino de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (el servidor de prueba)|`<Server>`|Especifica el nombre del servidor y la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para la conexión.|*server_name*[\\*instance_name*]<br /><br /> No puede usar "`localhost`" o "`.`" para representar el host local.|No, si el nombre del servidor ya se especifica con el parámetro **-s**_target server_ con la opción **replay** de la herramienta de administración.|  
|Modo de secuenciación|`<SequencingMode>`|Especifica el modo que se usa para la programación de eventos.|`synchronization` &#124; `stress`|No. De forma predeterminada, el valor es `stress`.|  
|Granularidad de escala de esfuerzo|`<StressScaleGranularity>`|Especifica si todas las conexiones en el Identificador de perfil de servicio (SPID) deben escalarse juntas (SPID) o independientemente (Conexión) en el modo de esfuerzo.|SPID &#124; Conexión|Sí. De forma predeterminada, el valor es `SPID`.|  
|Escala del tiempo de conexión|`<ConnectTimeScale>`|Se usa para escalar el tiempo de conexión en el modo de esfuerzo.|Un entero entre `1` y `100`.|No. De forma predeterminada, el valor es `100`.|  
|Escala del tiempo de reflexión|`<ThinkTimeScale>`|Se usa para escalar el tiempo de reflexión en el modo de esfuerzo.|Un entero entre `0` y `100`.|No. De forma predeterminada, el valor es `100`.|  
|Use agrupar conexiones|`<UseConnectionPooling>`|Especifica si la agrupación de conexiones se habilitará en cada Distributed Replay client.|Sí &#124; No|Sí. De forma predeterminada, el valor es `Yes`.|  
|Intervalo del monitor de mantenimiento|`<HealthmonInterval>`|Indica con qué periodicidad se debe ejecutar el monitor de mantenimiento (en segundos).<br /><br /> Este valor solo se utiliza en modo de sincronización.|Entero >= 1<br /><br /> (`-1` para deshabilitar)|No. De forma predeterminada, el valor es `60`.|  
|Tiempo de espera de la consulta|`<QueryTimeout>`|Especifica el valor del tiempo de espera máximo de una consulta, en segundos. Este valor solo es efectivo hasta que se haya devuelto la primera fila.|Entero >= 1<br /><br /> (`-1` para deshabilitar)|No. De forma predeterminada, el valor es `3600`.|  
|Subprocesos por cliente|`<ThreadsPerClient>`|Especifica el número de subprocesos de reproducción para cada cliente de reproducción.|Un entero entre `1` y `512`.|No. Si no se especifica, Distributed Replay usará el valor `255`.|  
  
### <a name="outputoptions-element"></a>Elemento \<OutputOptions>  
 Los valores especificados por el archivo de configuración de reproducción en el elemento `<OutputOptions>` incluyen lo siguiente:  
  
|Configuración|Elemento XML|Descripción|Valores permitidos|Obligatorio|  
|-------------|-----------------|-----------------|--------------------|--------------|  
|Registrar el recuento de filas|`<RecordRowCount>`|Indica si se debe registrar el recuento de filas para cada conjunto de resultados.|`Yes` &#124; `No`|No. De forma predeterminada, el valor es `Yes`.|  
|Registrar el conjunto de resultados|`<RecordResultSet>`|Indica si se debe registrar el contenido de todos los conjuntos de resultados.|`Yes` &#124; `No`|No. De forma predeterminada, el valor es `No`.|  
  
### <a name="example"></a>Ejemplo  
 El archivo de configuración de reproducción predeterminado:  
  
```  
<?xml version='1.0'?>  
<Options>  
    <ReplayOptions>  
        <Server></Server>  
        <SequencingMode>stress</SequencingMode>  
        <ConnectTimeScale></ConnectTimeScale>  
        <ThinkTimeScale></ThinkTimeScale>  
        <HealthmonInterval>60</HealthmonInterval>  
        <QueryTimeout>3600</QueryTimeout>  
        <ThreadsPerClient></ThreadsPerClient>  
    </ReplayOptions>  
    <OutputOptions>  
        <ResultTrace>  
            <RecordRowCount>Yes</RecordRowCount>  
            <RecordResultSet>No</RecordResultSet>  
        </ResultTrace>  
    </OutputOptions>  
</Options>  
```  

### <a name="possible-issue-when-running-with-synchronization-sequencing-mode"></a>Posible problema al ejecutarse con el modo de secuenciación de sincronización
 Es posible que encuentre un síntoma en el que la funcionalidad de reproducción aparezca como "detención" o reproduzca los eventos muy lentamente. Este fenómeno puede producirse si el seguimiento que se está reproduciendo se basa en datos o eventos que no existen en la base de datos de destino restaurada. 
 
 Un ejemplo es una carga de trabajo capturada que utiliza WAITFOR, como en la instrucción WAITFOR RECEIVE de Service Broker. Al usar el modo de secuenciación de sincronización, los lotes se reproducen en serie. Si se produce una acción INSERT en la base de datos de origen después de la copia de seguridad de la base de datos, pero antes de que se inicie el seguimiento de la captura de reproducción, la instrucción WAITFOR RECEIVE emitida durante la reproducción puede que tenga que esperar toda la duración del evento WAITFOR. Los eventos se configuran para que se reproduzcan después de que se detenga WAITFOR RECEIVE. Esto puede dar lugar a que el contador del monitor de rendimiento de solicitudes por lotes/s caiga a cero hasta que se complete WAITFOR. 
 
 Si necesita usar el modo de sincronización y desea evitar este comportamiento, debe hacer lo siguiente:
 
1.  Ponga en reposo las bases de datos que va a usar como destinos de reproducción.

2.  Permita que se completen todas las actividades pendientes.

3.  Realice una copia de seguridad de las bases de datos y permita que se completen las copias de seguridad.

4.  Inicie la captura de seguimiento de Distributed Replay y reanude la carga de trabajo normal. 
 
 
## <a name="see-also"></a>Consulte también  
 [Opciones de línea de comandos de la herramienta de administración &#40;utilidad Distributed Replay&#41;](../../tools/distributed-replay/administration-tool-command-line-options-distributed-replay-utility.md)   
 [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md)   
 [Foro de SQL Server Distributed Replay](https://social.technet.microsoft.com/Forums/sl/sqldru/)   
 [Usar Distributed Replay para la prueba de carga de SQL Server, parte 2](/archive/blogs/msdn/mspfe/using-distributed-replay-to-load-test-your-sql-serverpart-2)   
 [Usar Distributed Replay para la prueba de carga de SQL Server, parte 1](/archive/blogs/batuhanyildiz/using-distributed-replay-to-load-test-your-sql-serverpart-1)  
  
