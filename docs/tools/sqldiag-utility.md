---
title: SQLdiag , utilidad
description: Use la utilidad SQLdiag para recopilar registros y archivos de datos de SQL Server y otros tipos de servidores, y supervisar los servidores a lo largo del tiempo o solucionar problemas.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- command prompt utilities [SQL Server], SQLdiag
- stopping diagnostic collection
- storing diagnostic information
- performance [SQL Server], diagnostic collection
- diagnostic records [SQL Server]
- scripts [SQL Server], diagnostic collection
- logs [SQL Server], diagnostic collection
- starting diagnostic collection
- clustered instance of SQL Server
- monitoring performance [SQL Server], diagnostic collection
- security [SQL Server], diagnostic collection
- SQLDIAG service
- space [SQL Server], diagnostic collection
- SQLdiag utility
- disk space [SQL Server], diagnostic collection
- configuration files [SQL Server]
- automatic diagnostic collection
- clusters [SQL Server], diagnostic collection
ms.assetid: 45ba1307-33d1-431e-872c-a6e4556f5ff2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1c9f3528ac4f377e379afd26e6583df3fe63fb2b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353329"
---
# <a name="sqldiag-utility"></a>SQLdiag , utilidad
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
  La utilidad **SQLdiag** es una herramienta de recopilación de diagnósticos de uso general que se puede ejecutar como una aplicación de consola o como un servicio. Puede usar **SQLdiag** para recopilar los archivos de datos y de registro de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] y otros tipos de servidores, y para supervisar los servidores a lo largo del tiempo o solucionar problemas específicos de los mismos. **SQLdiag** se ha diseñado para acelerar y simplificar la recopilación de información de diagnóstico para los Servicios de soporte técnico de [!INCLUDE[msCoName](../includes/msconame-md.md)] .  
  
> [!NOTE]  
>  Esta utilidad podría modificarse y las aplicaciones o scripts que dependen del comportamiento o de los argumentos de su línea de comandos podrían no funcionar correctamente en versiones posteriores.  
  
 **SQLdiag** puede recopilar los siguientes tipos de información de diagnóstico:  
  
-   Registros de rendimiento de Windows  
  
-   Registros de eventos de Windows  
  
-   [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] seguimientos  
  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] información de bloqueo  
  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] información de configuración  
  
 Puede especificar los tipos de información que quiere que **SQLdiag** recopile si modifica el archivo de configuración SQLDiag.xml, que se describe en la siguiente sección.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sqldiag   
     { [/?] }  
     |  
     { [/I configuration_file]  
       [/O output_folder_path]  
       [/P support_folder_path]  
       [/N output_folder_management_option]  
       [/M machine1 [ machine2 machineN]| @machinelistfile]  
       [/C file_compression_type]  
       [/B [+]start_time]  
       [/E [+]stop_time]  
       [/A SQLdiag_application_name]  
       [/T { tcp [ ,port ] | np | lpc } ]  
       [/Q] [/G] [/R] [/U] [/L] [/X] }  
     |  
     { [START | STOP | STOP_ABORT] }  
     |  
     { [START | STOP | STOP_ABORT] /A SQLdiag_application_name }  
