---
title: Extensión de PowerShell
description: Obtenga información sobre cómo instalar y usar la extensión de PowerShell de Azure Data Studio, que proporciona una amplia compatibilidad con el editor de PowerShell para escribir y depurar scripts.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: SQLvariant
ms.author: aanelson
ms.reviewer: alayu, maghan, sstein
ms.custom: ''
ms.date: 04/19/2019
ms.openlocfilehash: 4de6c653503458ba839147b9a68b457330345755
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048505"
---
# <a name="powershell-editor-support-for-azure-data-studio"></a>Compatibilidad del Editor de PowerShell para Azure Data Studio

Esta extensión proporciona compatibilidad avanzada del Editor de PowerShell con [Azure Data Studio](https://github.com/Microsoft/azuredatastudio).
Ahora, puede escribir y depurar scripts de PowerShell mediante la excelente interfaz tipo IDE que proporciona Azure Data Studio.

![Extensión de PowerShell](media/powershell-extension/powershell-extension.png)

## <a name="features"></a>Características

- Resaltado de sintaxis
- Fragmentos de código
- IntelliSense para cmdlets y más
- Análisis basado en reglas proporcionado por el [Analizador de scripts de PowerShell](http://github.com/PowerShell/PSScriptAnalyzer)
- Ir a la definición de cmdlets y variables
- Buscar referencias de cmdlets y variables
- Detección de símbolos de documentos y área de trabajo
- Ejecutar la selección de código de PowerShell mediante <kbd>F8</kbd>
- Iniciar la ayuda en línea para el símbolo debajo del cursor con <kbd>Ctrl</kbd>+<kbd>F1</kbd>
- Compatibilidad básica de la consola interactiva.

## <a name="installing-the-extension"></a>Instalar la extensión

Para instalar la versión oficial de la extensión de PowerShell, siga el procedimiento que se indica en la [documentación de Azure Data Studio](./add-extensions.md).
En el panel Extensiones, busque la extensión “PowerShell” e instálela.  Recibirá notificaciones automáticamente sobre las futuras actualizaciones de la extensión.

También puede instalar un paquete VSIX desde la [página Versiones](https://github.com/PowerShell/vscode-powershell/releases) mediante la línea de comandos:

```powershell
azuredatastudio --install-extension PowerShell-<version>.vsix
```

## <a name="platform-support"></a>Compatibilidad con plataformas

- **Windows 7 a Windows 10** con Windows PowerShell v3 y posteriores, y PowerShell Core
- **Linux** con PowerShell Core (todas las distribuciones compatibles con PowerShell)
- **macOS** con PowerShell Core

Vea las [preguntas más frecuentes](https://github.com/PowerShell/vscode-powershell/wiki/FAQ) para obtener respuestas a preguntas comunes.

## <a name="installing-powershell-core"></a>Instalación de PowerShell Core

Si ejecuta Azure Data Studio en macOS o Linux, puede que también necesite instalar PowerShell Core.

PowerShell Core es un proyecto de código abierto hospedado en [GitHub](https://github.com/powershell/powershell).
Para obtener más información sobre cómo instalar PowerShell Core en las plataformas macOS o Linux, vea los artículos siguientes:

- [Instalación de PowerShell Core en Linux](/powershell/scripting/install/installing-powershell-core-on-linux)
- [Instalación de PowerShell Core en macOS](/powershell/scripting/install/installing-powershell-core-on-macos)

## <a name="example-scripts"></a>Scripts de ejemplo

Estos son algunos scripts de ejemplo de la carpeta `examples` de la extensión que puede usar para descubrir funciones de edición y depuración de PowerShell.  Vea el archivo [README.md](https://github.com/PowerShell/vscode-powershell/blob/master/examples/README.md) incluido para obtener más información sobre cómo usarlas.

Esta carpeta se encuentra en la ruta siguiente:

```powershell
$HOME/.azuredatastudio/extensions/ms-vscode.PowerShell-<version>/examples
```

o si usa la versión preliminar de la extensión.

 ```powershell
$HOME/.azuredatastudio/extensions/ms-vscode.powershell-preview-<version>/examples
```

Para abrir o ver los ejemplos de la extensión en Azure Data Studio, ejecute el código siguiente desde el símbolo del sistema de PowerShell:

```powershell
azuredatastudio (Get-ChildItem $Home\.azuredatastudio\extensions\ms-vscode.PowerShell-*\examples)[-1]
```

### <a name="creating-and-opening-files"></a>Crear y abrir archivos

Para crear y abrir un archivo dentro del editor, use la opción New-EditorFile desde el terminal integrado de PowerShell.

```powershell
PS C:\temp> New-EditorFile ExportData.ps1
```

Este comando no funciona con cualquier tipo de archivo, no solo con los archivos de PowerShell.

```powershell
PS C:\temp> New-EditorFile ImportData.py
```

Para abrir uno o más archivos en Azure Data Studio, use el comando `Open-EditorFile`.

```powershell
Open-EditorFile ExportData.ps1, ImportData.py
```

### <a name="no-focus-on-console-when-executing"></a>Sin foco en la consola al ejecutar

Los usuarios que suelan trabajar con SSMS estarán acostumbrados a ejecutar una consulta y ser capaces de volverla al ejecutar sin tener que cambiar al panel de consulta.  En este caso, el comportamiento predeterminado del editor de código puede resultarle extraño.  Para mantener el foco en el editor al ejecutar con <kbd>F8</kbd>, cambie la opción siguiente:

```json
"powershell.integratedConsole.focusConsoleOnExecute": false
```

El valor predeterminado es `true` con fines de accesibilidad.

Tenga en cuenta que este valor impedirá que el foco cambie a la consola, incluso si usa un comando que realice llamadas explícitas de entrada, como `Get-Credential`.

## <a name="sql-powershell-examples"></a>Ejemplos de PowerShell T-SQL

Para poder usar los ejemplos que aparecen a continuación, necesita instalar el módulo SqlServer desde la [Galería de PowerShell](https://www.powershellgallery.com/packages/SqlServer).

```powershell
Install-Module -Name SqlServer
```

> [!NOTE]
> Con la versión `21.1.18102` y posteriores, el módulo `SqlServer` es compatible con [PowerShell Core](https://github.com/PowerShell/PowerShell) 6.2 y posteriores, además de Windows PowerShell.

En este ejemplo, usamos el cmdlet `Get-SqlInstance` para obtener los objetos de SMO de servidor para los ServerA y ServerB.  En el resultado predeterminado de este comando, se incluyen el nombre de la instancia, la versión, el Service Pack y el nivel de actualización acumulativa de las instancias.

```powershell
Get-SqlInstance -ServerInstance ServerA, ServerB
```

Este es un ejemplo de la apariencia del resultado:

```console
Instance Name             Version    ProductLevel UpdateLevel  HostPlatform HostDistribution
-------------             -------    ------------ -----------  ------------ ----------------
ServerA                   13.0.5233  SP2          CU4          Windows      Windows Server 2016 Datacenter
ServerB                   14.0.3045  RTM          CU12         Linux        Ubuntu
```

El módulo `SqlServer` contiene un proveedor denominado `SQLRegistration` que permite acceder mediante programación a los siguientes tipos de conexiones de SQL Server guardadas:

+ Servidor de motor de base de datos (servidores registrados)
+ Servidor de administración central (CMS)
+ Analysis Services
+ Integration Services
+ Reporting Services

 En el ejemplo siguiente, realizaremos una operación de `dir` (alias de `Get-ChildItem`) para obtener la lista de todas las instancias de SQL Server del archivo de servidores registrados.

```powershell
dir 'SQLSERVER:\SQLRegistration\Database Engine Server Group' -Recurse
```

Este es un ejemplo de la apariencia del resultado:

```powershell
Mode Name
---- ----
-    ServerA
-    ServerB
-    localhost\SQL2017
-    localhost\SQL2016Happy
-    localhost\SQL2017
```

Puede usar el cmdlet `Get-SqlDatabase` para muchas operaciones que implican una base de datos u objetos contenidos en una.  Si proporciona valores para los parámetros `-ServerInstance` y `-Database`, solo se recuperará ese objeto de base de datos.  Pero, si solo especifica el parámetro `-ServerInstance`, se devolverá una lista completa de todas las bases de datos de esa instancia.

Este es un ejemplo de la apariencia del resultado:

```console
Name                 Status           Size     Space  Recovery Compat. Owner
                                            Available  Model     Level
----                 ------           ---- ---------- -------- ------- -----
AdventureWorks2017   Normal      336.00 MB   57.01 MB Simple       140 sa
master               Normal        6.00 MB  368.00 KB Simple       140 sa
model                Normal       16.00 MB    5.53 MB Full         140 sa
msdb                 Normal       48.44 MB    1.70 MB Simple       140 sa
PBIRS                Normal      144.00 MB   55.95 MB Full         140 sa
PBIRSTempDB          Normal       16.00 MB    4.20 MB Simple       140 sa
SSISDB               Normal      325.06 MB   26.21 MB Full         140 sa
tempdb               Normal       72.00 MB   61.25 MB Simple       140 sa
WideWorldImporters   Normal         3.2 GB     2.6 GB Simple       130 sa
```

En el ejemplo siguiente, se usa el cmdlet `Get-SqlDatabase` para recuperar una lista de todas las bases de datos en la instancia de ServerB y, después, se presenta una cuadrícula o tabla (mediante el cmdlet `Out-GridView`) para seleccionar las bases de datos para las que se realizarán copias de seguridad.  Cuando el usuario hace clic en el botón “Aceptar”, solo se realizarán copias de seguridad de las bases de datos resaltadas.

```powershell
Get-SqlDatabase -ServerInstance ServerB |
Out-GridView -PassThru |
Backup-SqlDatabase -CompressionOption On
```

En este ejemplo, se obtiene una lista de todas las instancias de SQL Server del archivo de servidores registrados y, después, se realiza una llamada a `Get-SqlAgentJobHistory`, que informa de todos los trabajos del Agente SQL con errores desde medianoche para todas las instancias de SQL Server de la lista.

```powershell
dir 'SQLSERVER:\SQLRegistration\Database Engine Server Group' -Recurse |
WHERE {$_.Mode -ne 'd' } |
FOREACH {
    Get-SqlAgentJobHistory -ServerInstance  $_.Name -Since Midnight -OutcomesType Failed
}
```

En este ejemplo, realizaremos una operación de `dir` (alias de `Get-ChildItem`) para obtener la lista de todas las instancias de SQL Server del archivo de servidores registrados y, después, usaremos el cmdlet `Get-SqlDatabase` para obtener una lista de las bases de datos de cada una de esas instancias.

```powershell
dir 'SQLSERVER:\SQLRegistration\Database Engine Server Group' -Recurse |
WHERE { $_.Mode -ne 'd' } |
FOREACH {
    Get-SqlDatabase -ServerInstance $_.Name
}
```

Este es un ejemplo de la apariencia del resultado:

```console
Name                 Status           Size  Space     Recovery Compat. Owner
                                            Available Model    Level
----                 ------           ---- ---------- -------- ------- -----
AdventureWorks2017   Normal      336.00 MB   57.01 MB Simple       140 sa
master               Normal        6.00 MB  368.00 KB Simple       140 sa
model                Normal       16.00 MB    5.53 MB Full         140 sa
msdb                 Normal       48.44 MB    1.70 MB Simple       140 sa
PBIRS                Normal      144.00 MB   55.95 MB Full         140 sa
PBIRSTempDB          Normal       16.00 MB    4.20 MB Simple       140 sa
SSISDB               Normal      325.06 MB   26.21 MB Full         140 sa
tempdb               Normal       72.00 MB   61.25 MB Simple       140 sa
WideWorldImporters   Normal         3.2 GB     2.6 GB Simple       130 sa
```

## <a name="reporting-problems"></a>Informar de problemas

Si tiene algún problema con la extensión de PowerShell, vea los [documentos de solución de problemas](https://github.com/PowerShell/vscode-powershell/blob/master/docs/troubleshooting.md) para obtener información sobre cómo diagnosticar problemas e informar sobre estos.

### <a name="security-note"></a>Nota de seguridad

En el caso de problemas de seguridad, vea [este recurso](https://github.com/PowerShell/vscode-powershell/blob/master/docs/troubleshooting.md#note-on-security).

## <a name="contributing-to-the-code"></a>Colaborar en el desarrollo del código

Para obtener más información sobre cómo colaborar en el desarrollo de esta extensión, vea la [documentación de desarrollo](https://github.com/PowerShell/vscode-powershell/blob/master/docs/development.md).

## <a name="maintainers"></a>Mantenedores

- [Keith Hill](https://github.com/rkeithhill) - [@r_keith_hill](http://twitter.com/r_keith_hill)
- Tyler Leonhardt - [@TylerLeonhardt](http://twitter.com/tylerleonhardt)
- [Rob Holt](https://github.com/rjmholt)

## <a name="license"></a>Licencia

Esta extensión se [concederá bajo la licencia MIT](https://github.com/PowerShell/vscode-powershell/blob/master/LICENSE.txt). Para obtener más información sobre los archivos binarios de terceros que incluimos con las versiones de este proyecto, vea el archivo de [avisos de terceros](https://github.com/PowerShell/vscode-powershell/blob/master/Third%20Party%20Notices.txt).

## <a name="code-of-conduct"></a>Código de conducta

El proyecto ha adoptado el [Código de conducta de código abierto de Microsoft][conduct-code].
Para obtener más información, vea las [Preguntas más frecuentes sobre el código de conducta][conduct-FAQ], o póngase en contacto con [opencode@microsoft.com][conduct-email] si tiene preguntas o comentarios.

[conduct-code]: https://opensource.microsoft.com/codeofconduct/
[conduct-FAQ]: https://opensource.microsoft.com/codeofconduct/faq/
[conduct-email]: mailto:opencode@microsoft.com