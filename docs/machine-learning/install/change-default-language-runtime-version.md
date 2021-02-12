---
title: Cambio de la versión predeterminada del entorno de ejecución del lenguaje R o Python
description: Obtenga información sobre cómo cambiar la versión predeterminada del entorno de ejecución de R o Python usada por una instancia de SQL con SQL Server 2017 Machine Learning Services o SQL Server R Services.
ms.custom: ''
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 08/14/2020
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2016||=sql-server-2017
ms.openlocfilehash: eacae756f788a5cf42c19cae27a975f98111fb71
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100068982"
---
# <a name="change-the-default-r-or-python-language-runtime-version"></a>Cambio de la versión predeterminada del entorno de ejecución del lenguaje R o Python

[!INCLUDE[SQL Server 2016 and 2017 only](../../includes/applies-to-version/sqlserver2016-2017-only.md)]

En este artículo se describe cómo cambiar la versión predeterminada de R o Python usada en [SQL Server 2016 R Services](../r/sql-server-r-services.md) o [SQL Server 2017 Machine Learning Services](../sql-server-machine-learning-services.md).

A continuación se enumeran las versiones del entorno de ejecución de R y Python que se incluyen en las distintas versiones de SQL Server.

| Versión de SQL Server | Servicio | Actualización acumulada | Versiones del entorno de ejecución de R | Versión del entorno de ejecución de Python |
|-|-|-|-|-|
| SQL Server 2016 | R Services | RTM - SP2 CU13 | 3.2.2 | No disponible |
| SQL Server 2016 | R Services | SP2 CU14 y versiones posteriores | 3.2.2 y 3.5.2 | No disponible |
| SQL Server 2017 | Machine Learning Services | RTM - CU21 | 3.3.3 | 3.5.2 |
| SQL Server 2017 | Machine Learning Services | CU22 y versiones posteriores | 3.3.3 y 3.5.2 | 3.5.2 y 3.7.2 |

## <a name="prerequisites"></a>Requisitos previos

Debe instalar una actualización acumulativa (CU) para cambiar la versión predeterminada del entorno de ejecución del lenguaje R o Python:

- **SQL Server 2016:** actualización acumulativa (CU) 14 de Service Pack (SP) 2 o posterior
- **SQL Server 2017:** actualización acumulativa (CU) 22 o posterior

Para descargar la actualización acumulativa más reciente, consulte las [últimas actualizaciones de Microsoft SQL Server](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md).

> [!NOTE]
> Si integra la actualización acumulativa con una nueva instalación de SQL Server, solo se instalarán las versiones más recientes del entorno de ejecución de R y Python.

## <a name="change-r-runtime-version"></a>Cambio de la versión del entorno de ejecución de R

Si ha instalado una de las actualizaciones acumulativas anteriores para SQL Server 2016 o 2017, puede tener varias versiones de R en una instancia de SQL. Cada versión se encuentra en una subcarpeta de la carpeta de la instancia con el nombre `R_SERVICES.`*&lt;major&gt;* . *&lt;minor&gt;* (es posible que la carpeta de la instalación original no tenga un número de versión anexado al nombre de la carpeta).

Si instala una CU que contiene R 3.5, la nueva carpeta `R_SERVICES` es:

- SQL Server 2016: `C:\Program Files\Microsoft SQL Server\MSSQL13.<INSTANCE_NAME>\R_SERVICES.3.5`
- SQL Server 2017: `C:\Program Files\Microsoft SQL Server\MSSQL14.<INSTANCE_NAME>\R_SERVICES.3.5`

Cada instancia de SQL usa una de estas versiones como versión predeterminada de R. Puede cambiar la versión predeterminada mediante la utilidad de línea de comandos **RegisterRext.exe**. La utilidad se encuentra en la carpeta R en cada instancia de SQL:

*&lt;Ruta de acceso de instancia de SQL&gt;* \R_SERVICES.n.n\library\RevoScaleR\rxLibs\x64\RegisterRext.exe

> [!Note]
> La funcionalidad descrita en este artículo solo está disponible con la copia de **RegisterRext.exe** incluida en las CU de SQL. No use la copia incluida con la instalación de SQL original.

Para cambiar la versión del entorno de ejecución de R, pase los siguientes argumentos de la línea de comandos a **RegisterRext.exe**:

- `/configure` -obligatorio, especifica que está configurando la versión predeterminada de R.

- `/instance:`*&lt;nombre de instancia&gt;* : opcional, la instancia de que desea configurar. Si no se especifica, se configura la instancia predeterminada.

- `/rhome:`*&lt;Ruta de acceso a la carpeta R_SERVICES[n.n]&gt;* : opcional, ruta de acceso a la carpeta de la versión del entorno de ejecución que desea establecer como la versión predeterminada de R.

  Si no especifica /rhome, la ruta de acceso configurada es la ruta de acceso en la que se encuentra **RegisterRext.exe**.

### <a name="examples"></a>Ejemplos

A continuación se muestran ejemplos de cómo cambiar la versión del tiempo de ejecución de R en SQL Server 2016 y 2017.

#### <a name="change-r-runtime-version-in-sql-server-2016"></a>Cambio de la versión del entorno de ejecución de R en SQL Server 2016

Por ejemplo, para configurar **R 3.5** como la versión predeterminada de R para la instancia MSSQLSERVER01 en SQL Server 2016:

```cmd
cd "C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER01\R_SERVICES.3.5\library\RevoScaleR\rxLibs\x64"

.\RegisterRext.exe /configure /rhome:"C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER01\R_SERVICES.3.5" /instance:MSSQLSERVER01
```

