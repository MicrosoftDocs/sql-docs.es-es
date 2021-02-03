---
description: ntext, text e image (Transact-SQL)
title: ntext, text e image (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ntext_TSQL
- ntext
dev_langs:
- TSQL
helpviewer_keywords:
- text data type, about text data type
- text [SQL Server], data types
- ntext data type
- ntext data type, about ntext data type
- image data type, about image data type
ms.assetid: b0d8769c-7598-4f97-8162-ace5f182b5bc
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: a7f89f1cb8585befbcd04aa03cdf8309cbe07d10
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187902"
---
# <a name="ntext-text-and-image-transact-sql"></a>ntext, text e image (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Tipos de datos de longitud fija y variable para almacenar valores de gran tamaño con datos de caracteres y binarios Unicode y no Unicode. Los datos Unicode utilizan el juego de caracteres UNICODE UCS-2.
  
>**IMPORTANTE:**  Los tipos de datos **ntext**, **text** e **image** se quitarán en una versión futura de SQL Server. Evite su uso en nuevos trabajos de desarrollo y piense en modificar las aplicaciones que los usan actualmente. Use [nvarchar(max)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), [varchar(max)](../../t-sql/data-types/char-and-varchar-transact-sql.md)y [varbinary(max)](../../t-sql/data-types/binary-and-varbinary-transact-sql.md) en su lugar.  
  
## <a name="arguments"></a>Argumentos
**ntext**  
Datos Unicode de longitud variable con una longitud máxima de cadena de 2^30 - 1 (1.073.741.823) bytes. El tamaño de almacenamiento, en bytes, es dos veces la longitud de cadena especificada. El sinónimo en ISO de **ntext** es **national text**.
  
**text**  
Datos no Unicode de longitud variable en la página de códigos del servidor y con una longitud máxima de cadena de 2^31-1 (2.147.483.647). Cuando la página de códigos del servidor utiliza caracteres de doble byte, el almacenamiento sigue siendo de 2.147.483.647 bytes. Dependiendo de la cadena de caracteres, el espacio de almacenamiento puede ser inferior a 2.147.483.647 bytes.
  
**image**  
Datos binarios de longitud variable desde 0 hasta 2^31-1 (2.147.483.647) bytes.
  
## <a name="remarks"></a>Observaciones  
Las siguientes funciones e instrucciones se pueden usar con datos de tipo **ntext**, **text** o **image**.
  
|Functions|Instrucciones|  
|---|---|
|[DATALENGTH &#40;Transact-SQL&#41;](../../t-sql/functions/datalength-transact-sql.md)|[READTEXT &#40;Transact-SQL&#41;](../../t-sql/queries/readtext-transact-sql.md)|  
|[PATINDEX &#40;Transact-SQL&#41;](../../t-sql/functions/patindex-transact-sql.md)|[SET TEXTSIZE &#40;Transact-SQL&#41;](../../t-sql/statements/set-textsize-transact-sql.md)|  
|[SUBSTRING &#40;Transact-SQL&#41;](../../t-sql/functions/substring-transact-sql.md)|[UPDATETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/updatetext-transact-sql.md)|  
|[TEXTPTR &#40;Transact-SQL&#41;](../../t-sql/functions/text-and-image-functions-textptr-transact-sql.md)|[WRITETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/writetext-transact-sql.md)|  
|[TEXTVALID &#40;Transact-SQL&#41;](../../t-sql/functions/text-and-image-functions-textvalid-transact-sql.md)||  
  
## <a name="see-also"></a>Consulte también
[CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[Conversiones de tipos de datos &#40;motor de base de datos&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)  
[Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)  
[SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)  
[Compatibilidad con la intercalación y Unicode](../../relational-databases/collations/collation-and-unicode-support.md)

