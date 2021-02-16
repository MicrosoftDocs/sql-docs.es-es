---
title: 'Acceso a datos externos: Oracle: PolyBase'
description: En este artículo se muestra cómo usar PolyBase con el fin de crear un origen de datos externos para tener acceso a datos de Oracle.
ms.date: 12/13/2019
ms.custom: seo-lt-2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
monikerRange: '>= sql-server-linux-ver15 || >= sql-server-ver15'
ms.openlocfilehash: 77478f132083417b20f3a722804f497ecf373d73
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351782"
---
# <a name="configure-polybase-to-access-external-data-in-oracle"></a>Configurar PolyBase para acceder a datos externos en Oracle

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

En el artículo se explica cómo usar PolyBase en una instancia de SQL Server para consultar datos externos en Oracle.

## <a name="prerequisites"></a>Prerrequisitos

Si no ha instalado PolyBase, consulte [Instalación de PolyBase](polybase-installation.md).

  Se debe crear una [Clave maestra](../../t-sql/statements/create-master-key-transact-sql.md) antes de crear una credencial de ámbito de base de datos. 

## <a name="configure-an-oracle-external-data-source"></a>Configuración de un origen de datos externos de Oracle

Para consultar los datos de un origen de datos de Oracle, debe crear tablas externas que hagan referencia a los datos externos. En esta sección se proporciona código de ejemplo para crear estas tablas externas.

En esta sección se utilizan los siguientes comandos de Transact-SQL:

- [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)
- [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md) 
- [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md)


1. Cree una credencial de ámbito de base de datos para acceder al origen de datos Oracle.

    ```sql
    /*  specify credentials to external data source
    *  IDENTITY: user name for external source. 
    *  SECRET: password for external source.
    */
    CREATE DATABASE SCOPED CREDENTIAL credential_name WITH IDENTITY = 'username', Secret = 'password';
    ```
    
   > [!IMPORTANT] 
   > El conector ODBC de Oracle para PolyBase solo admite la autenticación básica, no la autenticación Kerberos. 

1. Cree un origen de datos externo con [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

    ```sql
    /* 
    * LOCATION: Location string should be of format '<vendor>://<server>[:<port>]'.
    * PUSHDOWN: specify whether computation should be pushed down to the source. ON by default.
    * CONNECTION_OPTIONS: Specify driver location
    * CREDENTIAL: the database scoped credential, created above.
    */  
    CREATE EXTERNAL DATA SOURCE external_data_source_name
    WITH ( LOCATION = 'oracle://<server address>[:<port>]',
    -- PUSHDOWN = ON | OFF,
    CREDENTIAL = credential_name)
    ```

1. **Opcional:** Cree estadísticas en una tabla externa.

    Se recomienda crear estadísticas en las columnas de tabla externa, sobre todo en las que se usan para las combinaciones, filtros y agregados, para obtener un rendimiento óptimo de las consultas.

    ```sql
    CREATE STATISTICS statistics_name ON customer (C_CUSTKEY) WITH FULLSCAN; 
    ```

>[!IMPORTANT] 
>Una vez que se haya creado un origen de datos externos, se puede usar el comando [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md) para crear una tabla consultable a través de ese origen. 

Para obtener más información y ejemplos, consulte los artículos siguientes:

- [CREAR TABLA EXTERNA](../../t-sql/statements/create-external-table-transact-sql.md).
- [Información general de SQL Server PolyBase](polybase-guide.md).
