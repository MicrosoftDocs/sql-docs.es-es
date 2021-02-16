---
title: Bibliotecas de conectividad y marcos
description: En este artículo se enumeran los controladores de conectividad que las aplicaciones cliente pueden usar desde distintos lenguajes para conectarse a la instancia de Microsoft SQL Server que se ejecuta de forma local o en la nube, en Linux, en Windows o en Docker, y también en Azure SQL Database y Azure Synapse Analytics.
author: VanMSFT
ms.author: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 80efe5ff-09ba-48a0-ac93-a91d62cff47c
ms.openlocfilehash: bd3e6740bf460eda42a1448bcbe9745a41e1a387
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273126"
---
# <a name="connectivity-libraries-and-frameworks-for-microsoft-sql-server"></a>Bibliotecas de conectividad y marcos para Microsoft SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Consulte los [tutoriales de introducción](https://aka.ms/sqldev) para empezar a trabajar rápidamente con lenguajes de programación como C#, Java, Node.js, PHP y Python y compilar una aplicación con SQL Server en Linux o Windows, o bien en Docker en macOS.

En la tabla siguiente se enumeran las bibliotecas o *controladores* de conectividad que las aplicaciones cliente pueden usar desde distintos lenguajes para la conexión y el uso de la instancia de Microsoft SQL Server que se ejecuta de forma local o en la nube, en Linux, en Windows o en Docker, y también en Azure SQL Database y Azure Synapse Analytics. 

| Idioma | Plataforma | Recursos adicionales | Descargar | Introducción |
| :-- | :-- | :-- | :-- | :-- |
| C# | Windows, Linux, macOS | [Microsoft ADO.NET para SQL Server](../connect/ado-net/microsoft-ado-net-sql-server.md) | [Descargar](https://msdn.microsoft.com/vstudio/aa496123.aspx) | [Primeros pasos](https://www.microsoft.com/sql-server/developer-get-started/csharp/ubuntu)
| Java | Windows, Linux, macOS | [Controlador JDBC de Microsoft para SQL Server](../connect/jdbc/microsoft-jdbc-driver-for-sql-server.md) | [Descargar](https://go.microsoft.com/fwlink/?LinkId=245496) |  [Primeros pasos](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu)
| PHP | Windows, Linux, macOS| [Controlador de SQL para PHP para SQL Server](../connect/php/microsoft-php-driver-for-sql-server.md) | Sistema operativo: <br/> \* [Windows](https://www.microsoft.com/download/details.aspx?id=20098) <br/> \* [Linux](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) <br/> \* [macOS](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) |  [Primeros pasos](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu)
| Node.js | Windows, Linux, macOS | [Controlador de Node.js para SQL Server](../connect/node-js/node-js-driver-for-sql-server.md) |  [Primeros pasos](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu)
| Python | Windows, Linux, macOS | [Controlador de SQL para Python](../connect/python/python-driver-for-sql-server.md) <br/> \* [pyodbc](../connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development.md) |  [Primeros pasos](https://www.microsoft.com/sql-server/developer-get-started/python/ubuntu)
| Ruby | Windows, Linux, macOS | [Controlador de Ruby para SQL Server](../connect/ruby/ruby-driver-for-sql-server.md) | [Primeros pasos](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu)
| C++ | Windows, Linux, macOS | [Microsoft ODBC Driver for SQL Server](../connect/odbc/microsoft-odbc-driver-for-sql-server.md) | [Descargar](../connect/odbc/microsoft-odbc-driver-for-sql-server.md) |  

En la tabla siguiente se enumeran algunos ejemplos de marcos de Asignación relacional de objetos (ORM) y marcos web que las aplicaciones cliente pueden usar con la instancia de Microsoft SQL Server que se ejecuta de forma local o en la nube, en Linux, en Windows o en Docker, y también en Azure SQL Database y Azure Synapse Analytics. 

| Idioma | Plataforma | ORM |
| :-- | :-- | :-- |
| C# | Windows, Linux, macOS | [Entity Framework](/ef)<br>[Entity Framework Core](/ef/core/index) |
| Java | Windows, Linux, macOS |[Hibernate ORM](https://hibernate.org/orm)|
| PHP | Windows, Linux | [Laravel (Eloquent)](https://laravel.com/docs/5.0/eloquent) |
| Node.js | Windows, Linux, macOS | [Sequelize ORM](http://sequelize.org/) |
| Python | Windows, Linux, macOS |[Django](https://www.djangoproject.com/) |
| Ruby | Windows, Linux, macOS | [Ruby on Rails](https://rubyonrails.org/) |

## <a name="related-links"></a>Vínculos relacionados
- [Controladores de SQL Server](../connect/sql-connection-libraries.md) para conectarse desde aplicaciones cliente