#### <a name="change-r-runtime-version-in-sql-server-2017"></a>Cambio de la versión del entorno de ejecución de R en SQL Server 2017

Por ejemplo, para configurar **R 3.5** como la versión predeterminada de R para la instancia MSSQLSERVER01 en SQL Server 2017:

```cmd
cd "C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER01\R_SERVICES.3.5\library\RevoScaleR\rxLibs\x64"

.\RegisterRext.exe /configure /rhome:"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER01\R_SERVICES.3.5" /instance:MSSQLSERVER01
```

En estos ejemplos, no es necesario incluir el argumento `/rhome`, ya que se está especificando la misma carpeta en la que se encuentra **RegisterRext.exe**.

## <a name="change-python-runtime-version"></a>Cambio de la versión del entorno de ejecución de Python

Si ha instalado CU22 o posterior para SQL Server 2017, puede tener varias versiones de Python en una instancia de SQL. Cada versión se encuentra en una subcarpeta de la carpeta de la instancia con el nombre `PYTHON_SERVICES.`*&lt;major&gt;* . *&lt;minor&gt;* (es posible que la carpeta de la instalación original no tenga un número de versión anexado al nombre de la carpeta).

Por ejemplo, si instala una CU que contiene Python 3.7, se crea una nueva carpeta `PYTHON_SERVICES` :

`C:\Program Files\Microsoft SQL Server\MSSQL14.<INSTANCE_NAME>\PYTHON_SERVICES.3.7`  

Cada instancia de SQL usa una de estas versiones como la versión predeterminada de Python. Puede cambiar la versión predeterminada mediante la utilidad de la línea de comandos **RegisterRExt.exe**. La utilidad se encuentra en las carpetas de Python en cada instancia de SQL:

*&lt;Ruta de acceso de instancia de SQL&gt;* `\PYTHON_SERVICES.n.n\Lib\site-packages\revoscalepy\rxLibs\RegisterRExt.exe`

> [!Note]
> La funcionalidad descrita en este artículo solo está disponible con la copia de **RegisterRExt.exe** incluida en las CU de SQL. No use la copia incluida con la instalación de SQL original.

Para cambiar la versión del entorno de ejecución de Python, pase los siguientes argumentos de la línea de comandos a **RegisterRext.exe**:

- `/configure` -obligatorio, especifica que está configurando la versión predeterminada de Python.

- `/python`: especifica que está configurando la versión predeterminada de Python. Opcional si se especifica `/pythonhome`.

- `/instance:`*&lt;nombre de instancia&gt;* : opcional, la instancia de que desea configurar. Si no se especifica, se configura la instancia predeterminada.

- `/pythonhome:`*&lt;Ruta de acceso a la carpeta PYTHON_SERVICES[n.n]&gt;* : opcional, ruta de acceso a la carpeta de la versión del entorno de ejecución que desea establecer como la versión predeterminada de Python.

  Si no especifica /pythonhome, la ruta de acceso configurada es la ruta de acceso en la que se encuentra **RegisterRext.exe**.

### <a name="example"></a>Ejemplo

Por ejemplo, para configurar **Python 3.7** como versión predeterminada de Python para la instancia MSSQLSERVER01 en SQL Server 2017:

```cmd
cd "C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER01\PYTHON_SERVICES.3.7\Lib\site-packages\revoscalepy\rxLibs"

.\RegisterRext.exe /configure /pythonhome:"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES.3.7" /instance:MSSQLSERVER01
```

En este ejemplo, no es necesario incluir el argumento `/pythonhome`, ya que se está especificando la misma carpeta en la que se encuentra **RegisterRext.exe**.

## <a name="remove-a-runtime-version"></a>Eliminación de una versión del entorno de ejecución

Para quitar una versión de R o Python, use **RegisterRExt.exe** con el argumento de la línea de comandos `/cleanup`, utilizando los mismos argumentos `/rhome`, `/pythonhome`y `/instance` descritos anteriormente.

Por ejemplo, para quitar la carpeta **R 3.2** de la instancia MSSQLSERVER01:

```cmd
.\RegisterRext.exe /cleanup /rhome:"C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER01\R_SERVICES" /instance:MSSQLSERVER01
```

Por ejemplo, para quitar la carpeta **Python 3.7** de la instancia MSSQLSERVER01:

```cmd
.\RegisterRExt.exe /cleanup /python /pythonhome:"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER01\PYTHON_SERVICES.3.7" /instance:MSSQLSERVER01
```

**RegisterRext.exe** le pedirá que confirme la limpieza del entorno de ejecución de R especificado:

> *¿Seguro que desea eliminar permanentemente el entorno de ejecución determinado junto con todos los paquetes instalados en él? \[Sí(Y)/No(N)/Predeterminado(Sí)\]:*

Para confirmar, responda `Y` o presione Entrar. Como alternativa, puede omitir este mensaje pasando `/y` o `/Yes` junto con la opción `/cleanup`.

> [!NOTE]
> Puede quitar una versión solo si no está configurada como predeterminada y no se usa actualmente para ejecutar **RegisterRext.exe**.

## <a name="next-steps"></a>Pasos siguientes

- [Obtención de información de paquetes de R](../package-management/r-package-information.md)
- [Obtención de información de paquetes de Python](../package-management/python-package-information.md)
- [Instalación de paquetes con herramientas de R](../package-management/install-r-packages-standard-tools.md)
- [Instalación de paquetes con las herramientas de Python](../package-management/install-python-packages-standard-tools.md)
