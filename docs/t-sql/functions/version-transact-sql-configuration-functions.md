---
description: '&#x40;&#x40;Version - Funciones de configuración Transact SQL'
title: '@@VERSION (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 03/20/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '@@VERSION'
- '@@VERSION_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- '@@VERSION function'
- current SQL Server installation information
- versions [SQL Server], @@VERSION
- processors [SQL Server], types
ms.assetid: 385ba80e-7c28-41a5-9cdb-5648f3785983
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 39bf4fb339f27cd62929e5bc216e2bc27ad03def
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181803"
---
# <a name="x40x40version---transact-sql-configuration-functions"></a>&#x40;&#x40;Version - Funciones de configuración Transact SQL
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve información del sistema y la compilación para la instalación actual de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
@@VERSION  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devuelto
 **nvarchar**  
  
## <a name="remarks"></a>Observaciones  
 Los resultados de @@VERSION se presentan como una cadena nvarchar. Puede usar la función [SERVERPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/serverproperty-transact-sql.md) para recuperar los valores de propiedad individuales.  
  
 En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se devuelve la siguiente información.  
  
-   Versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
-   Arquitectura del procesador  
  
-   Fecha de compilación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
-   Instrucción de copyright  
  
-   Edición de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
-   Versión del sistema operativo  
  
 En [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], se devuelve la siguiente información.  
  
-   Edición: "Microsoft SQL Azure"  
  
-   Nivel de producto: "(RTM)"  
  
-   Versión del producto  
  
-   Fecha de compilación  
  
-   Instrucción de copyright  

> [!NOTE]  
> Somos conscientes de que hay un problema con el número de versión de producto en Azure SQL Database del que nos ha informado @@VERSION. La versión del motor de base de datos de SQL Server que ejecuta Azure SQL Database va siempre por delante de la versión local de SQL Server e incluye las correcciones de seguridad más recientes. Esto significa que el nivel de revisión siempre va a la par de la versión local de SQL Server, o incluso por delante, y que las características más recientes disponibles en SQL Server están disponibles en Azure SQL Database.
>
> Para determinar mediante programación la edición del motor, use SELECT SERVERPROPERTY('EngineEdition'). Esta consulta devolverá "5" para Azure SQL Database y "8" para Azure SQL Managed Instance.
>
> Una vez que se resuelva este problema, se actualizará la documentación.

  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-return-the-current-version-of-ssnoversion"></a>A. Devolver la versión actual de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 En el ejemplo siguiente se muestra cómo devolver la información de versión de la instalación actual.  
  
```sql
SELECT @@VERSION AS 'SQL Server Version';  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="b-return-the-current-version-of-ssdw"></a>B. Devolver la versión actual de [!INCLUDE[ssDW](../../includes/ssdw-md.md)]  
  
```sql
SELECT @@VERSION AS 'SQL Server PDW Version';  
```  
  
## <a name="see-also"></a>Consulte también  
 [SERVERPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/serverproperty-transact-sql.md)  
  
  

