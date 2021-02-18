---
title: Buscar y reemplazar
description: Obtenga información sobre cómo usar el cuadro de diálogo Buscar y reemplazar para buscar y reemplazar texto.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Find and Replace dialog box
ms.assetid: 09297893-d80b-4c88-86b4-52bfb639e521
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bd45603e4254be161b6e80a59e969f61784cc987
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354662"
---
# <a name="find-and-replace"></a>Buscar y reemplazar
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Utilice el cuadro de diálogo **Buscar y reemplazar** para buscar texto en un archivo y, opcionalmente, reemplazarlo. Pueden aparecer versiones del cuadro de diálogo **Buscar y reemplazar** con opciones ligeramente distintas, en función de la forma en que se haya abierto el cuadro de diálogo. En el menú **Editar** , seleccione **Buscar y reemplazar** y, a continuación, haga clic en **Búsqueda rápida** para abrir el cuadro de diálogo con las opciones de búsqueda, pero sin las opciones de reemplazo. En el menú **Editar** , seleccione **Buscar y reemplazar** y, a continuación, haga clic en **Reemplazo rápido** para abrir el cuadro de diálogo con las opciones de búsqueda y de reemplazo.  
  
 También hay botones de la barra de la herramientas y teclas de método abreviado disponibles para abrir el cuadro de diálogo **Buscar y reemplazar** .  
  
## <a name="find-what"></a>Buscar  
 Estos controles le permiten especificar la cadena o expresión para la búsqueda de coincidencias.  
  
 **Buscar**  
 Escriba el texto que desea buscar. El cuadro de diálogo intentará rellenar el texto de búsqueda probable utilizando el texto seleccionado con el cursor antes de abrir el cuadro de diálogo, o bien texto cercano o texto que se haya buscado con anterioridad. Podrá reutilizar una de las últimas 20 cadenas de búsqueda seleccionándola en esta lista desplegable.  
  
 **[cadena con caracteres comodín]**  
 Si quiere usar caracteres comodín, como asteriscos (`*`) y signos de interrogación de cierre (`?`), en la cadena de búsqueda, active la casilla **Usar** en **Opciones de búsqueda** y haga clic en **Caracteres comodín**.  
  
 **[expresión regular]**  
 Para que el motor de búsqueda interprete la cadena de búsqueda como una expresión regular, active la casilla **Usar** en **Opciones de búsqueda** y haga clic en **Expresiones regulares**.  
  
 **Generador de expresiones**  
 Este botón triangular, situado junto al cuadro **Buscar** , estará disponible cuando se active la casilla **Usar** en **Opciones de búsqueda**. Haga clic en este botón para mostrar una lista de caracteres comodín o expresiones regulares, en función de la opción **Usar** seleccionada. Cuando se elige un elemento de esta lista, se agrega a la cadena especificada en el cuadro **Buscar** .  
  
## <a name="replace-with"></a>Reemplazar por  
 Estos controles le permiten especificar qué debe insertarse en lugar de la cadena o expresión coincidente.  
  
 **Replace with**  
 Para reemplazar instancias de la cadena especificada en **Buscar** por otra cadena, escriba la cadena de reemplazo en este campo. Para eliminar instancias del texto especificado en **Buscar**, deje este campo en blanco. Seleccione la lista desplegable para que se muestren los últimos 20 elementos escritos. Para incluir expresiones regulares en la cadena especificada en el cuadro **Reemplazar con** , haga clic en la casilla **Usar** y, a continuación, haga clic en **Expresiones regulares**. Este cuadro solo aparecerá si este cuadro de diálogo se abrió haciendo clic en **Reemplazo rápido**.  
  
 **Replace with**  
 Para reemplazar instancias de la cadena especificada en el cuadro **Buscar** por otra cadena, escriba la cadena de reemplazo en este campo. Para eliminar instancias de la cadena especificada en el cuadro **Buscar** , deje este campo en blanco. Seleccione la lista desplegable para que se muestren los últimos 20 elementos escritos. Para incluir expresiones regulares en la cadena especificada en el cuadro **Reemplazar con** , haga clic en la casilla **Usar** y, a continuación, haga clic en **Expresiones regulares**.  
  
 **Generador de expresiones**  
 Este botón triangular, situado junto al cuadro **Reemplazar con** estará disponible cuando se active la casilla **Usar** en **Opciones de búsqueda**. Haga clic en este botón para mostrar una lista de caracteres comodín o expresiones regulares, en función de la opción **Usar** seleccionada. Cuando se hace clic un elemento de esta lista, se agrega a la cadena especificada en el cuadro **Reemplazar con** .  
  
 **Sustituya**  
 Haga clic en este botón para reemplazar la instancia actual de la cadena especificada en el cuadro **Buscar** por la cadena especificada en el cuadro **Reemplazar con** y buscar la siguiente instancia dentro del ámbito especificado en **Buscar en**.  
  
 **Reemplazar todo**  
 Haga clic en este botón para reemplazar todas las instancias de la cadena especificada en el cuadro **Buscar** por la cadena especificada en el cuadro **Reemplazar con** en todos los archivos del ámbito especificado en **Buscar en** .  
  
