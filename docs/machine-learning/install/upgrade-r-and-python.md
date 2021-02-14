---
title: Actualización de los entornos de ejecución de Python y R (enlace)
description: Actualice los entornos de ejecución de R y Python en SQL Server Machine Learning Services o en SQL Server R Services mediante sqlbindr.exe para enlazarlos con Machine Learning Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 09/30/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: =sql-server-2016||=sql-server-2017
ms.openlocfilehash: 7cdf5b2d0c223130245604f5b27674a714ab4d8a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082393"
---
# <a name="upgrade-python-and-r-runtime-with-binding-in-sql-server-machine-learning-services"></a>Actualización del entorno de ejecución de Python y R con enlace en SQL Server Machine Learning Services
[!INCLUDE [SQL Server 2016 and 2017](../../includes/applies-to-version/sqlserver2016-2017-only.md)]

En este artículo se describe cómo usar un proceso de instalación llamado **enlace** para actualizar los entornos de ejecución de R o Python en [SQL Server 2016 R Services](../r/sql-server-r-services.md) o [SQL Server 2017 Machine Learning Services](../sql-server-machine-learning-services.md). Puede obtener las [versiones más recientes de Python y R](#version-map) *enlazando* a [Microsoft Machine Learning Server](/machine-learning-server).

> [!IMPORTANT]
> En este artículo se describe un método anterior para actualizar los entornos de ejecución de R y Python, denominado *enlace*. Si ha instalado **actualización acumulativa (CU) 14 o posterior para SQL Server 2016 Services Pack (SP) 2** o la **actualización acumulativa (CU) 22 o posterior para SQL Server 2017**, consulte cómo [cambiar el entorno de ejecución del lenguaje R o Python predeterminado a una versión posterior](change-default-language-runtime-version.md) en su lugar.

## <a name="what-is-binding"></a>¿Qué es un enlace?

El enlace es un proceso de instalación que reemplaza el contenido de las carpetas **R_SERVICES** y **PYTHON_SERVICES** por archivos ejecutables, bibliotecas y herramientas más recientes de [Microsoft Machine Learning Server](/machine-learning-server).

Los componentes cargados que se incluyen con el modelo de servicio han cambiado. Las actualizaciones del servicio coinciden con la [escala de tiempo de soporte técnico para Microsoft R Server y Machine Learning Server](/machine-learning-server/resources-servicing-support) en el [ciclo de vida moderno](https://support.microsoft.com/help/30881/modern-lifecycle-policy).

A excepción de las versiones de los componentes y las actualizaciones del servicio, el enlace no cambia los aspectos básicos de la instalación:

- La integración de Python y R sigue formando parte de una instancia del motor de base de datos.
- La concesión de licencias permanece igual (no hay costos adicionales asociados al enlace).
- Las directivas de soporte técnico de SQL Server permanecen para el motor de base de datos.

En el resto de este artículo se explica el mecanismo de enlace y su funcionamiento en cada versión de SQL Server.

> [!NOTE]
> El enlace solo se aplica a instancias en base de datos que estén enlazadas a instancias de SQL Server. En este caso, el enlace no es necesario para una instalación independiente.

::: moniker range="=sql-server-2016"
**Consideraciones sobre el enlace de SQL Server 2016**

Para los clientes de SQL Server 2016 R Services, el enlace proporciona:

- Paquetes de R actualizados.
- Nuevos paquetes que no forman parte de la instalación original ([MicrosoftML](/machine-learning-server/r-reference/microsoftml/microsoftml-package))
- [Modelos](/machine-learning-server/install/microsoftml-install-pretrained-models) de aprendizaje automático entrenados previamente para el análisis de opiniones y la detección de imágenes.

Todo los enlaces pueden recibir nuevas actualizaciones en cada nueva versión principal y secundaria de [Microsoft Machine Learning Server](/machine-learning-server/index).
::: moniker-end

## <a name="version-map"></a>Mapa de versiones

Las tablas siguientes son mapas de versiones. Cada mapa muestra las versiones de los paquetes en todas las versiones. Puede revisar las rutas de actualización al enlazar a Microsoft Machine Learning Server (conocido anteriormente como R Server, antes de la adición de compatibilidad con Python a partir de Machine Learning Server 9.2.1).

El enlace no garantiza la versión más reciente de R o Anaconda. Al enlazar con Microsoft Machine Learning Server, se obtiene la versión de R o Python que se instala con el programa de instalación, que podría no ser la versión más reciente disponible en el sitio web.

::: moniker range="=sql-server-2016"
[**SQL Server 2016 R Services**](../install/sql-r-services-windows-install.md)

Componente |Versión inicial | [R Server 9.0.1](/machine-learning-server/install/r-server-install-windows) | [R Server 9.1](/machine-learning-server/install/r-server-install-windows) | [Machine Learning Server 9.2.1](/machine-learning-server/install/machine-learning-server-windows-install) | [Machine Learning Server 9.3](/machine-learning-server/install/machine-learning-server-windows-install) |  [Machine Learning Server 9.4.7](/machine-learning-server/install/machine-learning-server-windows-install)
----------|----------------|----------------|--------------|---------|-------|-------|
Microsoft R Open (MRO) en R | R 3.2.2     | R 3.3.2   |R 3.3.3   | R 3.4.1  | R 3.4.3 | R 3.5.2
[RevoScaleR](/machine-learning-server/r-reference/revoscaler/revoscaler) | 8.0.3  | 9.0.1 |  9.1 |  9.2.1 |  9.3 |  9.4.7 |
[MicrosoftML](/machine-learning-server/r-reference/microsoftml/microsoftml-package)| n/a | 9.0.1 |  9.1 |  9.2.1 |  9.3 | 9.4.7 |
[modelos previamente entrenados](/machine-learning-server/install/microsoftml-install-pretrained-models)| n/a | 9.0.1 |  9.1 |  9.2.1 |  9.3 | 9.4.7 |
[sqlrutils](/machine-learning-server/r-reference/sqlrutils/sqlrutils)| n/a | 1.0 |  1.0 |  1.0 |  1.0 | 1.0 |
[olapR](/machine-learning-server/r-reference/olapr/olapr) | n/a | 1.0 |  1.0 |  1.0 |  1.0 | 1.0 |
::: moniker-end

::: moniker range="=sql-server-2017"
[**SQL Server 2017 Machine Learning Services**](../install/sql-machine-learning-services-windows-install.md)

Componente |Versión inicial | Machine Learning Server 9.3 | Machine Learning Server 9.4.7 |
----------|----------------|---------|---------|
Microsoft R Open (MRO) en R | R 3.3.3 | R 3.4.3 | R 3.5.2 |
[RevoScaleR](/machine-learning-server/r-reference/revoscaler/revoscaler) |   9.2 |  9.3 | 9.4.7 |
[MicrosoftML](/machine-learning-server/r-reference/microsoftml/microsoftml-package) | 9.2  | 9.3| 9.4.7 |
[sqlrutils](/machine-learning-server/r-reference/sqlrutils/sqlrutils)| 1.0 |  1.0 | 1.0 |
[olapR](/machine-learning-server/r-reference/olapr/olapr) | 1.0 |  1.0 | 1.0 |
Anaconda 4.2 en Python 3.5  | 4.2/3.5.2 | 4.2/3.5.2 |
[revoscalepy](/machine-learning-server/python-reference/revoscalepy/revoscalepy-package) | 9.2  | 9.3| 9.4.7 |
[microsoftml](/machine-learning-server/python-reference/microsoftml/microsoftml-package) | 9.2  | 9.3| 9.4.7 |
[modelos previamente entrenados](/machine-learning-server/install/microsoftml-install-pretrained-models) | 9.2 | 9.3| 9.4.7 |
::: moniker-end

## <a name="how-component-upgrade-works"></a>Funcionamiento de la actualización de componentes

Las bibliotecas de archivos ejecutables, R y Python se actualizan al enlazar una instalación existente de R y Python con Machine Learning Server.

El enlace se ejecuta con el [instalador de Microsoft Machine Learning Server](/machine-learning-server/install/machine-learning-server-windows-install) al ejecutar el programa de instalación en una instancia existente del motor de base de datos de SQL Server que tenga la integración de R o Python. 

El programa de instalación detecta las características existentes y le pide que vuelva a enlazar con Machine Learning Server.

Durante el enlace, el contenido de `C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES` y `\PYTHON_SERVICES` se sobrescribe por los archivos ejecutables y las bibliotecas más recientes de `C:\Program Files\Microsoft\ML Server\R_SERVER` y `\PYTHON_SERVER`.

El enlace solo se aplica a las características de Python y R. Los paquetes de código abierto para Python y R constan de:

- Anaconda
- Microsoft R Open
- Paquetes propios RevoScaleR
- Revoscalepy

El enlace no cambia el modelo de compatibilidad de la instancia del motor de base de datos ni la versión de SQL Server.

El enlace es reversible. Puede revertir al servicio de SQL Server [desenlazando la instancia](#bkmk_Unbind) y reparando la instancia del motor de base de datos de SQL Server.

<a name="bkmk_BindWizard"></a>

## <a name="bind-to-machine-learning-server-using-setup"></a>Enlace a Machine Learning Server mediante el programa de instalación

Siga los pasos para enlazar SQL Server a Microsoft Machine Learning Server mediante el programa de instalación. 

1. En SSMS, ejecute `SELECT @@version` para comprobar que el servidor cumple los requisitos mínimos de compilación.

   En el caso de SQL Server 2016 R Services, los requisitos mínimos son [Service Pack 1](https://www.microsoft.com/download/details.aspx?id=54276) y [CU3](https://support.microsoft.com/help/4019916/cumulative-update-3-for-sql-server-2016-sp1).

1. Compruebe la versión de los paquetes base de R y RevoScaleR para confirmar que las versiones existentes son inferiores a las que tiene previsto reemplazar. 

    ```sql
    EXECUTE sp_execute_external_script
    @language=N'R'
    ,@script = N'str(OutputDataSet);
    packagematrix <- installed.packages();
    Name <- packagematrix[,1];
    Version <- packagematrix[,3];
    OutputDataSet <- data.frame(Name, Version);'
    , @input_data_1 = N''
    WITH RESULT SETS ((PackageName nvarchar(250), PackageVersion nvarchar(max) ))
    ```

1. Cierre SSMS y todas las herramientas que tengan una conexión abierta a SQL Server. El enlace sobrescribe los archivos del programa. Si SQL Server tiene sesiones abiertas, se producirá un error en el enlace con el código de error de enlace 6.

1. Descargue Microsoft Machine Learning Server en el equipo que tenga la instancia que quiere actualizar. Se recomienda la [versión más reciente](/machine-learning-server/install/machine-learning-server-windows-install#download-machine-learning-server-installer).

1. Descomprima la carpeta e inicie ServerSetup.exe, que se encuentra en MLSWIN93.

1. En **Configurar la instalación**, confirme los componentes que se van a actualizar y revise la lista de instancias compatibles.

1. En la página **Contrato de licencia**, seleccione **Acepto estos términos** para aceptar los términos de licencia de Machine Learning Server. 

1. En las páginas sucesivas, dé su consentimiento a otras condiciones de licencia para cualquier componente de código abierto que seleccione, como Microsoft R Open o la distribución Anaconda de Python.

1. En la página **Ya casi estamos**, anote la carpeta de instalación. La carpeta predeterminada es \Archivos de programa\Microsoft\ML Server.

    Si quiere cambiar la carpeta de instalación, seleccione **Avanzado** para volver a la primera página del asistente, aunque deberá repetir todas las selecciones anteriores.

Si se produce un error en la actualización, consulte [Códigos de error de SqlBindR](#sqlbindr-error-codes) para obtener más información.

## <a name="offline-binding-no-internet-access"></a>Enlace sin conexión (sin acceso a Internet)

En el caso de los sistemas sin conexión a Internet, puede descargar el instalador y los archivos .cab en un equipo conectado a Internet y, a continuación, transferir los archivos al servidor aislado.

El instalador (ServerSetup.exe) incluye los paquetes de Microsoft (RevoScaleR, MicrosoftML, olapR y sqlRUtils). Los archivos .cab proporcionan otros componentes principales. Por ejemplo, el archivo .cab "SRO" proporciona R Open, la distribución de R de código abierto de Microsoft.

En las siguientes instrucciones se explica cómo colocar los archivos para una instalación sin conexión.

1. Descargue el instalador de MLSWIN93. Se descarga como archivo comprimido. Se recomienda la [versión más reciente](/machine-learning-server/install/machine-learning-server-windows-install#download-machine-learning-server-installer), pero también se pueden instalar [versiones anteriores](/machine-learning-server/install/r-server-install-windows-offline#download-required-components).

1. Descargue los archivos .cab. Los enlaces siguientes son para la versión 9.3. Si necesita versiones anteriores, encontrará más enlaces en [R Server 9.1](/machine-learning-server/install/r-server-install-windows-offline#download-required-components). Recuerde que Python/Anaconda solo se puede agregar a una instancia de SQL Server Machine Learning Services. Existen modelos previamente entrenados para R y Python; el archivo .cab ofrece modelos en los lenguajes que está usando.

    | Característica | Descargar |
    |---------|----------|
    | R       | [SRO_3.4.3.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=867186&clcid=1033) |
    | Python  | [SPO_9.3.0.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=859054) | 
    | Modelos entrenados previamente | [MLM_9.3.0.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=859053) |

1. Transfiera los archivos .zip y .cab al servidor de destino.

1. En el servidor, escriba `%temp%` en el comando Ejecutar para obtener la ubicación física del directorio temp. La ruta de acceso física varía según la máquina, pero suele ser `C:\Users\<your-user-name>\AppData\Local\Temp`.

1. Coloque los archivos .cab en la carpeta %temp%.

1. Descomprima el instalador.

1. Ejecute ServerSetup.exe y siga las indicaciones en pantalla para completar la instalación.

## <a name="command-line-operations"></a><a name="bkmk_BindCmd"></a>Operaciones de línea de comandos


> [!TIP]
> ¿No encuentra SqlBindR? Es probable que no haya ejecutado el programa de instalación.
> SqlBindR solo está disponible después de haber ejecutado el programa de instalación de Machine Learning Server.

1. Abra un símbolo del sistema como administrador y vaya hasta la carpeta que contiene sqlbindr.exe. La ubicación predeterminada es C:\Archivos de programa\Microsoft\MLServer\Setup.

2. Escriba el comando siguiente para ver una lista de instancias disponibles: `SqlBindR.exe /list`
  
   Anote el nombre completo de la instancia como se muestra. Por ejemplo, el nombre de la instancia podría ser MSSQL14.MSSQLSERVER para una instancia predeterminada, o bien algo parecido a SERVERNAME.MYNAMEDINSTANCE.

3. Ejecute el comando **SqlBindR.exe** con el argumento */bind*. Especifique el nombre de la instancia que se va a actualizar con el nombre de instancia que se devolvió en el paso anterior.

   Por ejemplo, para actualizar la instancia predeterminada, escriba: `SqlBindR.exe /bind MSSQL14.MSSQLSERVER`

4. Cuando haya finalizado la actualización, reinicie el servicio Launchpad asociado a cualquier instancia que se haya modificado.

## <a name="revert-or-unbind-an-instance"></a><a name="bkmk_Unbind"></a>Reversión o desenlace de una instancia

Puede restaurar una instancia enlazada a una instalación inicial de los componentes de R y Python, establecida por el programa de instalación de SQL Server. Hay tres partes para revertir el servicio de SQL Server.

+ [Paso 1: Desenlace de Microsoft Machine Learning Server](#step-1-unbind)
+ [Paso 2: Restauración de la instancia al estado original](#step-2-restore)
+ [Paso 3: Reinstalación de los paquetes que haya agregado a la instalación](#step-3-reinstall-packages)

<a name="step-1-unbind"></a> 

### <a name="step-1-unbind"></a>Paso 1: Unbind

Tiene dos opciones para revertir el enlace: volver a ejecutar el programa de instalación o usar la utilidad de línea de comandos SqlBindR.

#### <a name="unbind-using-setup"></a><a name="bkmk_wizunbind"></a> Desenlace mediante el programa de instalación

1. Busque el instalador de Machine Learning Server. Si ha quitado el instalador, es posible que tenga que volver a descargarlo o copiarlo desde otro equipo.
2. Asegúrese de ejecutar el instalador en el equipo que tiene la instancia que quiere desenlazar.
2. El instalador identifica las instancias locales que son candidatas para el desenlace.
3. Anule la selección de la casilla situada junto a la instancia que quiera revertir a la configuración original.
4. Acepte todos los contratos de licencia.
5. Seleccione **Finalizar**. El proceso tarda unos minutos.

#### <a name="unbind-using-the-command-line"></a><a name="bkmk_cmdunbind"></a> Desenlace mediante la línea de comandos

1. Abra un símbolo del sistema y vaya a la carpeta que contiene **sqlbindr.exe**, como se describe en la sección anterior.

2. Ejecute el comando **SqlBindR.exe** con el argumento */unbind* y especifique la instancia.

   Por ejemplo, el siguiente comando revierte la instancia predeterminada:
   
    `SqlBindR.exe /unbind MSSQL14.MSSQLSERVER`

<a name="step-2-restore"></a> 

###  <a name="step-2-repair-the-sql-server-instance"></a>Paso 2: Reparación de la instancia de SQL Server

Ejecute el programa de instalación de SQL Server para reparar la instancia del motor de base de datos que tiene las características de R y Python. Se conservan las actualizaciones existentes. El siguiente paso se aplica si se ha perdido una actualización para las actualizaciones de servicio en los paquetes de Python y R.

Solución alternativa: Desinstale completamente la instancia del motor de base de datos y reinstálela y, a continuación, aplique todas las actualizaciones de servicio.

<a name="step-3-reinstall-packages"></a> 

### <a name="step-3-add-any-third-party-packages"></a>Paso 3: Incorporación de paquetes de terceros

Es posible que haya agregado otros paquetes de código abierto o de terceros a la biblioteca de paquetes. Dado que al invertir el enlace se cambia la ubicación de la biblioteca de paquetes predeterminada, deberá volver a instalar los paquetes en la biblioteca que ahora usan R y Python. Para obtener más información, consulte la [información](../package-management/r-package-information.md) y la [instalación de paquetes de R](../package-management/install-additional-r-packages-on-sql-server.md), así como la [información](../package-management/python-package-information.md) y la [instalación de paquetes de Python](../package-management/install-additional-python-packages-on-sql-server.md).

## <a name="sqlbindrexe-command-syntax"></a>Sintaxis de comandos de SqlBindR.exe

### <a name="usage"></a>Uso

`sqlbindr [/list] [/bind <SQL_instance_ID>] [/unbind <SQL_instance_ID>]`

### <a name="parameters"></a>Parámetros

|Nombre|Descripción|
|------|------|
|*list*| Muestra una lista de todos los id. de instancia de SQL Server en el equipo actual.|
|*bind*| Actualiza la instancia de SQL Server especificada a la versión más reciente de R Server y garantiza que la instancia obtenga automáticamente las actualizaciones futuras de R Server.|
|*unbind*|Desinstala la versión más reciente de R Server de la instancia de SQL Server especificada e impide que las actualizaciones futuras de R Server afecten a la instancia.|

<a name="sqlbindr-error-codes"><a/>

## <a name="binding-errors"></a>Errores de enlace

El instalador de Machine Learning Server y SqlBindR devuelven los siguientes mensajes y códigos de error.

|Código de error  | Message           | Detalles               |
|------------|-------------------|-----------------------|
|Error de enlace 0 | Correcto | Enlace pasado sin errores. |
|Error de enlace 1 | Argumentos no válidos | Error de sintaxis. |
|Error de enlace 2 | Acción no válida | Error de sintaxis. |
|Error de enlace 3 | Instancia no válida | Existe una instancia, pero no es válida para el enlace. |
|Error de enlace 4 | No enlazable | |
|Error de enlace 5 | Ya enlazado | Ha ejecutado el comando *bind* , pero la instancia especificada ya está enlazada. |
|Error de enlace 6 | Error de enlace | Se produjo un error al desenlazar la instancia. Este error puede producirse si se ejecuta el instalador de Machine Learning Server sin seleccionar ninguna característica. El enlace requiere que se seleccione una instancia de MSSQL y R y Python, dando por sentado que la instancia es SQL Server 2017. Este error también se produce si SqlBindR no se pudo escribir en la carpeta Archivos de programa. Las sesiones abiertas o los identificadores en SQL Server producirán este error. Si recibe este error, reinicie el equipo y repita los pasos de enlace antes de iniciar más sesiones.|
|Error de enlace 7 | Sin enlace | La instancia del motor de base de datos tiene R Services o SQL Server Machine Learning Services. La instancia no está enlazada con Microsoft Machine Learning Server. |
|Error de enlace 8 | Error de desenlace | Se produjo un error al desenlazar la instancia. |
|Error de enlace 9 | No se encontraron instancias | No se encontraron instancias del motor de base de datos en este equipo. |

## <a name="known-issues"></a>Problemas conocidos

En esta sección se enumeran los problemas conocidos específicos a la hora de usar la utilidad SqlBindR.exe o de las actualizaciones de Machine Learning Server que pueden afectar a las instancias de SQL Server.

### <a name="restoring-packages-that-were-previously-installed"></a>Restauración de paquetes instalados previamente

SqlBindR.exe no puede restaurar los paquetes originales o los componentes de R con la actualización a Microsoft R Server 9.0.1. Use reparar en la instancia de SQL Server y aplique todas las versiones de servicio. Reinicie la instancia.

Una versión posterior de SqlBindR restaura automáticamente las características originales de R, de manera que no es necesario reinstalar los componentes de R o volver a revisar el servidor. Sin embargo, debe instalar todas las actualizaciones de paquetes de R que se hayan agregado después de la instalación inicial.

Use los comandos de R para sincronizar los paquetes instalados con el sistema de archivos mediante registros en la base de datos. Para obtener más información, consulte [Administración de paquetes de R para SQL Server](../package-management/install-additional-r-packages-on-sql-server.md).

### <a name="problems-with-overwritten-sqlbinrini-file-in-sql-server"></a>Problemas con el archivo sqlbinr.ini sobrescrito en SQL Server

Escenario: Este problema se produce al enlazar Machine Learning Server 9.4.7 a SQL Server 2017.  Al actualizar y enlazar Python, o al actualizar a una nueva CU, no entiende que Python está enlazado y sobrescribe los archivos. No hay ningún problema conocido con R.

Como alternativa, cree un archivo `sqlbindr.ini` en el directorio PYTHON_SERVICES que no esté vacío. El contenido no afecta al modo en el que funciona el archivo.

Cree un archivo `sqlbindr.ini`, que contenga **9.4.7.82**, y guárdelo en esta ubicación:  

`C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES`

### <a name="problems-with-multiple-upgrades-from-sql-server"></a>Problemas con varias actualizaciones de SQL Server

Escenario: Instancia actualizada anteriormente de SQL Server 2016 R Services a 9.0.1. Se ejecutó el nuevo instalador para Microsoft R Server 9.1.0. El instalador muestra una lista de todas las instancias válidas.
De forma predeterminada, el instalador selecciona las instancias enlazadas previamente. Si continúa, las instancias enlazadas anteriormente se desenlazarán. Como resultado, se quita la instalación 9.0.1 anterior, incluidos los paquetes relacionados, pero no se instalará la nueva versión de Microsoft R Server (9.1.0).

Como solución alternativa, puede modificar la instalación existente de R Server como se indica a continuación:
1. En el Panel de control, abra **Agregar o quitar programas**.
2. Busque Microsoft R Server y seleccione **Cambiar/modificar**.
3. Cuando se inicie el instalador, seleccione las instancias que quiera enlazar a la versión 9.1.0.

Microsoft Machine Learning Server 9.2.1 y 9.3 no presentan este problema.

### <a name="binding-or-unbinding-leaves-multiple-temporary-folders"></a>El enlace o desenlace deja varias carpetas temporales

Quite las carpetas temporales una vez completada la instalación.

> [!NOTE]
> Espere a que haya concluido la instalación. Quitar las bibliotecas de R asociadas a una versión y agregar las nuevas bibliotecas de R puede tardar mucho tiempo. Una vez finalizada la operación, se quitarán las carpetas temporales.

## <a name="see-also"></a>Consulte también

+ [Cambio de la versión predeterminada del entorno de ejecución del lenguaje R o Python](./change-default-language-runtime-version.md)
+ [Instalación de Machine Learning Server para Windows (con conexión a Internet)](/machine-learning-server/install/machine-learning-server-windows-install)
+ [Instalación de Machine Learning Server para Windows (sin conexión)](/machine-learning-server/install/machine-learning-server-windows-offline)
+ [Problemas conocidos de Machine Learning Server](/machine-learning-server/resources-known-issues)
+ [Anuncios de características de la versión anterior de R Server](/r-server/whats-new-in-r-server)
+ [Características en desuso, ya no admitidas o modificadas](/machine-learning-server/resources-deprecated-features)