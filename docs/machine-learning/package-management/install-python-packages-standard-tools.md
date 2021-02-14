---
title: Instalación de paquetes con las herramientas de Python
description: Obtenga más información sobre cómo usar las herramientas de Python estándar para instalar paquetes de Python nuevos en una instancia de SQL Server Machine Learning Services.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 01/21/2020
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2017
ms.openlocfilehash: 37d7f95f4140e0cab778ac4864757fbe75b1ff0c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082636"
---
# <a name="install-packages-with-python-tools-on-sql-server"></a>Instalación de paquetes con las herramientas de Python en SQL Server
[!INCLUDE [SQL Server 2017 only](../../includes/applies-to-version/sqlserver2017-only.md)]

En este artículo se describe cómo usar las herramientas de Python estándar para instalar paquetes de Python nuevos en una instancia de SQL Server Machine Learning Services. En general, el proceso de instalación de paquetes nuevos es similar al de un entorno estándar de Python. Sin embargo, se requieren algunos pasos adicionales si el servidor no tiene conexión a Internet.

Para obtener más información sobre la ubicación de los paquetes y las rutas de acceso de instalación, consulte [Obtención de información de paquetes de Python](python-package-information.md).

## <a name="prerequisites"></a>Prerequisites

+ Debe tener [SQL Server Machine Learning Services](../install/sql-machine-learning-services-windows-install.md) instalado con la opción de lenguaje Python.

### <a name="other-considerations"></a>Otras consideraciones

+ Los paquetes deben ser compatibles con Python 3.5 y ejecutarse en Windows.

