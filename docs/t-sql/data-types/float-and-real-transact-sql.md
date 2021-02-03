---
description: float y real (Transact-SQL)
title: float y real (Transact-SQL)
ms.custom: ''
ms.date: 09/10/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- float
- real_TSQL
- real
- float_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- numeric data type, floating point
- float data type
- floating point data [SQL Server]
- real data type
ms.assetid: 08ea66b7-624e-4d8b-86bc-750ff76cdfc5
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4b7f4df0fd5efea427bf806543c3f397e6b1d1f7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185341"
---
# <a name="float-and-real-transact-sql"></a>float y real (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Tipos de datos numéricos y aproximados que se utilizan con datos numéricos de coma flotante. Los datos de coma flotante son aproximados; por tanto, no todos los valores del rango del tipo de datos se pueden representar con exactitud. El sinónimo ISO para **real** es **float(24)** .
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
**float** [ **(** _n_ **)** ]Donde *n* es el número de bits que se usan para almacenar la mantisa del número de **float** en notación científica y, por tanto, dicta su precisión y el tamaño de almacenamiento. Si se especifica *n*, debe ser un valor entre **1** y **53**. El valor predeterminado de *n* es **53**.
  
|Valor *n*|Precision|Tamaño de almacenamiento|  
|---|---|---|
|**1-24**|7 dígitos|4 bytes|  
|**25-53**|15 dígitos|8 bytes|  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] trata *n* como uno de dos valores posibles. Si **1**<=n<=**24**, *n* se trata como **24**. Si **25**<=n<=**53**, *n* se trata como **53**.  
  
El tipo de datos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **float**[ **(n)** ] cumple con el estándar ISO para todos los valores de *n* desde **1** hasta **53**. El sinónimo de **double precision** es **float(53)** .

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Observaciones  
  
|Tipo de datos|Intervalo|Storage|  
|---|---|---|
|**float**|De - 1,79E+308 a -2,23E-308, 0 y de 2,23E-308 a 1,79E+308|Depende del valor de *n*|  
|**real**|De - 3,40E + 38 a -1,18E - 38, 0 y de 1,18E - 38 a 3,40E + 38|4 bytes|  
  
##  <a name="converting-float-and-real-data"></a>Convertir datos float y real  
Los valores de **float** se truncan cuando se convierten a cualquier tipo de entero.
  
Cuando convierte de valores de **float** o **real** a datos de carácter, suele resultar más útil usar la función de cadena STR que usar CAST( ). Esto se debe a que STR permite más control sobre el formato. Para más información, vea [STR &#40;Transact-SQL&#41;](../../t-sql/functions/str-transact-sql.md) y [Funciones &#40;Transact-SQL&#41;](../../t-sql/functions/functions.md).
  
Antes de [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)], la conversión de los valores **float** a **decimal** o **numeric** se restringía únicamente a los valores con una precisión de 17 dígitos. Cualquier valor **float** menor de 5E-18 (cuando se establece mediante la notación científica de 5E-18 o la notación decimal de 0,0000000000000000050000000000000005) se redondea hacia abajo a 0. A partir de [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] esto ya no es una restricción.
  
## <a name="see-also"></a>Consulte también
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
[CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[Conversiones de tipos de datos &#40;motor de base de datos&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)  
[Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
[SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)
  
  
