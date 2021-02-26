---
title: Novedades de SQL Server Machine Learning Services
titleSuffix: ''
description: Anuncios de nuevas características para cada versión de SQL Server Machine Learning Services and SQL Server 2016 R Services.
ms.date: 11/17/2020
ms.topic: overview
author: dphansen
ms.author: davidph
ms.custom: sqlseattle
ms.prod: sql
ms.technology: machine-learning-services
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 31ecd76efc4796aebe2a3ee40a8bf40891d38a83
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272936"
---
# <a name="whats-new-in-sql-server-machine-learning-services"></a>Novedades de SQL Server Machine Learning Services
[!INCLUDE [SQL Server 2016 and later](../includes/applies-to-version/sqlserver2016.md)]

En este artículo se describen las nuevas funcionalidades y características que se incluyen en cada versión de [Machine Learning Services de SQL Server](sql-server-machine-learning-services.md). Se han agregado funciones de aprendizaje automático a SQL Server en cada versión a medida que se continúa la expansión, ampliación y profundización de la integración entre la plataforma de datos, el análisis avanzado y la ciencia de datos. 

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
## <a name="new-in-sql-server-2019"></a>Novedades de SQL Server 2019

En esta versión se agregan las características principales solicitadas para las operaciones de aprendizaje automático de Python y R en SQL Server. Para obtener más información sobre todas las características de esta versión, consulte [Novedades de SQL Server 2019](../sql-server/what-s-new-in-sql-server-ver15.md) y [Notas de la versión de SQL Server 2019](../sql-server/sql-server-version-15-release-notes.md).

> [!NOTE]
> Para obtener la documentación sobre las novedades de Java en SQL Server 2019, vea las [Novedades de Extensiones de lenguaje de SQL Server](../language-extensions/language-extensions-whats-new.md).

A continuación, se muestran las características nuevas para SQL Server Machine Learning Services, disponibles en **Windows** y en **Linux**:

- Se ha agregado compatibilidad con la plataforma Linux en Machine Learning Services para Python y R. Comience con la [Instalación de SQL Server Machine Learning Services en Linux](../linux/sql-server-linux-setup-machine-learning.md).
- [Conexión de bucle invertido con SQL Server desde un script de Python o R](connect/loopback-connection.md). 
- [CREATE EXTERNAL LIBRARY (Transact-SQL)](../t-sql/statements/create-external-library-transact-sql.md) para Python y R.
- [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) introduce dos parámetros nuevos que le permiten generar fácilmente varios modelos a partir de datos con particiones. Obtenga más información en este tutorial, [Creación de modelos basados en particiones en R](tutorials/r-tutorial-create-models-per-partition.md).
- La compatibilidad con el clúster de conmutación por error está disponible para el servicio Launchpad, siempre que el servicio SQL Server Launchpad se haya iniciado en todos los nodos. Para obtener más información, vea [Instalación de clúster de conmutación por error de SQL Server](../sql-server/failover-clusters/install/sql-server-failover-cluster-installation.md).
- Cambios en los mecanismos de aislamiento para Machine Learning Services. Para obtener más información, vea [SQL Server 2019 en Windows: Cambios de aislamiento para Machine Learning Services](install/sql-server-machine-learning-services-2019.md).

::: moniker-end

::: moniker range=">=sql-server-2017"
## <a name="new-in-sql-server-2017"></a>Novedades de SQL Server 2017

