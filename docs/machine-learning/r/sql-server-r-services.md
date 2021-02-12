---
title: ¿Qué es SQL Server 2016 R Services?
titleSuffix: ''
description: R Services es una característica de SQL Server 2016 que ofrece la posibilidad de ejecutar scripts de R con datos relacionales. Para realizar un análisis predictivo y aprendizaje automático, se pueden usar plataformas y paquetes de código abierto, además de paquetes de Microsoft R. Los scripts se ejecutan en la base de datos sin mover los datos fuera de SQL Server o a través de la red. En este artículo se explican los conceptos básicos de SQL Server R Services.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 08/06/2020
ms.topic: overview
author: dphansen
ms.author: davidph
monikerRange: =sql-server-2016
ms.openlocfilehash: 87c3afd4e8fc96bf753067c06de2a60741bbbadb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100020119"
---
# <a name="what-is-sql-server-2016-r-services"></a>¿Qué es SQL Server 2016 R Services?

[!INCLUDE[SQL Server 2016 only](../../includes/applies-to-version/sqlserver2016-only.md)]

R Services es una característica de SQL Server 2016 que ofrece la posibilidad de ejecutar scripts de R con datos relacionales. Para realizar un análisis predictivo y aprendizaje automático, se pueden usar plataformas y paquetes de código abierto, además de [paquetes de Microsoft R](#packages). Los scripts se ejecutan en la base de datos sin mover los datos fuera de SQL Server o a través de la red. En este artículo se explican los conceptos básicos de SQL Server R Services.

> [!Note]
> Se ha cambiado el nombre de R Services a [Machine Learning Services](../sql-server-machine-learning-services.md) en SQL Server 2017 y versiones posteriores, y es compatible con Python y R.

## <a name="what-is-r-services"></a>¿Qué es R Services?

SQL Server R Services permite ejecutar scripts de R en la base de datos. Se puede usar para preparar y limpiar los datos, realizar ingeniería de características, y entrenar, evaluar e implementar modelos de aprendizaje automático en una base de datos. La característica ejecuta los scripts donde residen los datos y elimina la transferencia de los datos a otro servidor a través de la red.

Las distribuciones base de R se incluyen en R Services. Puede usar paquetes y plataformas de código abierto, además de los paquetes [RevoScaleR](../r/ref-r-revoscaler.md), [MicrosoftML](../r/ref-r-microsoftml.md), [olapR]../r/ref-r-olapr.md) y [sqlrutils](../r/ref-r-sqlrutils.md) de Microsoft para R.

R Services usa una plataforma de extensibilidad para ejecutar scripts de R en SQL Server. Más información sobre cómo funciona:

+ [Plataforma de extensibilidad](../concepts/extensibility-framework.md)
+ [Extensión de R](../concepts/extension-r.md)

## <a name="what-can-i-do-with-r-services"></a>¿Qué se puede hacer con R Services?

R Services puede usarse para compilar y entrenar modelos de aprendizaje automático y de aprendizaje profundo en SQL Server. También es posible implementar modelos existentes en R Services y usar datos relacionales para las predicciones.

Estos son algunos de los ejemplos del tipo de predicciones para los que se puede usar SQL Server R Services:

|Tipo de predicción|Ejemplo|
|-|-|
|Clasificación o categorización|División automática de los comentarios de los clientes en categorías positivas y negativas|
|Regresión o predicción de valores continuos|Predicción del precio de viviendas en función del tamaño y la ubicación|
|Detección de anomalías|Detección de transacciones bancarias fraudulentas |
|Recomendaciones|Sugerencias de productos que pueden interesar a los compradores en Internet en función de compras anteriores|

### <a name="how-to-execute-r-scripts"></a>Cómo ejecutar scripts de R

Hay dos maneras de ejecutar scripts de R en R Services:

