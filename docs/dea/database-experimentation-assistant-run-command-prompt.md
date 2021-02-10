---
title: Ejecutar Asistente para experimentación con bases de datos en un símbolo del sistema
description: Obtenga información acerca de cómo capturar un seguimiento en Asistente para experimentación con bases de datos (DEA) y, a continuación, analizar los resultados en un símbolo del sistema.
ms.custom: seo-lt-2019
ms.date: 02/25/2020
ms.prod: sql
ms.prod_service: dea
ms.suite: sql
ms.technology: dea
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: pochiraju
ms.author: rajpo
ms.reviewer: mathoma
ms.openlocfilehash: 0e52cf31a94f2fd63f92305b80f7f7ae049c2809
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066450"
---
# <a name="run-database-experimentation-assistant-at-a-command-prompt"></a>Ejecutar Asistente para experimentación con bases de datos en un símbolo del sistema

En este artículo se describe cómo capturar un seguimiento en Asistente para experimentación con bases de datos (DEA) y, a continuación, analizar los resultados en un símbolo del sistema.

   > [!NOTE]
   > Para obtener más información acerca de cada operación de DEA, intente ejecutar el siguiente comando:
   >
   > `Deacmd.exe -o <operation> --help`
   >
   > Se requiere un nombre de operación; las operaciones válidas son **Analysis**, **StartCapture** y **StopCapture**.

## <a name="start-a-new-workload-capture-by-using-the-dea-command"></a>Inicio de una nueva captura de la carga de trabajo mediante el comando de DEA

Para iniciar una nueva captura de carga de trabajo, en un símbolo del sistema, ejecute el siguiente comando:

`Deacmd.exe -o StartCapture -n <Trace FileName> -x <Trace Format> -h <SQLServerInstance> -f <database name> -e <Encrypt Connection> -m <Authetication Mode> -u <user name> -p <password> -l <Location of Output Folder> -d <duration>`

**Ejemplo**

`Deacmd.exe -o StartCapture -n sql2008capture -x 0 -h localhost -f adventureworks -e --trust -m 0 -l c:\test  -d 60`

**Opciones adicionales**

Al iniciar una nueva captura de carga de trabajo con el `Deacmd.exe` comando, puede utilizar las siguientes opciones adicionales:

| Opción| Descripción |  
| --- | --- |
| -n,--name | Desee nombre del archivo de seguimiento |
| -x,--Format | Desee formato del seguimiento (Trace = 0, XEvents = 1) |
| -d,--Duration | Desee duración máxima de la captura, en minutos |
| -l, --location | Desee Ubicación de la carpeta de salida para almacenar los archivos Trace/XEvent en el equipo host |
| -t,--Type | (Valor predeterminado: 0) tipo/edición de SQL Server (SqlServer = 0, AzureSQLDB = 1, Azure SQL Instancia administrada = 2) |
| -h, --host | Desee SQL Server nombre de host o nombre de instancia para iniciar la captura |
| -e,--Encrypt | (Valor predeterminado: true) Cifre la conexión a SQL Server instancia. El valor predeterminado es true. |
| --confianza | (Valor predeterminado: false) Confiar en el certificado del servidor mientras se conecta a la instancia de SQL Server. El valor predeterminado es false. |
| -f,--DatabaseName | (Valor predeterminado:) Nombre de la base de datos para filtrar los seguimientos; si no se especifica, la captura se iniciará en todas las bases de datos. |
| -m,--authmode | (Valor predeterminado: 0) modo de autenticación (Windows = 0, autenticación de SQL = 1) |
| -u,--username | Nombre de usuario para conectarse al SQL Server |
| -p,--contraseña | Contraseña para conectarse al SQL Server |

## <a name="replay-a-workload-capture"></a>Reproducir una captura de carga de trabajo

**Usar Distributed Replay**

Si utiliza Distributed Replay, realice los pasos siguientes.

1. Inicie sesión en el equipo del controlador de Distributed Replay.
2. Para convertir el seguimiento de la carga de trabajo capturado con el comando de DEA en un archivo IRF, en un símbolo del sistema, ejecute el siguiente comando:

    `DReplay preprocess -m "dreplaycontroller" -i "Path to first trace file" -d "<Folder path on controller>\IrfFolder"`

3. Inicie una captura de seguimiento en el equipo de destino que ejecuta SQL Server mediante StartReplayCaptureTrace. SQL.

    a.  En SQL Server Management Studio (SSMS), abra <Dea_InstallPath \> \Scripts\StartReplayCaptureTrace.SQL.

    b.  Ejecute `Set @durationInMins=0` para que la captura de seguimiento no se detenga automáticamente después de un tiempo especificado.

    c.  Para establecer el tamaño máximo de archivo por archivo de seguimiento, ejecute `Set @maxfilesize` . El tamaño recomendado es 200 (en MB).

    d.  Edite `@Tracefile` para establecer un nombre único para el archivo de seguimiento.

    e.  Edite `@dbname` para especificar un nombre de base de datos si la carga de trabajo solo se debe capturar en una base de datos específica. De forma predeterminada, se captura la carga de trabajo en todo el servidor.

