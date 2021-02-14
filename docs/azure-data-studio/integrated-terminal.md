---
title: Terminal integrado
description: Obtenga información sobre cómo abrir un terminal integrado en Azure Data Studio. Un terminal integrado puede resultar más cómodo que uno independiente.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: 054bfd71962a5775374e8faa567dc24242d846c1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048345"
---
# <a name="integrated-terminal"></a>Terminal integrado

En Azure Data Studio, puede abrir un terminal integrado, que se inicia al principio en la raíz de su área de trabajo. Esto le puede resultar práctico, ya que no tiene que cambiar de ventana ni modificar el estado de un terminal existente para realizar una tarea de línea de comandos rápida.

Para abrir el terminal:

* Use el método abreviado de teclado **Ctrl+`** con el carácter de acento grave.
* Use el comando de menú **Ver** | **Terminal integrado**.
* En la **paleta de comandos** (**Ctrl+Mayús+P**), use el comando **Ver: Alternar terminal integrado**.

![Terminal](media/integrated-terminal/terminal-screen.png)

> [!NOTE]
> Todavía puede abrir un shell externo con el comando **Abrir en símbolo del sistema** del explorador (**Abrir en terminal** en Mac o Linux) si prefiere trabajar fuera de Azure Data Studio.

## <a name="managing-multiple-terminals"></a>Administración de varios terminales

Puede crear varios terminales abiertos en diferentes ubicaciones y navegar fácilmente entre ellos. Las instancias del terminal se pueden agregar al hacer clic en el icono de signo más situado en la parte superior derecha del panel **TERMINAL** o al desencadenar el comando **Ctrl+Mayús+`** . De esta forma, se crea otra entrada en la lista desplegable que se puede usar para cambiar entre ellos.

![Varios terminales](media/integrated-terminal/terminal-multiple-instances.png)

Para quitar instancias del terminal, haga clic en el botón de la papelera.

