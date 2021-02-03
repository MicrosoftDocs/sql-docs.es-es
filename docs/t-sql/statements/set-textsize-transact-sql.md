---
description: SET TEXTSIZE (Transact-SQL)
title: SET TEXTSIZE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/12/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TEXTSIZE_TSQL
- TEXTSIZE
- SET_TEXTSIZE_TSQL
- SET TEXTSIZE
dev_langs:
- TSQL
helpviewer_keywords:
- SET TEXTSIZE statement
- SELECT statement [SQL Server], text size returned
- size [SQL Server], text and image data
- TEXTSIZE option
- text size returned [SQL Server]
ms.assetid: 787154a6-39a6-4dd6-a6d0-67b4364f95d5
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2b89190f7e6e60f2ed81f4b9c4b6ada685ad4f9f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189460"
---
# <a name="set-textsize-transact-sql"></a>SET TEXTSIZE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Especifica el tamaño, en bytes, de los datos **varchar(max)** , **nvarchar(max)** , **varbinary(max)** , **text**, **ntext** e **image** devueltos al cliente por una instrucción SELECT.  
  
> [!IMPORTANT]
>  Los tipos de datos **ntext**, **text** e **image** se quitarán en una versión futura de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evite su uso en nuevos trabajos de desarrollo y piense en modificar las aplicaciones que los usan actualmente. Use **nvarchar(max)**, **varchar(max)** y **varbinary(max)** en su lugar.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
SET TEXTSIZE { number }   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *número*  
 Es la longitud de los datos **varchar(max)**, **nvarchar(max)**, **varbinary(max)**, **text**, **ntext** o **image**, en bytes. *number* es un entero con un valor máximo de 2 147 483 647 (2 GB).  Un valor -1 indica un tamaño ilimitado. Un valor 0 restablece el tamaño al valor predeterminado de 4 KB.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client (10.0 y versiones posteriores) y el controlador ODBC para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] especifican automáticamente `-1` (ilimitado) al conectarse.  
  
 **Controladores anteriores a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008:** el controlador ODBC de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client y el proveedor OLE DB de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client (versión 9) para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] establecen automáticamente TEXTSIZE en 2 147 483 647 al conectarse.  
  
## <a name="remarks"></a>Observaciones  
 La configuración de SET TEXTSIZE afecta a la función @@TEXTSIZE.  
  
 La opción de SET TEXTSIZE se establece en tiempo de ejecución, no en tiempo de análisis.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** .  
  
## <a name="see-also"></a>Consulte también  
 [@@TEXTSIZE &#40;Transact-SQL&#41;](../../t-sql/functions/textsize-transact-sql.md)   
 [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Instrucciones SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)  
  
  
