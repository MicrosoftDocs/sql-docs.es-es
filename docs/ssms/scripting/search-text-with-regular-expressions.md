---
title: Buscar texto mediante expresiones regulares
description: Aprenda a usar una expresión regular en el campo "Buscar" de un cuadro de diálogo Buscar y reemplazar para especificar un patrón con el que comparar.
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vs.regularexpressionbuilder
helpviewer_keywords:
- regular expressions [SQL Server Management Studio]
- Query Editor [SQL Server Management Studio], regular expression searches
- searches [SQL Server Management Studio], regular expressions
ms.assetid: a057690c-d118-4159-8e4d-2ed5ccfe79d3
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f09834f72bbcf5932961045a803c57dc9702a242
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344171"
---
# <a name="search-text-with-regular-expressions"></a>Buscar texto mediante expresiones regulares

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Las expresiones regulares son una notación concisa y flexible para buscar y reemplazar patrones de texto. Se puede utilizar un conjunto específico de expresiones regulares en el campo **Buscar** del cuadro de diálogo [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] **Buscar y reemplazar**.

## <a name="find-using-regular-expressions"></a>Búsqueda mediante expresiones regulares
1. Para permitir el uso de expresiones regulares en el campo **Buscar** en operaciones de **Búsqueda rápida**, **Buscar en archivos**, **Reemplazo rápido** o **Reemplazar en archivos**, seleccione la opción **Usar** de **Opciones de búsqueda** y, luego, elija **Expresiones regulares**.

2. El botón triangular de la **Lista de referencia** situado junto al campo **Buscar** se activa. Haga clic en este botón para obtener una lista de las expresiones regulares más utilizadas. Al elegir alguno de los elementos del Generador de expresiones, éste se inserta en la cadena **Buscar** .

> [!NOTE]
> Existen diferencias de sintaxis entre las expresiones regulares que se pueden utilizar en las cadenas de **Buscar** y las que son válidas en la programación de [!INCLUDE[msCoName](../../includes/msconame-md.md)] .NET Framework. Por ejemplo, en **Buscar y reemplazar**, las llaves {} se utilizan para expresiones etiquetadas. Así, la expresión "zo{1}" devuelve todas las repeticiones de "zo" seguido de la etiqueta 1, como "Alonzo1" y "Gonzo1". Sin embargo, en .NET Framework, la notación {} se utiliza para cuantificadores. Así, la expresión "zo{1}" devuelve todas las repeticiones de "z" seguido exactamente de una "o", como "zone", pero no "zoo".

En la tabla siguiente se describen las expresiones regulares disponibles en la **Lista de referencias**.

