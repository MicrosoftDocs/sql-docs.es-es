---
title: Concesión de permisos para la ejecución de scripts de Python y R
description: Obtenga información sobre cómo puede conceder a los usuarios permiso para ejecutar scripts de Python y R externos en SQL Server Machine Learning Services y proporcionar permisos de lectura, escritura o lenguaje de definición de datos (DDL) a las bases de datos.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/14/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019, contperf-fy20q4
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 9214a27e0f52108f7e22ebf2108bfc06a3d6fbaf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340364"
---
# <a name="grant-users-permission-to-execute-python-and-r-scripts-with-sql-server-machine-learning-services"></a>Concesión de permisos de usuario para ejecutar scripts de Python y R con SQL Server Machine Learning Services
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Obtenga información sobre cómo puede conceder a los usuarios permiso para ejecutar scripts de Python y R externos en [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) y proporcionar permisos de lectura, escritura o lenguaje de definición de datos (DDL) a las bases de datos.

Para obtener más información, vea la sección permisos de [Seguridad para el marco de extensibilidad](../../machine-learning/concepts/security.md#permissions).

<a name="permissions-external-script"></a>

## <a name="permission-to-run-scripts"></a>Permiso para ejecutar scripts

Para cada usuario que ejecuta scripts de R o Python con SQL Server Machine Learning Services y que no sea administrador, debe concederle el permiso para ejecutar scripts externos en cada base de datos en la que se usa el lenguaje.

Para conceder permisos para la ejecución de un script externo, ejecute el siguiente script:

```sql
USE <database_name>
GO
GRANT EXECUTE ANY EXTERNAL SCRIPT TO [UserName]
```

> [!NOTE]
> Los permisos no son específicos del lenguaje de script admitido. En otras palabras, no hay niveles de permisos independientes para el script de R comparado con el script de Python.

<a name="permissions-db"></a>

## <a name="grant-databases-permissions"></a>Concesión de permisos de base de datos

Mientras un usuario ejecuta scripts, es posible que el usuario tenga que leer datos de otras bases de datos. El usuario también podría necesitar crear nuevas tablas para almacenar los resultados y escribir datos en las tablas.

Para cada cuenta de usuario de Windows o inicio de sesión de SQL que ejecute scripts de R o Python, asegúrese de que tiene los permisos adecuados en la base de datos específica: 

+ `db_datareader` para la lectura de datos.
+ `db_datawriter` para guardar objetos en la base de datos.
+ `db_ddladmin` para crear objetos como procedimientos almacenados o tablas que contienen datos entrenados y serializados.

Por ejemplo, la siguiente instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] concede al inicio de sesión de SQL *MySQLLogin* los derechos necesarios para ejecutar consultas de T-SQL en la base de datos *ML_Samples*. Para ejecutar esta instrucción, el inicio de sesión de SQL debe existir en el contexto de seguridad del servidor.

```sql
USE ML_Samples
GO
EXEC sp_addrolemember 'db_datareader', 'MySQLLogin'
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre los permisos incluidos en cada rol, vea [Roles de nivel de base de datos](../../relational-databases/security/authentication-access/database-level-roles.md).
