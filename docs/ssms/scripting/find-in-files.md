---
title: Buscar en archivos
description: Obtenga información sobre cómo buscar una cadena o expresión en un conjunto de archivos especificado y cómo especificar la ubicación de los resultados de la búsqueda.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Find in Files dialog box
ms.assetid: bf92770a-33df-43ef-85ad-5a9223649b98
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 642f0a2f82d279357300d87db3821496d881eb11
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271655"
---
# <a name="find-in-files"></a>Buscar en archivos
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  La pestaña **Buscar en archivos** de la ventana Buscar y reemplazar permite buscar en el código de un conjunto especificado de archivos una cadena o expresión. Las coincidencias que se encuentran y las acciones que se toman aparecen en la ventana Resultados de la búsqueda seleccionada en **Opciones de resultados**.  
  
 También hay botones de la barra de la herramientas y teclas de método abreviado disponibles para abrir el cuadro de diálogo **Buscar y reemplazar** .  
  
 Las secciones siguientes presentan los controles disponibles en la pestaña **Buscar en archivos** .  
  
## <a name="find-what"></a>Buscar  
 Estos controles de la pestaña **Buscar en archivos** permiten especificar la cadena o expresión que se buscará.  
  
 **Buscar**  
 Escriba el texto que desea buscar. El cuadro de diálogo intentará rellenar el texto de búsqueda probable utilizando el texto seleccionado con el cursor antes de abrir el cuadro de diálogo, o bien texto cercano o texto que se haya buscado con anterioridad. Podrá reutilizar una de las últimas 20 cadenas de búsqueda seleccionándola en esta lista desplegable.  
  
 **[cadena con caracteres comodín]**  
 Si quiere usar caracteres comodín, como asteriscos (`*`) y signos de interrogación de cierre (`?`), en la cadena de búsqueda, active la casilla **Usar** en **Opciones de búsqueda** y haga clic en **Caracteres comodín**.  
  
 **[expresión regular]**  
 Para que el motor de búsqueda interprete la cadena de búsqueda como una expresión regular, active la casilla **Usar** en **Opciones de búsqueda** y haga clic en **Expresiones regulares**.  
  
 **Generador de expresiones**  
 Este botón triangular, situado junto al cuadro **Buscar** , estará disponible cuando se active la casilla **Usar** en **Opciones de búsqueda**. Haga clic en este botón para mostrar una lista de caracteres comodín o expresiones regulares, en función de la opción **Usar** seleccionada. Si elige un elemento de esta lista, se agrega a la cadena de **Buscar** .  
  
## <a name="look-in"></a>Buscar en  
 La opción elegida en **Buscar en** determina si **Buscar en archivos** busca únicamente en los archivos activos o en todos los archivos almacenados en determinadas carpetas. Seleccione un ámbito de búsqueda en la lista, escriba una ruta de acceso a una carpeta o haga clic en el botón **Examinar** para mostrar el cuadro de diálogo **Elegir carpetas de búsqueda** y elija un conjunto de carpetas para realizar la búsqueda.  
  
> [!NOTE]  
>   Si la opción **Buscar en** seleccionada hace que busque un archivo que ha desprotegido del control de código fuente, solo se buscará en la versión del archivo descargada en el equipo local.  
  
 **Look in**  
 Seleccione un ámbito predefinido de búsqueda en esta lista, o use el cuadro de diálogo **Elegir carpetas de búsqueda** para escribir su propio grupo de directorios.  
  
 **Documento actual**  
 Esta opción está disponible cuando se abre un documento en un editor. Solo busca la cadena de **Buscar** en el documento activo.  
  
 **Todos los documentos abiertos**  
 Busca en todos los archivos abiertos actualmente para edición.  
  
 **Proyecto actual**  
 Busca en todos los archivos del proyecto activo.  
  
 **Toda la solución**  
 Busca en todos los archivos de la solución activa.  
  
 **Incluir subcarpetas**  
 Especifica que se buscarán las subcarpetas de la carpeta especificada en **Buscar en** . Requiere un grupo de directorios personalizados.  
  
 **Browse**  
 Haga clic en este botón para que aparezca el cuadro de diálogo **Elegir carpetas de búsqueda** , donde puede reunir, editar, guardar y seleccionar los grupos de directorios con nombre que se van a usar en el cuadro **Buscar en** .  
  
