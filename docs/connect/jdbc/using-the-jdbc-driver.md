---
title: Empleo del controlador JDBC | Microsoft Docs
description: En esta sección se proporciona una serie de instrucciones rápidas para establecer una conexión sencilla a una base de datos de SQL Server mediante el controlador Microsoft JDBC Driver para SQL Server.
ms.custom: ''
ms.date: 02/26/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 6faaf05b-8b70-4ed2-9b44-eee5897f1cd0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e74135523d2ba4cbde27e588eee999b3450def37
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837523"
---
# <a name="using-the-jdbc-driver"></a>Empleo del controlador JDBC

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

En esta sección se proporciona una serie de instrucciones rápidas para establecer una conexión sencilla a una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)]. Antes de establecer la conexión a una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se debe instalar en el equipo local o en un servidor, y el controlador JDBC se debe instalar en el equipo local.  
  
## <a name="choosing-the-right-jar-file"></a>Elegir el archivo JAR adecuado

El controlador Microsoft JDBC Driver proporciona diferentes archivos JAR que se usarán en función de su configuración preferida de Java Runtime Environment (JRE), del modo siguiente:

El controlador Microsoft JDBC Driver 9.2 para SQL Server proporciona los archivos de biblioteca de clases **mssql-jdbc-9.2.1.jre8.jar**, **mssql-jdbc-9.2.1.jre11.jar** y **mssql-jdbc-9.2.1.jre15.jar**.

El controlador Microsoft JDBC Driver 8.4 para SQL Server proporciona los archivos de biblioteca de clases **mssql-jdbc-8.4.1.jre8.jar**, **mssql-jdbc-8.4.1.jre11.jar** y **mssql-jdbc-8.4.1.jre14.jar**.

El controlador Microsoft JDBC Driver 8.2 para SQL Server proporciona los archivos de biblioteca de clases **mssql-jdbc-8.2.2.jre8.jar**, **mssql-jdbc-8.2.2.jre11.jar** y **mssql-jdbc-8.2.2.jre13.jar**.

El controlador Microsoft JDBC Driver 7.4 para SQL Server proporciona los archivos de biblioteca de clases **mssql-jdbc-7.4.1.jre8.jar**, **mssql-jdbc-7.4.1.jre11.jar** y **mssql-jdbc-7.4.1.jre12.jar**.

El controlador Microsoft JDBC Driver 7.2 para SQL Server proporciona los archivos de biblioteca de clases **mssql-jdbc-7.2.2.jre8.jar** y **mssql-jdbc-7.2.2.jre11.jar**.

El controlador Microsoft JDBC Driver 7.0 para SQL Server proporciona los archivos de biblioteca de clases **mssql-jdbc-7.0.0.jre8.jar** y **mssql-jdbc-7.0.0.jre10.jar**.

El controlador Microsoft JDBC Driver 6.4 para SQL Server proporciona los archivos de biblioteca de clases **mssql-jdbc-6.4.0.jre7.jar**, **mssql-jdbc-6.4.0.jre8.jar** y **mssql-jdbc-6.4.0.jre9.jar**.

El controlador Microsoft JDBC Driver 6.2 para SQL Server proporciona los archivos de biblioteca de clases **mssql-jdbc-6.2.2.jre7.jar** y **mssql-jdbc-6.2.2.jre8.jar**.
  
Los controladores Microsoft JDBC Driver 6.0 y 4.2 para SQL Server proporcionan los archivos de biblioteca de clases **sqljdbc41.jar** y **sqljdbc42.jar**.
  
El controlador Microsoft JDBC Driver 4.1 para SQL Server proporciona el archivo de biblioteca de clases **sqljdbc41.jar**.

Su elección también determinará las características disponibles. Para obtener más información sobre el archivo JAR que se debe seleccionar, vea [Requisitos del sistema para el controlador JDBC](../../connect/jdbc/system-requirements-for-the-jdbc-driver.md).  
  
## <a name="setting-the-classpath"></a>Establecer classpath

Los archivos JAR del controlador JDBC Driver de Microsoft no forman parte del SDK de Java y deben incluirse en la ruta de clases de la aplicación de usuario.

Si usa el controlador JDBC Driver 4.1 o 4.2, configure la ruta de clases para incluir el archivo **sqljdbc41.jar** o **sqljdbc42.jar** de la descarga del controlador correspondiente.

