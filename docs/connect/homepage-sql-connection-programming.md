---
title: Página principal de la programación de clientes de SQL | Microsoft Docs
description: Página con vínculos anotados a descargas y documentación para distintos lenguajes y sistemas operativos, para conectarse a SQL Server o a una instancia de Azure SQL Database.
author: David-Engel
ms.date: 11/07/2018
ms.prod: sql
ms.prod_service: connectivity
ms.custom: ''
ms.technology: connectivity
ms.topic: conceptual
ms.reviewer: v-daveng
ms.author: v-daenge
ms.openlocfilehash: 10d04306621722abd9c9a170028ba632a8d95c9e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045505"
---
# <a name="homepage-for-client-programming-to-microsoft-sql-server"></a>Página principal de la programación de clientes de Microsoft SQL Server


Esta es la página principal de la programación de clientes para interactuar con Microsoft SQL Server y con Azure SQL Database en la nube. Este artículo ofrece la siguiente información:

- Enumera y describe las combinaciones disponibles de idiomas y controladores.
  - Se proporciona información para los sistemas operativos Linux (Ubuntu y otros), macOS y Windows.
- Proporciona vínculos a la documentación detallada para cada combinación.
- Muestra las áreas y subáreas de la documentación jerárquica para determinados lenguajes, si procede.


#### <a name="azure-sql-database"></a>Azure SQL Database

En cualquier lenguaje determinado, el código para conectarse a SQL Server es prácticamente idéntico al código para conectarse a Azure SQL Database.

Para obtener más información acerca de las cadenas de conexión para conectarse a Azure SQL Database, consulte:

