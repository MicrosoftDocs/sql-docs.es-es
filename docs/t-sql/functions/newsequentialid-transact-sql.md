---
description: NEWSEQUENTIALID (Transact-SQL)
title: NEWSEQUENTIALID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/08/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- NEWSEQUENTIALID
- NEWSEQUENTIALID_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- NEWSEQUENTIALID function
- GUIDs [SQL Server]
ms.assetid: e06d2cab-f1ff-42f1-8550-6aaec57be36f
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 6d3e8e018a2a85dffdb42867114e76547a6290b0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174378"
---
# <a name="newsequentialid-transact-sql"></a>NEWSEQUENTIALID (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Crea un identificador único global (GUID) que es mayor que cualquier GUID generado previamente por esta función en un equipo específico desde que se inició Windows. Después de reiniciar Windows, el GUID se puede volver a iniciar desde un intervalo más bajo, pero sigue siendo globalmente único. Cuando una columna de GUID se utiliza como identificador de fila, el uso de NEWSEQUENTIALID puede ser más rápido que el uso de la función NEWID. La razón es que la función NEWID causa actividad aleatoria y utiliza menos páginas de datos en caché. El uso de NEWSEQUENTIALID también ayuda a rellenar por completo las páginas de datos y de índices.  
  
> [!IMPORTANT]  
>  Si la protección de la privacidad es de particular importancia, no utilice esta función. Es posible estimar el valor del GUID generado a continuación y, por tanto, obtener acceso a los datos asociados con dicho GUID.  
  
 NEWSEQUENTIALID es un contenedor de la función [UuidCreateSequential](/windows/win32/api/rpcdce/nf-rpcdce-uuidcreatesequential) de Windows que tiene aplicado algo de [orden de bytes aleatorio](/archive/blogs/dbrowne/how-to-generate-sequential-guids-for-sql-server-in-net).
  
> [!WARNING]  
>  La función UuidCreateSequential tiene dependencias de hardware. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se pueden general clústeres de valores secuenciales cuando las bases de datos (por ejemplo, las bases de datos independientes) se mueven a otros equipos. Cuando se usa Always On en [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)], se pueden generar clústeres de valores secuenciales si la base de datos conmuta por error a otro equipo.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
NEWSEQUENTIALID ( )  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]


## <a name="return-type"></a>Tipo de valor devuelto  
 **uniqueidentifier**  
  
## <a name="remarks"></a>Comentarios  
 NEWSEQUENTIALID() solo se puede usar con restricciones DEFAULT en columnas de tabla de tipo **uniqueidentifier**. Por ejemplo:  
  
```sql  
CREATE TABLE myTable (ColumnA uniqueidentifier DEFAULT NEWSEQUENTIALID());   
```  
  
 Cuando NEWSEQUENTIALID() se utiliza en expresiones DEFAULT, no se puede combinar con otros operadores escalares. Por ejemplo, no se puede ejecutar lo siguiente:  
  
```sql 
CREATE TABLE myTable (ColumnA uniqueidentifier DEFAULT dbo.myfunction(NEWSEQUENTIALID()));  
```  
  
 En el ejemplo anterior, `myfunction()` es una función escalar definida por el usuario que acepta y devuelve un valor `uniqueidentifier`.  
  
 No se puede hacer referencia a NEWSEQUENTIALID en las consultas.  
  
 Puede usar NEWSEQUENTIALID para generar GUID a fin de reducir la contención de páginas y E/S aleatoria de nivel hoja en los índices.  
  
 Cada GUID generado utilizando NEWSEQUENTIALID es único en ese equipo. Los GUID generados utilizando NEWSEQUENTIALID solo son únicos en varios equipos si el equipo de origen tiene una tarjeta de red.  
  
## <a name="see-also"></a>Consulte también  
 [NEWID &#40;Transact-SQL&#41;](../../t-sql/functions/newid-transact-sql.md)   
 [Operadores de comparación &#40;Transact-SQL&#41;](../../t-sql/language-elements/comparison-operators-transact-sql.md)  
  
