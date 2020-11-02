---
description: MSSQLSERVER_5009
title: MSSQLSERVER_5009
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 5009 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 1ca7fb52969d9ec08d8c80c48ec1325277a13fa7
ms.sourcegitcommit: ead0b8c334d487a07e41256ce5d6acafa2d23c9d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2020
ms.locfileid: "92418913"
---
# <a name="mssqlserver_5009"></a>MSSQLSERVER_5009
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Detalles

|Atributo|Value|
|---|---|
|Nombre de producto|SQL Server|
|Id. de evento|5009|
|Origen de eventos|MSSQLSERVER|
|Componente|SQLEngine|
|Nombre simbólico|ALT_BADDISKS|
|Texto del mensaje|No se encuentra o no se puede inicializar uno o más archivos enumerados en la instrucción|
||

## <a name="explanation"></a>Explicación

Este error indica que se ha especificado un nombre o un identificador de archivo en la instrucción ALTER DATABASE o en el comando DBCC SHRINK * que no se ha podido resolver.

Considere el siguiente escenario:

- Tiene una base de datos de Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que usa un modelo de recuperación completa u optimizado para cargas masivas de registros.
- Agrega un nuevo archivo de datos denominado *db_file1* a la base de datos.
- Establece el tipo de archivo de `db_file1` como datos.
- Se da cuenta de que ha especificado el tipo de archivo de forma incorrecta.
- Quita el archivo `db_file1` y, después, hace una copia de seguridad del registro de transacciones de esta base de datos.
- Agrega un nuevo archivo de registro denominado *db_file1* a la misma base de datos.
- Intenta quitar el archivo de registro denominado *db_file1* mediante la instrucción ALTER DATABASE o con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio.

En este escenario, recibe un mensaje de error similar al siguiente:

> Mensaje 5009, nivel 16, estado 9, línea 1 No se encuentra o no se puede inicializar uno o más archivos enumerados en la instrucción.

## <a name="possible-causes"></a>Causas posibles

Este problema se produce si el nombre lógico del archivo que intenta quitar no es único en las tablas del catálogo del sistema. Por ejemplo, este error se genera si el archivo existía anteriormente en la base de datos y se quitó.

Al intentar quitar un archivo que tiene el mismo nombre lógico, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intenta eliminar el archivo lógico que se ha quitado, lo que da como resultado este mensaje de error.

## <a name="user-action"></a>Acción del usuario

Para solucionar este problema, siga estos pasos:

> [!NOTE]
> Estos pasos hacen que se reutilicen los valores del identificador de archivo.

1. Use la instrucción ALTER DATABASE para crear un nuevo archivo lógico con un nombre diferente y con el mismo tipo de datos. Por ejemplo, asigne al archivo lógico el nombre `different_remove_file_name` en lugar de `db_file1`, como en el siguiente ejemplo:

    ```sql
    ALTER DATABASE [DBNAME] ADD FILE ( NAME = N'different_remove_file_name',
    FILENAME = N'D:\MSSQL.1\MSSQL\DATA\db_file1.ndf', SIZE = 1MB, MAXSIZE = 1MB)
    ```

    > [!NOTE]
    > Puede usar cualquier nombre de archivo o cualquier ruta de acceso de archivo.

1. Use la instrucción ALTER DATABASE para quitar el archivo lógico que ha creado en el paso 1, tal y como se indica en el ejemplo siguiente:

    ```sql
    ALTER DATABASE [DBNAME] REMOVE FILE [different_remove_file_name]
    ```

1. Cree una copia de seguridad del registro de transacciones de la base de datos.
1. Intente quitar de nuevo el archivo lógico con el nombre *db_file1* .
