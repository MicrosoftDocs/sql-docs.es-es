---
title: ¿Qué es SQL Server Machine Learning Services (Python y R)?
titleSuffix: ''
description: Machine Learning Services es una característica de SQL Server que proporciona la capacidad de ejecutar scripts de Python y R con datos relacionales. Para llevar a cabo análisis predictivo y aprendizaje automático, se pueden usar marcos y paquetes de código abierto, además de paquetes de Python y R de Microsoft. Los scripts se ejecutan en la base de datos sin mover los datos fuera de SQL Server o a través de la red. En este artículo se explican los conceptos básicos de SQL Server Machine Learning Services y cómo empezar a usarlo.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/10/2020
ms.topic: overview
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 23206301c399449042380976e985ec3981933caa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340317"
---
# <a name="what-is-sql-server-machine-learning-services-with-python-and-r"></a>¿Qué es Machine Learning Services para SQL Server con Python y R?
[!INCLUDE [SQL Server 2017 SQL MI](../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Machine Learning Services es una característica de SQL Server que proporciona la capacidad de ejecutar scripts de Python y R con datos relacionales. Para llevar a cabo análisis predictivo y aprendizaje automático, se pueden usar marcos y paquetes de código abierto, además de [paquetes de Python y R de Microsoft](#packages). Los scripts se ejecutan en la base de datos sin mover los datos fuera de SQL Server o a través de la red. En este artículo se explican los conceptos básicos de SQL Server Machine Learning Services y cómo empezar a usarlo.

Para obtener información sobre el aprendizaje automático en otras plataformas de SQL, consulte la [documentación del aprendizaje automático de SQL](index.yml).

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
> [!NOTE]
> Para ejecutar Java en SQL Server, consulte la [documentación sobre la extensión de lenguaje Java](../language-extensions/java-overview.md).
::: moniker-end

## <a name="execute-python-and-r-scripts-in-sql-server"></a>Ejecución de scripts de Python y R en SQL Server

SQL Server Machine Learning Services permite ejecutar scripts de Python y R en la base de datos. Se puede usar para preparar y limpiar los datos, realizar ingeniería de características, y entrenar, evaluar e implementar modelos de aprendizaje automático en una base de datos. La característica ejecuta los scripts donde residen los datos y elimina la transferencia de los datos a otro servidor a través de la red.

Puede ejecutar scripts de Python y R en una instancia de SQL Server con el procedimiento almacenado [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

Machine Learning Services incluye las distribuciones base de Python y R. Se pueden instalar y usar marcos y paquetes de código abierto, como PyTorch, TensorFlow y scikit-learn, además de los paquetes de Microsoft.

Machine Learning Services usa un marco de extensibilidad para ejecutar scripts de Python y R en SQL Server. Más información sobre cómo funciona:

+ [Plataforma de extensibilidad](concepts/extensibility-framework.md)
+ [Extensión de Python](concepts/extension-python.md)
+ [Extensión de R](concepts/extension-r.md)

## <a name="get-started-with-machine-learning-services"></a>Introducción a Machine Learning Services

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
1. [Instale Machine Learning Services de SQL Server en Windows](install/sql-machine-learning-services-windows-install.md) o [en Linux](../linux/sql-server-linux-setup-machine-learning.md?toc=/sql/machine-learning/toc.json). También puede usar [Machine Learning Services en clústeres de macrodatos](../big-data-cluster/machine-learning-services.md) y [Machine Learning Services en Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview).

1. Configure las herramientas de desarrollo. Puede [ejecutar scripts de Python y R en cuadernos de Azure Data Studio](install/sql-machine-learning-azure-data-studio.md). También puede ejecutar T-SQL en [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md).

1. Escriba su primer script de Python o R.

   + [Tutoriales de Python para aprendizaje automático de SQL](tutorials/python-tutorials.md)
   + [Tutoriales de R para aprendizaje automático de SQL](tutorials/r-tutorials.md)
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
+ Escriba su primer script de Python o R.

   + [Tutoriales de Python para aprendizaje automático de SQL](tutorials/python-tutorials.md)
   + [Tutoriales de R para aprendizaje automático de SQL](tutorials/r-tutorials.md)
::: moniker-end

::: moniker range="=sql-server-2017"
1. [Instale Machine Learning Services de SQL Server en Windows](install/sql-machine-learning-services-windows-install.md).

1. Configure las herramientas de desarrollo. Puede [ejecutar scripts de Python y R en cuadernos de Azure Data Studio](install/sql-machine-learning-azure-data-studio.md). También puede usar T-SQL en [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md).

1. Escriba su primer script de Python o R.

   + [Tutoriales de Python para aprendizaje automático de SQL](tutorials/python-tutorials.md)
   + [Tutoriales de R para aprendizaje automático de SQL](tutorials/r-tutorials.md)
::: moniker-end

<a name="versions"></a>

## <a name="python-and-r-versions"></a>Versiones de Python y R

A continuación se muestran las versiones de Python y R incluidas en Machine Learning Services.

| Versión de SQL Server | Versión de Python | Versión de R |
|-|-|-|
| SQL Server 2017 | 3.5.2 | 3.3.3 |
| SQL Server 2019 | 3.7.3 | 3.5.2 |

Para la versión de R en SQL Server 2016, vea la [sección Versión de R de ¿Qué es R Services?](r/sql-server-r-services.md?view=sql-server-2016&preserve-view=true#version)

<a name="packages"></a>

## <a name="python-and-r-packages"></a>Paquetes de Python y R

Además de los paquetes de empresa de Microsoft, pueden usarse usar marcos y paquetes de código abierto. Los paquetes de Python y R de código abierto más comunes están preinstalados en Machine Learning Services. También se incluyen los siguientes paquetes de Python y R de Microsoft:

| Idioma | Paquete | Descripción |
|-|-|-|
| Python | [revoscalepy](python/ref-py-revoscalepy.md) | Es el paquete principal para Python escalable. Transformaciones y manipulación de datos, resumen estadístico, visualización y muchas formas de modelado. Además, las funciones de este paquete distribuyen automáticamente las cargas de trabajo entre los núcleos disponibles para su procesamiento paralelo. |
| Python | [microsoftml](python/ref-py-microsoftml.md) | Agrega algoritmos de aprendizaje automático para crear modelos personalizados dedicados al análisis de texto, imágenes y opiniones. | 
| R | [RevoScaleR](r/ref-r-revoscaler.md) | Este es el paquete principal para R escalable. Permite realizar transformaciones y manipulaciones de datos, resúmenes estadísticos, visualizaciones y muchas formas de modelado. Además, las funciones de este paquete distribuyen automáticamente las cargas de trabajo entre los núcleos disponibles para su procesamiento paralelo. |
| R | [MicrosoftML (R)](r/ref-r-microsoftml.md) | Agrega algoritmos de aprendizaje automático para crear modelos personalizados dedicados al análisis de texto, imágenes y opiniones. |
| R | [olapR](r/ref-r-olapr.md) | Se trata de funciones de R usadas para las consultas MDX en un cubo OLAP de SQL Server Analysis Services. |
| R | [sqlrutils](r/ref-r-sqlrutils.md) | Este es un mecanismo para usar scripts de R en un procedimiento almacenado de T-SQL, registrar dicho procedimiento almacenado en una base de datos y ejecutarlo en un [entorno de desarrollo de R](r/set-up-a-data-science-client.md). |
| R | [Microsoft R Open](https://mran.microsoft.com/rro) | Microsoft R Open (MRO) es la distribución mejorada de Microsoft R. Se trata de una plataforma de código abierto completa dedicada al análisis estadístico y la ciencia de datos. Basada en R y compatible al 100 % con ese lenguaje, incluye capacidades adicionales para mejorar el rendimiento y la reproducibilidad. |

Para obtener más información sobre los paquetes que se instalan con Machine Learning Services y cómo instalar otros paquetes, consulte:

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
+ [Obtención de información de paquetes de Python](package-management/python-package-information.md)
+ [Instalación de paquetes de Python con sqlmlutils](package-management/install-additional-python-packages-on-sql-server.md)
+ [Obtención de información de paquetes de R](package-management/r-package-information.md)
+ [Instalación de nuevos paquetes de R con sqlmlutils](package-management/install-additional-r-packages-on-sql-server.md)
::: moniker-end
::: moniker range="=sql-server-2017"
+ [Obtención de información de paquetes de Python](package-management/python-package-information.md)
+ [Instalación de paquetes con las herramientas de Python en SQL Server](package-management/install-python-packages-standard-tools.md)
+ [Obtención de información de paquetes de R](package-management/r-package-information.md)
+ [Uso de T-SQL (CREATE EXTERNAL LIBRARY) para instalar paquetes de R en SQL Server](package-management/install-r-packages-with-tsql.md)
::: moniker-end

## <a name="next-steps"></a>Pasos siguientes

+ [Instalación de SQL Server Machine Learning Services en Windows](install/sql-machine-learning-services-windows-install.md) o [en Linux](../linux/sql-server-linux-setup-machine-learning.md?toc=/sql/machine-learning/toc.json)
+ [Tutoriales de Python para aprendizaje automático de SQL](tutorials/python-tutorials.md)
+ [Tutoriales de R para aprendizaje automático de SQL](tutorials/r-tutorials.md)