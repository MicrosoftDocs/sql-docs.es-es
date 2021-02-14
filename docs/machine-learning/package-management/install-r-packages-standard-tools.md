---
title: Instalación de paquetes con herramientas de R
description: Aprenda a usar herramientas de R estándar para instalar nuevos paquetes de R en una instancia de SQL Server Machine Learning Services o SQL Server R Services.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/20/2019
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2016||=sql-server-2017
ms.openlocfilehash: e54df6240cf1bf4dcaee7813eaf614a84c31a0f6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082626"
---
# <a name="install-packages-with-r-tools"></a>Instalación de paquetes con herramientas de R

[!INCLUDE [SQL Server 2016 and 2017 only](../../includes/applies-to-version/sqlserver2016-2017-only.md)]

En este artículo se describe cómo usar herramientas de R estándar para instalar nuevos paquetes de R en una instancia de SQL Server Machine Learning Services o SQL Server R Services. Puede instalar paquetes en un servidor SQL Server que tenga una conexión a Internet, así como uno que esté aislado de Internet.

Además de las herramientas de R estándar, puede instalar paquetes de R mediante:

+ [RevoScaleR](install-r-packages-with-revoscaler.md)
::: moniker range="=sql-server-2017"
+ [T-SQL](install-r-packages-with-tsql.md) (CREATE EXTERNAL LIBRARY)
::: moniker-end

## <a name="general-considerations"></a>Consideraciones generales

+ El código de R que se ejecuta en SQL Server solo puede usar los paquetes instalados en la biblioteca de instancia predeterminada. SQL Server no puede cargar paquetes de bibliotecas externas, aunque la biblioteca esté en el mismo equipo.
Esto incluye las bibliotecas de R instaladas con otros productos de Microsoft.