En esta versión se agregan [compatibilidad con Python y algoritmos de aprendizaje automático líderes del sector](https://cloudblogs.microsoft.com/sqlserver/2017/04/19/python-in-sql-server-2017-enhanced-in-database-machine-learning/). Después de cambiar el nombre para reflejar el nuevo ámbito, SQL Server 2017 marca la introducción de [SQL Server Machine Learning Services (en base de datos)](sql-server-machine-learning-services.md), con compatibilidad de lenguaje para Python y R. 

Para obtener todos los anuncios de características, vea [Novedades de SQL Server 2017](../sql-server/what-s-new-in-sql-server-2017.md).

### <a name="r-enhancements"></a>Mejoras de R

El componente R de SQL Server Machine Learning Services es la próxima generación de SQL Server 2016 R Services, con versiones actualizadas de R, RevoScaler y otros paquetes.

Las nuevas funcionalidades de R incluyen la [**administración de paquetes**](package-management/install-r-packages-with-tsql.md), con los siguientes aspectos destacados: 

+ Los roles de base de datos ayudan a los DBA a administrar paquetes y asignar permisos para la instalación de paquetes.
+ [CREATE EXTERNAL LIBRARY](../t-sql/statements/create-external-library-transact-sql.md) ayuda a los DBA a administrar paquetes en el conocido lenguaje T-SQL.
+ Las funciones de [RevoScaleR](package-management/install-r-packages-with-revoscaler.md) ayudan a instalar, quitar o enumerar los paquetes que pertenecen a los usuarios. Para obtener más información, vea [Uso de las funciones de RevoScaleR para buscar o instalar paquetes de R en SQL Server](package-management/install-r-packages-with-revoscaler.md).

### <a name="r-libraries"></a>Bibliotecas de R

| Paquete | Descripción |
|---------|-------------|
| [**MicrosoftML**](r/ref-r-microsoftml.md) | En esta versión, MicrosoftML se incluye en una instalación predeterminada de R, lo que elimina el paso de actualización necesario en los servicios de SQL Server 2016 R anteriores. MicrosoftML proporciona algoritmos de aprendizaje automático de última generación y transformaciones de datos que se pueden escalar o ejecutar en contextos de cálculo remotos. Los algoritmos incluyen redes neuronal profundas personalizables, árboles de decisión y bosques de decisión rápidos, regresión lineal y regresión logística.  |

### <a name="python-integration-for-in-database-analytics"></a>Integración de Python para el análisis en base de datos

Python es un lenguaje que ofrece una gran flexibilidad y capacidad para distintas tareas de aprendizaje automático. Las bibliotecas de código abierto para Python incluyen varias plataformas para redes neuronales personalizables, así como bibliotecas conocidas para el procesamiento de lenguaje natural. 

Como Python se integra con el motor de base de datos, puede mantener el análisis cerca de los datos y eliminar los costos y riesgos de seguridad asociados con el movimiento de datos. Puede implementar soluciones de aprendizaje automático basadas en Python mediante herramientas como Visual Studio. Las aplicaciones de producción pueden obtener predicciones, modelos u objetos visuales del tiempo de ejecución de Python 3.5 mediante métodos de acceso a datos de SQL Server.

La integración de T-SQL y Python se admite a través del procedimiento almacenado del sistema [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Puede llamar a cualquier código de Python mediante este procedimiento almacenado. El código se ejecuta en una arquitectura segura y dual que permite la implementación de nivel empresarial de modelos y scripts de Python, a los que se puede llamar desde una aplicación mediante un procedimiento almacenado simple. Se consiguen mejoras de rendimiento adicionales mediante el streaming de datos desde SQL a procesos de Python y la paralelización de anillos de MPI.

Puede usar la función [PREDICT](../t-sql/queries/predict-transact-sql.md) de T-SQL para realizar la [puntuación nativa](predictions/native-scoring-predict-transact-sql.md) en un modelo previamente entrenado que se haya guardado antes en el formato binario requerido.

### <a name="python-libraries"></a>Bibliotecas de Python

| Paquete | Descripción |
|---------|-------------|
|[**revoscalepy**](python/ref-py-revoscalepy.md)| Equivalente de RevoScaleR en Python. Puede crear modelos de Python para regresiones lineales y logísticas, árboles de decisión, árboles mejorados y bosques aleatorios, que se pueden usar en paralelo y ejecutarse en contextos de cálculo remotos. Este paquete admite el uso de varios orígenes de datos y contextos de cálculo remotos. El científico de datos o el desarrollador puede ejecutar código de Python en una instancia remota de SQL Server para explorar datos o crear modelos sin mover datos. |
|[**microsoftml**](python/ref-py-microsoftml.md) |El equivalente al paquete de R MicrosoftML en Python. |

### <a name="pre-trained-models"></a>Modelos entrenados previamente

Existen [**modelos previamente entrenados**](install/sql-pretrained-models-install.md) para Python y R. Use estos modelos para el reconocimiento de imágenes y el análisis de opiniones positivas y negativas, con el fin de generar predicciones sobre datos propios. 

### <a name="standalone-server-as-a-shared-feature-in-sql-server-setup"></a>Servidor independiente como una característica compartida en el programa de instalación de SQL Server

En esta versión también se agrega [SQL Server Machine Learning Server (independiente)](r/r-server-standalone.md), un servidor de ciencia de datos totalmente independiente, que admite el análisis predictivo y estadístico en R y Python. Como sucede con R Services, este servidor es la próxima versión de SQL Server 2016 R Server (independiente). Con el servidor independiente, puede distribuir y escalar soluciones de R o Python sin dependencias en SQL Server.
::: moniker-end

::: moniker range=">=sql-server-2016"
## <a name="new-in-sql-server-2016"></a>Novedades de SQL Server 2016

En esta versión se han introducido funciones de aprendizaje automático en SQL Server a través de **SQL Server 2016 R Services**, un motor de análisis en la base de datos para procesar scripts de R en datos residentes dentro de una instancia del motor de base de datos.

Además, se ha publicado **SQL Server 2016 R Server (independiente)** como una manera de instalar R Server en un servidor de Windows. Inicialmente, el programa de instalación de SQL Server proporcionaba la única forma de instalar R Server para Windows. En versiones posteriores, los desarrolladores y científicos de datos que querían R Server en Windows podían usar otro instalador independiente para lograr el mismo objetivo. El servidor independiente en SQL Server es funcionalmente equivalente al producto de servidor independiente, [Microsoft R Server para Windows](/machine-learning-server/install/r-server-install-windows).

Para obtener todos los anuncios de características, vea [Novedades de SQL Server 2016](../sql-server/what-s-new-in-sql-server-2016.md).

| Release |Actualización de características |
|---------|----------------|
| Adiciones de CU | La [**puntuación en tiempo real**](predictions/real-time-scoring.md) se basa en bibliotecas nativas de C++ para leer un modelo almacenado en un formato binario optimizado y, después, generar predicciones sin tener que llamar al tiempo de ejecución de R. Esto hace que las operaciones de puntuación sean mucho más rápidas. Con la puntuación en tiempo real, puede ejecutar un procedimiento almacenado o realizar una puntuación en tiempo real desde código de R. La puntuación en tiempo real también está disponible para SQL Server 2016, si la instancia se actualiza a la versión más reciente de [!INCLUDE[rsql-platform-md](../includes/rsql-platform-md.md)]. |
| Versión inicial | [**Integración de R para el análisis en base de datos**](r/sql-server-r-services.md). <br/><br/> Paquetes de R para llamar a funciones de R en T-SQL y viceversa. Las funciones de RevoScaleR proporcionan análisis de R a escala mediante la fragmentación de datos en partes de componente, la coordinación y la administración del procesamiento distribuido, y la agregación de resultados. En SQL Server 2016 R Services (en base de datos), el motor de RevoScaleR se integra con una instancia del motor de base de datos, donde se agrupan los datos y los análisis en el mismo contexto de procesamiento. <br/><br/>Integración de T-SQL y R a través de [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Puede llamar a cualquier código de R mediante este procedimiento almacenado. Esta infraestructura segura permite la implementación de nivel empresarial de modelos y scripts de Rn, a los que se puede llamar desde una aplicación mediante un procedimiento almacenado simple. Se consiguen mejoras de rendimiento adicionales mediante el streaming de datos desde SQL a procesos de R y la paralelización de anillos de MPI. <br/><br/>Puede usar la función [PREDICT](../t-sql/queries/predict-transact-sql.md) de T-SQL para realizar la [puntuación nativa](predictions/native-scoring-predict-transact-sql.md) en un modelo previamente entrenado que se haya guardado antes en el formato binario requerido.|

::: moniker-end

::: moniker range=">=sql-server-2017"
## <a name="linux-support"></a>Compatibilidad de Linux

SQL Server 2019 agrega compatibilidad con Linux para R y Python cuando se instalan los paquetes de aprendizaje automático con una instancia del motor de base de datos. Para obtener más información, vea [Instalación de SQL Server Machine Learning Services en Linux](../linux/sql-server-linux-setup-machine-learning.md).

En Linux, SQL Server 2017 no tiene integración de R o Python, pero puede usar la [puntuación nativa](predictions/native-scoring-predict-transact-sql.md) en Linux porque esa funcionalidad está disponible a través de [PREDICT](../t-sql/queries/predict-transact-sql.md) de T-SQL, que se ejecuta en Linux. La puntuación nativa habilita la puntuación de alto rendimiento a partir de un modelo previamente entrenado, sin llamar a un tiempo de ejecución de R ni siquiera requerirlo.
::: moniker-end

## <a name="next-steps"></a>Pasos siguientes

+ [Instalación de SQL Server Machine Learning Services (en base de datos)](install/sql-machine-learning-services-windows-install.md)