```  
  
## <a name="arguments"></a>Argumentos  
 **/?**  
 Muestra información de uso.  
  
 **/I** _configuration_file_  
 Establece el archivo de configuración que usará **SQLdiag** . De forma predeterminada, **/I** está establecido en SQLDiag.Xml.  
  
 **/O** _output_folder_path_  
 Redirige la salida de **SQLdiag** a la carpeta especificada. Si no se especifica la opción **/O** , la salida de **SQLdiag** se escribe en una subcarpeta denominada SQLDIAG en la carpeta de inicio de **SQLdiag** . Si no existe la carpeta SQLDIAG, **SQLdiag** intenta crearla.  
  
> [!NOTE]  
>  La ubicación de la carpeta de salida es relativa a la ubicación de la carpeta Support que se puede especificar con **/P**. Para definir una ubicación completamente distinta de la carpeta de salida, especifique la ruta de acceso completa del directorio para **/O**.  
  
 **/P** _support_folder_path_  
 Define la ruta de la carpeta Support. De forma predeterminada, **/P** está establecido en la carpeta en que reside el ejecutable **SQLdiag** . La carpeta Support contiene los archivos de compatibilidad de **SQLdiag** , como el archivo de configuración XML, los scripts Transact-SQL y otros archivos que usa la utilidad durante la recopilación de diagnósticos. Si usa esta opción para especificar una ruta de acceso alternativa a los archivos de compatibilidad, **SQLdiag** copiará automáticamente los archivos de compatibilidad que necesita en la carpeta especificada, si todavía no existen.  
  
> [!NOTE]  
>  Para establecer la carpeta actual como ruta de acceso de la carpeta Support, especifique **%cd%** en la línea de comandos como se indica a continuación:  
>   
>  **SQLDIAG /P %cd%**  
  
 **/N** _output_folder_management_option_  
 Establece si **SQLdiag** sobrescribe o cambia el nombre de la carpeta de salida cuando se inicia. Opciones disponibles:  
  
 1 = Sobrescribe la carpeta de salida (valor predeterminado)  
  
 2 = Cuando se inicia **SQLdiag** , cambia el nombre de la carpeta de salida a SQLDIAG_00001, SQLDIAG_00002, etc. Una vez cambiado el nombre de la carpeta de salida actual, **SQLdiag** escribe la salida en la carpeta de salida predeterminada SQLDIAG.  
  
> [!NOTE]  
>  **SQLdiag** no anexa la salida a la carpeta de salida actual cuando se inicia. Solo puede sobrescribir la carpeta de salida predeterminada (opción 1) o cambiar el nombre de la carpeta (opción 2) y, a continuación, escribir la salida en la nueva carpeta de salida predeterminada denominada SQLDIAG.  
  
 **/M** _machine1_ [ *machine2* *machineN*] | *\@machinelistfile*  
 Invalida los equipos especificados en el archivo de configuración. De forma predeterminada, el archivo de configuración es SQLDiag.Xml, o se establece con el parámetro **/I** . Al especificar más de un equipo, separe cada nombre de equipo con un espacio.  
  
 Con *\@machinelistfile* se especifica un nombre de archivo de la lista de máquinas que se va a almacenar en el archivo de configuración.  
  
 **/C** _file_compression_type_  
 Establece el tipo de compresión de archivo que se usa en los archivos de la carpeta de salida de **SQLdiag** . Opciones disponibles:  
  
 0 = ninguno (valor predeterminado)  
  
 1 = utiliza la compresión NTFS  
  
 **/B** [ **+** ]*start_time*  
 Especifica la fecha y la hora en que se empiezan a recopilar los datos de diagnóstico en el siguiente formato:  
  
 AAAAMMDD_HH:MM:SS  
  
 La hora se especifica usando el reloj de 24 horas. Por ejemplo, 2:00 p.m. se debe especificar como **14:00:00**.  
  
 Use **+** sin la fecha (solo HH:MM:SS) para especificar una hora que sea relativa a la fecha y la hora actuales. Por ejemplo, si especifica **/B +02:00:00**, **SQLdiag** esperará 2 horas antes de empezar a recopilar información.  
  
 No inserte ningún espacio entre **+** y el valor de *start_time* especificado.  
  
 Si especifica una hora de inicio que se encuentra en el pasado, **SQLdiag** cambia forzosamente la fecha de inicio para que la fecha y la hora de inicio se encuentren en el futuro. Por ejemplo, si especifica **/B 01:00:00** y la hora actual es 08:00:00, **SQLdiag** cambia forzosamente la fecha de inicio para que sea el día siguiente.  
  
 Tenga en cuenta que **SQLdiag** usa la hora local del equipo en el que se ejecuta la utilidad.  
  
 **/E** [ **+** ]*stop_time*  
 Especifica la fecha y la hora en que se dejan de recopilar los datos de diagnóstico en el siguiente formato:  
  
 AAAAMMDD_HH:MM:SS  
  
 La hora se especifica usando el reloj de 24 horas. Por ejemplo, 2:00 p.m. se debe especificar como **14:00:00**.  
  
 Use **+** sin la fecha (solo HH:MM:SS) para especificar una hora que sea relativa a la fecha y la hora actuales. Por ejemplo, si especifica una hora de inicio y una hora de finalización mediante **/B +02:00:00 /E +03:00:00**, **SQLdiag** espera 2 horas para empezar a recopilar información y después recopila información durante 3 horas antes de detenerse y cerrarse. Si no se especifica **/B** , **SQLdiag** empieza a recopilar información de diagnóstico inmediatamente y finaliza en la fecha y la hora especificadas por **/E**.  
  
 No inserte ningún espacio entre **+** y el valor especificado de *start_time* o *end_time*.  
  
 Tenga en cuenta que **SQLdiag** usa la hora local del equipo en el que se ejecuta la utilidad.  
  
 **/A**  _SQLdiag_application_name_  
 Permite la ejecución de varias instancias de la utilidad **SQLdiag** en la misma instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
 Cada *SQLdiag_application_name* identifica una instancia diferente de **SQLdiag**. No existe ninguna relación entre una instancia de *SQLdiag_application_name* y un nombre de instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
 *SQLdiag_application_name* se puede usar para iniciar o detener una instancia concreta del servicio **SQLdiag** .  
  
 Por ejemplo:  
  
 **SQLDIAG START /A**  _SQLdiag_application_name_  
  
 También se puede usar con la opción **/R** para registrar una instancia concreta de **SQLdiag** como servicio. Por ejemplo:  
  
 **SQLDIAG /R /A** _SQLdiag_application_name_  
  
> [!NOTE]  
>  **SQLdiag** agrega el prefijo DIAG$ de forma automática al nombre de instancia especificado para *SQLdiag_application_name*. De este modo, se proporciona un nombre de servicio significativo si se registra **SQLdiag** como servicio.  
  
 /T { tcp [ ,*port* ] | np | lpc }  
 Se conecta a una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] con el protocolo especificado.  
  
 tcp [,*port*]  
 TCP/IP (Protocolo de control de transmisión/protocolo de Internet). Si lo desea, puede especificar un número de puerto para la conexión.  
  
 np  
 Canalizaciones con nombre. De manera predeterminada, la instancia predeterminada de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] escucha en la canalización con nombre: `\\.\pipe\sql\query` y en `\\.\pipe\MSSQL$<instancename>\sql\query` para una instancia con nombre. No puede conectar a una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] usando un nombre de canalización alternativa.  
  
 lpc  
 Llamada a procedimiento local. Este protocolo de memoria compartida está disponible si el cliente se está conectando a una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] del mismo equipo.  
  
 **/Q**  
 Ejecuta **SQLdiag** en modo silencioso. **/Q** suprime todos los mensajes, como los de contraseña.  
  
 **/G**  
 Ejecuta **SQLdiag** en modo genérico. Cuando se especifica **/G** , al iniciarse **SQLdiag** , no aplica las comprobaciones de conectividad de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ni si el usuario es miembro del rol fijo de servidor **sysadmin** . En su lugar, **SQLdiag** consulta con Windows para determinar si un usuario tiene los derechos adecuados para recopilar cada diagnóstico solicitado.  
  
 Si no se especifica **/G** , **SQLdiag** comprueba si el usuario es miembro del grupo **Administradores** de Windows y no recopila diagnósticos de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] si el usuario no es miembro del grupo **Administradores** .  
  
 **/R**  
 Registra **SQLdiag** como servicio. Cualquier argumento de la línea de comandos que se especifique al registrar **SQLdiag** como servicio se guarda para futuras ejecuciones del servicio.  
  
 Cuando se registra **SQLdiag** como servicio, el nombre predeterminado del servicio es SQLDIAG. Puede modificar el nombre de servicio con el argumento **/A** .  
  
 Use el argumento de línea de comandos **START** para iniciar el servicio:  
  
 **SQLDIAG START**  
  
 También puede usar el comando **net start** para iniciar el servicio:  
  
 **net  start SQLDIAG**  
  
 **/U**  
 Elimina del Registro **SQLdiag** como servicio.  
  
 Use también el argumento **/A** si elimina del Registro una instancia con nombre de **SQLdiag** .  
  
 **/L**  
 Ejecuta **SQLdiag** en modo continuo cuando se especifica además una hora de inicio o de finalización con los argumentos **/B** o **/E** , respectivamente. **SQLdiag** se reinicia automáticamente después de que se detenga la recopilación de diagnósticos debido a un cierre programado. Por ejemplo, con los argumentos **/E** o **/X** .  
  
> [!NOTE]  
>  **SQLdiag** omite el argumento **/L** si no se especifica una hora de inicio o de finalización con los argumentos de la línea de comandos **/B** y **/E** .  
  
 El uso de **/L** no implica el modo de servicio. Para usar **/L** cuando se ejecuta **SQLdiag** como servicio, especifíquelo en la línea de comandos al registrar el servicio.  
  
 **/X**  
 Ejecuta **SQLdiag** en modo de instantánea. **SQLdiag** toma una instantánea de todos los diagnósticos configurados y después se cierra automáticamente.  
  
 **START** | **STOP** | **STOP_ABORT**  
 Inicia o detiene el servicio **SQLdiag** . **STOP_ABORT** obliga al servicio a cerrarse lo más rápido posible sin terminar la recopilación de diagnósticos que está realizando.  
  
 Cuando se emplean estos argumentos de control del servicio, deben ser los primeros argumentos utilizados en la línea de comandos. Por ejemplo:  
  
 **SQLDIAG START**  
  
 El argumento **/A** , que especifica una instancia con nombre de **SQLdiag**, es el único que se puede usar con **START**, **STOP** o **STOP_ABORT** para controlar una instancia específica del servicio **SQLdiag** . Por ejemplo:  
  
 **SQLDIAG START /A** _SQLdiag_application_name_  
  
## <a name="security-requirements"></a>Requisitos de seguridad  
 A menos que **SQLdiag** se ejecute en modo genérico (especificando el argumento de la línea de comandos **/G** ), el usuario que ejecuta **SQLdiag** debe ser miembro del grupo **Administradores** de Windows y del rol fijo de servidor [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] **sysadmin**. De forma predeterminada, **SQLdiag** conecta con [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] mediante la autenticación de Windows, pero también admite la autenticación de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
## <a name="performance-considerations"></a>Consideraciones de rendimiento  
 Los efectos sobre el rendimiento de la ejecución de **SQLdiag** dependen del tipo de datos de diagnóstico cuya recopilación se haya configurado. Por ejemplo, si ha configurado **SQLdiag** para recopilar la información de seguimiento de [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] , cuanto mayor sea el número de clases de eventos de las que decida realizar un seguimiento, más se verá afectado el rendimiento del servidor.  
  
 El impacto sobre el rendimiento de la ejecución de **SQLdiag** es aproximadamente equivalente a la suma de los costos de recopilar los diagnósticos configurados por separado. Por ejemplo, si recopila un seguimiento con **SQLdiag** , incurrirá en el mismo costo de rendimiento que si lo recopila con [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)]. El impacto que el uso de **SQLdiag** tiene sobre el rendimiento es mínimo.  
  
## <a name="required-disk-space"></a>Espacio en disco requerido  
 Dado que **SQLdiag** puede recopilar diferentes tipos de información de diagnóstico, el espacio libre en disco necesario para ejecutar **SQLdiag** varía. La cantidad de información de diagnóstico recopilada depende de la naturaleza y del volumen de la carga de trabajo que el servidor está procesando y puede oscilar entre unos pocos megabytes o varios gigabytes.  
  
## <a name="configuration-files"></a>Archivos de configuración  
 Durante el inicio, **SQLdiag** lee el archivo de configuración y los argumentos de la línea de comandos que se han especificado. Puede especificar los tipos de información de diagnóstico que **SQLdiag** recopila en el archivo de configuración. De forma predeterminada, **SQLdiag** usa el archivo de configuración SQLDiag.Xml, que se extrae cada vez que se ejecuta la herramienta y se encuentra ubicado en la carpeta de inicio de la utilidad **SQLdiag** . El archivo de configuración usa el esquema XML, SQLDiag_schema.xsd, que también se extrae en el directorio de inicio de la utilidad desde el archivo ejecutable cada vez que se ejecuta **SQLdiag** .  
  
### <a name="editing-the-configuration-files"></a>Editar los archivos de configuración  
 Puede copiar y editar SQLDiag.Xml para cambiar los tipos de datos de diagnóstico que **SQLdiag** recopila. Cuando edite el archivo de configuración, use siempre un editor XML que pueda validar dicho archivo comparándolo con su esquema XML, como por ejemplo, [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)]. No debe editar SQLDiag.Xml directamente. En su lugar, realice una copia de SQLDiag.Xml y cambie su nombre por otro nombre de archivo en la misma carpeta. Después, edite el nuevo archivo y use el argumento **/I** para pasarlo a **SQLdiag**.  
  
#### <a name="editing-the-configuration-file-when-sqldiag-runs-as-a-service"></a>Editar el archivo de configuración cuando SQLdiag se ejecuta como servicio  
 Si ya ha ejecutado **SQLdiag** como servicio y necesita editar el archivo de configuración, elimine del Registro el servicio SQLDIAG. Para ello, especifique el argumento de la línea de comandos **/U** y después vuelva a registrar el servicio mediante el argumento de la línea de comandos **/R** . Si elimina el servicio del registro y lo vuelve a registrar, se eliminará la información de configuración antigua que se almacenó en caché en el Registro de Windows.  
  
## <a name="output-folder"></a>Carpeta de salida  
 Si no especifica una carpeta de salida con el argumento **/O** , **SQLdiag** crea una subcarpeta denominada SQLDIAG en la carpeta de inicio de **SQLdiag** . Para la recopilación de información de diagnóstico que implica un elevado volumen de seguimientos, como [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] , asegúrese de que la carpeta de salida se encuentra en una unidad local con espacio suficiente para almacenar la salida del diagnóstico solicitado.  
  
 Cuando se reinicia **SQLdiag** , sobrescribe el contenido de la carpeta de salida. Para evitarlo, especifique **/N 2** en la línea de comandos.  
  
## <a name="data-collection-process"></a>Proceso de recopilación de datos  
 Cuando se inicia **SQLdiag** , realiza las comprobaciones de inicialización necesarias para recopilar los datos de diagnóstico que se han especificado en SQLDiag.Xml. Este proceso puede tardar varios segundos. Una vez que **SQLdiag** ha empezado a recopilar los datos de diagnóstico cuando se ejecuta como aplicación de consola, se muestra un mensaje que informa de que se ha iniciado la recopilación de **SQLdiag** y de que puede pulsar CTRL+C para detenerla. Cuando **SQLdiag** se ejecuta como servicio, se escribe un mensaje similar en el registro de eventos de Windows.  
  
 Si usa **SQLdiag** para diagnosticar un problema que puede reproducir, espere hasta que reciba este mensaje antes de reproducir el problema en el servidor.  
  
 **SQLdiag** recopila la mayor parte de los datos de diagnóstico en paralelo. Para recopilar toda la información de diagnóstico se conecta con herramientas, como la utilidad [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] **sqlcmd** o el procesador de comandos de Windows, excepto cuando la información se recopila de los registros de rendimiento y de los registros de eventos de Windows. **SQLdiag** usa un subproceso de trabajo por cada equipo para supervisar la recopilación de datos de diagnóstico de esas otras herramientas y, a menudo, espera simultáneamente a que varias herramientas completen su proceso. Durante el proceso de recopilación, **SQLdiag** enruta la salida de cada diagnóstico a la carpeta de salida.  
  
## <a name="stopping-data-collection"></a>Detener la recopilación de datos  
 Cuando **SQLdiag** empieza a recopilar los datos de diagnóstico, continua haciéndolo a menos que el usuario lo detenga o que se configure para que se detenga a una hora especificada. Puede configurar **SQLdiag** para que se detenga a una hora especificada mediante el argumento **/E** , que permite especificar una hora de detención, o mediante el argumento **/X** , que hace que **SQLdiag** se ejecute en el modo de instantánea.  
  
 Cuando **SQLdiag** se detenga, detendrá todos los diagnósticos que haya iniciado. Por ejemplo, detiene los seguimientos de [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] que estaba recopilando, deja de ejecutar los scripts de [!INCLUDE[tsql](../includes/tsql-md.md)] que estaba ejecutando y detiene los subprocesos que ha generado durante la recopilación de datos. Una vez completada la recopilación de datos de diagnóstico, **SQLdiag** se cierra.  
  
> [!NOTE]  
>  No se permite pausar el servicio **SQLdiag** . Si intenta pausar el servicio **SQLdiag** , se detiene cuando finaliza la recopilación de los diagnósticos que estaba recopilando cuando se pausó. Si reinicia **SQLdiag** después de detenerlo, la aplicación se reinicia y sobrescribe el contenido de la carpeta de salida. Para evitar sobrescribir la carpeta de salida, especifique **/N 2** en la línea de comandos.  
  
 **Para detener SQLdiag cuando se ejecuta como aplicación de consola**  
  
 Si está ejecutando **SQLdiag** como aplicación de consola, pulse CTRL+C en la ventana de la consola en la que se está ejecutando **SQLdiag** para detenerlo. Tras pulsar CTRL+C, se muestra un mensaje en la ventana de la consola que le informa de que la recopilación de datos de **SQLDiag** está finalizando y de que debe esperar hasta que se cierre el proceso, lo que puede tardar varios minutos.  
  
 Presione CTRL+C dos veces para terminar todos los procesos de diagnóstico secundarios y cierre la aplicación inmediatamente.  
  
 **Para detener SQLdiag cuando se ejecuta como servicio**  
  
 Si ejecuta **SQLdiag** como servicio, ejecute **SQLDiag STOP** en la carpeta de inicio de **SQLdiag** para detenerlo.  
  
 Si ejecuta varias instancias de **SQLdiag** en el mismo equipo, también puede pasar el nombre de instancia de **SQLdiag** a la línea de comandos cuando detenga el servicio. Por ejemplo, para detener una instancia de **SQLdiag** denominada Instance1, use la siguiente sintaxis:  
  
```  
SQLDIAG STOP /A Instance1  
```  
  
> [!NOTE]  
>  **/A** es el único argumento de la línea de comandos que se puede usar con **START**, **STOP** o **STOP_ABORT**. Si necesita especificar una instancia con nombre de **SQLdiag** con uno de los verbos de control del servicio, especifique **/A** tras el verbo de control en la línea de comandos como se muestra en el ejemplo de sintaxis anterior. Cuando se emplean estos verbos de control, deben ser los primeros argumentos utilizados en la línea de comandos.  
  
 Para detener el servicio lo más rápido posible, ejecute **SQLDIAG STOP_ABORT** en la carpeta de inicio de la utilidad. Este comando anula todas las recopilaciones de diagnósticos en curso sin esperar a que terminen de ejecutarse.  
  
> [!NOTE]  
>  Use **SQLDiag STOP** o **SQLDIAG STOP_ABORT** para detener el servicio **SQLdiag** . No use la consola Servicios de Windows para detener **SQLdiag** u otros servicios de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
## <a name="automatically-starting-and-stopping-sqldiag"></a>Iniciar y detener automáticamente SQLdiag  
 Para iniciar y detener automáticamente la recopilación de datos de diagnóstico a una hora especificada, use los argumentos **/B**_start\_time_ y **/E**_stop\_time_ (con la notación de 24 horas). Por ejemplo, si está solucionando un problema que suele aparecer a las 02:00:00 aproximadamente, puede configurar **SQLdiag** para empezar a recopilar automáticamente datos de diagnóstico a la 01:00 y dejar de recopilarlos a las 03:00:00. Use los argumentos **/B** y **/E** para especificar la hora de inicio y detención. Utilice la notación de 24 horas para especificar una hora y una fecha de inicio y de finalización exactas con el formato AAAAMMDD_HH:MM:SS. Para especificar una hora de inicio o detención relativa, anteponga **+** a la hora de inicio o de detención y omita la parte de la fecha (AAAAMMDD_) como se muestra en el siguiente ejemplo, lo que hace que **SQLdiag** espere 1 hora antes de empezar a recopilar información; después recopila la información durante 3 horas hasta que se detiene y se cierra:  
  
```  
sqldiag /B +01:00:00 /E +03:00:00  
```  
  
 Cuando se especifica un valor *start_time* relativo, **SQLdiag** se inicia a una hora relativa respecto a la hora y la fecha actuales. Cuando se especifica un valor *end_time* relativo, **SQLdiag** finaliza a una hora relativa respecto al valor *start_time* especificado. Si la hora o la fecha de inicio y de finalización que ha especificado corresponden al pasado, **SQLdiag** cambiará forzosamente la fecha de inicio para que la fecha y la hora de inicio correspondan al futuro.  
  
 Esto tiene importantes implicaciones para las fechas de inicio y de finalización que elija. Considere el ejemplo siguiente:  
  
```  
sqldiag /B +01:00:00 /E 08:30:00  
```  
  
 Si la hora actual es 08:00, la hora de finalización termina antes de que empiece la recopilación de diagnóstico. Dado que **SQLDiag** ajusta automáticamente las fechas de inicio y de finalización al día siguiente cuando corresponden al pasado, en este ejemplo, la recopilación de diagnósticos se inicia a las 09:00 de hoy (se ha especificado una hora de inicio relativa con **+** ) y continúa hasta las 08:30 del día siguiente.  
  
### <a name="stopping-and-restarting-sqldiag-to-collect-daily-diagnostics"></a>Detener y reiniciar SQLdiag para que recopile diagnósticos diariamente  
 Para recopilar diariamente un conjunto de diagnósticos especificado sin tener que iniciar y detener manualmente **SQLdiag**, use el argumento **/L** . El argumento **/L** hace que **SQLdiag** se ejecute de forma continua al reiniciarse automáticamente después de un cierre programado. Cuando se especifica **/L** y **SQLdiag** se detiene porque ha llegado la hora de finalización especificada con el argumento **/E** , o se detiene porque se está ejecutando en el modo de instantánea con el argumento **/X** , **SQLdiag** se reinicia en vez de cerrarse.  
  
 En el siguiente ejemplo se especifica que **SQLdiag** se ejecute en modo continuo para que se reinicie automáticamente después de recopilar los datos de diagnóstico entre las 03:00:00 y las 05:00:00.  
  
```  
sqldiag /B 03:00:00 /E 05:00:00 /L  
```  
  
 En el siguiente ejemplo se especifica que **SQLdiag** se ejecute en modo continuo para que se reinicie automáticamente después de tomar una instantánea de datos de diagnóstico a las 03:00:00.  
  
```  
sqldiag /B 03:00:00 /X /L  
```  
  
## <a name="running-sqldiag-as-a-service"></a>Ejecutar SQLdiag como servicio  
 Si desea usar **SQLdiag** para recopilar los datos de diagnóstico durante largos períodos de tiempo en los que puede ser necesario cerrar sesión en el equipo en el que se está ejecutando **SQLdiag** , puede ejecutarlo como servicio.  
  
 **Para registrar SQLDiag para que se ejecute como servicio**  
  
 Puede registrar **SQLdiag** para que se ejecute como servicio si especifica el argumento **/R** en la línea de comandos. De esta forma se registra **SQLdiag** para que se ejecute como servicio. El nombre de servicio de **SQLdiag** es SQLDIAG. Cualquier otro argumento que especifique en la línea de comandos cuando registre **SQLDiag** como servicio se conservará y se reutilizará cuando se inicie el servicio.  
  
 Para cambiar el nombre predeterminado del servicio SQLDIAG, use el argumento de la línea de comandos **/A** . **SQLdiag** agrega el prefijo DIAG$ de forma automática a cualquier nombre de instancia de **SQLdiag** especificado con **/A** para crear nombres de servicio significativos.  
  
 **Para eliminar el servicio SQLDIAG del registro**  
  
 Para eliminar el servicio del Registro, especifique el argumento **/U** . Si elimina del Registro **SQLdiag** como servicio, también se eliminan las claves del servicio del Registro de Windows.  
  
 **Para iniciar o reiniciar el servicio SQLDIAG**  
  
 Para iniciar o reiniciar el servicio SQLDIAG, ejecute **SQLDiag START** desde la línea de comandos.  
  
 Si ejecuta varias instancias de **SQLdiag** con el argumento **/A** , también puede pasar el nombre de instancia de **SQLdiag** en la línea de comandos cuando inicie el servicio. Por ejemplo, para iniciar una instancia de **SQLdiag** denominada Instance1, use la siguiente sintaxis:  
  
```  
SQLDIAG START /A Instance1  
```  
  
 También puede usar el comando **net start** para iniciar el servicio SQLDIAG.  
  
 Cuando se reinicia **SQLdiag**, sobrescribe el contenido de la carpeta de salida actual. Para evitarlo, especifique **/N 2** en la línea de comandos para cambiar el nombre de la carpeta de salida cuando se inicie la utilidad.  
  
 No se permite pausar el servicio **SQLdiag** .  
  
## <a name="running-multiple-instances-of-sqldiag"></a>Ejecutar varias instancias de SQLdiag  
 Para ejecutar varias instancias de **SQLdiag** en el mismo equipo, especifique **/A**_SQLdiag\_application\_name_ en la línea de comandos. Resulta útil para recopilar distintos conjuntos de diagnósticos de la misma instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] de forma simultánea. Por ejemplo, puede configurar una instancia con nombre de **SQLdiag** para que ejecute una recopilación de datos ligera de forma constante. De este modo, si se produce un problema específico en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], puede ejecutar la instancia predeterminada de **SQLdiag** para recopilar los diagnósticos relativos a ese problema o para recopilar un conjunto de diagnósticos que los servicios de soporte al cliente de [!INCLUDE[msCoName](../includes/msconame-md.md)] le hayan solicitado para diagnosticar un problema.  
  
## <a name="collecting-diagnostic-data-from-clustered-sql-server-instances"></a>Recopilar datos de diagnóstico de instancias de SQL Server en clúster  
 **SQLdiag** permite recopilar datos de diagnóstico de instancias de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en clúster. Para recopilar diagnósticos de instancias de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] agrupadas, asegúrese de que se especifica **"."** para el atributo **name** del elemento **\<Machine>** en el archivo de configuración SQLDiag.Xml y no especifique el argumento **/G** en la línea de comandos. De forma predeterminada, se especifica **"."** para el atributo **name** del archivo de configuración y se desactiva el argumento **/G** . Normalmente, cuando se realiza la recopilación desde una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en clúster, no es necesario editar el archivo de configuración ni cambiar los argumentos de la línea de comandos.  
  
 Si se especifica **"."** como nombre de equipo, **SQLdiag** detecta que se está ejecutando en un clúster y recupera simultáneamente la información de diagnóstico de todas las instancias virtuales de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] instaladas en el clúster. Si quiere recopilar información de diagnóstico solo de una de las instancias virtuales de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] que se ejecutan en el equipo, especifique ese [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] virtual para el atributo **name** del elemento **\<Machine>** de SQLDiag.Xml.  
  
> [!NOTE]  
>  Para recopilar información de seguimiento de [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] desde instancias agrupadas de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , deben habilitarse los recursos compartidos con fines administrativos (ADMIN$) en el clúster.  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la utilidad del símbolo del sistema &#40;motor de base de datos&#41;](../tools/command-prompt-utility-reference-database-engine.md)  
  
  