- [Uso de .NET Core (C#) para consultar una base de datos de Azure SQL](/azure/sql-database/sql-database-connect-query-dotnet-core)
- Otros artículos de Azure SQL Database que están cerca del artículo anterior en la tabla de contenido, sobre otros lenguajes. Por ejemplo, vea [Uso de PHP para consultar una base de datos de Azure SQL Database](/azure/sql-database/sql-database-connect-query-php).


#### <a name="build-an-app-webpages"></a>Páginas web de compilación de una aplicación

En las páginas web de *compilación de una aplicación* aparecen ejemplos de código, junto con información de configuración, en un formato alternativo. Para obtener más información, consulte más adelante en este artículo la [sección *Sitio web de compilación de una aplicación*](#an-204-aka-ms-sqldev).



<a name="an-050-languages-clients" />

## <a name="languages-and-drivers-for-client-programs"></a>Lenguajes y controladores para programas cliente


En la tabla siguiente, cada imagen de lenguaje es un vínculo a los detalles sobre cómo usar el lenguaje con SQL Server. Cada vínculo salta a una sección posterior de este artículo.

:::row:::
    :::column:::
        [![Logotipo de C#][image-ref-320-csharp]](#an-110-ado-net-docu)  

        [![Logotipo de Node.js][image-ref-340-node]](#an-140-node-js-docu)  

        [![Logotipo de Python][image-ref-370-python]](#an-180-python-docu)  
    :::column-end:::
    :::column:::
        [![ORM Entity Framework, de .NET Framework][image-ref-333-ef]](#an-116-csharp-ef-orm)  

        [**`ODBC for C++`**](#an-160-odbc-cpp-docu)<br/>[![cpp-big-plus][image-ref-322-cpp]](#an-160-odbc-cpp-docu)  

        [![Logotipo de Ruby][image-ref-380-ruby]](#an-190-ruby-docu)  
    :::column-end:::
    :::column:::
        [![Logotipo de Java][image-ref-330-java]](#an-130-jdbc-docu)  

        [![Logotipo de PHP][image-ref-360-php]](#an-170-php-docu)  
    :::column-end:::
:::row-end:::

#### <a name="downloads-and-installs"></a>Descargas e instalaciones

El siguiente artículo se dedica a la descarga e instalación de varios controladores de conexión de SQL, para su uso en los lenguajes de programación:

- [Controladores de SQL Server](./sql-connection-libraries.md)



<a name="an-110-ado-net-docu" />

## <a name="c-logoimage-ref-320-csharp-c-using-adonet"></a>![Logotipo de C#][image-ref-320-csharp] C# con ADO.NET

Los lenguajes administrados de .NET, como C# y Visual Basic, son los usuarios más comunes de ADO.NET. *ADO.NET* es un nombre casual para un subconjunto de clases de .NET Framework.

#### <a name="code-examples"></a>Ejemplos de código

| Ejemplo | Descripción |
| :-- | :-- |
| [Prueba de concepto de la conexión a SQL mediante ADO.NET](./ado-net/step-3-connect-sql-ado-net.md) | Un pequeño ejemplo de código centrado en la conexión y consulta de SQL Server. |
| [Conectar de forma resistente a SQL con ADO.NET](./ado-net/step-4-connect-resiliently-sql-ado-net.md) | Lógica de reintento en un ejemplo de código, ya que en ocasiones las conexiones pueden experimentar momentos de pérdida de conectividad.<br /><br />La lógica de reintento se aplica bien a las conexiones que se mantienen a través de Internet a cualquier base de datos en la nube, como Azure SQL Database. |
| [Azure SQL Database: Demostración de cómo usar .NET Core en Windows, Linux y macOS para crear un programa de C#, para conectarse y consultar](/azure/sql-database/sql-database-connect-query-dotnet-core) | Ejemplo de Azure SQL Database. |
| [Compilación de una aplicación: C#, ADO.NET, Windows](https://www.microsoft.com/sql-server/developer-get-started/csharp/win/) | Información de configuración, junto con ejemplos de código. |
| &nbsp; | <br /> |

#### <a name="documentation"></a>Documentación

| Área | Descripción |
| :-- | :-- |
| [C# con ADO.NET](./ado-net/microsoft-ado-net-sql-server.md)| Raíz de la documentación. |
| [Espacio de nombres: System.Data](/dotnet/api/system.data) | Un conjunto de clases usadas para ADO.NET. |
| [Espacio de nombres: Microsoft.Data.SqlClient](/dotnet/api/microsoft.data.SqlClient) | Conjunto de clases que se usa para el proveedor de datos de Microsoft .NET para SQL Server |
| &nbsp; | <br /> |



<a name="an-116-csharp-ef-orm" />

## <a name="entity-framework-logoimage-ref-333-ef-entity-framework-ef-with-cx23"></a>![Logotipo de Entity Framework][image-ref-333-ef] Entity Framework (EF) con C&#x23;

Entity Framework (EF) proporciona asignación relacional de objetos (ORM). ORM facilita a su código fuente de programación orientada a objetos (OOP) la manipulación de los datos que se recuperaron de una base de datos SQL relacional.

EF tiene relaciones directas o indirectas con las siguientes tecnologías:

- .NET Framework
- [LINQ to SQL](/dotnet/framework/data/adonet/sql/linq/) o [LINQ to Entities](/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities)
- Mejoras en la sintaxis del lenguaje, como el operador **=>** en C#.
- Programas útiles que generan código fuente para las clases, que se asignan a las tablas de la base de datos SQL. Por ejemplo, [EdmGen.exe](/dotnet/framework/data/adonet/ef/edm-generator-edmgen-exe).


#### <a name="original-ef-and-new-ef"></a>EF original y EF nuevo

La [página de inicio de Entity Framework](/ef/) presenta EF con una descripción similar a esta:

- Entity Framework es un asignador relacional de objetos (O/RM) que permite a los desarrolladores de .NET trabajar con una base de datos mediante objetos .NET. Elimina la necesidad de usar la mayoría del código fuente de acceso a datos que los programadores suelen tener que escribir.

*Entity Framework* es un nombre compartido por dos ramas de código fuente independientes. Una rama EF es más antigua, y el público ahora puede mantener su código fuente. La otra instancia de EF es nueva. Las dos instancias de EF se describen a continuación:

| Versión | Descripción |
| :-- | :-- |
| [EF 6.x](/ef/ef6/) | Microsoft lanzó por primera vez EF en agosto de 2008. En marzo 2015, Microsoft anunció que EF 6.x era la versión final que desarrollaría Microsoft. Microsoft publicó el código fuente en el dominio público.<br /><br />Inicialmente, EF formaba parte de .NET Framework. Pero EF 6. x se quitó de .NET Framework.<br /><br />[Código fuente de EF 6.x en GitHub, en el repositorio *aspnet/EntityFramework6*](https://github.com/aspnet/EntityFramework6) |
| [EF Core](/ef/core/) | Microsoft lanzó la instancia de EF Core recién desarrollada en junio de 2016. EF Core está diseñado para ofrecer una mayor flexibilidad y portabilidad. EF Core puede ejecutarse en sistemas operativos diferentes de Microsoft Windows. Y EF Core puede interactuar con bases de datos diferentes de Microsoft SQL Server y otras bases de datos relacionales.<br /><br />**Ejemplos de código de C&#x23:**<br />[Introducción a Entity Framework Core](/ef/core/get-started/index)<br />[Introducción a EF Core en .NET Framework con una base de datos existente](/ef/core/get-started/full-dotnet/existing-db) |
| &nbsp; | <br /> |

EF y las tecnologías relacionadas son muy eficaces, y el desarrollador que desee dominar todo el área tiene mucho que aprender.

&nbsp;



<a name="an-130-jdbc-docu" />

## <a name="java-logoimage-ref-330-java-java-and-jdbc"></a>![Logotipo de Java][image-ref-330-java] Java y JDBC

Microsoft proporciona un controlador de Java Database Connectivity (JDBC) para utilizarlo con SQL Server o con Azure SQL Database. Se trata de un controlador JDBC de tipo 4 que proporciona conectividad a bases de datos mediante las interfaces de programación de aplicaciones (API) estándar JDBC.

#### <a name="code-examples"></a>Ejemplos de código

| Ejemplo | Descripción |
| :-- | :-- |
| [Ejemplos de código](./jdbc/sample-jdbc-driver-applications.md) | Ejemplos de código que enseñan sobre los tipos de datos, los conjuntos de resultados y los datos de gran tamaño. |
| [Ejemplo de URL de conexión](./jdbc/connection-url-sample.md) | Describe cómo usar una dirección URL de conexión para conectarse a SQL Server. A continuación, úselo para emplear una instrucción SQL para recuperar datos. |
| [Ejemplo de origen de datos](./jdbc/data-source-sample.md) | Describe cómo utilizar un origen de datos para conectarse a SQL Server. A continuación, use un procedimiento almacenado para recuperar los datos. |
| [Uso de Java para consultar una instancia de Azure SQL Database](/azure/sql-database/sql-database-connect-query-java) | Ejemplo de Azure SQL Database. |
| [Creación de aplicaciones de Java con SQL Server en Ubuntu](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/) | Información de configuración, junto con ejemplos de código. |
| &nbsp; | <br /> |

#### <a name="documentation"></a>Documentación

La documentación de JDBC incluye las siguientes áreas principales:

| Área | Descripción |
| :-- | :-- |
| [Java Database Connectivity (JDBC)](./jdbc/microsoft-jdbc-driver-for-sql-server.md) | Raíz de nuestra documentación de JDBC. |
| [Referencia](./jdbc/reference/jdbc-driver-api-reference.md) | Interfaces, clases y miembros. |
| [Guía de programación del controlador JDBC para SQL](./jdbc/programming-guide-for-jdbc-sql-driver.md) | Información de configuración, junto con ejemplos de código. |
| &nbsp; | <br /> |



<a name="an-140-node-js-docu" />

## <a name="nodejs-logoimage-ref-340-node-nodejs"></a>![Logotipo de Node.js][image-ref-340-node] Node.js

Con Node.js puede conectarse a SQL Server desde Windows, Linux o macOS. La raíz de nuestra documentación de node.js se encuentra [aquí](./node-js/node-js-driver-for-sql-server.md).

El controlador de conexión de Node.js para SQL Server se implementa en JavaScript. El controlador usa el protocolo TDS, que es compatible con todas las versiones actuales de SQL Server. El controlador es un proyecto de código abierto [disponible en GitHub](https://tediousjs.github.io/tedious/).

#### <a name="code-examples"></a>Ejemplos de código

| Ejemplo | Descripción |
| :-- | :-- |
| [Prueba de concepto de la conexión a SQL mediante Node.js](./node-js/step-3-proof-of-concept-connecting-to-sql-using-node-js.md) | Código fuente básico para conectarse a SQL Server y ejecutar una consulta. |
| [Azure SQL Database: Uso de Node.js para consultar](/azure/sql-database/sql-database-connect-query-nodejs) | Ejemplo de Azure SQL Database en la nube. |
| [Creación de aplicaciones de node. js para usar SQL Server en macOS](https://www.microsoft.com/sql-server/developer-get-started/node/mac/) | Información de configuración, junto con ejemplos de código. |
| &nbsp; | <br /> |



<a name="an-160-odbc-cpp-docu" />

## <a name="odbc-for-c"></a>ODBC para C++

![Logotipo de ODBC][image-ref-350-odbc] ![cpp-big-plus][image-ref-322-cpp]

La conectividad abierta de bases de datos (ODBC) se desarrolló en los años noventa y es anterior a .NET Framework. ODBC está diseñado para ser independiente de cualquier sistema de base de datos determinado y de cualquier sistema operativo.

A lo largo de los años, se han creado y publicado muchos controladores ODBC en grupos dentro y fuera de Microsoft. La gama de controladores implica varios lenguajes de programación de cliente. La lista de destinos de datos va más allá SQL Server.

Otros controladores de conectividad usan ODBC internamente.

#### <a name="code-example"></a>Ejemplo de código

- [Ejemplo de código de C++, con ODBC](../odbc/reference/sample-odbc-program.md)

#### <a name="documentation-outline"></a>Esquema de documentación

El contenido ODBC de esta sección se centra en el acceso a SQL Server o Azure SQL Database desde C++. En la tabla siguiente se muestra un esquema aproximado de la documentación principal de ODBC.


| Área | Subárea | Descripción |
| :--- | :------ | :---------- |
| [ODBC para C++](./odbc/microsoft-odbc-driver-for-sql-server.md) | Raíz de la documentación. |
| [Linux o macOS](./odbc/linux-mac/system-requirements.md) | &nbsp; | Información sobre el uso de ODBC en los sistemas operativos Linux o macOS. |
| [Windows](./odbc/windows/microsoft-odbc-driver-for-sql-server-on-windows.md)     | &nbsp; | Información sobre el uso de ODBC en el sistema operativo Windows. |
| [Administración](../odbc/admin/odbc-data-source-administrator.md) | &nbsp; | Herramienta administrativa para administrar orígenes de datos ODBC. |
| [Microsoft](../odbc/microsoft/microsoft-supplied-odbc-drivers.md)  | &nbsp; | Varios controladores ODBC creados y proporcionados por Microsoft. |
| [Conceptual y referencia](../odbc/reference/introduction-to-odbc.md) | &nbsp; | Información conceptual sobre la interfaz ODBC, además de la referencia tradicional. |
| &nbsp; " | [Apéndices](../odbc/reference/appendixes/odbc-appendixes.md)    | Tablas de transición de estado, biblioteca de cursores ODBC, etc. |
| &nbsp; " | [Desarrollo de aplicaciones](../odbc/reference/develop-app/checking-feature-support-and-variability.md)  | Funciones, identificadores y mucho más. |
| &nbsp; " | [Desarrollo de controladores](../odbc/reference/develop-driver/developing-an-odbc-driver.md) | Cómo desarrollar su propio controlador ODBC, si tiene un origen de datos especializado. |
| &nbsp; " | [Instalación](../odbc/reference/install/odbc-subkey.md) | Instalación de ODBC, subclaves y mucho más. |
| &nbsp; " | [Sintaxis](../odbc/reference/syntax/odbc-reference.md)   | API para la instalación, configuración, traslación y acceso a datos. |
| &nbsp; | &nbsp; | <br /> |



<a name="an-170-php-docu" />

## <a name="php-logoimage-ref-360-php-php"></a>![Logotipo de PHP][image-ref-360-php] PHP

Puede usar PHP para interactuar con SQL Server. La raíz de la documentación de PHP está [aquí](./php/microsoft-php-driver-for-sql-server.md).

#### <a name="code-examples"></a>Ejemplos de código

| Ejemplo | Descripción |
| :-- | :-- |
| [Prueba de concepto de la conexión a SQL mediante PHP](./php/step-3-proof-of-concept-connecting-to-sql-using-php.md) | Un pequeño ejemplo de código centrado en la conexión y consulta de SQL Server. |
| [Paso 4: Conectar de forma resistente a SQL con PHP](./php/step-4-connect-resiliently-to-sql-with-php.md) | Lógica de reintento en un ejemplo de código, ya que en ocasiones las conexiones a través de Internet y la nube pueden experimentar momentos de pérdida de conectividad. |
| [Azure SQL Database: uso de PHP para realizar consultas](/azure/sql-database/sql-database-connect-query-php) | Ejemplo de Azure SQL Database. |
| [Creación de aplicaciones de PHP para usar SQL Server en RHEL](https://www.microsoft.com/sql-server/developer-get-started/php/rhel/) | Información de configuración, junto con ejemplos de código. |
| &nbsp; | <br /> |



<a name="an-180-python-docu" />

## <a name="python-logoimage-ref-370-python-python"></a>![Logotipo de Python][image-ref-370-python] Python


Puede usar Python para interactuar con SQL Server.

#### <a name="code-examples"></a>Ejemplos de código

| Ejemplo | Descripción |
| :-- | :-- |
| [Prueba de concepto de la conexión a SQL con Python mediante pyodbc](./python/pyodbc/step-3-proof-of-concept-connecting-to-sql-using-pyodbc.md) | Un pequeño ejemplo de código centrado en la conexión y consulta de SQL Server. |
| [Azure SQL Database: uso de Python para consultar](/azure/sql-database/sql-database-connect-query-python) | Ejemplo de Azure SQL Database. |
| [Creación de aplicaciones PHP para usar SQL Server en SLES](https://www.microsoft.com/sql-server/developer-get-started/python/sles/) | Información de configuración, junto con ejemplos de código. |
| &nbsp; | <br /> |

#### <a name="documentation"></a>Documentación

| Área | Descripción |
| :--- | :---------- |
| [Python para SQL Server](./python/python-driver-for-sql-server.md) | Raíz de la documentación. |
| [controlador pymssql](./python/pymssql/python-sql-driver-pymssql.md) | Microsoft no mantiene ni prueba el controlador pymssql.<br /><br />El controlador de conexión pymssql es una interfaz sencilla para las bases de datos SQL, para su uso en programas de Python. Pymssql se basa en FreeTDS para proporcionar una interfaz de Python DB-API (PEP-249) para Microsoft SQL Server. |
| [controlador pyodbc](./python/pyodbc/python-sql-driver-pyodbc.md)   | El controlador de conexión pyodbc es un módulo de Python de código abierto que facilita el acceso a las bases de datos ODBC. Implementa la especificación de DB API 2.0, pero está empaquetada con mayor comodidad de Python. |
| &nbsp; | <br /> |


<a name="an-190-ruby-docu" />

## <a name="ruby-logoimage-ref-380-ruby-ruby"></a>![Logotipo de Ruby][image-ref-380-ruby] Ruby

Puede usar Ruby para interactuar con SQL Server. La raíz de nuestra documentación de Ruby está [aquí](./ruby/ruby-driver-for-sql-server.md).

#### <a name="code-examples"></a>Ejemplos de código

| Ejemplo | Descripción |
| :-- | :-- |
| [Prueba de concepto de la conexión a SQL con Ruby](./ruby/step-3-proof-of-concept-connecting-to-sql-using-ruby.md) | Un pequeño ejemplo de código centrado en la conexión y consulta de SQL Server. |
| [Azure SQL Database: Uso de Ruby para consultar](/azure/sql-database/sql-database-connect-query-ruby) | Ejemplo de Azure SQL Database. |
| [Creación de aplicaciones de Ruby para usar SQL Server en macOS](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/) | Información de configuración, junto con ejemplos de código. |
| &nbsp; | <br /> |



<a name="an-204-aka-ms-sqldev" />

## <a name="build-an-app-website-for-sql-client-development"></a>[Sitio web de compilación de una aplicación, para desarrollo de cliente SQL](https://www.microsoft.com/sql-server/developer-get-started/)


En nuestras páginas web de [*compilación de una aplicación*](https://www.microsoft.com/sql-server/developer-get-started/), puede elegir entre una larga lista de lenguajes de programación para conectarse a SQL Server. Y el programa cliente puede ejecutar varios sistemas operativos.

La *compilación de una aplicación* enfatiza la simplicidad y la integridad para el desarrollador que se está iniciando. En los pasos siguientes se explican las tareas siguientes:

1. Cómo instalar Microsoft SQL Server
2. Cómo descargar e instalar herramientas y controladores.
3. Cómo realizar las configuraciones necesarias, según sea necesario para el sistema operativo elegido.
4. Cómo compilar el código fuente proporcionado.
5. Cómo ejecutar el programa.

A continuación se muestran un par de esquemas aproximados de los detalles proporcionados en el sitio web:

#### <a name="java-on-ubuntu"></a>Java en Ubuntu

1. Configurar el entorno
    - Paso 1.1: instalar SQL Server
    - Paso 1.2: instalar Java
    - Paso 1.3: instalar el kit de desarrollo de Java (JDK)
    - Paso 1.4: instalar Maven
2. Creación de una aplicación de Java con SQL Server
    - Paso 2.1: crear una aplicación de Java que se conecta a SQL Server y ejecuta consultas
    - Paso 2.2: crear una aplicación de Java que se conecta a SQL Server con el popular marco Hibernate
3. Haga que su aplicación de Java sea 100 veces más rápida.
    - Paso 3.1: crear una aplicación de Java para mostrar los índices del almacén de columnas

#### <a name="python-on-windows"></a>Python en Windows

1. Configurar el entorno
    - Paso 1.1: instalar SQL Server
    - Paso 1.2: instalar Python
    - Paso 1.3: instalar el controlador ODBC y la utilidad de línea de comandos SQL para SQL Server
2. Creación de una aplicación de Python con SQL Server
    - Paso 2.1: instalar el controlador de Python para SQL Server
    - Paso 2.2: crear una base de datos para la aplicación
    - Paso 2.3: crear una aplicación de Python que se conecta a SQL Server y ejecuta consultas
3. Haga que su aplicación de Python sea 100 veces más rápida.
    - Paso 3.1: crear una nueva tabla con 5 millones mediante sqlcmd
    - Paso 3.2: crear una aplicación de Python que consulta esta tabla y mide el tiempo empleado
    - Paso 3:3: mida cuánto tiempo se tarda en ejecutar la consulta
    - Paso 3.4: agregar un índice de almacén de columnas a la tabla
    - Paso 3.5: mida cuánto tiempo se tarda en ejecutar la consulta con un índice de almacén de columnas

Las siguientes capturas de pantalla le ofrecen una idea del aspecto que tiene nuestro sitio web de documentación de desarrollo de SQL.

#### <a name="choose-a-language"></a>Elegir un idioma

![Sitio web de desarrollo de SQL, introducción][image-ref-390-aka-ms-sqldev-choose-language]

&nbsp;

#### <a name="choose-an-operating-system"></a>Seleccionar un sistema operativo

![Sitio web de desarrollo de SQL, Java Ubuntu][image-ref-400-aka-ms-sqldev-java-ubuntu]

&nbsp;



## <a name="other-development"></a>Otro desarrollo


En esta sección se proporcionan vínculos sobre otras opciones de desarrollo. Por ejemplo, el uso de estos mismos lenguajes para el desarrollo de Azure en general. La información no se centra solo en los destinos de Azure SQL Database y Microsoft SQL Server.

#### <a name="developer-hub-for-azure"></a>Centro para desarrolladores de Azure

- [Centro para desarrolladores de Azure](/azure/)
- [Azure para desarrolladores de .NET](/dotnet/azure/)
- [Azure para desarrolladores de Java](/java/azure/)
- [Azure para desarrolladores de Node.js](/nodejs/azure/)
- [Azure para desarrolladores de Python](/python/azure/)
- [Creación de una aplicación web de PHP en Azure](/azure/app-service-web/app-service-web-get-started-php)

#### <a name="other-languages"></a>Otros idiomas

- [Creación de aplicaciones de Go con SQL Server en Windows](https://www.microsoft.com/sql-server/developer-get-started/go/windows/)



<!-- Image references. -->

[image-ref-322-cpp]: ./media/homepage-sql-connection-drivers/gm-cpp-4point-p61f.png
[image-ref-320-csharp]: ./media/homepage-sql-connection-drivers/gm-csharp-c10c.png
[image-ref-333-ef]: ./media/homepage-sql-connection-drivers/gm-entity-framework-ef20d.png
[image-ref-330-java]: ./media/homepage-sql-connection-drivers/gm-java-j18c.png
[image-ref-340-node]: ./media/homepage-sql-connection-drivers/gm-node-n30.png
[image-ref-350-odbc]: ./media/homepage-sql-connection-drivers/gm-odbc-ic55826-o35.png
[image-ref-360-php]: ./media/homepage-sql-connection-drivers/gm-php-php60.png
[image-ref-370-python]: ./media/homepage-sql-connection-drivers/gm-python-py72.png
[image-ref-380-ruby]: ./media/homepage-sql-connection-drivers/gm-ruby-un-r82.png
[image-ref-390-aka-ms-sqldev-choose-language]: ./media/homepage-sql-connection-drivers/gm-aka-ms-sqldev-choose-language-g21.png
[image-ref-400-aka-ms-sqldev-java-ubuntu]: ./media/homepage-sql-connection-drivers/gm-aka-ms-sqldev-java-ubuntu-c31.png