|Expression|Sintaxis|Descripción|
|----------------|------------|-----------------|
|Un carácter cualquiera|.|Devuelve cualquier carácter único excepto un salto de línea.|
|Cero o más|*|Devuelve cero o más repeticiones de la expresión anterior, realizando todas las correspondencias posibles.|
|Uno o más|+|Devuelve al menos una repetición de la expresión anterior.|
|Inicio de línea|^|Fija la cadena coincidente al comienzo de una línea.|
|Fin de línea|$|Fija la cadena coincidente al final de una línea.|
|Inicio de palabra|\<|Coincide únicamente cuando una palabra comienza en este punto del texto.|
|Fin de palabra|>|Coincide únicamente cuando una palabra finaliza en este punto del texto.|
|Salto de línea|\n|Devuelve un salto de línea independiente de la plataforma. En una expresión de Reemplazar, inserta un salto de línea.|
|Un carácter cualquiera del conjunto|[]|Devuelve cualquier carácter situado dentro de []. Para especificar un intervalo de caracteres, escriba los caracteres inicial y final separados por un guión (-), como en [a-z].|
|Un carácter cualquiera no perteneciente al conjunto|[^…]|Devuelve cualquier carácter que no se encuentre en el juego de caracteres que sigue a ^.|
|Or|&#124;|Devuelve la expresión situada antes o la situada después del símbolo OR (&#124;). Se utiliza fundamentalmente dentro de un grupo. Por ejemplo, (sponge&#124;mud) bath devuelve "sponge bath" y "mud bath".|
|Escape|\|Devuelve el carácter que sigue a la barra diagonal inversa (\\) como un literal. Esto permite buscar los caracteres utilizados en la notación de expresiones regulares, como { y ^. Por ejemplo, \\^ busca el carácter ^.|
|Expresión de etiqueta|{}|Devuelve texto etiquetado con la expresión entre comillas.|
|Identificador de C/C++|:i|Devuelve la expresión ([a-zA-Z_$][a-zA-Z0-9_$]*).|
|Cadena entre comillas|:q|Devuelve la expresión (("[^"]*")&#124;('[^']\*')).|
|Espacio o tabulación|:b|Devuelve caracteres de espacio o tabulación.|
|Entero|:z|Devuelve la expresión ([0-9]+).|

La lista de todas las expresiones regulares válidas en las operaciones de **Buscar y reemplazar** es más amplia que lo que se puede mostrar en este **Generador de expresiones**. En una cadena **Buscar** también puede insertar las expresiones regulares siguientes:

|Expression|Sintaxis|Descripción|
|----------------|------------|-----------------|
|Cero o más como mínimo|@|Devuelve cero o más repeticiones de la expresión anterior, devolviendo la menor cantidad de caracteres posible.|
|Uno o más como mínimo|#|Devuelve una o más repeticiones de la expresión anterior, devolviendo la menor cantidad de caracteres posible.|
|Repetir n veces|^n|Devuelve n repeticiones de la expresión anterior. Por ejemplo, [0-9]^4 devuelve cualquier secuencia de cuatro dígitos.|
|Agrupación|()|Agrupa una subexpresión.|
|N-ésimo texto etiquetado|\n|En una expresión **Buscar y reemplazar** , indica el texto devuelto por la expresión con etiqueta enésima, donde n es un número de 1 a 9.<br /><br /> En una expresión de **Reemplazar** , \0 inserta todo el texto coincidente.|
|Campo justificado a la derecha|\\(w,n)|En una expresión de **Reemplazar** , justifica a la derecha la expresión etiquetada n-ésima de un campo con un ancho de al menos *w* caracteres.|
|Campo justificado a la izquierda|\\(-w,n)|En una expresión de **Reemplazar** , justifica a la izquierda la expresión etiquetada n-ésima de un campo con un ancho de al menos *w* caracteres.|
|Evitar coincidencia|~(X)|Evita una coincidencia cuando aparece X en este punto de la expresión. Por ejemplo, real~(ity) devuelve "real" de "realty" y "really", pero no "real" de "reality".|
|Carácter alfanumérico|:a|Devuelve la expresión ([a-zA-Z0-9]).|
|Carácter alfabético|:c|Devuelve la expresión ([a-zA-Z]).|
|Dígito decimal|:d|Devuelve la expresión ([0-9]).|
|Dígito hexadecimal|:h|Devuelve la expresión ([0-9a-fA-F]+).|
|Número racional|:n|Devuelve la expresión (([0-9]+.[0-9]*)&#124;([0-9]\*.[0-9]+)&#124;([0-9]+)).|
|Cadena alfabética|:w|Devuelve la expresión ([a-zA-Z]+).|
|Escape|\e|Unicode U+001B.|
|Bell|\g|Unicode U+0007.|
|Retroceso|\h|Unicode U+0008.|
|Pestaña|\t|Devuelve un carácter de tabulación, Unicode U+0009.|
|carácter Unicode|\x#### o \u####|Devuelve un carácter dado por un valor Unicode donde #### son dígitos hexadecimales. Es posible especificar un carácter fuera del plano básico multilingüe (es decir, un suplente) con el punto de código ISO 10646 o con dos puntos de código Unicode que proporcionen los valores del par suplente.|

En la tabla siguiente se muestra la sintaxis de coincidencias para propiedades estándar de caracteres Unicode. La abreviatura de dos letras es la misma que la que aparece en la base de datos de propiedades de caracteres Unicode. Éstas se pueden especificar como parte de un juego de caracteres. Por ejemplo, la expresión [:Nd:Nl:No] devuelve cualquier tipo de dígito.

|Expression|Sintaxis|Descripción|
|----------------|------------|-----------------|
|Letra en mayúscula|:Lu|Devuelve cualquier letra en mayúscula. Por ejemplo, :Luhe devuelve "The" pero no "the".|
|Letra en minúscula|:Ll|Devuelve cualquier letra en minúscula. Por ejemplo, :Llhe devuelve "the" pero no "The".|
|Letra tipo título|:Lt|Devuelve caracteres que combinan una letra en mayúscula con una en minúscula, como Nj y Dz.|
|Letra modificadora|:Lm|Devuelve letras o signos de puntuación, como comas, acentos y comillas dobles, utilizados para indicar modificaciones en la letra anterior.|
|Otra letra|:Lo|Devuelve otras letras, como letra gótica ahsa.|  
|Dígito decimal|:Nd|Devuelve dígitos decimales, como 0-9 y sus equivalentes de ancho completo.|
|Dígito de letra|:Nl|Devuelve dígitos de letra, como numerales romanos y el número cero ideográfico.|
|Otro dígito|:No|Devuelve otros dígitos, como el antiguo número uno en cursiva.|
|Puntuación de apertura|:Ps|Devuelve puntuación de apertura, como corchetes y llaves de apertura.|
|Puntuación de cierre|:Pe|Devuelve puntuación de cierre, como corchetes y llaves de cierre.|
|Puntuación de comillas iniciales|:Pi|Devuelve comillas dobles iniciales.|
|Puntuación de comillas finales|:Pf|Devuelve comillas simples y comillas dobles finales.|
|Puntuación de guión|:Pd|Devuelve el guión.|
|Puntuación de conector|:Pc|Devuelve el guión bajo o el subrayado.|
|Otros signos de puntuación|:Po|Devuelve (,), ?, ", !, @, #, %, &, *, \\, (:), (;), ', y /.|
|Separador de espacio|:Zs|Devuelve espacios en blanco.|
|Separador de línea|:Zl|Devuelve el carácter Unicode U+2028.|
|Separador de párrafo|:Zp|Devuelve el carácter Unicode U+2029.|
|Marca de no-espaciado|:Mn|Devuelve marcas de no-espaciado.|
|Marca de combinación|:Mc|Devuelve marcas de combinación.|
|Marca contenedora|:Me|Devuelve marcas contenedoras.|
|Símbolo matemático|:Sm|Coincide con +, =, ~, &#124;, \<, and >.|
|Símbolo de moneda|:Sc|Devuelve $ y otros símbolos de moneda.|
|Símbolo de modificador|:Sk|Devuelve símbolos de modificador, como acentos circunflejos, acentos graves y acentos largos.|
|Otro símbolo|:So|Devuelve otros símbolos, como el signo de Copyright, el signo de antígrafo y el signo de grado.|
|Otro control|:Cc|Devuelve el fin de línea.|
|Otro formato|:Cf|Caracteres de control de formato, como los caracteres de control bidireccional.|
|Suplente|:Cs|Devuelve la mitad de un par suplente.|
|Otros caracteres de uso privado|:Co|Devuelve cualquier carácter del área de uso privado.|
|Otros no asignados|:Cn|Devuelve caracteres no asignados a caracteres Unicode.|
|Especificación del número de apariciones del carácter o grupo precedente. Para obtener más información, consulte el artículo de [coincidencia exacta n veces](/dotnet/standard/base-types/quantifiers-in-regular-expressions#match-exactly-n-times-n).|{n}, donde "n" es el número de apariciones|`x(ab){2}x` coincide con "xababx"<br/>`x(ab){2,3}x` coincide con "xababx" y "xabababx", pero no con "xababababx"|

Además de las propiedades de caracteres Unicode, es posible especificar las siguientes propiedades adicionales como parte de un juego de caracteres.

|Expression|Sintaxis|Descripción|
|----------------|------------|-----------------|
|Alpha|:Al|Devuelve cualquier carácter. Por ejemplo, :Alhe devuelve palabras como "The", "then" y "reached".|
|Numeric|:Nu|Devuelve cualquier número o dígito.|
|Signos de puntuación|:Pu|Devuelve cualquier signo de puntuación, como ?, @, ', etc.|
|Espacio en blanco|:Wh|Devuelve cualquier tipo de espacio en blanco, incluidos los espacios de publicación y los ideográficos.|
|Bidireccional|:Bi|Devuelve caracteres de alfabetos con escritura de derecha a izquierda, como el árabe y el hebreo.|
|Hangul|:Ha|Devuelve caracteres de combinación y Hangul Jamos coreanos.|
|Hiragana|:Hi|Devuelve caracteres Hiragana.|
|Katakana|:Ka|Devuelve caracteres Katakana.|
|Ideográfico/Han/Kanji|:Id|Devuelve caracteres ideográficos, como Han y Kanji.|

## <a name="see-also"></a>Consulte también  
- [Buscar y reemplazar](./search-and-replace.md)
- [Buscar texto con caracteres comodín](./search-text-with-wildcards.md)