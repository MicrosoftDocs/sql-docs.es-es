---
description: Sistema de control de versiones para la documentación de SQL
title: Sistema de control de versiones para la documentación de SQL
ms.date: 08/12/2020
ms.prod: sql
ms.technology: release-landing
ms.topic: conceptual
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||=azuresqldb-mi-current||=azure-sqldw-latest||>=aps-pdw-2016||>=sql-server-linux-2017||>=sql-server-2016
ms.openlocfilehash: 56ef939790de90d02add427f14f8817df885383f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079756"
---
# <a name="versioning-system-for-sql-documentation"></a>Sistema de control de versiones para la documentación de SQL

[!INCLUDE[includes_appliesto-ss-asdb-asdw-pdw-md.md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

En este artículo se explica el _sistema de control de versiones_ de la documentación de SQL. El sistema de control de versiones conoce los productos y sus versiones. El sistema le permite elegir el producto y la versión que le interesen. Después, el sistema muestra la documentación adecuada.

## <a name="applies-to-products"></a>Productos SE APLICA A

La mayoría de los artículos de SQL Server contienen las palabras **SE APLICA A** debajo del título. En la misma línea, se incluye una lista útil de _productos_ de SQL con indicadores de si el artículo es relevante para el producto. Por ejemplo, el producto SQL Server se podría indicar como relevante, mientras que Azure SQL Database se podría indicar como irrelevante para el artículo.

La línea **SE APLICA A** desconoce las _versiones_ de los productos. Nos esforzamos por evitar discrepancias entre la línea **SE APLICA A** y el aspecto de los productos de nuestras configuraciones del sistema de control de versiones.

## <a name="history-of-separate-file-sets"></a>Historial de conjuntos de archivos independientes

En SQL Server 2014 y versiones anteriores, cada versión tiene su propia copia independiente completa de los archivos de documentación. Por ejemplo, la documentación de SQL Server 2014 comenzó como una copia de la de SQL Server 2012. Después, la copia de 2014 se editó durante el ciclo de desarrollo del producto.

Este enfoque anterior significaba que, si se detectaba un error en la documentación de 2014, el error también podría existir en las de 2012 y 2008. Esto dificultaba la corrección de los errores y el mantenimiento general.

## <a name="multiple-versions-in-the-same-files"></a>Varias versiones en los mismos archivos

Por este y otros motivos, los archivos de documentación para SQL Server 2016 también sirven para 2017, 2019 y, probablemente, para \<vNext\>. Esta consolidación se ha hecho práctica porque ahora se asignan _monikers de control de versiones_ a los archivos de documentación de SQL Server. Los monikers de control de versiones se asignan, o se insertan explícitamente, en el grado de granularidad que tenga sentido para cada archivo de documentación determinado.

## <a name="versioning-control-in-the-ui"></a>Control de versiones en la interfaz de usuario

Al ver un artículo de documentación de SQL mediante el sitio web :::no-loc text="Docs":::, el moniker de control de versiones seleccionado actualmente es visible encima de la tabla de contenido (TDC). El control es una lista desplegable.

![media_versioning-control-10-sql-server-2017.png](media/versioning-control-10-sql-server-2017.png)

Si quiere ver la documentación de otra versión de SQL Server, haga clic en la flecha de expansión situada al final del moniker de la versión actual. Después, haga clic para elegir cualquier combinación de producto y versión que quiera. Al hacer clic en otra versión, la documentación que se muestra cambia repentinamente para mostrar las diferencias de la nueva versión elegida seleccionada. Es posible que haya cambios, o no, y ambos casos son comunes.

![media_versioning-control-20-expanded.png](media/versioning-control-20-expanded.png)

### <a name="https-parameter-no-loc-textview"></a>Parámetro HTTPS :::no-loc text="view=":::

Cada artículo cuya dirección web comienza con `https://docs.microsoft.com/sql/` tiene un parámetro denominado `?view=` anexado a su dirección. Este valor de parámetro es el código de moniker de control de versiones.

El _código_ de moniker de la dirección `https` siempre coincide con el _nombre_ del moniker que se muestra en el control de versiones.

## <a name="products-not-editions"></a>Productos, no ediciones

### <a name="editions"></a>Ediciones

En los años 90 y en los 2000, Microsoft SQL Server solo tenía un producto. Había varias _ediciones_ de cada versión de SQL Server, como las ediciones _Developer_ y _Enterprise_ de SQL Server 2008. Las ediciones representaban conjuntos de características ligeramente distintas, pero el producto principal era el mismo. Es posible que las versiones nuevas de SQL Server todavía tengan varias ediciones.

### <a name="products"></a>Productos

Con el aumento más reciente de la informática en la nube y Microsoft Azure, Microsoft ha lanzado su producto Azure SQL Database. Aunque hay mucho código compartido por el producto local de SQL Server tradicional y el producto Azure SQL Database, son dos productos realmente independientes.

En el caso de SQL, los monikers de control de versiones distinguen entre los productos, pero no entre las ediciones.

#### <a name="azure-cloud-sql-products"></a>Productos SQL de la nube de Azure

Casi todos los artículos cuyas direcciones web empiezan con `https://docs.microsoft.com/sql/` se aplican al menos a una versión del producto denominado SQL Server. Un gran subconjunto de esos artículos también se aplica a uno o varios de nuestros productos de servicio SQL que se hospedan en nuestra nube de Azure. Uno de estos productos de nube de SQL se denomina Azure SQL Database.

Naturalmente, el producto Azure SQL Database solo tiene una versión. Casi todos los artículos que se aplican a Azure SQL Database, pero no a SQL Server, tienen direcciones web que comienzan por `https://docs.microsoft.com/azure/sql-database/`.

## <a name="scenarios-of-version-filtering"></a>Escenarios de filtrado de versiones

El sistema de control de versiones funciona mediante el filtrado de todo el contenido de la documentación que no se aplica al moniker activo actualmente. Cada vez que selecciona otro moniker de control de versiones, cambia el conjunto de contenido que está oculto. El filtrado oculta el contenido en los niveles siguientes:

- Secciones u oraciones dentro de un artículo.
- Entradas para los artículos de la tabla de contenido (TDC).

A continuación se muestran escenarios en los que se explican los efectos de elegir otro moniker.

### <a name="scenario-1-within-the-current-article"></a>Escenario 1: En el artículo actual

El escenario siguiente se centra en las secciones del artículo actual:

1. El moniker de control de versiones actual es **SQL Server 2017**.
2. Está leyendo una sección en la que se describe una característica que se agregó por primera vez a la versión 2017 de SQL Server.
3. Cambie el moniker a **SQL Server 2016**.
4. Observe que la sección que estaba leyendo desaparece.
5. Vuelva a cambiar el moniker, esta vez a **SQL Server 2019**.
6. Observe que la sección 2017 que estaba leyendo vuelve a aparecer en la pantalla.

En el escenario anterior, es probable que la sección sobre la nueva característica de 2017 esté marcada con un _intervalo de moniker_ que incluye el código de moniker siguiente:

- `>=sql-server-2017`

Cuando se ha elegido el moniker **SQL Server 2019**, el sistema de control de versiones ha comprobado que 2019 es mayor o igual que 2017, y ha mostrado la sección.

### <a name="scenario-2-click-a-link-to-a-hidden-article"></a>Escenario 2: Hacer clic en un vínculo a un artículo oculto

En el siguiente escenario poco habitual se explica lo que sucede si se hace clic en un vínculo a un artículo que está oculto actualmente en la tabla de contenido (TDC). En resumen, el vínculo funciona:

1. El moniker de control de versiones actual es **SQL Server 2017**.
2. En el artículo actual :::no-loc text="A":::, hace clic en un vínculo a un artículo :::no-loc text="B"::: que solo se aplica a SQL Server 2016.
    - Antes del clic, la tabla de contenido tiene oculta su entrada para el artículo :::no-loc text="B":::.
3. Después del clic, se muestra el artículo :::no-loc text="B":::.
    - La representación del artículo :::no-loc text="B"::: obliga al control de versiones a cambiar al moniker **SQL Server 2016**.
    - Porque el moniker original **SQL Server 2017** se ha tenido que abandonar. Este abandono provoca que se muestre un mensaje informativo junto a la parte superior de la página web. En el [mensaje](#anchor-message-unavailable-for-moniker) se explica que se ha tenido que cambiar el moniker actual para acomodar el nuevo artículo :::no-loc text="B":::.

### <a name="scenario-3-navigate-to-an-https-address"></a>Escenario 3: Navegación a una dirección https

El siguiente artículo se ha agregado como novedad para SQL Server 2017. En el artículo se describen las características que se han agregado a la versión 2017 de SQL Server. La mayoría o todas esas características nuevas también forman parte de la versión 2019. Estos son los atributos del artículo.

| Atributo | Valor |
| :-------- | :---- |
| Título | Novedades de SQL Server 2017 |
| Intervalo de moniker | `=">= sql-server-2017"` |
| Dirección `https` | `https://docs.microsoft.com/sql/sql-server/what-s-new-in-sql-server-2017` |
| &nbsp; | &nbsp; |

Dada la dirección `https` base, en la tabla siguiente se explica lo que sucede cuando el usuario anexa el parámetro `?view=`, con varios valores.

| Valor de `?view=` | Comportamiento de la navegación a direcciones `https` |
| :---------------- | :------------------------------ |
| _(Sin parámetros)._ | El sistema de control de versiones probaría su valor de moniker predeterminado. Normalmente, se establece en la última versión que no es una versión preliminar de SQL Server.<br/><br/>Un valor predeterminado de SQL Server 2017 o 2019 coincidiría con el atributo `>= sql-server-2017`.<br/><br/>El sistema anexaría el parámetro a la dirección `https`, posiblemente como `?view=sql-server-2017`.<br/>Después, el control de lista desplegable de control de versiones se establecería en el nombre de moniker que coincide. |
| `sql-server-2016` | El sistema de control de versiones comprobaría que el intervalo de moniker del artículo no incluye la versión 2016.<br/><br/>Después, el sistema elegiría uno de los monikers que satisface el intervalo.<br/><br/>Como en el caso de la versión 2016, se anexaría el parámetro `?view=` y el nombre del control coincidiría con el valor del parámetro. |
| `sql-server-2017` | El sistema de control de versiones entiende que el valor del parámetro se incluye en el intervalo de moniker del artículo.<br/><br/>El control de versiones se establecerá para coincidir con el valor del parámetro. |
| `sql-server-2019` | Es similar al caso del valor `sql-server-2017`, excepto que el parámetro y el control se establecen en 2019. |
| &nbsp; | &nbsp; |

### <a name="all-sql---hide-nothing-special-moniker"></a><a name="anchor-allsql-hidenothing"></a> Moniker especial All SQL - Hide nothing (Todo SQL: no ocultar nada)

Hay un nombre de producto de moniker especial, **All SQL** (Todo SQL) y su única versión **Hide nothing** (No ocultar nada). El propósito de este moniker es realizar pruebas internas de determinados cambios. Si lo usa un cliente, es probable que este moniker desoriente más que informar.

Algunos artículos tienen información relacionada con varias versiones de SQL Server. Cada moniker normal oculta las secciones con versiones que, de lo contrario, podrían mostrar información inexacta, confusa o contradictoria para la versión del moniker. El moniker **All SQL** especial mostraría todas las secciones de la versión y es posible que no sea evidente que se muestra información inexacta.

## <a name="message-the-requested-page-is-not-available-for-moniker"></a><a name="anchor-message-unavailable-for-moniker"></a> Mensaje: La página solicitada no está disponible para \<moniker\>.

El siguiente escenario conduce a la presentación de un mensaje informativo junto a la parte superior de la página web :::no-loc text="Docs"::::

1. El moniker de control de versiones actual es **SQL Server 2017**.
2. Está leyendo un artículo que es relevante para SQL Server 2017.
    - El artículo _no_ es relevante para el producto Azure SQL Database.
3. Intenta cambiar el moniker a **Azure SQL Database: actual**.
4. Comprueba que el intento se ha rechaza y se muestra un mensaje.

Al final de este escenario, verá que se muestra el siguiente mensaje informativo junto a la parte superior de la página web de documentación:

> La página solicitada no está disponible para Azure SQL Database - current (Azure SQL Database: actual). Se le ha redirigido a la versión más reciente del producto para la que está disponible esta página.

La versión _más reciente_ podría excluir las versiones que todavía no se han publicado de forma completa y que se encuentran en estado de _versión preliminar_.

![media_versioning-control-30-viewfallbackfrom.png](media/versioning-control-30-viewfallbackfrom.png)

## <a name="previous-versions-of-sql-server"></a>Versiones anteriores de SQL Server

El sistema de control de versiones está totalmente implementado para SQL Server, desde la versión 2016 en adelante.

- _2012 y versiones anteriores:_ &nbsp; el sistema de control de versiones no se usa para SQL Server 2012 o versiones anteriores.
    - El moniker especial **SQL Server - older** (SQL Server: anterior) está diseñado para ocultar prácticamente todos los artículos. Las excepciones poco frecuentes son un par de artículos que los clientes de versiones anteriores podrían necesitar una vez.
    - [Versiones anteriores de SQL Server, 2012-2005](./previous-versions-sql-server.md)

- _2014:_ &nbsp; el sistema de control de versiones está implementado parcialmente para SQL Server 2014. Puede elegir SQL Server 2014 en el control de versiones y funcionará. Pero los archivos de 2014 están dedicados exclusivamente a 2014, al igual que los archivos de 2008 están dedicados únicamente a 2008.
    - [Documentación de SQL Server 2014 sin conexión](./sql-server-offline-documentation.md)

- _2016 y versiones posteriores:_ &nbsp; el sistema de control de versiones está totalmente implementado para SQL Server 2016 y versiones posteriores.
    - [Bienvenido a la documentación de SQL Server 2016 y versiones posteriores](./index.yml?preserve-view=true&view=sql-server-2016)
    - [Documentación de SQL Server 2016 sin conexión](sql-server-offline-documentation.md)

## <a name="see-also"></a>Consulte también

[Versiones anteriores de SQL Server, 2014-2005](./previous-versions-sql-server.md)  
[Guía de navegación de la documentación de SQL Server](sql-docs-navigation-guide.md)  
[Cómo colaborar en la documentación de SQL Server](sql-server-docs-contribute.md)