> [!TIP]
> Si usa mucho varios terminales, puede agregar enlaces de teclado para los comandos `focusNext`, `focusPrevious` y `kill` descritos en la sección [Enlaces de teclado](#key-bindings) para permitir la navegación entre ellos solo con el teclado.

## <a name="configuration"></a>Configuración

El valor predeterminado del shell usado es `$SHELL` en Linux y macOS, PowerShell en Windows 10 y cmd.exe en versiones anteriores de Windows. Se pueden invalidar manualmente si se establece `terminal.integrated.shell.*` en [configuración](settings.md). Los argumentos se pueden pasar al shell del terminal en Linux y macOS con la configuración `terminal.integrated.shellArgs.*`.

### <a name="windows"></a>Windows

Para configurar correctamente el shell en Windows, hay que buscar el ejecutable correcto y actualizar la configuración. A continuación se muestra una lista de los ejecutables comunes del shell y sus ubicaciones predeterminadas:

```json
// 64-bit cmd if available, otherwise 32-bit
"terminal.integrated.shell.windows": "C:\\Windows\\sysnative\\cmd.exe"
// 64-bit PowerShell if available, otherwise 32-bit
"terminal.integrated.shell.windows": "C:\\Windows\\sysnative\\WindowsPowerShell\\v1.0\\powershell.exe"
// Git Bash
"terminal.integrated.shell.windows": "C:\\Program Files\\Git\\bin\\bash.exe"
// Bash on Ubuntu (on Windows)
"terminal.integrated.shell.windows": "C:\\Windows\\sysnative\\bash.exe"
```

> [!NOTE]
> Para usarse como un terminal integrado, el ejecutable del shell debe ser una aplicación de consola para que se pueda redirigir `stdin/stdout/stderr`.

> [!TIP]
> El shell del terminal integrado se ejecuta con los permisos de Azure Data Studio. Si necesita ejecutar un comando del shell con privilegios elevados (administrador) u otros permisos, puede usar las utilidades de la plataforma, como `runas.exe`, en un terminal.

### <a name="shell-arguments"></a>Argumentos del shell

Puede pasar argumentos al shell cuando se inicia.

Por ejemplo, para habilitar la ejecución de Bash como un shell de inicio de sesión (que ejecuta `.bash_profile`), pase el argumento `-l` (con comillas dobles):

```json
// Linux
"terminal.integrated.shellArgs.linux": ["-l"]
```

## <a name="terminal-display-settings"></a>Configuración de la pantalla del terminal

Puede personalizar la fuente y el alto de línea del terminal integrado con la siguiente configuración:

* `terminal.integrated.fontFamily`
* `terminal.integrated.fontSize`
* `terminal.integrated.lineHeight`

## <a name="terminal-key-bindings"></a><a id="key-bindings"></a>Enlaces de teclado del terminal

El comando **Ver: Alternar terminal integrado** está enlazado con **Ctrl+`** para mostrar u ocultar rápidamente el panel del terminal integrado en la vista.

A continuación se muestran los métodos abreviados de teclado para navegar rápidamente por el terminal integrado:

|Clave|Get-Help|  
|---|---|  
|**Ctrl+\`**|Mostrar el terminal integrado|  
|**Ctrl+Mayús+\`**|Crear un terminal|  
|**Ctrl+Flecha arriba**|Desplazarse hacia arriba|  
|**Ctrl+Flecha abajo**|Desplazarse hacia abajo|  
|**Ctrl+RePág**|Retroceder una página|  
|**Ctrl+AvPág**|Avanzar una página|  
|**Ctrl+Inicio**|Desplazarse hasta el principio|  
|**Ctrl+Fin**|Desplazarse hasta la parte inferior|  
|**Ctrl+K**|Borrar el terminal|  

Hay disponibles otros comandos del terminal y se pueden enlazar a sus métodos abreviados de teclado preferidos.

Son las siguientes:

* `workbench.action.terminal.focus`: centre el terminal. Esto es parecido a la alternación, pero centra el terminal en lugar de ocultarlo, si está visible.
* `workbench.action.terminal.focusNext`: centra la siguiente instancia del terminal.
* `workbench.action.terminal.focusPrevious`: centra la instancia anterior del terminal.
* `workbench.action.terminal.kill`: quita la instancia actual del terminal.
* `workbench.action.terminal.runSelectedText`: ejecute el texto seleccionado en la instancia del terminal.
* `workbench.action.terminal.runActiveFile`: ejecute el archivo activo en la instancia del terminal.

### <a name="run-selected-text"></a>Ejecución del texto seleccionado

Para usar el comando `runSelectedText`, seleccione texto en un editor y ejecute el comando **Terminal: Ejecutar texto seleccionado en el terminal activo** mediante la **paleta de comandos** (**Ctrl+Mayús+P**). El terminal intenta ejecutar el texto seleccionado:

![Ejecución del texto seleccionado](media/integrated-terminal/terminal_run_selected.png)

Si no se selecciona ningún texto en el editor activo, se ejecuta la línea en la que está el cursor en el terminal.

### <a name="copy--paste"></a>Copiado y pegado

Los enlaces de teclado para copiar y pegar siguen los estándares de la plataforma:

* Linux: **Ctrl+Mayús+C** y **Ctrl+Mayús+V**
* Mac: **Cmd+C** y **Cmd+V**
* Windows: **Ctrl+C** y **Ctrl+V**

### <a name="find"></a>Buscar

El terminal integrado tiene una funcionalidad de búsqueda básica que se puede desencadenar con **Ctrl+F**.

Si quiere que **Ctrl+F** vaya al shell en lugar de iniciar el widget de búsqueda en Linux y Windows, debe quitar el enlace de teclado de la siguiente forma:

```js
{ "key": "ctrl+f", "command": "-workbench.action.terminal.focusFindWidget",
                      "when": "terminalFocus" },
```

### <a name="rename-terminal-sessions"></a>Cambio del nombre de las sesiones del terminal

Ahora se puede cambiar el nombre de las sesiones del terminal integrado mediante el comando **Terminal: Cambiar de nombre** (`workbench.action.terminal.rename`). El nuevo nombre se muestra en la lista desplegable de selección del terminal.

### <a name="forcing-key-bindings-to-pass-through-the-terminal"></a>Forzamiento del paso de los enlaces de teclado mediante el terminal

Mientras el foco está en el terminal integrado, muchos enlaces de teclado no funcionarán porque las pulsaciones de las teclas se pasan al propio terminal, quien las consume. Se puede usar la configuración `terminal.integrated.commandsToSkipShell` para solucionar este problema. Contiene una matriz de nombres de comando cuyos enlaces de teclado omiten el procesamiento del shell y, en su lugar, se procesan mediante el sistema de enlaces de teclado de Azure Data Studio. De forma predeterminada, esto incluye todos los enlaces de teclado del terminal, además de una selección de algunos enlaces de teclado de uso frecuente.

