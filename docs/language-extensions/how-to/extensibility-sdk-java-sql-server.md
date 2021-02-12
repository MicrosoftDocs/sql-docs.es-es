---
title: SDK de extensibilidad de Microsoft para Java
description: Vea los procedimientos para implementar un programa de Java para SQL Server mediante el SDK de extensibilidad de Microsoft para Java.
ms.prod: sql
ms.technology: language-extensions
ms.date: 11/05/2019
ms.topic: conceptual
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 153ea1440b3a56eabd98533600de1620c9d6cb93
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072649"
---
# <a name="microsoft-extensibility-sdk-for-java-for-sql-server"></a>SDK de extensibilidad de Microsoft para Java para SQL Server
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Vea los procedimientos para implementar un programa de Java para SQL Server mediante el SDK de extensibilidad de Microsoft para Java. El SDK es una interfaz para la extensión de lenguaje de Java que sirve para intercambiar datos con SQL Server y para ejecutar código Java desde SQL Server.

El SDK se instala como parte de SQL Server 2019, versión candidata para lanzamiento 1, en Windows y Linux:

+ Ruta de instalación predeterminada en Windows: **[directorio principal de instalación de instancia]\MSSQL\Binn\mssql-java-lang-extension.jar**
+ Ruta de instalación predeterminada en Linux: **/opt/mssql/lib/mssql-java-lang-extension.jar**

El código es abierto y puede encontrarse en el [repositorio de GitHub sobre extensiones de lenguaje de SQL Server](https://github.com/microsoft/sql-server-language-extensions).

## <a name="implementation-requirements"></a>Requisitos de implementación

La interfaz del SDK define un conjunto de requisitos que se deben cumplir para que SQL Server pueda comunicarse con el tiempo de ejecución de Java. Para usar el SDK, debe seguir algunas reglas de implementación en la clase principal. A continuación, SQL Server puede ejecutar un método específico en la clase Java e intercambiar datos mediante la extensión del lenguaje Java.

Para obtener un ejemplo de cómo puede usar el SDK, consulte [Tutorial: Búsqueda de una cadena mediante expresiones regulares (regex) en Java](../tutorials/search-for-string-using-regular-expressions-in-java.md).

## <a name="sdk-classes"></a>Clases de SDK

El SDK consta de tres clases.

Dos clases abstractas que definen la interfaz que usa la extensión de Java para intercambiar datos con SQL Server:

- **AbstractSqlServerExtensionExecutor**
- **AbstractSqlServerExtensionDataset**

La tercera clase es una clase auxiliar, que contiene una implementación de un objeto de conjunto de datos. Se trata de una clase opcional que puede aprovechar, lo que facilita la introducción. También puede usar su propia implementación de esta clase.

- **PrimitiveDataset**

A continuación encontrará descripciones de cada clase en el SDK. El código fuente de las clases del SDK está disponible en el [repositorio de GitHub sobre extensiones de lenguaje de SQL Server](https://github.com/microsoft/sql-server-language-extensions/tree/master/language-extensions/java/sdk).

### <a name="class-abstractsqlserverextensionexecutor"></a>Clase: AbstractSqlServerExtensionExecutor

La clase abstracta **AbstractSqlServerExtensionExecutor** contiene la interfaz que se usa para ejecutar código de Java mediante la extensión del lenguaje Java para SQL Server.

La clase Java principal debe heredar de esta clase. Por heredar de esta clase se entiende que hay ciertos métodos en la clase que debe implementar en su propia clase.

Para heredar de esta clase abstracta, se debe extender con el nombre de clase abstracta en la declaración de la clase:

```java
public class <MyClass> extends AbstractSqlServerExtensionExecutor {}
```

Como mínimo, la clase principal debe implementar el método execute(...).

#### <a name="method-execute"></a>Método execute

El método execute es el método al que se llama desde SQL Server a través de la extensión del lenguaje Java para invocar código de Java desde SQL Server. Se trata de un método clave en el que se incluyen las operaciones principales que desea ejecutar desde SQL Server.

Para pasar argumentos de método a Java desde SQL Server, use el parámetro `@param` en `sp_execute_external_script`. El método **execute** toma sus argumentos de ese modo.

```java
public AbstractSqlServerExtensionDataset execute(AbstractSqlServerExtensionDataset input, LinkedHashMap<String, Object> params)  {}
```

#### <a name="method-init"></a>Método init

El método init se ejecuta después del constructor y antes del método execute. Las operaciones que se deban llevar a cabo antes de execute(...) se pueden efectuar en este método.

```java
public void init(String sessionId, int taskId, int numtask) {}
```

### <a name="class-abstractsqlserverextensiondataset"></a>Clase: AbstractSqlServerExtensionDataset

La clase abstracta **AbstractSqlServerExtensionDataset** contiene la interfaz para controlar los datos de entrada y salida que usa la extensión de Java.


### <a name="class-primitivedataset"></a>Clase: PrimitiveDataset

La clase **PrimitiveDataset** es una implementación de **AbstractSqlServerExtensionDataset** que almacena tipos simples como matrices primitivas.

Se proporciona en el SDK simplemente como una clase auxiliar opcional. Si no usa esta clase, deberá implementar su propia clase que herede de **AbstractSqlServerExtensionDataset**.  

## <a name="next-steps"></a>Pasos siguientes

+ [Tutorial: Búsqueda de una cadena mediante expresiones regulares (regex) en Java](../tutorials/search-for-string-using-regular-expressions-in-java.md)
+ [Cómo llamar a Java en SQL Server](call-java-from-sql.md)