+ La biblioteca de paquetes de R se encuentra en la carpeta Archivos de programa de la instancia de SQL Server y, de forma predeterminada, se requieren permisos de administrador para instalar contenido en esta carpeta. Para obtener más información, consulte [Ubicación de la biblioteca de paquetes](../package-management/r-package-information.md#default-r-library-location).

  ::: moniker range="=sql-server-2017"
  Los usuarios que no son administradores pueden instalar paquetes mediante RevoScaleR 9.0.1 y versiones posteriores, o bien mediante CREATE EXTERNAL LIBRARY. El usuario **dbo_owner**, o un usuario con permiso CREATE EXTERNAL LIBRARY, puede instalar paquetes de R en la base de datos actual. Para más información, consulte:
  + [Uso de RevoScaleR para instalar paquetes de R](install-r-packages-with-revoscaler.md)
  + [Uso de T-SQL (CREATE EXTERNAL LIBRARY) para instalar paquetes de R en SQL Server](install-r-packages-with-tsql.md)
  ::: moniker-end

  ::: moniker range="=sql-server-2016"
  Los usuarios que no son administradores pueden instalar paquetes mediante RevoScaleR 9.0.1 y versiones posteriores. El usuario **dbo_owner** puede instalar paquetes de R en la base de datos actual. Para obtener más información, vea [Uso de RevoScaleR para instalar paquetes de R](install-r-packages-with-revoscaler.md).
  ::: moniker-end

+ En un entorno de SQL Server protegido, probablemente le interese evitar lo siguiente:
  + Paquetes que requieren acceso a la red
  + Paquetes que requieren acceso con privilegios elevados al sistema de archivos
  + Paquetes que se usan para el desarrollo web u otras tareas que no obtienen ningún beneficio al ejecutarse en SQL Server

## <a name="online-installation-with-internet-access"></a>Instalación en línea (con acceso a Internet)

Si el servidor SQL Server tiene acceso a Internet, puede usar las herramientas de instalación de paquetes estándar para instalar paquetes de R.

1. Determine la ubicación de la biblioteca de instancias (vea [Obtención de información de paquetes de R](../package-management/r-package-information.md)) y vaya a la carpeta donde están instaladas las herramientas de R.

   ::: moniker range="=sql-server-2016"
   Por ejemplo, la ruta de acceso predeterminada para una instancia predeterminada de SQL Server es la siguiente:

   `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\bin\x64\`
   ::: moniker-end

   ::: moniker range="=sql-server-2017"
   Por ejemplo, la ruta de acceso predeterminada para una instancia predeterminada de SQL Server es la siguiente:

   `C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\bin\x64\`
   ::: moniker-end

1. Ejecute **R** o **Rgui** como administrador desde esta carpeta.

1. Ejecute el comando R `install.packages` y especifique el nombre del paquete. Si el paquete tiene dependencias, el instalador descarga automáticamente las dependencias y las instala.

Si tiene varias instancias en paralelo de SQL Server, ejecute la instalación por separado para cada instancia en la que desee usar el paquete. Actualmente los paquetes no se pueden compartir entre instancias.

## <a name="offline-installation-no-internet-access"></a><a name = "bkmk_offlineInstall"></a> Instalación sin conexión (sin acceso a Internet)

Con frecuencia, los servidores que hospedan bases de datos de producción no tienen conexión a Internet. Para instalar paquetes de R en ese entorno, descargue y prepare paquetes y dependencias de antemano (como archivos comprimidos) y, a continuación, copie los archivos en una carpeta en el servidor. Una vez que los archivos estén en su lugar, los paquetes se pueden instalar sin conexión.

La identificación de todas las dependencias resulta complicada. Para R, se recomienda usar [**miniCRAN**](https://andrie.github.io/miniCRAN/) para crear un repositorio local.
**miniCRAN** toma una lista de los paquetes que quiere instalar, analiza las dependencias y recopila todos los archivos comprimidos. A continuación, crea un repositorio único que se puede copiar en la instancia de SQL Server aislada. El paquete [**igraph**](https://igraph.org/r/) también es útil para analizar las dependencias de paquete.

Para obtener más información, consulte [Creación de un repositorio de paquete de R local mediante miniCRAN](create-a-local-package-repository-using-minicran.md).

Una vez que el archivo comprimido está en la instancia de SQL Server, puede instalarlo con las herramientas de R estándar en el servidor.

1. Determine la ubicación de la biblioteca de instancias (vea [Obtención de información de paquetes de R](../package-management/r-package-information.md)) y vaya a la carpeta donde están instaladas las herramientas de R. 

   ::: moniker range="=sql-server-2016"
   Por ejemplo, la ruta de acceso predeterminada para una instancia predeterminada de SQL Server es la siguiente:

   `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\bin\x64\`
   ::: moniker-end

   ::: moniker range="=sql-server-2017"
   Por ejemplo, la ruta de acceso predeterminada para una instancia predeterminada de SQL Server es la siguiente:

   `C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\bin\x64\`
   ::: moniker-end

1. Ejecute **R** o **Rgui** como administrador desde esta carpeta.

1. Ejecute el comando `install.packages` de R y especifique el paquete o el nombre del repositorio, así como la ubicación de los archivos comprimidos. Por ejemplo:

   ```R
   install.packages("C:\\Temp\\Downloaded packages\\mynewpackage.zip", repos=NULL)
   ```

   Este comando extrae el paquete de R `mynewpackage` de su archivo comprimido local e instala el paquete. Si el paquete tiene dependencias, el instalador comprueba si hay paquetes existentes en la biblioteca. Si ha creado un repositorio que incluye las dependencias, el instalador instala también los paquetes necesarios.

   > [!NOTE]
   > Si los paquetes necesarios no están presentes en la biblioteca de instancias y no se encuentran en los archivos comprimidos, se produce un error en la instalación del paquete de destino.

Como alternativa a **miniCRAN**, puede realizar estos pasos manualmente:

1. Identifique todas las dependencias de paquete.
1. Compruebe si los paquetes necesarios ya están instalados en el servidor. Si el paquete está instalado, compruebe que la versión es correcta.
1. Descargue el paquete y todas las dependencias en un equipo independiente con acceso a Internet.
1. Coloque el paquete y las dependencias en un único archivo de paquete.
1. Comprima el archivo si aún no está en formato comprimido.
1. Mueva los archivos a una carpeta a la que tenga acceso el servidor.
1. Ejecute un comando de instalación compatible o una instrucción DDL para instalar el paquete en la biblioteca de instancias.

## <a name="see-also"></a>Consulte también

+ [Obtención de información de paquetes de R](r-package-information.md)
+ [Sugerencias para usar paquetes de R](tips-for-using-r-packages.md)
+ [Tutoriales de lenguaje R de SQL Server](../tutorials/r-tutorials.md)