---
description: SECUENCIAS (Transact-SQL)
title: SECUENCIAS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/30/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- SEQUENCES
- SEQUENCES_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SEQUENCES view
- INFORMATION_SCHEMA.SEQUENCES view
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 60a542af207d0de4936dda79c2560dc14dac9ba6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185470"
---
# <a name="sequences-transact-sql"></a>SECUENCIAS (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Devuelve una fila por cada secuencia a la que puede tener acceso el usuario actual en la base de datos actual.

Para recuperar información de estas vistas, especifique el nombre completo de **INFORMATION_SCHEMA**_.view_name_.

|Nombre de la columna|Tipo de datos|Descripción|
|-----------------|---------------|-----------------|
|**SEQUENCE_CATALOG**|**nvarchar(128)**|Calificador de secuencia|
|**SEQUENCE_SCHEMA**|**nvarchar (** 128) * *|Nombre del esquema que contiene la secuencia|
|**SEQUENCE_NAME**|**nvarchar(128)**|Nombre de secuencia|
|**DATA_TYPE**|**nvarchar (** 128 **)**|El tipo de datos Sequence|
|**NUMERIC_PRECISION**|**tinyint**|La precisión de la secuencia|
|**NUMERIC_PRECISION_RADIX**|**smallint**|Base de la precisión de datos numéricos aproximados, datos numéricos exactos, datos enteros o datos monetarios. En caso contrario se devuelve NULL.|
|**NUMERIC_SCALE**|**int**|Escala de datos numéricos aproximados, datos numéricos exactos, datos enteros o datos monetarios. En caso contrario se devuelve NULL.|
|**START_VALUE**|**int**|El primer valor que el objeto de secuencia devolverá.|
|**MINIMUM_VALUE**|**int**|Los límites para el objeto de secuencia. El valor mínimo predeterminado para un nuevo objeto de secuencia es el valor mínimo del tipo de datos del objeto de secuencia. Es cero para el tipo de datos tinyint y un número negativo para todos los demás.|
|**MAXIMUM_VALUE**|**int**|Los límites para el objeto de secuencia. El valor máximo predeterminado para un nuevo objeto de secuencia es el valor máximo del tipo de datos del objeto de secuencia.|
|**IMPORTACIÓN**|**int**|Valor utilizado para incrementar (o disminuir si es negativo) el valor del objeto de secuencia para cada llamada a la función NEXT VALUE FOR. Si el incremento es un valor negativo, el objeto de secuencia es descendente; de lo contrario, es ascendente. El incremento no puede ser 0. El incremento predeterminado para un nuevo objeto de secuencia es 1.
|**CYCLE_OPTION**|**int**|Propiedad especifica si el objeto de secuencia se debería reiniciar desde el valor mínimo (o el máximo para los objetos de secuencia descendente) o producir una excepción cuando se supera el valor mínimo o máximo. La opción de ciclo predeterminado para los nuevos objetos de secuencia es NO CYCLE.
|**DECLARED_DATA_TYPE**|**int**|El tipo de datos para el tipo de datos definido por el usuario.|
|**DECLARED_DATA_PRECISION**|**int**|Precisión del tipo de datos definido por el usuario.|
|**DECLARED_NUJMERIC_SCALE**|**int**|Escala numérica para el tipo de datos definido por el usuario.|

**Ejemplo** de En el ejemplo siguiente, se devuelve información acerca de los esquemas de la base de datos de prueba:

```sql
SELECT * FROM test.INFORMATION_SCHEMA.SEQUENCES;
```

## <a name="see-also"></a>Consulte también

- [Vistas de esquema de información &#40;Transact-SQL&#41;](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)
- [sys.sequences &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sequences-transact-sql.md)