Si usa el controlador JDBC Driver 6.2, configure la ruta de clases para incluir **mssql-jdbc-6.2.2.jre7.jar** o **mssql-jdbc-6.2.2.jre8.jar**.

Si usa el controlador JDBC Driver 6.4, configure la ruta de clases para incluir **mssql-jdbc-6.4.0.jre7.jar**, **mssql-jdbc-6.4.0.jre8.jar** o **mssql-jdbc-6.4.0.jre9.jar**.

Si usa el controlador JDBC Driver 7.0, configure la ruta de clases para incluir **mssql-jdbc-7.0.0.jre8.jar** o **mssql-jdbc-7.0.0.jre10.jar**.

Si usa el controlador JDBC Driver 7.2, configure la ruta de clases para incluir **mssql-jdbc-7.2.2.jre8.jar** o **mssql-jdbc-7.2.2.jre11.jar**.

Si usa el controlador JDBC Driver 7.4, establezca la ruta de clases para incluir **mssql-jdbc-7.4.1.jre8.jar**, **mssql-jdbc-7.4.1.jre11.jar** o **mssql-jdbc-7.4.1.jre12.jar**.

Si usa JDBC Driver 8.2, establezca la ruta de clases para incluir **mssql-jdbc-8.2.2.jre8.jar**, **mssql-jdbc-8.2.2.jre11.jar** o **mssql-jdbc-8.2.2.jre13.jar**.

Si usa JDBC Driver 8.4, establezca la ruta de clases para incluir **mssql-jdbc-8.4.1.jre8.jar**, **mssql-jdbc-8.4.1.jre11.jar** o **mssql-jdbc-8.4.1.jre14.jar**.

Si usa el controlador JDBC 9.2, establezca la ruta de clases para incluir **mssql-jdbc-9.2.1.jre8.jar**, **mssql-jdbc-9.2.1.jre11.jar** o **mssql-jdbc-9.2.1.jre15.jar**.

Si falta una entrada en la ruta de clases para el archivo JAR adecuado, una aplicación lanzará la excepción común `Class not found`.  

### <a name="for-microsoft-jdbc-driver-92"></a>Para el controlador Microsoft JDBC Driver 9.2

Los archivos **mssql-jdbc-9.2.1.jre8.jar**, **mssql-jdbc-9.2.1.jre11.jar** o **mssql-jdbc-9.2.1.jre15.jar** se instalan en las ubicaciones siguientes:

```bash
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-9.2.1.jre8.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-9.2.1.jre11.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-9.2.1.jre15.jar
```

El siguiente fragmento de código es un ejemplo de la instrucción CLASSPATH usada para una aplicación Windows:

`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 9.2 for SQL Server\sqljdbc_9.2\enu\mssql-jdbc-9.2.1.jre11.jar`

El siguiente fragmento de código es un ejemplo de la instrucción CLASSPATH usada para una aplicación Unix/Linux:

`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_9.2/enu/mssql-jdbc-9.2.1.jre11.jar`

Asegúrese de que la instrucción CLASSPATH solo contenga un [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], como **mssql-jdbc-9.2.1.jre8.jar**, **mssql-jdbc-9.2.1.jre11.jar** o **mssql-jdbc-9.2.1.jre15.jar**.


### <a name="for-microsoft-jdbc-driver-84"></a>Para el controlador Microsoft JDBC Driver 8.4

Los archivos **mssql-jdbc-8.4.1.jre8.jar**, **mssql-jdbc-8.4.1.jre11.jar** o **mssql-jdbc-8.4.1.jre14.jar** se instalan en las ubicaciones siguientes:

```bash
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-8.4.1.jre8.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-8.4.1.jre11.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-8.4.1.jre14.jar
```

El siguiente fragmento de código es un ejemplo de la instrucción CLASSPATH usada para una aplicación Windows:

`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 8.4 for SQL Server\sqljdbc_8.4\enu\mssql-jdbc-8.4.1.jre11.jar`

El siguiente fragmento de código es un ejemplo de la instrucción CLASSPATH usada para una aplicación Unix/Linux:

`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_8.4/enu/mssql-jdbc-8.4.1.jre11.jar`

Asegúrese de que la instrucción CLASSPATH solo contenga un [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], como **mssql-jdbc-8.4.1.jre8.jar**, **mssql-jdbc-8.4.1.jre11.jar** o **mssql-jdbc-8.4.1.jre14.jar**.


