---
title: ¿Qué es la extensión del lenguaje Java?
titleSuffix: SQL Server Language Extensions
description: La extensión de lenguaje Java es una característica de SQL Server que se usa para ejecutar código Java externo. Los datos relacionales se pueden usar en el código Java externo mediante el uso del marco de extensibilidad.
author: dphansen
ms.author: davidph
ms.date: 11/10/2020
ms.topic: overview
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 55f8ca913c058bb90e4abe6b91f997d130af22b3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070541"
---
# <a name="what-is-java-language-extension"></a>¿Qué es la extensión del lenguaje Java?
[!INCLUDE [SQL Server 2019 and later](../includes/applies-to-version/sqlserver2019.md)]

La extensión de lenguaje Java es una característica de SQL Server que se usa para ejecutar código Java externo. Los datos relacionales se pueden usar en el código Java externo mediante el uso del [marco de extensibilidad](concepts/extensibility-framework.md). La extensión de lenguaje Java forma parte de las [Extensiones de lenguaje de SQL Server](language-extensions-overview.md).

El entorno de ejecución predeterminado de Java es Zulu Open JRE. También puede usar otro JRE o SDK de Java.

## <a name="what-you-can-do-with-the-java-language-extension"></a>Qué puede hacer con la extensión de lenguaje Java

La extensión de lenguaje Java usa el marco de extensibilidad para ejecutar código Java externo. La ejecución del código está aislada de los procesos principales del motor, pero está totalmente integrada con la ejecución de consultas de SQL Server. Puede ejecutar código Java en el origen de los datos, lo que elimina la necesidad de extraer datos a través de la red.

El lenguaje Java externo se define con [CREATE EXTERNAL LANGUAGE](../t-sql/statements/create-external-language-transact-sql.md). El procedimiento almacenado del sistema [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) se usa como interfaz para ejecutar el código Java.

## <a name="get-started-with-java-language-extension"></a>Introducción a la extensión de lenguaje Java

1. [Instale la extensión de lenguaje Java de SQL Server en Windows](install/windows-java.md) o [en Linux](../linux/sql-server-linux-setup-language-extensions-java.md).

1. Configure una herramienta de desarrollo.

    + Use el IDE que prefiera para desarrollar código Java.
    + Instale el [SDK de extensibilidad para Java de Microsoft](how-to/extensibility-sdk-java-sql-server.md) para ejecutar código Java en SQL Server.
    + Use [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md) para ejecutar código externo en SQL Server.
    + Use el procedimiento almacenado del sistema [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) para ejecutar el código Java en SQL Server.

1. Escriba su primer código Java.

    + [Tutorial: Expresiones regulares con Java](tutorials/search-for-string-using-regular-expressions-in-java.md)

## <a name="limitations"></a>Limitaciones

El número de valores de los búferes de entrada y salida no puede ser superior a `MAX_INT (2^31-1)`, ya que es el número máximo de elementos que se pueden asignar en una matriz en Java.

## <a name="next-steps"></a>Pasos siguientes

+ Instalación de la [extensión de lenguaje Java de SQL Server en Windows](install/windows-java.md) o [en Linux](../linux/sql-server-linux-setup-language-extensions-java.md)
+ Instalación del [SDK de extensibilidad de Microsoft para Java](how-to/extensibility-sdk-java-sql-server.md)