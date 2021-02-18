---
title: Conversión de tipos de datos de Python y SQL
description: Revise las conversiones implícitas y explícitas de tipos de datos entre Python y SQL Server en soluciones de ciencia de datos y Machine Learning.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/06/2020
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 99a677874a29b3784ed36f29876a332add21c920
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344428"
---
# <a name="data-type-mappings-between-python-and-sql-server"></a>Asignaciones de tipos de datos entre Python y SQL Server
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

En este artículo se enumeran los tipos de datos admitidos y las conversiones de tipos de datos que se realizan cuando se usa la característica de integración de Python en SQL Server Machine Learning Services.

En comparación con SQL Server, Python admite un número limitado de tipos de datos. Por consiguiente, cada vez que se usan datos de SQL Server en scripts de Python, es posible que los datos de SQL se conviertan implícitamente en un tipo de datos de Python compatible. Si bien, a menudo no se puede realizar una conversión exacta automáticamente y se devuelve un error.

## <a name="python-and-sql-data-types"></a>Tipos de datos de SQL y Python

En esta tabla se enumeran las conversiones implícitas que se proporcionan. No se admiten otros tipos de datos.

| Tipo SQL             | Tipo de Python | Descripción |
|----------------------|-------------|-------------|
| **bigint**           | `float64`   |
| **binary**           | `bytes`     |
| **bit**              | `bool`      |
| **char**             | `str`       |
| **date**             | `datetime`  |
| **datetime**         |`datetime`   | Compatible con SQL Server 2017 CU6 y versiones posteriores (con matrices **NumPy** de tipo `datetime.datetime` o **Pandas** `pandas.Timestamp`). `sp_execute_external_script` ahora admite tipos de `datetime` con fracciones de segundo.|
| **float**            | `float64`   |
| **nchar**            | `str`       |
| **nvarchar**         | `str`       |
| **nvarchar(max)**    | `str`       |
| **real**             | `float64`   |
| **smalldatetime**    | `datetime`  |
| **smallint**         | `int32`     |
| **tinyint**          | `int32`     |
| **uniqueidentifier** | `str`       |
| **varbinary**        | `bytes`     |
| **varbinary(max)**   | `bytes`     |
| **varchar(n)**       | `str`       |
| **ntext**     | `str`       |

## <a name="see-also"></a>Consulte también

+ [Asignaciones de tipos de datos entre R y SQL Server](../r/r-libraries-and-data-types.md)