### <a name="for-microsoft-jdbc-driver-82"></a>Para el controlador Microsoft JDBC Driver 8.2

Los archivos **mssql-jdbc-8.2.2.jre8.jar**, **mssql-jdbc-8.2.2.jre11.jar** o **mssql-jdbc-8.2.2.jre13.jar** se instalan en las ubicaciones siguientes:

```bash
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-8.2.2.jre8.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-8.2.2.jre11.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-8.2.2.jre13.jar
```

El siguiente fragmento de código es un ejemplo de la instrucción CLASSPATH usada para una aplicación Windows:

`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 8.2 for SQL Server\sqljdbc_8.2\enu\mssql-jdbc-8.2.2.jre11.jar`

El siguiente fragmento de código es un ejemplo de la instrucción CLASSPATH usada para una aplicación Unix/Linux:

`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_8.2/enu/mssql-jdbc-8.2.2.jre11.jar`

Asegúrese de que la instrucción CLASSPATH solo contenga un [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], como **mssql-jdbc-8.2.2.jre8.jar**, **mssql-jdbc-8.2.2.jre11.jar** o **mssql-jdbc-8.2.2.jre13.jar**.

### <a name="for-microsoft-jdbc-driver-74"></a>Para el controlador Microsoft JDBC Driver 7.4

Los archivos **mssql-jdbc-7.4.1.jre8.jar**, **mssql-jdbc-7.4.1.jre11.jar** o **mssql-jdbc-7.4.1.jre12.jar** se instalan en las siguientes ubicaciones:

```bash
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-7.4.1.jre8.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-7.4.1.jre11.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-7.4.1.jre12.jar
```

El siguiente fragmento de código es un ejemplo de la instrucción CLASSPATH usada para una aplicación Windows:

`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 7.4 for SQL Server\sqljdbc_7.4\enu\mssql-jdbc-7.4.1.jre11.jar`

El siguiente fragmento de código es un ejemplo de la instrucción CLASSPATH usada para una aplicación Unix/Linux:

`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_7.4/enu/mssql-jdbc-7.4.1.jre11.jar`

Asegúrese de que la instrucción CLASSPATH solo contenga un [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], como **mssql-jdbc-7.4.1.jre8.jar**, **mssql-jdbc-7.4.1.jre11.jar** o **mssql-jdbc-7.4.1.jre12.jar**.

### <a name="for-microsoft-jdbc-driver-72"></a>Para el controlador Microsoft JDBC Driver 7.2

Los archivos **mssql-jdbc-7.2.2.jre8.jar** o **mssql-jdbc-7.2.2.jre11.jar** se instalan en las siguientes ubicaciones:

```bash
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-7.2.2.jre8.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-7.2.2.jre11.jar
```

El siguiente fragmento de código es un ejemplo de la instrucción CLASSPATH usada para una aplicación Windows:

`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 7.2 for SQL Server\sqljdbc_7.2\enu\mssql-jdbc-7.2.2.jre11.jar`

El siguiente fragmento de código es un ejemplo de la instrucción CLASSPATH usada para una aplicación Unix/Linux:

`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_7.2/enu/mssql-jdbc-7.2.2.jre11.jar`

Asegúrese de que la instrucción CLASSPATH solo contenga solo un [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], como **mssql-jdbc-7.2.2.jre8.jar** o **mssql-jdbc-7.2.2.jre11.jar**.
  
### <a name="for-microsoft-jdbc-driver-70"></a>Para el controlador Microsoft JDBC Driver 7.0

Los archivos **mssql-jdbc-7.0.0.jre8.jar** o **mssql-jdbc-7.0.0.jre10.jar** se instalan en las siguientes ubicaciones:

```bash
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-7.0.0.jre8.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-7.0.0.jre10.jar
```

El siguiente fragmento de código es un ejemplo de la instrucción CLASSPATH usada para una aplicación Windows:  

`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 7.0 for SQL Server\sqljdbc_7.0\enu\mssql-jdbc-7.0.0.jre10.jar`  
  
El siguiente fragmento de código es un ejemplo de la instrucción CLASSPATH usada para una aplicación Unix/Linux:  
  
`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_7.0/enu/mssql-jdbc-7.0.0.jre10.jar`  
  
Asegúrese de que la instrucción CLASSPATH solo contenga un [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], como **mssql-jdbc-7.0.0.jre8.jar** o **mssql-jdbc-7.0.0.jre10.jar**.