4. Para reproducir el archivo IRF en la instancia de SQL Server de destino, en un símbolo del sistema, ejecute el siguiente comando:

    `DReplay replay -m "dreplaycontroller" -d "<Folder Path on Dreplay Controller>\IrfFolder" -o -s "SQL2016Target" -w "dreplaychild1,dreplaychild2,dreplaycild3,dreplaychild4"`

    a.  Para supervisar el estado, en un símbolo del sistema, ejecute `DReplay status -f 1` .

    b.  Para detener la reproducción, por ejemplo, si ve que el porcentaje de paso es inferior al esperado, en un símbolo del sistema, ejecute `DReplay cancel` .

5. Detenga la captura de seguimiento en la instancia de SQL Server de destino.
6. En SSMS, Abra `<Dea_InstallPath>\Scripts\StopCaptureTrace.sql` .
7. Edite `@Tracefile` para que coincida con la ruta de acceso del archivo de seguimiento en el equipo de destino que ejecuta SQL Server.
8. Ejecute el script en el equipo de destino que ejecuta SQL Server.

**Usar reproducción integrada**

Si usa la reproducción integrada, no tendrá que configurar Distributed Replay. La capacidad de usar la reproducción integrada a través de la línea de comandos está en camino, pero, mientras tanto, puede usar la GUI para ejecutar la reproducción con la reproducción integrada.

## <a name="analyze-traces-using-the-dea-command"></a>Análisis de seguimientos mediante el comando de DEA

Para iniciar un nuevo análisis de seguimiento, en un símbolo del sistema, ejecute el siguiente comando:

`Deacmd.exe -o analysis -a <Target1 trace filepath> -b <Target2 trace filepath> -r reportname -h <SQLserverInstance> -e <encryptconnection> -u <username>`

**Ejemplo**

`Deacmd.exe -o analysis -a C:\Trace\SQL2008Source\Trace.trc -b C:\ Trace\SQL2014Trace\Trace.trc -r upgrade20082014 -h localhost -e`

Para ver los informes de análisis de estos archivos de seguimiento, debe usar la GUI para ver los gráficos y organizar las métricas.  Sin embargo, la base de datos de análisis se escribe en la instancia de SQL Server especificada, por lo que también puede consultar las tablas de análisis generadas directamente.

**Opciones adicionales**

Al analizar seguimientos mediante el comando DEA, puede usar las siguientes opciones adicionales:

| Opción| Descripción |  
| --- | --- |
| -a,--tracea | Desee Ruta de acceso al archivo de eventos para una instancia de. Ejemplo de C:\traces\Sql2008trace.trc.  Si hay un lote de archivos, seleccione el primer archivo y DEA comprobará automáticamente si hay archivos de sustitución incremental. Si los archivos están en BLOB, proporcione la ruta de acceso de la carpeta donde desea almacenar los archivos de eventos de forma local.  Ejemplo de C:\traces\ |
| -b,--traceB | Desee Ruta de acceso al archivo de eventos para la instancia B. Ejemplo de C:\traces\Sql2014trace.trc. Si hay un lote de archivos, seleccione el primer archivo y DEA comprobará automáticamente si hay archivos de sustitución incremental. Si los archivos están en BLOB, proporcione la ruta de acceso de la carpeta donde desea almacenar los archivos de eventos de forma local.  Ejemplo de C:\traces\ |
| -r,--ReportName | Desee nombre del análisis actual. El informe de análisis que se genera se identificará con este nombre. |
| -t,--Type | (Valor predeterminado: 0) tipo/edición de SQL Server (SqlServer = 0, AzureSQLDB = 1, Azure SQL Instancia administrada = 2) |
| -h, --host | Desee SQL Server nombre de host o nombre de instancia |
| -e,--Encrypt | (Valor predeterminado: true) Cifre la conexión a SQL Server instancia. El valor predeterminado es true. |
| --confianza | (Valor predeterminado: false) Confiar en el certificado del servidor mientras se conecta a la instancia de SQL Server. El valor predeterminado es false. |
| -m,--authmode | (Valor predeterminado: 0) modo de autenticación (Windows = 0, autenticación de SQL = 1) |
| -u,--username | Nombre de usuario para conectarse al SQL Server |
| --p | Contraseña para conectarse al SQL Server |
| --AB | (Valor predeterminado: false) La ubicación de almacenamiento del seguimiento A está en BLOB. Si se usa, también debe especificar--Abu (seguimiento de una dirección URL de BLOB) |
| --BB | (Valor predeterminado: false) La ubicación de almacenamiento del seguimiento B está en BLOB. Si se usa, también debe especificar--BBU (dirección URL del BLOB de seguimiento B) |
| --Abu | Dirección URL de BLOB para una instancia con clave SAS |
| --BBU | Dirección URL de BLOB para instancia B con clave SAS |

## <a name="see-also"></a>Consulte también

- Para obtener más información sobre el uso de DEA, consulte [información general de Asistente para experimentación con bases de datos](database-experimentation-assistant-overview.md).
