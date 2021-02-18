---
title: Obtención de información de paquetes de Python
description: Aprenda a obtener información sobre los paquetes de Python instalados, incluidas las versiones y las ubicaciones de instalación, en SQL Server Machine Learning Services.
ms.custom: ''
ms.prod: sql
ms.technology: machine-learning
ms.date: 06/03/2020
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 29ed535b25d364ffa552aba20b10b446e0561cf9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352522"
---
# <a name="get-python-package-information"></a>Obtención de información de paquetes de Python

[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
En este artículo se describe cómo obtener información sobre los paquetes de Python instalados, incluidas las versiones y las ubicaciones de instalación, en [Machine Learning Services en SQL Server](../sql-server-machine-learning-services.md) y en [Clústeres de macrodatos](../../big-data-cluster/machine-learning-services.md). Los scripts de Python de ejemplo muestran cómo mostrar información de paquetes, como la ruta de instalación y la versión.
::: moniker-end
::: moniker range="=sql-server-2017"
En este artículo se describe cómo obtener información sobre los paquetes de Python instalados, incluidas las versiones y las ubicaciones de instalación, en [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md). Los scripts de Python de ejemplo muestran cómo mostrar información de paquetes, como la ruta de instalación y la versión.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
En este artículo se describe cómo obtener información sobre los paquetes de Python instalados, incluidas las versiones y las ubicaciones de instalación, en [Machine Learning Services en Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview). Los scripts de Python de ejemplo muestran cómo mostrar información de paquetes, como la ruta de instalación y la versión.
::: moniker-end

## <a name="default-python-library-location"></a>Ubicación predeterminada de las bibliotecas de Python

Al instalar Machine Learning con SQL Server, se crea una biblioteca de paquetes en el nivel de instancia para cada idioma que se instale. La biblioteca de instancias es una carpeta protegida que está registrada en SQL Server.

Todo el script o código que se ejecuta en la base de datos en SQL Server debe cargar funciones desde la biblioteca de instancias. SQL Server no puede obtener acceso a los paquetes instalados en otras bibliotecas. También se aplica a los clientes remotos: cualquier código de Python que se ejecute en el contexto del proceso del servidor solo puede usar paquetes instalados en la biblioteca de instancias.
Para proteger los recursos del servidor, la biblioteca de instancias predeterminadas solo la puede modificar un administrador del equipo.

::: moniker range="=sql-server-2017"
La ruta de acceso predeterminada de los archivos binarios de Python es:

`C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES`

Se da por sentado que la instancia predeterminada de SQL es MSSQLSERVER. Si se instala SQL Server como instancia con nombre definida por el usuario, se usará el nombre especificado.
::: moniker-end

::: moniker range=">=sql-server-ver15"
La ruta de acceso predeterminada de los archivos binarios de Python es:

`C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\PYTHON_SERVICES`

Se da por sentado que la instancia predeterminada de SQL es MSSQLSERVER. Si se instala SQL Server como instancia con nombre definida por el usuario, se usará el nombre especificado.
::: moniker-end

Para habilitar los scripts externos, ejecute los comandos SQL siguientes:

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH override;
```

::: moniker range="=azuresqldb-mi-current"
> [!IMPORTANT]
> En Azure SQL Managed Instance, la ejecución de los comandos sp_configure y RECONFIGURE desencadena un reinicio de SQL Server para que la configuración de RG surta efecto. Esto puede generar unos segundos de no disponibilidad.
::: moniker-end

Ejecute la instrucción SQL siguiente si quiere comprobar la biblioteca predeterminada de la instancia actual. En este ejemplo se devuelve la lista de carpetas incluidas en la variable `sys.path` de Python. La lista incluye el directorio actual y la ruta de acceso de la biblioteca estándar.

```sql
EXECUTE sp_execute_external_script
  @language =N'Python',
  @script=N'import sys; print("\n".join(sys.path))'
```

Para obtener más información sobre la variable `sys.path` y cómo se usa para establecer la ruta de acceso de búsqueda del intérprete para los módulos, consulte [The Module Search Path](https://docs.python.org/2/tutorial/modules.html#the-module-search-path) (La ruta de acceso de búsqueda de un módulo).

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current"
> [!NOTE]
> No intente instalar paquetes de Python directamente en la biblioteca de paquetes SQL con **pip** o métodos similares. En su lugar, use **sqlmlutils** para instalar paquetes en una instancia de SQL. Para obtener más información, vea [Instalación de paquetes de Python con sqlmlutils](install-additional-python-packages-on-sql-server.md).
::: moniker-end

## <a name="default-microsoft-python-packages"></a>Paquetes predeterminados de Python en Microsoft

Los siguientes paquetes de Python en Microsoft se instalan con SQL Server Machine Learning Services al seleccionar la característica de Python durante la instalación.

| Paquetes | Versión |  Descripción |
| ---------|---------|--------------|
| [revoscalepy](/machine-learning-server/python-reference/revoscalepy/revoscalepy-package) | 9.4.7 | Se usa para los contextos de procesos remotos, streaming, ejecución en paralelo de funciones rx para la importación y transformación de datos, modelado, visualización y análisis. |
| [microsoftml](/machine-learning-server/python-reference/microsoftml/microsoftml-package) | 9.4.7 | Agrega algoritmos de aprendizaje automático en Python. |

Para obtener información sobre la versión de Python que se incluye, consulte [Versiones de Python y R](../sql-server-machine-learning-services.md#versions).

### <a name="component-upgrades"></a>Actualizaciones de componentes

De forma predeterminada, los paquetes de Python se actualizan mediante Service Packs y actualizaciones acumulativas. Los paquetes adicionales y las actualizaciones de versión completa de los componentes básicos de Python solo son posibles mediante las actualizaciones de producto o enlazando la compatibilidad de Python con Microsoft Machine Learning Server.

Para obtener más información, consulte [Actualización de componentes de R y Python en SQL Server](../install/upgrade-r-and-python.md).

## <a name="default-open-source-python-packages"></a>Paquetes de código abierto predeterminados de Python

Al seleccionar la opción de lenguaje de Python durante la instalación, se instala la distribución Anaconda 4.2 (en Python 3.5). Además de las bibliotecas de código de Python, la instalación estándar incluye datos de ejemplo, pruebas unitarias y scripts de ejemplo.

> [!IMPORTANT]
> Nunca debe sobrescribir manualmente la versión de Python instalada por el programa de instalación de SQL Server por versiones más recientes en la web. Los paquetes de Microsoft Python se basan en versiones específicas de Anaconda. La modificación de la instalación podría desestabilizarla.

## <a name="list-all-installed-python-packages"></a>Visualización de todos los paquetes de Python instalados

En el siguiente script de ejemplo se muestra una lista de todos los paquetes de Python instalados en la instancia de SQL Server.

```sql
EXECUTE sp_execute_external_script
  @language = N'Python',
  @script = N'
import pkg_resources
import pandas
OutputDataSet = pandas.DataFrame(sorted([(i.key, i.version) for i in pkg_resources.working_set]))'
WITH result sets((Package NVARCHAR(128), Version NVARCHAR(128)));
```

## <a name="find-a-single-python-package"></a>Búsqueda de un único paquete de Python

Si ha instalado un paquete de Python y quiere asegurarse de que está disponible para una instancia de SQL Server determinada, puede ejecutar un procedimiento almacenado para buscar el paquete y devolver los mensajes.

Por ejemplo, el código siguiente busca el paquete `scikit-learn`.
Si se encuentra el paquete, el código imprime la versión del paquete.

```sql
EXECUTE sp_execute_external_script
  @language = N'Python',
  @script = N'
import pkg_resources
pkg_name = "scikit-learn"
try:
    version = pkg_resources.get_distribution(pkg_name).version
    print("Package " + pkg_name + " is version " + version)
except:
    print("Package " + pkg_name + " not found")
'
```

Resultado:

```text
STDOUT message(s) from external script: Package scikit-learn is version 0.20.2
```

<a name="bkmk_SQLPythonVersion"></a>
## <a name="view-the-version-of-python"></a>Vista de la versión de Python

En el ejemplo de código siguiente se devuelve la versión de Python instalada en la instancia de SQL Server.

```sql
EXECUTE sp_execute_external_script
  @language = N'Python',
  @script = N'
import sys
print(sys.version)
'
```

## <a name="next-steps"></a>Pasos siguientes

::: moniker range="=sql-server-2017"
+ [Instalación de paquetes con las herramientas de Python](install-python-packages-standard-tools.md)
::: moniker-end
::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current"
+ [Instalación de nuevos paquetes de Python con sqlmlutils](install-additional-r-packages-on-sql-server.md)
::: moniker-end