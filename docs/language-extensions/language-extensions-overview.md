---
title: ¿Qué son las extensiones de lenguaje de SQL Server?
titleSuffix: ''
description: Las extensiones de lenguaje son una característica de SQL Server que se usa para ejecutar código externo. En SQL Server, se admiten Java, R y Python. Los datos relacionales se pueden usar en el código externo mediante el uso del marco de extensibilidad.
author: dphansen
ms.author: davidph
ms.date: 11/06/2020
ms.topic: overview
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 50ebe13c330dba1067c2ccc61bcbc779f7bf0b05
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070630"
---
# <a name="what-is-sql-server-language-extensions"></a>¿Qué son las extensiones de lenguaje de SQL Server?
[!INCLUDE [SQL Server 2019 and later](../includes/applies-to-version/sqlserver2019.md)]

Las extensiones de lenguaje son una característica de SQL Server que se usa para ejecutar código externo. Los datos relacionales se pueden usar en el código externo mediante el uso del [marco de extensibilidad](concepts/extensibility-framework.md). En SQL Server 2019, se admiten entornos de ejecución de R, Python y Java.

> [!NOTE]
> Para ejecutar Python o R en SQL Server, consulte la documentación de [Machine Learning Services](../machine-learning/sql-server-machine-learning-services.md). Con SQL Server 2019 y versiones posteriores, puede usar un tiempo de ejecución personalizado de Python y R con extensiones de lenguaje. Para más información, consulte cómo instalar el [entorno de ejecución personalizado de Python](../machine-learning/install/custom-runtime-python.md) y el [entorno de ejecución personalizado de R](../machine-learning/install/custom-runtime-r.md).

## <a name="what-you-can-do-with-language-extensions"></a>Qué puede hacer con las extensiones de lenguaje

Las extensiones de lenguaje usan el [marco de extensibilidad](concepts/extensibility-framework.md) para ejecutar código externo. La ejecución del código está aislada de los procesos principales del motor, pero está totalmente integrada con la ejecución de consultas de SQL Server. Puede ejecutar código en el origen de los datos, lo que elimina la necesidad de extraer datos a través de la red.

Los lenguajes externos se definen con [CREATE EXTERNAL LANGUAGE](../t-sql/statements/create-external-language-transact-sql.md). El procedimiento almacenado del sistema [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) se usa como interfaz para ejecutar el código.

Las extensiones de lenguaje proporcionan varias ventajas:

+ Seguridad de datos. La ejecución del lenguaje externo más cerca del origen de datos evita el movimiento de datos no seguro.
+ Velocidad. Las bases de datos están optimizadas para operaciones basadas en conjuntos. 
+ Facilidad de implementación e integración. [!INCLUDE [ssNoVersion](../includes/ssnoversion-md.md)] es el punto central de operaciones para muchas otras tareas y aplicaciones de administración de datos. Si usa los datos que residen en la base de datos, tendrá la garantía de que los datos usados por la extensión de lenguaje son coherentes y están actualizados.

## <a name="next-steps"></a>Pasos siguientes

+ Instalación de [extensiones de lenguaje de SQL Server en Windows](install/windows-java.md) o en [Linux](../linux/sql-server-linux-setup-language-extensions-java.md)
+ Instalación de un [tiempo de ejecución personalizado de Python para SQL Server](../machine-learning/install/custom-runtime-python.md)
+ Instalación de un [tiempo de ejecución personalizado de R para SQL Server](../machine-learning/install/custom-runtime-r.md)
+ Instalación del [SDK de extensibilidad de Microsoft para Java](how-to/extensibility-sdk-java-sql-server.md)
