---
description: DROP EXTERNAL LIBRARY (Transact-SQL)
title: DROP EXTERNAL LIBRARY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/26/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning
ms.topic: reference
f1_keywords:
- DROP EXTERNAL LIBRARY
- DROP_EXTERNAL_LIBRARY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DROP EXTERNAL LIBRARY
author: dphansen
ms.author: davidph
manager: cgronlund
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 681e7082c2019cba4924d96e1617dc0031a76c35
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197575"
---
# <a name="drop-external-library-transact-sql"></a>DROP EXTERNAL LIBRARY (Transact-SQL)  
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Elimina una biblioteca de paquetes existente. Los tiempos de ejecución externos admitidos, como R, Python o Java, usan bibliotecas de paquetes.

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
> [!NOTE]
> En SQL Server 2017, se admiten el lenguaje R y la plataforma Windows. En SQL Server 2019 y versiones posteriores se admiten R, Python y Java en las plataformas Windows y Linux.
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
> [!NOTE]
> En Azure SQL Managed Instance, se admiten los lenguajes R y Python.
::: moniker-end

## <a name="syntax"></a>Sintaxis

```syntaxsql
DROP EXTERNAL LIBRARY library_name
[ AUTHORIZATION owner_name ];
```

### <a name="arguments"></a>Argumentos

**library_name**

Especifica el nombre de una biblioteca de paquetes existente.

Las bibliotecas tienen como ámbito el usuario. Los nombres de biblioteca deben ser únicos dentro del contexto de un usuario o propietario específico.

**owner_name**

Especifica el nombre del usuario o rol que es propietario de la biblioteca externa.

Los propietarios de bases de datos pueden eliminar bibliotecas creadas por otros usuarios.

## <a name="permissions"></a>Permisos

Para eliminar una biblioteca necesita el privilegio ALTER ANY EXTERNAL LIBRARY. De forma predeterminada, cualquier propietario de base de datos o el propietario del objeto también puede eliminar una biblioteca externa.

### <a name="return-values"></a>Valores devueltos

Si la instrucción se ejecuta correctamente, se devuelve un mensaje informativo.

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Observaciones

A diferencia de otras instrucciones `DROP` de SQL Server, esta instrucción admite la especificación de una cláusula AUTHORIZATION opcional. Esto permite a los **dbo** o a los usuarios con el rol **db_owner** quitar una biblioteca de paquetes cargada por un usuario normal en la base de datos.

Varios paquetes, denominados *paquetes del sistema*, se instalan previamente en una instancia de SQL. El usuario no puede agregar, actualizar ni quitar paquetes del sistema.

## <a name="examples"></a>Ejemplos

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
Agregue el paquete de R personalizado, `customPackage`, a una base de datos:

```sql
CREATE EXTERNAL LIBRARY customPackage 
FROM (CONTENT = 'C:\temp\customPackage_v1.1.zip')
WITH (LANGUAGE = 'R');
GO
```
::: moniker-end

Elimine la biblioteca `customPackage`.

```sql
DROP EXTERNAL LIBRARY customPackage;
```

## <a name="see-also"></a>Consulte también

[CREATE EXTERNAL LIBRARY (Transact-SQL)](create-external-library-transact-sql.md)  
[ALTER EXTERNAL LIBRARY (Transact-SQL)](alter-external-library-transact-sql.md)  
[sys.external_library_files](../../relational-databases/system-catalog-views/sys-external-library-files-transact-sql.md)  
[sys.external_libraries](../../relational-databases/system-catalog-views/sys-external-libraries-transact-sql.md)  
