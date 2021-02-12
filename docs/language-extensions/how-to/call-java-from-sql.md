---
title: Llamar al tiempo de ejecución de Java
titleSuffix: SQL Server Language Extensions
description: Obtenga información sobre cómo llamar clases de Java desde procedimientos almacenados de SQL Server mediante las extensiones de lenguaje de SQL Server.
author: dphansen
ms.author: davidph
ms.date: 06/25/2020
ms.topic: how-to
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 263979480f9cb4d4326560f0bb8bae5157cf777f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072803"
---
# <a name="how-to-call-the-java-runtime-in-sql-server-language-extensions"></a>Procedimiento para llamar al tiempo de ejecución de Java en las extensiones de lenguaje de SQL Server
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Las [extensiones del lenguaje de SQL Server](../language-extensions-overview.md) usan el procedimiento almacenado del sistema [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) como interfaz para llamar al tiempo de ejecución de Java. 

En este artículo de procedimientos se explican los detalles de implementación de las clases y los métodos de Java que se ejecutan en SQL Server.

## <a name="where-to-place-java-classes"></a>Dónde se deben colocar las clases de Java

Hay dos métodos para llamar a las clases de Java en SQL Server:

1. Coloque archivos **.class** o **.jar** en la [ruta de clase de Java](#classpath). 

2. Cargue clases compiladas en un archivo **.jar** y otras dependencias en la base de datos mediante el DDL de [biblioteca externa](#external-library). 

> [!NOTE]
> Como recomendación general, use archivos **.jar** en vez de archivos **.class** individuales. Se trata de una práctica común en Java que facilitará la experiencia global. Consulte también [Cómo crear un archivo jar a partir de archivos de clase](create-a-java-jar-file-from-class-files.md).

<a name="classpath"></a>

## <a name="use-classpath"></a>Usar Classpath

### <a name="basic-principles"></a>Principios básicos

A continuación se muestran algunos principios básicos a la hora de ejecutar Java en SQL Server.

* Debe haber clases de Java personalizadas compiladas en los archivos **.class** o **.jar** de la ruta de clase de Java. El [parámetro CLASSPATH](#set-classpath) proporciona la ruta de acceso a los archivos de Java compilados. 

* El método Java al que se está llamando se debe proporcionar en el parámetro **script** del procedimiento almacenado.

* Si la clase pertenece a un paquete, se debe proporcionar **packageName**.

* **params** sirve para pasar parámetros a una clase de Java. No se admiten las llamadas a un método que requiere argumentos. Por lo tanto, los parámetros son la única forma de pasar valores de argumento al método. 

> [!NOTE]
> Esta nota modifica las operaciones admitidas y no admitidas que son específicas de Java en SQL Server 2019, versión candidata para lanzamiento 1.
> * En el procedimiento almacenado se admiten los parámetros de entrada, pero no se admiten los parámetros de salida.

### <a name="call-java-class"></a>Llamar a una clase de Java

El procedimiento almacenado del sistema [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) es la interfaz que se usa para llamar al tiempo de ejecución de Java. En el ejemplo siguiente se muestra un `sp_execute_external_script` que usa la extensión de Java, así como parámetros para especificar la ruta de acceso, el script y el código personalizado.

> [!NOTE]
> Tenga en cuenta que no es necesario definir el método al que se va a llamar. De forma predeterminada, se llama a un método denominado **execute**. Esto significa que debe seguir el [SDK de extensibilidad para Java en SQL Server](extensibility-sdk-java-sql-server.md) e implementar un método execute en la clase Java.

```sql
DECLARE @param1 int
SET @param1 = 3

EXEC sp_execute_external_script
  @language = N'Java'
, @script = N'<packageName>.<ClassName>'
, @input_data_1 = N'<Input Query>'
, @param1 = @param1
```

<a name="set-classpath"></a>

### <a name="set-classpath"></a>Establecer CLASSPATH

Una vez compilada la clase o clases Java y creado un archivo jar en la ruta de clase de Java, tiene dos opciones para proporcionar la ruta de clase a la extensión Java de SQL Server:

1. Usar bibliotecas externas

    La opción más sencilla consiste en hacer que SQL Server busque automáticamente las clases creando bibliotecas externas y apuntando la biblioteca hacia un archivo jar. [Usar bibliotecas externas para Java](#external-library)

2. Registrar una variable de entorno del sistema

    Puede crear una variable de entorno del sistema y proporcionar las rutas de acceso al archivo jar que contiene las clases. Cree una variable de entorno del sistema denominada **CLASSPATH**.

<a name="external-library"></a>

## <a name="use-external-library"></a>Usar una biblioteca externa

En SQL Server 2019, versión candidata para lanzamiento 1, puede usar bibliotecas externas para el lenguaje Java en Windows y Linux. Puede compilar las clases en un archivo .jar y cargarlo junto con otras dependencias en la base de datos mediante el DDL [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md).

Ejemplo de cómo cargar un archivo .jar con la biblioteca externa:

```sql 
CREATE EXTERNAL LIBRARY myJar
FROM (CONTENT = '<local path to .jar file>') 
WITH (LANGUAGE = 'Java'); 
GO
```

Al crear una biblioteca externa, SQL Server obtendrá acceso automático a las clases de Java y no será necesario establecer ningún permiso especial en la ruta de clase.

Ejemplo de cómo llamar a un método en una clase desde un paquete cargado como biblioteca externa:

```sql
EXEC sp_execute_external_script
  @language = N'Java'
, @script = N'MyPackage.MyCLass'
, @input_data_1 = N'SELECT * FROM MYTABLE'
with result sets ((column1 int))
```

Para obtener más información, vea [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md).

## <a name="loopback-connection-to-sql-server"></a>Conexión de bucle invertido en SQL Server

Use una conexión de bucle invertido para volver a conectar con SQL Server a través de JDBC a fin de leer o escribir datos de Java ejecutado desde `sp_execute_external_script`. Se puede utilizar cuando no sea posible usar los argumentos **InputDataSet** y **OutputDataSet** de `sp_execute_external_script`.
Para realizar una conexión de bucle invertido en Windows use el ejemplo siguiente:

```
jdbc:sqlserver://localhost:1433;databaseName=Adventureworks;integratedSecurity=true;
``` 

Para realizar una conexión de bucle invertido en Linux, el controlador JDBC requiere que se definan tres propiedades de conexión en el certificado siguiente:

[Autenticación de certificado de cliente](https://github.com/microsoft/mssql-jdbc/wiki/Client-Certificate-Authentication-for-Loopback-Scenarios)


## <a name="next-steps"></a>Pasos siguientes

+ [Tutorial: Búsqueda de una cadena mediante expresiones regulares en Java](../tutorials/search-for-string-using-regular-expressions-in-java.md)