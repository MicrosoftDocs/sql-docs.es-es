---
description: sys.syscomments (Transact-SQL)
title: sys.sysde comentarios (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.syscomments_TSQL
- syscomments
- syscomments_TSQL
- sys.syscomments
dev_langs:
- TSQL
helpviewer_keywords:
- sys.syscomments compatibility view
- syscomments system table
ms.assetid: 767dd410-6bc9-4c4a-ab0f-6d2cf6163426
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 1d72b83ad031b426f403046ea6110da742c0b95e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171804"
---
# <a name="syssyscomments-transact-sql"></a>sys.syscomments (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una entrada por cada vista, regla, valor predeterminado, desencadenador, restricción CHECK, restricción DEFAULT y procedimiento almacenado de la base de datos. La columna de **texto** contiene las instrucciones de definición de SQL originales.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Se recomienda utilizar restricciones sys.sql_modules en su lugar. Para obtener más información, vea [sys.sql_modules &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md).  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Identificador del objeto al que se aplica el texto.|  
|**número**|**smallint**|Número dentro del grupo de procedimientos, en el caso de procedimientos agrupados.<br /><br /> 0 = Las no son procedimientos.|  
|**colid**|**smallint**|Número de secuencia de fila para las definiciones de objeto de más de 4.000 caracteres.|  
|**status**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**ctext**|**varbinary(8000)**|Los bytes sin formato de la instrucción de definición de SQL.|  
|**texttype**|**smallint**|0 = Comentario proporcionado por el usuario<br /><br /> 1 = Comentario proporcionado por el sistema<br /><br /> 4 = Comentario cifrado|  
|**language**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**cifra**|**bit**|Indica si se ofusca la definición de procedimiento.<br /><br /> 0 = no ofuscado<br /><br /> 1 = ofuscó<br /><br /> Importante para ofuscar definiciones de procedimientos almacenados, utilice CREATE PROCEDURE con la palabra clave Encryption. **\* \* \* \***|  
|**comprimido**|**bit**|Siempre devuelve 0. Ello indica que el procedimiento está comprimido.|  
|**text**|**nvarchar(4000)**|Texto real de la instrucción de definición de SQL.<br /><br /> La semántica de la expresión descodificada es equivalente al texto original; no obstante, no existen garantías sintácticas. Por ejemplo, los espacios en blanco se quitan de la expresión descodificada.<br /><br /> Esta [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] vista compatible con obtiene información de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] las estructuras actuales y puede devolver más caracteres que la definición de **nvarchar (4000)** . **sp_help** devuelve **nvarchar (4000)** como tipo de datos de la columna de texto. Al trabajar con **syscomments** , considere la posibilidad de usar **nvarchar (Max)**. En el caso de los nuevos trabajos de desarrollo, no use **syscomments**.|  
  
## <a name="see-also"></a>Consulte también  
 [Asignar tablas del sistema a vistas del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Vistas de compatibilidad &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
