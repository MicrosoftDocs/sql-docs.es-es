---
description: SESSIONPROPERTY (Transact-SQL)
title: SESSIONPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SESSIONPROPERTY
- SESSIONPROPERTY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SET statement, SESSIONPROPERTY function
- SESSIONPROPERTY function
- sessions [SQL Server], SET options settings
ms.assetid: 1f3730b4-1495-4d3a-af43-e57952812df9
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 4ce8fb3ed5f419efdcde0eac6c22c61bca543c09
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212201"
---
# <a name="sessionproperty-transact-sql"></a>SESSIONPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve la configuración de las opciones SET de una sesión.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
SESSIONPROPERTY (option)  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *Opción*  
 Es la configuración de opción actual para esta sesión. *option* puede ser cualquiera de los siguientes valores.  
  
|Opción|Descripción|  
|------------|-----------------|  
|ANSI_NULLS|Especifica si se aplica el comportamiento conforme a ISO de iguales (=) y No iguales a (<>) cuando se utilizan con valores null.<br /><br /> 1 = ON<br /><br /> 0 = OFF|  
|ANSI_PADDING|Controla el modo en que la columna almacena valores más cortos que el tamaño que tiene definido y cómo almacena valores con espacios en blanco finales en los datos binarios y de caracteres.<br /><br /> 1 = ON<br /><br /> 0 = OFF|  
|ANSI_WARNINGS|Especifica si se aplica el comportamiento del estándar ISO de provocar mensajes de error o advertencias para ciertas condiciones, incluyendo los errores de división por 0 y de desbordamiento.<br /><br /> 1 = ON<br /><br /> 0 = OFF|  
|ARITHABORT|Determina si se cancela una consulta cuando se produce un error de desbordamiento o división por cero durante su ejecución.<br /><br /> 1 = ON<br /><br /> 0 = OFF|  
|CONCAT_NULL_YIELDS_ NULL|Determina si los resultados de la concatenación se tratan como valores NULL o como valores de cadena vacía.<br /><br /> 1 = ON<br /><br /> 0 = OFF|  
|NUMERIC_ROUNDABORT|Especifica si se generan mensajes de error y advertencias cuando el redondeo en una expresión provoca una pérdida de precisión.<br /><br /> 1 = ON<br /><br /> 0 = OFF|  
|QUOTED_IDENTIFIER|Especifica si las reglas de ISO en cuanto a si hay que seguir las comillas delimitadoras de identificadores y cadenas literales.<br /><br /> 1 = ON<br /><br /> 0 = OFF|  
|\<Any other string>|NULL = La entrada no es válida.|  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 **sql_variant**  
  
## <a name="remarks"></a>Observaciones  
 Las opciones SET se indican mediante la combinación de las opciones de nivel de servidor, de nivel de base de datos y especificadas por el usuario.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se devuelve la configuración de la opción `CONCAT_NULL_YIELDS_NULL`.  
  
```sql  
SELECT   SESSIONPROPERTY ('CONCAT_NULL_YIELDS_NULL')  
```  
  
## <a name="see-also"></a>Consulte también  
 [sql_variant &#40;Transact-SQL&#41;](../../t-sql/data-types/sql-variant-transact-sql.md)   
 [SET ANSI_NULLS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-nulls-transact-sql.md)   
 [SET ANSI_PADDING &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-padding-transact-sql.md)   
 [SET ANSI_WARNINGS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-warnings-transact-sql.md)   
 [SET ARITHABORT &#40;Transact-SQL&#41;](../../t-sql/statements/set-arithabort-transact-sql.md)   
 [SET CONCAT_NULL_YIELDS_NULL &#40;Transact-SQL&#41;](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md)   
 [SET NUMERIC_ROUNDABORT &#40;Transact-SQL&#41;](../../t-sql/statements/set-numeric-roundabort-transact-sql.md)   
 [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/set-quoted-identifier-transact-sql.md)  
  
  