## <a name="find-options"></a>Opciones de búsqueda  
 Puede expandir y contraer la sección **Opciones de búsqueda** . Pueden activarse y desactivarse las siguientes opciones.  
  
 **Coincidir mayúsculas y minúsculas**  
 Cuando se active esta casilla, en las ventanas Resultados de la búsqueda solo se mostrarán las instancias de la cadena especificada en **Buscar** que coincidan tanto en contenido como en el uso de mayúsculas y minúsculas. Por ejemplo, una búsqueda de **MiObjeto** con la casilla **Coincidir mayúsculas y minúsculas** activada presentaría en los resultados "MiObjeto", pero no "miobjeto" ni "MIOBJETO".  
  
 **Solo palabras completas**  
 Cuando esta casilla se activa, las ventanas Resultados de la búsqueda solo mostrarán las instancias de la cadena especificada en **Buscar** cuyas palabras completas coincidan. Por ejemplo, una búsqueda de **MyObject** devolverá "MyObject", pero no devolverá "CMyObject" ni "MyObjectC".  
  
 **Uso**  
 Indica cómo se interpretan los caracteres especiales especificados en los cuadros de texto **Buscar** o **Reemplazar con** . Esta opción incluye **Caracteres comodín** y **Expresiones regulares**.  
  
 **Regular Expressions**  
 Notaciones especiales que definen patrones de texto a partir de los cuales buscar coincidencias. Para obtener una lista, vea [Buscar texto mediante expresiones regulares](./search-text-with-regular-expressions.md).  
  
 **Caracteres comodín**  
 Caracteres especiales, como asteriscos (`*`) y signos de interrogación de cierre (`?`), que representan uno o varios caracteres. Para obtener una lista, vea [Buscar texto con caracteres comodín](./search-text-with-wildcards.md).  
  
 **Buscar en estos tipos de archivo**  
 Esta lista indica los tipos de archivos que se van a buscar en los directorios especificados en **Buscar en**. Si el campo se deja en blanco, se busca en todos los archivos de los directorios especificados en **Buscar en** .  
  
```  
*.[ext]; *.[ext] (manual)  
```  
  
 Para encontrar archivos de un tipo particular, escriba un carácter comodín asterisco (`*`) para el nombre de archivo, seguido de un punto (`.`) y de la extensión del archivo. Para encontrar más de un tipo, escriba varias cadenas de búsqueda separadas por un punto y coma (`;`).  
  
```  
*.[ext]; *.[ext] (from list)  
```  
  
 Seleccione cualquier elemento de la lista para indicar una cadena de búsqueda preconfigurada que buscará archivos de tipos específicos.  
  
## <a name="result-options"></a>Opciones de resultados  
 Determina la ubicación de los resultados cuando hace clic en **Buscar todos**. Puede expandir o contraer la sección **Opciones de resultados** . Pueden activarse y desactivarse las siguientes opciones.  
  
 **Ventana Resultados de la búsqueda 1**  
 Cuando se activa esta casilla, los resultados de la búsqueda actual se anexarán al contenido de la ventana Resultados de la búsqueda 1. Esta ventana se abre automáticamente para mostrar los resultados de la búsqueda. Para abrir manualmente esta ventana, haga clic en **Otras ventanas** en el menú **Ver** y, a continuación, haga clic en **Resultados de la búsqueda 1**.  
  
 **Ventana Resultados de la búsqueda 2**  
 Cuando se activa esta casilla, los resultados de la búsqueda actual se anexarán al contenido de la ventana Resultados de la búsqueda 2. Esta ventana se abre automáticamente para mostrar los resultados de la búsqueda. Para abrir manualmente esta ventana, haga clic en **Otras ventanas** en el menú **Ver** y, a continuación, haga clic en **Resultados de la búsqueda 2**.  
  
 **Mostrar solo nombres de archivo**  
 Muestra una entrada por cada archivo que contiene una coincidencia de búsqueda, en lugar de una entrada por cada coincidencia en la ventana Resultados de la búsqueda 1 o Resultados de la búsqueda 2. Esta opción no está disponible en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 **Mantener los archivos modificados abiertos después de Reemplazar todo**  
 Cuando se selecciona, deja abiertos todos los archivos en los que se han realizado reemplazos, de forma que puede deshacerlos o cambiarlos. Las restricciones de memoria pueden limitar el número de archivos que pueden permanecer abiertos después de una operación de reemplazo.  
  
> [!CAUTION]  
>  Puede utilizar **Deshacer** solo en los archivos que permanecen abiertos para edición. Si no se selecciona esta opción, los archivos que no estén abiertos para edición permanecerán cerrados y no estará disponible la opción **Deshacer** para ellos.  
  
## <a name="find-and-replace-views"></a>Vistas Buscar y reemplazar  
 Las pestañas de la parte superior de la ventana Buscar y reemplazar incluyen menús **Ver** . Estos menús le permiten elegir un conjunto de campos para que se muestren en el panel activo. Puede dejar la ventana Buscar y reemplazar acoplada en una ubicación adecuada y después cambiar de pestaña y de vista para realizar cualquier tipo de operación de búsqueda y reemplazo.  
  
 **Cambiar a Búsqueda rápida**  
 Esta pestaña de la barra de herramientas cambia el cuadro de diálogo a **Búsqueda rápida** .  
  
 **Cambiar a Buscar en archivos**  
 Esta pestaña de la barra de herramientas cambia el cuadro de diálogo a **Buscar en archivos** .  
  
 **Cambiar a Buscar símbolo**  
 Esta pestaña de la barra de herramientas cambia el cuadro de diálogo a **Find in Symbols (Buscar en símbolos)** .  
  
## <a name="see-also"></a>Consulte también  
 [Métodos abreviados de teclado de SQL Server Management Studio](../../ssms/sql-server-management-studio-keyboard-shortcuts.md)