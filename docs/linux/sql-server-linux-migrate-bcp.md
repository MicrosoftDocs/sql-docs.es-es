---
title: Copia masiva de datos en SQL Server en Linux
description: En este artículo se describe la utilidad bcp. Use bcp para importar grandes cantidades de filas en tablas de SQL Server o para exportar datos de tablas de SQL Server a archivos de datos.
author: VanMSFT
ms.author: vanto
ms.date: 01/30/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 7b93d0d7-7946-4b78-b33a-57d6307cdfa9
ms.openlocfilehash: c27e4095854abd409336c97ac5b8458bf6f62894
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345616"
---
# <a name="bulk-copy-data-with-bcp-to-sql-server-on-linux"></a>Copia masiva de datos con bcp en SQL Server en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En este artículo se muestra cómo usar la utilidad de línea de comandos [bcp](../tools/bcp-utility.md) para realizar una copia masiva datos entre una instancia de SQL Server en Linux y un archivo de datos en un formato especificado por el usuario.

Puede usar `bcp` para importar un número elevado de filas en tablas de SQL Server o para exportar datos de tablas de SQL Server a archivos de datos. Excepto cuando se usa con la opción queryout, `bcp` no requiere ningún conocimiento de Transact-SQL. La utilidad de línea de comandos `bcp` funciona con Microsoft SQL Server en un entorno local o en la nube, en Linux, Windows o Docker y Azure SQL Database y Azure Synapse Analytics.

En este artículo aprenderá a:
- Importación de datos en una tabla mediante el comando `bcp in`
- Exportación de datos de una tabla mediante el comando `bcp out`

## <a name="install-the-sql-server-command-line-tools"></a>Instalación de las herramientas de línea de comandos de SQL Server

`bcp` forma parte de las herramientas de línea de comandos de SQL Server, que no se instalan automáticamente con SQL Server en Linux. Si aún no ha instalado las herramientas de línea de comandos de SQL Server en el equipo Linux, debe instalarlas. Para obtener más información sobre cómo instalar las herramientas, seleccione la distribución de Linux en la siguiente lista:

- [Red Hat Enterprise Linux (RHEL)](sql-server-linux-setup-tools.md#RHEL)
- [Ubuntu](sql-server-linux-setup-tools.md#ubuntu)
- [SUSE Linux Enterprise Server (SLES)](sql-server-linux-setup-tools.md#SLES)

## <a name="import-data-with-bcp"></a>Importación de datos con bcp

En este tutorial se crean una base de datos y una tabla de ejemplo en la instancia local de SQL Server (**localhost**) y, luego, se usa `bcp` para cargar la tabla de ejemplo desde un archivo de texto en el disco.

### <a name="create-a-sample-database-and-table"></a>Creación de una base de datos y una tabla de ejemplo

Comencemos por crear una base de datos de ejemplo con una tabla sencilla que se use durante el resto de este tutorial.

1. En el cuadro Linux, abra un terminal de comandos.

2. Copie y pegue los siguientes comandos en la ventana del terminal. Estos comandos usan la utilidad de línea de comandos **sqlcmd** para crear una base de datos de ejemplo (**BcpSampleDB**) y una tabla (**TestEmployees**) en la instancia local de SQL Server (**localhost**). Recuerde reemplazar `username` y `<your_password>` según sea necesario antes de ejecutar los comandos.

Cree la base de datos **BcpSampleDB**:
```bash 
sqlcmd -S localhost -U sa -P <your_password> -Q "CREATE DATABASE BcpSampleDB;"
```
Cree la tabla **TestEmployees** en la base de datos **BcpSampleDB**:
```bash 
sqlcmd -S localhost -U sa -P <your_password> -d BcpSampleDB -Q "CREATE TABLE TestEmployees (Id INT IDENTITY(1,1) NOT NULL PRIMARY KEY, Name NVARCHAR(50), Location NVARCHAR(50));"
```
### <a name="create-the-source-data-file"></a>Creación del archivo de datos de origen
Copie y pegue el siguiente comando en la ventana del terminal. Se usa el comando `cat` integrado para crear un archivo de datos de texto de ejemplo con tres registros. Guarde el archivo en el directorio particular como **~/test_data.txt**. Los campos de los registros se delimitan mediante una coma.

```bash
cat > ~/test_data.txt << EOF
1,Jared,Australia
2,Nikita,India
3,Tom,Germany
EOF
```

Para comprobar que el archivo de datos se ha creado correctamente, ejecute el siguiente comando en la ventana del terminal:
```bash 
cat ~/test_data.txt
```

Esto debería mostrar lo siguiente en la ventana del terminal:
```bash
1,Jared,Australia
2,Nikita,India
3,Tom,Germany
```

### <a name="import-data-from-the-source-data-file"></a>Importación de datos desde el archivo de datos de origen
Copie y pegue los siguientes comandos en la ventana del terminal. Este comando usa `bcp` para conectarse a la instancia local de SQL Server (**localhost**) e importar los datos del archivo de datos ( **~/test_data.txt**) en la tabla (**TestEmployees**) de la base de datos (**BcpSampleDB**). Recuerde reemplazar el nombre de usuario y `<your_password>` según sea necesario antes de ejecutar los comandos.

```bash 
bcp TestEmployees in ~/test_data.txt -S localhost -U sa -P <your_password> -d BcpSampleDB -c -t  ','
```

Esta es una breve introducción a los parámetros de línea de comandos que se han usado con `bcp` en este ejemplo:
- `-S`: especifica la instancia de SQL Server a la que hay que conectarse.
- `-U`: especifica el identificador de inicio de sesión para conectar con SQL Server.
- `-P`: especifica la contraseña del identificador de inicio de sesión.
- `-d`: especifica la base de datos a la que conectarse.
- `-c`: realiza operaciones con un tipo de datos de caracteres.
- `-t`: especifica el terminador de campo. Se usa `comma` como terminador de campo para los registros del archivo de datos

> [!NOTE]
> En este ejemplo no se especifica un terminador de fila personalizado. Las filas del archivo de datos de texto se han finalizado correctamente con `newline` al usar antes el comando `cat` para crear el archivo de datos.

Para comprobar que los datos se han importado correctamente, ejecute el siguiente comando en la ventana del terminal. Recuerde reemplazar `username` y `<your_password>` según sea necesario antes de ejecutar el comando.
```bash 
sqlcmd -S localhost -d BcpSampleDB -U sa -P <your_password> -I -Q "SELECT * FROM TestEmployees;"
```

Esto debería mostrar los siguientes resultados:
```bash
Id          Name                Location
----------- ------------------- -------------------
          1 Jared               Australia
          2 Nikita              India
          3 Tom                 Germany

(3 rows affected)
```

## <a name="export-data-with-bcp"></a>Exportación de datos con bcp

En este tutorial se usa `bcp` para exportar datos de la tabla de ejemplo creada anteriormente a un nuevo archivo de datos.

Copie y pegue los siguientes comandos en la ventana del terminal. Estos comandos usan la utilidad de línea de comandos `bcp` para exportar datos de la tabla **TestEmployees** de la base de datos **BcpSampleDB** a un nuevo archivo de datos denominado **~/test_export.txt**.  Recuerde reemplazar el nombre de usuario y `<your_password>` según sea necesario antes de ejecutar el comando.

```bash 
bcp TestEmployees out ~/test_export.txt -S localhost -U sa -P <your_password> -d BcpSampleDB -c -t ','
```

Para comprobar que los datos se han exportado correctamente, ejecute el siguiente comando en la ventana del terminal:
```bash 
cat ~/test_export.txt
```

Esto debería mostrar lo siguiente en la ventana del terminal:
```
1,Jared,Australia
2,Nikita,India
3,Tom,Germany
```

## <a name="see-also"></a>Consulte también
- [Utilidad bcp](../tools/bcp-utility.md)
- [Especificación de formatos de datos por razones de compatibilidad mediante bcp](../relational-databases/import-export/specify-data-formats-for-compatibility-when-using-bcp-sql-server.md)
- [Importación en bloque de datos mediante las instrucciones BULK INSERT o OPENROWSET(BULK...)](../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md)
- [BULK INSERT (Transact-SQL)](../t-sql/statements/bulk-insert-transact-sql.md)