### <a name="for-microsoft-jdbc-driver-64"></a>Para el controlador Microsoft JDBC Driver 6.4

Los archivos **mssql-jdbc-6.4.0.jre7.jar**, **mssql-jdbc-6.4.0.jre8.jar o **mssql-jdbc-6.4.0.jre9.jar** se instalan en las siguientes ubicaciones:  

```bash  
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-6.4.0.jre7.jar
  
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-6.4.0.jre8.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-6.4.0.jre9.jar
```

El siguiente fragmento de código es un ejemplo de la instrucción CLASSPATH usada para una aplicación Windows:  
  
`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 6.4 for SQL Server\sqljdbc_6.4\enu\mssql-jdbc-6.4.0.jre9.jar`  
  
El siguiente fragmento de código es un ejemplo de la instrucción CLASSPATH usada para una aplicación Unix/Linux:  
  
`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_6.4/enu/mssql-jdbc-6.4.0.jre9.jar`  
  
Asegúrese de que la instrucción CLASSPATH solo contenga un [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], como **mssql-jdbc-6.4.0.jre7.jar**, **mssql-jdbc-6.4.0.jre8.jar o **mssql-jdbc-6.4.0.jre9.jar**.

### <a name="for-microsoft-jdbc-driver-62"></a>Para el controlador Microsoft JDBC Driver 6.2

Los archivos **mssql-jdbc-6.2.2.jre7.jar** o **mssql-jdbc-6.2.2.jre8.jar** se instalan en las siguientes ubicaciones:

```bash
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-6.2.2.jre7.jar
  
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-6.2.2.jre8.jar
```

El siguiente fragmento de código es un ejemplo de la instrucción CLASSPATH usada para una aplicación Windows:  
  
`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 6.2 for SQL Server\sqljdbc_6.2\enu\mssql-jdbc-6.2.2.jre8.jar`  
  
El siguiente fragmento de código es un ejemplo de la instrucción CLASSPATH usada para una aplicación Unix/Linux:  
  
`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_6.2/enu/mssql-jdbc-6.2.2.jre8.jar`  
  
Asegúrese de que la instrucción CLASSPATH solo contenga un [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], como mssql-jdbc-6.2.2.jre7.jar o mssql-jdbc-6.2.2.jre8.jar.  

### <a name="for-microsoft-jdbc-driver-41-42-and-60"></a>Para el controlador Microsoft JDBC Driver 4.1, 4.2 y 6.0

El archivo sqljdbc.jar, sqljdbc4.jar, sqljdbc41.jar o sqljdbc42.jar se instalan en la siguiente ubicación:  

```bash
\<installation directory>\sqljdbc_<version>\<language>\sqljdbc.jar  
  
\<installation directory>\sqljdbc_<version>\<language>\sqljdbc4.jar  
  
\<installation directory>\sqljdbc_<version>\<language>\sqljdbc41.jar  
  
\<installation directory>\sqljdbc_<version>\<language>\sqljdbc42.jar  
```

El siguiente fragmento de código es un ejemplo de la instrucción CLASSPATH usada para una aplicación Windows:  
  
`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 6.0 for SQL Server\sqljdbc_4.2\enu\sqljdbc42.jar`  
  
El siguiente fragmento de código es un ejemplo de la instrucción CLASSPATH usada para una aplicación Unix/Linux:  

`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_4.2/enu/sqljdbc42.jar`  
  
Asegúrese de que la instrucción CLASSPATH contenga solamente un elemento [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], como por ejemplo, sqljdbc.jar, sqljdbc4.jar, sqljdbc41.jar o sqljdbc42.jar.  
  
> [!NOTE]  
> En los sistemas de Windows, los nombres de directorio superiores a 8.3 o los nombres de carpetas con espacios pueden causar problemas con las rutas de clase. Si sospecha estos tipos de problemas, debería mover temporalmente el archivo sqljdbc.jar, sqljdbc4.jar o sqljdbc41.jar a un directorio de nombre simple, tal como `C:\Temp`, cambiar la ruta de clases y determinar si el problema se resuelve.  
  
### <a name="applications-that-are-run-directly-at-the-command-prompt"></a>Aplicaciones que se ejecutan directamente en el símbolo del sistema

