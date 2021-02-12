---
title: Uso de T-SQL (CREATE EXTERNAL LIBRARY) para instalar paquetes de R
description: Agregue nuevos paquetes de R a SQL Server 2016 R Services o SQL Server Machine Learning Services (en base de datos).
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/20/2019
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2017
ms.openlocfilehash: 058cd3a573abfa427b1e71ac08ac66c4b0e17932
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074356"
---
# <a name="use-t-sql-create-external-library-to-install-r-packages-on-sql-server"></a>Uso de T-SQL (CREATE EXTERNAL LIBRARY) para instalar paquetes de R en SQL Server
[!INCLUDE [SQL Server 2017 only](../../includes/applies-to-version/sqlserver2017-only.md)]

En este artículo se explica cómo instalar nuevos paquetes de R en una instancia de SQL Server en la que el aprendizaje automático está habilitado. Hay varios enfoques entre los que elegir. El uso de T-SQL funciona mejor para los administradores de servidores que no están familiarizados con R.

La instrucción [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md) permite agregar un paquete o un conjunto de paquetes a una instancia o una base de datos específica sin ejecutar código de R o Python directamente. Sin embargo, este método requiere la preparación del paquete y permisos de base de datos adicionales.

+ Todos los paquetes deben estar disponibles como un archivo comprimido local, en lugar de descargarse a petición desde Internet.

+ Todas las dependencias deben identificarse por el nombre y la versión e incluirse en el archivo ZIP. Se produce un error en la instrucción si los paquetes necesarios no están disponibles, incluidas las dependencias de paquetes de nivel inferior. 

+ Debe tener el rol **db_owner** o contar con el permiso CREATE EXTERNAL LIBRARY en un rol de base de datos. Para obtener más información, consulte [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md).

## <a name="download-packages-in-archive-format"></a>Descarga de paquetes en formato de archivo

Si va a instalar un único paquete, descargue el paquete en formato comprimido.

Es más común instalar varios paquetes debido a las dependencias de paquete. Cuando un paquete requiere otros paquetes, debe comprobar que todos ellos son accesibles entre sí durante la instalación. Se recomienda [crear un repositorio local](create-a-local-package-repository-using-minicran.md) mediante [miniCRAN](https://andrie.github.io/miniCRAN/) para ensamblar una colección completa de paquetes, así como [igraph](https://igraph.org/r/) para analizar las dependencias de los paquetes. La instalación de una versión incorrecta de un paquete o la omisión de una dependencia de paquetes puede producir un error en una instrucción CREATE EXTERNAL LIBRARY. 

## <a name="copy-the-file-to-a-local-folder"></a>Copia del archivo en una carpeta local

Copie el archivo comprimido que contiene todos los paquetes en una carpeta local del servidor. Si no tiene acceso al sistema de archivos del servidor, también puede pasar un paquete completo como una variable, con un formato binario. Para obtener más información, vea [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md).

## <a name="run-the-statement-to-upload-packages"></a>Ejecución de la instrucción para cargar paquetes

Abra una ventana de **consulta** mediante una cuenta con privilegios administrativos.

Ejecute la instrucción T-SQL `CREATE EXTERNAL LIBRARY` para cargar la colección de paquetes comprimidos en la base de datos.

Por ejemplo, la siguiente instrucción nombra como origen del paquete un repositorio miniCRAN que contiene el paquete **randomForest**, junto con sus dependencias. 

```sql
CREATE EXTERNAL LIBRARY [randomForest]
FROM (CONTENT = 'C:\Temp\Rpackages\randomForest_4.6-12.zip')
WITH (LANGUAGE = 'R');
```

No se puede usar un nombre arbitrario; el nombre de la biblioteca externa debe tener el mismo nombre que se espera usar al cargar o llamar al paquete.

## <a name="verify-package-installation"></a>Comprobación de la instalación del paquete

Si la biblioteca se crea correctamente, puede ejecutar el paquete en SQL Server, llamándolo dentro de un procedimiento almacenado.
    
```sql
EXEC sp_execute_external_script
@language =N'R',
@script=N'library(randomForest)'
```

## <a name="see-also"></a>Consulte también

+ [Obtención de información de paquetes de R](r-package-information.md)
+ [Tutoriales de R](../tutorials/r-tutorials.md)