> [!CAUTION]  
>  Asegúrese de que **Buscar en** esté establecido de modo que solo incluya los archivos que quiere modificar.  
  
 Se muestra un aviso que incluye la opción **Mantener archivos modificados abiertos** . Para conservar la opción **Deshacer** , deberá seleccionar esta opción. La opción **Deshacer** solo estará disponible en los archivos que permanezcan abiertos para editarlos después de haberlos modificado.  
  
 **Omitir archivo**  
 Esta opción estará disponible cuando el valor especificado en **Buscar en** incluya varios archivos. Haga clic en este botón si no desea realizar búsquedas en el archivo actual o si no desea modificarlo. La búsqueda continuará en el siguiente archivo de la lista de **Buscar en**.  
  
## <a name="look-in"></a>Buscar en  
 **Look in**  
 Seleccione la ubicación donde debe buscarse el texto especificado en **Buscar**. Las opciones son **Documento actual**, que busca en la ventana de documento que estaba seleccionada al abrir el cuadro de diálogo, y **Todos los documentos abiertos**, que busca en todas las ventanas de documentos abiertas en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
## <a name="find-options"></a>Opciones de búsqueda  
 Puede expandir y contraer la sección **Opciones de búsqueda** . Pueden activarse y desactivarse las siguientes opciones.  
  
 **Coincidir mayúsculas y minúsculas**  
 Cuando se active esta casilla, en las ventanas Resultados de la búsqueda solo se mostrarán las instancias de la cadena especificada en **Buscar** que coincidan tanto en contenido como en el uso de mayúsculas y minúsculas. Por ejemplo, una búsqueda de "**MyObject**" con la casilla **Coincidir mayúsculas y minúsculas** activada devolverá "MyObject", pero no "myobject" ni "MYOBJECT".  
  
 **Solo palabras completas**  
 Cuando se active esta opción, en las ventanas Resultados de la búsqueda solo se mostrarán las instancias de la cadena especificada en **Buscar** cuyas palabras completas coincidan. Por ejemplo, una búsqueda de **MyObject** devolverá "MyObject", pero no devolverá "CMyObject" ni "MyObjectC".  
  
 **Buscar hacia atrás**  
 Busca desde la posición del cursor hacia el principio del documento.  
  
 **Buscar en texto oculto**  
 Busca instancias del texto que están ocultas y texto contraído.  
  
 **Uso**  
 Indica cómo se interpretan los caracteres especiales especificados en los cuadros de texto **Buscar** o **Reemplazar con** . Esta opción incluye **Caracteres comodín** y **Expresiones regulares**.  
  
 **Regular Expressions**  
 Notaciones especiales que definen patrones de texto a partir de los cuales buscar coincidencias. Para obtener una lista, vea [Buscar texto mediante expresiones regulares](./search-text-with-regular-expressions.md).  
  
 **Caracteres comodín**  
 Caracteres especiales, como asteriscos (`*`) y signos de interrogación de cierre (`?`), que representan uno o varios caracteres. Para obtener una lista, vea [Buscar texto con caracteres comodín](./search-text-with-wildcards.md).  
  
 **Buscar siguiente**  
 Comienza a buscar el texto del cuadro **Buscar** .  
  
 **Sustituya**  
 Haga clic en este botón para reemplazar la instancia actual de la cadena especificada en **Buscar** por la cadena especificada en **Reemplazar con** y buscar la siguiente instancia dentro del ámbito especificado en **Buscar en**.  
  
 **Replace All**  
 Elija este botón para reemplazar todas las instancias de la cadena especificada en **Buscar** por la cadena especificada en **Reemplazar con** en todos los archivos del ámbito especificado en **Buscar en**.  
  
> [!CAUTION]  
>  Asegúrese de que **Buscar en** esté establecido de modo que solo incluya los archivos que quiere modificar.  
  
## <a name="find-and-replace-views"></a>Vistas Buscar y reemplazar  
 Las pestañas de la parte superior de la ventana Buscar y reemplazar incluyen menús **Ver** . Estos menús le permiten elegir un conjunto de campos para que se muestren en el panel activo. Puede dejar la ventana **Buscar y reemplazar** acoplada en una ubicación adecuada y, después, cambiar de pestaña y de vista para realizar cualquier tipo de operación de búsqueda y reemplazo.  
  
 **Búsqueda rápida**  
 Esta pestaña de la barra de herramientas cambia el cuadro de diálogo a **Búsqueda rápida** .  
  
 **Buscar en archivos**  
 Esta pestaña de la barra de herramientas cambia el cuadro de diálogo a **Buscar en archivos** .  
  
 **Reemplazo rápido**  
 Esta pestaña de la barra de herramientas cambia el cuadro de diálogo a **Reemplazo rápido** .  
  
 **Reemplazar en archivos**  
 Esta pestaña de la barra de herramientas cambia el cuadro de diálogo a **Reemplazar en archivos** .  
  
## <a name="see-also"></a>Consulte también  
 [Métodos abreviados de teclado de SQL Server Management Studio](../../ssms/sql-server-management-studio-keyboard-shortcuts.md)