+ La biblioteca de paquetes de Python se encuentra en la carpeta Archivos de programa de la instancia de SQL Server y, de forma predeterminada, se requieren permisos de administrador para instalar contenido en esta carpeta. Para obtener más información, consulte [Ubicación de la biblioteca de paquetes](../package-management/python-package-information.md#default-python-library-location).

+ La instalación de paquetes se realiza por instancia. Si tiene varias instancias de Machine Learning Services, debe agregar el paquete a cada una de ellas.

+ Los servidores de bases de datos suelen estar bloqueados. En muchos casos, el acceso a Internet está totalmente bloqueado. En el caso de los paquetes con una larga lista de dependencias, deberá identificar estas dependencias de antemano y estar listo para instalar manualmente cada una de ellas.

+ Antes de agregar un paquete, piense si es adecuado para el entorno de SQL Server.

  + Se recomienda usar Python en la base de datos para las tareas que se benefician de la estrecha integración con el motor de base de datos, como el aprendizaje automático, en lugar de para las tareas que simplemente consultan la base de datos.

  + Si agrega paquetes que ejercen demasiada presión de cálculo en el servidor, el rendimiento se verá afectado.

  + En un entorno de SQL Server protegido, probablemente le interese evitar lo siguiente:
    + Paquetes que requieren acceso a la red
    + Paquetes que requieren acceso con privilegios elevados al sistema de archivos
    + Paquetes que se usan para el desarrollo web u otras tareas que no obtienen ningún beneficio al ejecutarse en SQL Server

## <a name="add-a-python-package-on-sql-server"></a>Adición de un paquete de Python en SQL Server

Para instalar un paquete de Python nuevo que se puede usar en un script en SQL Server, instale el paquete en la instancia de Machine Learning Services. Si tiene varias instancias de Machine Learning Services, debe agregar el paquete a cada una de ellas.

El paquete instalado en los ejemplos siguientes es [CNTK](/cognitive-toolkit/), un marco de aprendizaje profundo de Microsoft que admite la personalización, el entrenamiento y el uso compartido de distintos tipos de redes neuronales.

### <a name="for-offline-install-download-the-python-package"></a>Para la instalación sin conexión, descargue el paquete de Python.

Si va a instalar paquetes de Python en un servidor sin acceso a Internet, debe descargar el archivo WHL de un equipo con acceso a Internet y, a continuación, copiar el archivo en el servidor.

Por ejemplo, en un equipo conectado a Internet, puede descargar el archivo `cntk-2.1-cp35-cp35m-win_amd64.whl` del sitio [https://cntk.ai/PythonWheel/CPU-Only](https://cntk.ai/PythonWheel/CPU-Only/cntk-2.1-cp35-cp35m-win_amd64.whl) y, a continuación, copiar el archivo en una carpeta local del equipo con SQL Server.

> [!IMPORTANT]
> Asegúrese de obtener la versión de Windows del paquete. Si el archivo termina en .gz, probablemente no sea la versión correcta.

Para más información sobre las descargas del marco CNTK para varias plataformas y para varias versiones de Python, consulte el artículo sobre cómo [configurar CNTK en un equipo](/cognitive-toolkit/Setup-CNTK-on-your-machine).

### <a name="locate-the-python-library"></a>Búsqueda de la biblioteca de Python

Busque la ubicación predeterminada de la biblioteca de Python que usa SQL Server. Si ha instalado varias instancias, busque la carpeta `PYTHON_SERVICES` para la instancia en la que quiere agregar el paquete.

Por ejemplo, si Machine Learning Services se instaló con los valores predeterminados y el aprendizaje automático se habilitó en la instancia predeterminada, la ruta de acceso es:

```console
cd "C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES"
```

> [!TIP]
> Para depuraciones y pruebas futuras, es posible que quiera configurar un entorno de Python específico para la biblioteca de instancias.

### <a name="install-the-package-using-pip"></a>Instalación del paquete con pip

Use el instalador **pip** para instalar paquetes nuevos. Puede encontrar `pip.exe` en la subcarpeta `Scripts` de la carpeta `PYTHON_SERVICES`. El programa de instalación de SQL Server no agrega la subcarpeta `Scripts` a la ruta de acceso del sistema, por lo que debe especificar la ruta de acceso completa o puede agregar la carpeta Scripts a la variable PATH en Windows.

> [!NOTE]
> Si usa Visual Studio 2017 o Visual Studio 2015 con las extensiones de Python, puede ejecutar `pip install` desde la ventana **Entornos de Python**. Haga clic en **Paquetes** y, en el cuadro de texto, proporcione el nombre o la ubicación del paquete que quiere instalar. No es necesario escribir `pip install`, porque se rellena automáticamente.

+ Si el equipo tiene acceso a Internet, proporcione el nombre del paquete:

  ```console
  scripts\pip.exe install cntk
  ```
  También puede especificar la dirección URL de un paquete y una versión específicos, por ejemplo:

  ```console
  scripts\pip.exe install https://cntk.ai/PythonWheel/CPU-Only/cntk-2.1-cp35-cp35m-win_amd64.whl
  ```

+ Si el equipo no tiene acceso a Internet, especifique el archivo WHL que descargó anteriormente. Por ejemplo:

  ```console
  scripts\pip.exe install C:\Downloads\cntk-2.1-cp35-cp35m-win_amd64.whl
  ```

Es posible que se le pida que eleve los permisos para completar la instalación.
A medida que progresa la instalación, puede ver los mensajes de estado en la ventana del símbolo del sistema.

### <a name="load-the-package-or-its-functions-as-part-of-your-script"></a>Carga del paquete o sus funciones como parte del script

Una vez completada la instalación, puede empezar a usar el paquete inmediatamente en scripts de Python en SQL Server.

Para usar las funciones del paquete en el script, inserte la instrucción `import <package_name>` estándar en las líneas iniciales del script:

```sql
EXECUTE sp_execute_external_script 
  @language = N'Python', 
  @script = N'
import cntk
# Python statements ...
'
```

## <a name="see-also"></a>Consulte también

+ [Obtención de información de paquetes de Python](python-package-information.md)
+ [Tutoriales de Python para SQL Server Machine Learning Services](../tutorials/python-tutorials.md)
+ [API de Python para CNTK](https://cntk.ai/pythondocs/tutorials.html).