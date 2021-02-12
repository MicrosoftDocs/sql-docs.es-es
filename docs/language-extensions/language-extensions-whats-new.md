---
title: Novedades de las extensiones de lenguaje de SQL Server
titleSuffix: ''
description: Obtenga información sobre las novedades de las extensiones de lenguaje de SQL Server que amplía, amplía y profundiza la integración entre lenguajes externos y la plataforma de datos.
author: dphansen
ms.author: davidph
ms.date: 11/09/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: e72b202230573352eca54bd8bc9d2afc0a58f29e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070627"
---
# <a name="whats-new-in-sql-server-language-extensions"></a>Novedades de las extensiones de lenguaje de SQL Server
[!INCLUDE [SQL Server 2019 and later](../includes/applies-to-version/sqlserver2019.md)]

Se han agregado funcionalidades de [extensiones de lenguaje](language-extensions-overview.md) a SQL Server en cada versión a medida que se continúa la expansión, ampliación y profundización de la integración entre los lenguajes externos y la plataforma de datos.

## <a name="sql-server-2019"></a>SQL Server 2019

A continuación, puede encontrar las funcionalidades nuevas para [Extensión de lenguajes](language-extensions-overview.md) en SQL Server 2019. Para obtener más información sobre todas las características de esta versión, consulte [Novedades de SQL Server 2019](../sql-server/what-s-new-in-sql-server-ver15.md) y [Notas de la versión de SQL Server 2019](../sql-server/sql-server-version-15-release-notes.md).

### <a name="new-python-and-r-language-extensions"></a>Nuevas extensiones de lenguajes Python y R

- Hay disponible un [entorno de ejecución personalizado de Python](../machine-learning/install/custom-runtime-python.md) con las Extensiones de lenguajes. Para más información, consulte cómo [instalar un entorno de ejecución personalizado de Python en Windows](../machine-learning/install/custom-runtime-python.md?view=sql-server-ver15&preserve-view=true) o [instalar un entorno de ejecución personalizado de Python en Linux](../machine-learning/install/custom-runtime-python.md?view=sql-server-linux-ver15&preserve-view=true).

- Hay disponible un [entorno de ejecución personalizado de R](../machine-learning/install/custom-runtime-r.md) con las Extensiones de lenguajes. Para más información, consulte cómo [instalar un entorno de ejecución personalizado de R en Windows](../machine-learning/install/custom-runtime-r.md?view=sql-server-ver15&preserve-view=true) o [instalar un entorno de ejecución personalizado de R en Linux](../machine-learning/install/custom-runtime-r.md?view=sql-server-linux-ver15&preserve-view=true).

### <a name="new-java-language-extension"></a>Nueva extensión de lenguaje Java

- La instancia predeterminada de Java Runtime en Windows y Linux es Open Zulu JRE y se incluye con la instalación de [Extensiones de lenguaje de SQL Server en Windows](install/windows-java.md) y la instalación de [Extensiones de lenguaje de SQL Server en Linux](../linux/sql-server-linux-setup-language-extensions-java.md).
- [Tipos de datos de Java](how-to/java-to-sql-data-types.md) admitidos.
- [CREATE EXTERNAL LANGUAGE](../t-sql/statements/create-external-language-transact-sql.md) para registrar un lenguaje externo (por ejemplo, Java) en SQL Server.
- [SDK de extensibilidad de Microsoft para Java](how-to/extensibility-sdk-java-sql-server.md).
- En Windows y Linux, se puede acceder al código de Java en una biblioteca externa mediante la instrucción [CREATE EXTERNAL LIBRARY (Transact-SQL)](../t-sql/statements/create-external-library-transact-sql.md). Más información: [Cómo llamar a Java desde SQL Server](how-to/call-java-from-sql.md).
- [Extensión del lenguaje Java](language-extensions-overview.md) en Windows y Linux. Puede poner código Java compilado a disposición de SQL Server asignando permisos y estableciendo la ruta de acceso. Las aplicaciones cliente con acceso a SQL Server pueden usar datos y ejecutar el código llamando a [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), el mismo procedimiento utilizado para la integración de R y Python en SQL Server Machine Learning Services.

## <a name="next-steps"></a>Pasos siguientes

+ Instale [extensiones de lenguaje de SQL Server en Windows](install/windows-java.md) o [en Linux](../linux/sql-server-linux-setup-language-extensions-java.md).
