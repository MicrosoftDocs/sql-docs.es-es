---
title: Sugerencias para usar paquetes de R
titleSuffix: SQL machine learning
description: Obtenga sugerencias útiles sobre el uso de paquetes de R en SQL Server si no está familiarizado con R o con SQL Server.
ms.prod: sql
ms.technology: machine-learning
ms.date: 08/06/2019
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: e3dd91e29d88fc2ab6f7216f0bac38632caa7b25
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352511"
---
# <a name="tips-for-using-r-packages"></a>Sugerencias para usar paquetes de R

[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

En este artículo se proporcionan sugerencias útiles sobre el uso de paquetes de R en SQL Server. Estas sugerencias están destinadas a administradores de bases de datos que no están familiarizados con R y a desarrolladores de R experimentados que no están familiarizados con el acceso a paquetes en una instancia de SQL Server.

## <a name="if-youre-new-to-r"></a>Si no está familiarizado con R

Como administrador que instala paquetes de R por primera vez, le conviene conocer algunos aspectos básicos sobre la administración de paquetes de R.

### <a name="package-dependencies"></a>Dependencias de paquetes

Los paquetes de R suelen depender de muchos otros paquetes, algunos de los cuales podrían no estar disponibles en la biblioteca de R predeterminada que usa la instancia. A veces, un paquete requiere una versión de paquete dependiente diferente de la que está instalada. Las dependencias de paquete se indican en un archivo DESCRIPTION que está insertado en el paquete, pero a veces están incompletas. Puede usar un paquete denominado [iGraph](https://igraph.org/r/) para articular por completo el gráfico de dependencias.

Si necesita instalar varios paquetes o quiere asegurarse de que todos los usuarios de la organización obtengan la versión y el tipo de paquete correctos, se recomienda usar el paquete [miniCRAN](https://mran.microsoft.com/package/miniCRAN) para analizar la cadena de dependencia completa. minicRAN crea un repositorio local que se puede compartir entre varios usuarios o equipos. Para obtener más información, consulte [Crear un repositorio de paquete local mediante miniCRAN](create-a-local-package-repository-using-minicran.md).

### <a name="package-sources-versions-and-formats"></a>Orígenes, versiones y formatos de paquetes

Puede obtener paquetes de R de varios orígenes, como [CRAN](https://cran.r-project.org/) y [Bioconductor](https://www.bioconductor.org/). En el sitio oficial del lenguaje R (<https://www.r-project.org/>) encontrará muchos de estos recursos. Microsoft ofrece [MRAN](https://mran.microsoft.com/) para la distribución de R de código abierto ([MRO](https://mran.microsoft.com/open)) y otros paquetes. Muchos de estos paquetes se publican en GitHub, donde los desarrolladores pueden obtener el código fuente.

::: moniker range=">=sql-server-2016"
Los paquetes de R se ejecutan en varias plataformas informáticas. Asegúrese de que las versiones que instala son archivos binarios de Windows.
::: moniker-end

::: moniker range=">=sql-server-linux-ver15"
Los paquetes de R se ejecutan en varias plataformas informáticas. Asegúrese de que las versiones que instala son archivos binarios de Linux.
::: moniker-end

### <a name="know-which-library-youre-installing-to-and-which-packages-are-already-installed"></a>Sepa en qué biblioteca está instalando y qué paquetes ya están instalados

Si previamente ha modificado el entorno de R en el equipo, antes de instalar nada, asegúrese de que la variable de entorno de R `.libPath` usa una sola ruta de acceso.

Esta ruta de acceso debe apuntar a la carpeta R_SERVICES de la instancia. Para más información, incluida la forma de determinar qué paquetes ya están instalados, vea [Obtención de información de paquetes de R](../package-management/r-package-information.md).

## <a name="if-youre-new-to-sql-server"></a>Si no está familiarizado con SQL Server

Como desarrollador de R que trabaja con código que se ejecuta en SQL Server, las directivas de seguridad que protegen el servidor restringen su capacidad para controlar el entorno de R. En las sugerencias siguientes se describen situaciones habituales y se proporcionan consejos para trabajar en este entorno.

### <a name="r-user-libraries-not-supported-on-sql-server"></a>Bibliotecas de usuario de R: no se admiten en SQL Server

Los desarrolladores de R que necesitan instalar nuevos paquetes de R están acostumbrados a instalarlos a su voluntad, mediante una biblioteca de usuario privada siempre que la biblioteca predeterminada no esté disponible o cuando el desarrollador no sea un administrador del equipo. Por ejemplo, en un entorno de desarrollo de R al uso, el usuario tendría que agregar la ubicación del paquete a la variable de entorno de R `libPath`, o bien hacer referencia a la ruta de acceso completa del paquete, como aquí:

```R
library("c:/Users/<username>/R/win-library/packagename")
```

Esto no funciona cuando se ejecutan soluciones de R en SQL Server, ya que los paquetes de R deben instalarse en una biblioteca específica predeterminada que esté asociada a la instancia. Cuando un paquete no está disponible en la biblioteca predeterminada, aparece este error al intentar llamar al paquete:

*Error in library(xxx): there is no package called 'xxx'* (Error en la biblioteca (xxx): no hay ningún paquete llamado "xxx")

Para obtener información sobre cómo instalar paquetes de R en SQL Server, consulte [Instalación de nuevos paquetes de R en SQL Server Machine Learning Services o SQL Server R Services](install-additional-r-packages-on-sql-server.md).

### <a name="how-to-avoid-package-not-found-errors"></a>Cómo evitar los errores de tipo "paquete no encontrado"

Si sigue las instrucciones que se indican a continuación, podrá evitar los errores de tipo "paquete no encontrado".

+ Elimine las dependencias de las bibliotecas de usuario.

    Instalar los paquetes de R necesarios en una biblioteca de usuario personalizada es una práctica de desarrollo incorrecta. Esto puede provocar errores si otro usuario que no tiene acceso a la ubicación de la biblioteca ejecuta una solución.

    Además, si un paquete está instalado en la biblioteca predeterminada, el tiempo de ejecución de R lo cargará desde la biblioteca predeterminada, aunque se haya especificado una versión diferente en el código de R.

+ Asegúrese de que el código pueda ejecutarse en un entorno compartido.

+ Evite instalar paquetes como parte de una solución. Si no tiene permisos para instalar paquetes, se producirá un error en el código. Incluso aunque tenga permisos para instalar paquetes, debe hacerlo por separado del resto de código que quiera ejecutar.

+ Comprobar el código para asegurarse de que no hay ninguna llamada a paquetes no instalados.

+ Actualice el código para quitar las referencias directas a las rutas de acceso de paquetes de R o bibliotecas de R.

+ Sepa qué biblioteca de paquetes está asociada a la instancia. Para más información, vea [Obtener información sobre paquetes de R](../package-management/r-package-information.md).

## <a name="see-also"></a>Vea también

::: moniker range="<=sql-server-2017"
+ [Instalación de paquetes con herramientas de R](install-r-packages-standard-tools.md)
::: moniker-end
::: moniker range=">sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current"
+ [Instalación de nuevos paquetes de R con sqlmlutils](install-additional-r-packages-on-sql-server.md)
::: moniker-end