+ La manera más común es usar el procedimiento almacenado de T-SQL [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

+ También puede usar su cliente de R preferido y escribir scripts que fuercen la ejecución (denominada *contexto de proceso remoto*) en una instancia de SQL Server remota. Para obtener más información, vea cómo [configurar el desarrollo de R de un cliente de ciencia de datos](../r/set-up-a-data-science-client.md).

<a name="version"></a>

## <a name="r-versions"></a>Versiones de R

A continuación se enumeran las versiones del runtime de R que se incluyen en SQL Server 2016 R Services.

Versión de SQL Server | Versiones predeterminadas del runtime de R |
|-|-|
| SQL Server 2016 RTM - SP2 CU13 | 3.2.2 |
| SQL Server 2016 SP2 CU14 y versiones posteriores | 3.2.2 y 3.5.2 |

La actualización acumulativa (CU) 14 para SQL Server 2016 Service Pack (SP) 2 y versiones posteriores incluyen los runtime de R más recientes. Para obtener más información, vea [Cambio a la versión predeterminada del runtime de lenguaje predeterminado](../install/change-default-language-runtime-version.md).

Para otras versiones de R, o para ejecutar Python, use [Machine Learning Services para SQL Server 2017 y versiones posteriores](../sql-server-machine-learning-services.md).

<a name="packages"></a>

## <a name="r-packages"></a>Paquetes de R

Además de los paquetes de empresa de Microsoft, pueden usarse usar marcos y paquetes de código abierto. Los paquetes de R de código abierto más comunes están preinstalados en R Services. También se incluyen los siguientes paquetes de Microsoft R:

| Paquete | Descripción |
|-|-|
| [RevoScaleR](../r/ref-r-revoscaler.md) | Este es el paquete principal para R escalable. Permite realizar transformaciones y manipulaciones de datos, resúmenes estadísticos, visualizaciones y muchas formas de modelado. Además, las funciones de este paquete distribuyen automáticamente las cargas de trabajo entre los núcleos disponibles para su procesamiento paralelo. |
| [MicrosoftML (R)](../r/ref-r-microsoftml.md) | Agrega algoritmos de aprendizaje automático para crear modelos personalizados dedicados al análisis de texto, imágenes y opiniones. |
| [olapR](../r/ref-r-olapr.md) | Se trata de funciones de R usadas para las consultas MDX en un cubo OLAP de SQL Server Analysis Services. |
| [sqlrutils](../r/ref-r-sqlrutils.md) | Este es un mecanismo para usar scripts de R en un procedimiento almacenado de T-SQL, registrar dicho procedimiento almacenado en una base de datos y ejecutarlo en un [entorno de desarrollo de R](../r/set-up-a-data-science-client.md). |
| [Microsoft R Open](https://mran.microsoft.com/rro) | Microsoft R Open (MRO) es la distribución mejorada de Microsoft R. Se trata de una plataforma de código abierto completa dedicada al análisis estadístico y la ciencia de datos. Basada en R y compatible al 100 % con ese lenguaje, incluye capacidades adicionales para mejorar el rendimiento y la reproducibilidad. |

## <a name="how-do-i-get-started-with-rservices"></a>¿Cómo empezar con R Services?

1. [Instalación de SQL Server 2016 R Services](../install/sql-r-services-windows-install.md)

1. Configure las herramientas de desarrollo. Puede usar:

    + [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) o [SQL Server Management Studio (SSMS)](../../ssms/sql-server-management-studio-ssms.md) para usar T-SQL y el procedimiento almacenado [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) con el fin de ejecutar el script de R.
    + R en su propio equipo portátil o estación de trabajo de desarrollo para ejecutar scripts. Puede extraer datos de forma local u ordenar la ejecución de forma remota en SQL Server con [RevoScaleR](../r/ref-r-revoscaler.md). Para obtener más información, vea cómo [configurar el desarrollo de R de un cliente de ciencia de datos](../r/set-up-a-data-science-client.md).

1. Escritura del primer script de R

    + Inicio rápido: [Creación y ejecución de scripts de R sencillos en SQL Server](../tutorials/quickstart-r-create-script.md)
    + Inicio rápido: [Creación y entrenamiento de un modelo predictivo en R](../tutorials/quickstart-r-train-score-model.md)
    + Tutorial: [Uso de R en T-SQL](../tutorials/r-taxi-classification-introduction.md): explore datos, realice ingeniería de características, entrene e implemente modelos y haga predicciones (serie de cinco partes).
    + Tutorial: [Uso de R Services en herramientas de R](../tutorials/walkthrough-data-science-end-to-end-walkthrough.md): explore datos, cree gráficos y trazados, realice ingeniería de características, entrene e implemente modelos y haga predicciones (serie de seis partes).

## <a name="next-steps"></a>Pasos siguientes

+ [Instalación de SQL Server 2016 R Services](../install/sql-r-services-windows-install.md)
+ [Configuración de un cliente de ciencia de datos para el desarrollo de R](../r/set-up-a-data-science-client.md)