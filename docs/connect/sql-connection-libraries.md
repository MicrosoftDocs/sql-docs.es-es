---
title: Bibliotecas de conexiones para Microsoft SQL Database | Microsoft Docs
description: Proporciona vínculos de descarga para los módulos que permiten la conexión a Microsoft SQL Server y a Azure SQL Database, desde diversos lenguajes de programación de cliente.
author: David-Engel
ms.prod: sql
ms.technology: connectivity
ms.custom: ''
ms.topic: article
ms.date: 03/06/2020
ms.author: v-daenge
ms.openlocfilehash: b64d5d6d20e7d83385b040a49516cb9728b5f048
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058150"
---
# <a name="connection-modules-for-microsoft-sql-database"></a>Módulos de conexión para las bases de datos SQL de Microsoft

En este artículo se proporcionan vínculos de descarga a módulos de conexión o *controladores* que los programas cliente pueden usar para interactuar con [Microsoft SQL Server](../relational-databases/databases/databases.md) y con su gemelo en la nube, [Azure SQL Database](/azure/sql-database/). Hay controladores disponibles para diversos lenguajes de programación, que se ejecutan en los siguientes sistemas operativos:

- Linux
- macOS
- Windows

**Falta de coincidencia de OOP a relacional:**

*Relacional*: Los programas cliente que se escriben en un lenguaje de programación orientada a objetos (OOP) suelen usar controladores SQL, que devuelven datos consultados en un formato más relacional que orientado a objetos. C# con ADO.NET es un ejemplo. En ocasiones, la falta de coincidencia de formatos relacionales y OOP hace que el código OOP sea más difícil de escribir y comprender.

*ORM*: Otros controladores o marcos devuelven datos consultados en el formato OOP, evitando la falta de coincidencia. Estos controladores funcionan esperando que las clases se hayan definido para que coincidan con las columnas de datos de tablas SQL concretas. A continuación, el controlador realiza la *asignación relacional de objetos* (ORM) para devolver los datos consultados como una instancia de una clase. Entity Framework (EF) de Microsoft para C#, e Hibernate para Java, son dos ejemplos.

En el presente artículo se dedican secciones independientes para estos dos tipos de controladores de conexión.

<a name="anchor-20-drivers-relational-access" />

## <a name="drivers-for-relational-access"></a>Controladores para el acceso relacional

| Idioma | Descargar el controlador de SQL |
| :------- | :---------------------- |
| C# | [ADO.NET](https://www.microsoft.com/net/download/)<br />[Microsoft.Data.SqlClient](https://www.nuget.org/packages/Microsoft.Data.SqlClient/)<br /><br />[.NET Core para: Linux-Ubuntu, macOS, Windows](https://dotnet.microsoft.com/download) |
| C++ | [ODBC](./odbc/download-odbc-driver-for-sql-server.md)<br /><br />[OLE DB](./oledb/download-oledb-driver-for-sql-server.md) |
| Java | [JDBC](./jdbc/download-microsoft-jdbc-driver-for-sql-server.md) |
| Node.js | [Controlador de Node.js, instrucciones de instalación](./node-js/step-1-configure-development-environment-for-node-js-development.md) |
| PHP | [PHP](./php/download-drivers-php-sql-server.md) |
| Python | [pyodbc, instrucciones de instalación](./python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development.md)<br />[Descargar ODBC](./odbc/download-odbc-driver-for-sql-server.md) |
| Ruby | [Controlador de Ruby, instrucciones de instalación](./ruby/step-1-configure-development-environment-for-ruby-development.md)<br />[Página de descarga de Ruby](https://rubyinstaller.org/downloads/) |
| &nbsp; | &nbsp; |

<a name="anchor-40-drivers-orm-access" />

## <a name="drivers-for-orm-access"></a>Controladores para el acceso ORM

En la tabla siguiente se muestran ejemplos de marcos de asignación relacional de objetos (ORM) que las aplicaciones cliente utilizan para conectarse a Microsoft SQL Database.

| Idioma | Descargar controlador de ORM |
| :------- | :------------------ |
| C# | [Entity Framework Core](/ef/core/)<br />[Entity Framework (6.x o posterior)](/ef/) |
| Java | [Hibernate ORM](https://hibernate.org/orm)|
| PHP | [Eloquent ORM, incluido en la instalación de Laravel](https://laravel.com/docs/) |
| Node.js | [Sequelize ORM](https://sequelize.org/) |
| Python | [Django](https://www.djangoproject.com/) |
| Ruby | [Ruby on Rails](https://rubyonrails.org/) |
| &nbsp; | &nbsp; |

<a name="anchor-60-build-an-app-webpages" />

## <a name="build-an-app-webpages"></a>Páginas web de compilación de una aplicación

**[https://aka.ms/sqldev](https://aka.ms/sqldev)** le lleva a un conjunto de páginas web de *compilación de una aplicación*. Las páginas web proporcionan información sobre numerosas combinaciones de lenguaje de programación, sistema operativo y controlador de conexión SQL. Entre la información proporcionada por las páginas web de compilación de una aplicación se encuentran los siguientes elementos:

- Detalles sobre cómo empezar desde el principio para cada combinación de idioma + sistema operativo + controlador.
  - Instrucciones para instalar los controladores de conexión de SQL más recientes.
- Ejemplos de código para cada uno de los siguientes elementos:
  - Ejemplos de código relacional de objetos.
  - Ejemplos de código ORM.
  - Demostraciones de índices de almacén de columnas para un rendimiento mucho más rápido.

**Primera página, de las páginas web de compilación de una aplicación:**  
![Páginas web de compilación de una aplicación, captura de pantalla de la primera página](media/homepage-sql-connection-drivers/gm-aka-ms-sqldev-choose-language-g21.png)

**Menu de Java - Ubuntu, de las páginas web de compilación de una aplicación**  
![Páginas web de compilación de una aplicación, menú Java Ubuntu](media/homepage-sql-connection-drivers/gm-aka-ms-sqldev-java-ubuntu-c31.png)

&nbsp;

## <a name="related-links"></a>Vínculos relacionados

- [Ejemplos de código para conectarse a Azure SQL Database en la nube, con Java y otros lenguajes](/azure/sql-database/sql-database-connect-query-java).

<!--
Image references, **obsolete** markdown syntax alternative:

![Build-an-app webpages, first page screenshot][image-ref-163-buildanapp-webpages-first-page]
![Build-an-app webpages, menu Java Ubuntu][image-ref-167-buildanapp-webpages-menu-java-ubuntu]

[image-ref-163-buildanapp-webpages-first-page]: ./media/homepage-sql-connection-drivers/gm-aka-ms-sqldev-choose-language-g21.png
[image-ref-167-buildanapp-webpages-menu-java-ubuntu]: ./media/homepage-sql-connection-drivers/gm-aka-ms-sqldev-java-ubuntu-c31.png
-->