La ruta de clase se configura en el sistema operativo. Anexe sqljdbc.jar, sqljdbc4.jar o sqljdbc41.jar a la ruta de clase del sistema. Además, puede especificar la ruta de clase en la línea de comandos de Java que ejecuta la aplicación con la opción `java -classpath`.  
  
### <a name="applications-that-run-in-an-ide"></a>Aplicaciones que se ejecutan en un IDE  

Cada proveedor de IDE ofrece un método distinto para establecer la ruta de clase en el IDE. Establecer sin más la ruta de clase en el sistema operativo no funciona. Debe agregar sqljdbc.jar, sqljdbc4.jar o sqljdbc41.jar a la ruta de clase IDE.  
  
### <a name="servlets-and-jsps"></a>Servlets y JSP  

Los servlets y JSP se ejecutan en un motor de servlet/JSP, como Tomcat. La ruta de clase se debe establecer de acuerdo con la documentación del motor de servlet/JSP. Establecer sin más la ruta de clase en el sistema operativo no funciona. Algunos motores de servlet/JSP incluyen pantallas de configuración que puede utilizar para establecer la ruta de clase del motor. En este caso, debe adjuntar el archivo JAR del controlador JDBC correcto a la ruta de clase del motor y reiniciar el motor. En otras situaciones, puede configurar el controlador copiando sqljdbc.jar, sqljdbc4.jar o sqljdbc41.jar en un directorio específico, como lib, durante la instalación del motor. La ruta de clase del controlador del motor también se puede especificar en un archivo de configuración específico del motor.  
  
### <a name="enterprise-java-beans"></a>Enterprise Java Beans  

Enterprise Java Beans (EJB) se ejecuta en un contenedor EJB. Los contenedores EJB son distribuidos por varios proveedores. Los applets Java se ejecutan en un explorador, pero se descargan desde un servidor web. Copie sqljdbc.jar, sqljdbc4.jar o sqljdbc41.jar en la raíz del servidor web y especifique el nombre del archivo JAR en la pestaña del archivo HTML del applet, por ejemplo, `<applet ... archive=mssql-jdbc-***.jar>`.  
  
## <a name="making-a-simple-connection-to-a-database"></a>Establecer una conexión sencilla con una base de datos

Mediante la biblioteca de clases sqljdbc.jar, primero las aplicaciones deben registrar el controlador del modo siguiente:  
  
`Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");`  

Una vez cargado el controlador, puede establecer una conexión con una URL de conexión y el método getConnection de la clase DriverManager:

```java
String connectionUrl = "jdbc:sqlserver://localhost:1433;" +  
   "databaseName=AdventureWorks;user=MyUserName;password=*****;";  
Connection con = DriverManager.getConnection(connectionUrl);  
```

A partir de la API 4.0 de JDBC, el método `DriverManager.getConnection()` se ha mejorado para que los controladores JDBC se carguen automáticamente. Por tanto, no es necesario que las aplicaciones llamen al método `Class.forName` para registrar o cargar el controlador al usar bibliotecas .jar de controladores.  
  
Cuando se llama al método getConnection de la clase DriverManager, se busca un controlador apropiado en el conjunto de controladores JDBC registrados. El archivo sqljdbc4.jar, sqljdbc41.jar o sqljdbc42.jar incluye el archivo "META-INF/services/java.sql.Driver", que contiene como controlador registrado el **com.microsoft.sqlserver.jdbc.SQLServerDriver**. Las aplicaciones existentes, que actualmente cargan los controladores usando el método Class.forName, seguirán trabajando sin modificación.  
  
> [!NOTE]  
> La biblioteca de clases sqljdbc4.jar, sqljdbc41.jar o sqljdbc42.jar no se puede utilizar con versiones anteriores de Java Runtime Environment (JRE). Vea [Requisitos del sistema para el controlador JDBC](../../connect/jdbc/system-requirements-for-the-jdbc-driver.md) para obtener la lista de versiones de JRE compatibles con [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)].  

Para obtener más información sobre cómo conectar con orígenes de datos y usar una dirección URL de conexión, vea [Creación de la dirección URL de conexión](../../connect/jdbc/building-the-connection-url.md) y [Establecimiento de las propiedades de conexión](../../connect/jdbc/setting-the-connection-properties.md).  
  
## <a name="see-also"></a>Consulte también  

[Introducción al controlador JDBC](../../connect/jdbc/overview-of-the-jdbc-driver.md)  
