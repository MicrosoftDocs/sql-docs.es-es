---
description: sp_validname (Transact-SQL)
title: sp_validname (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_validname
- sp_validname_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_validname
ms.assetid: d51c53c2-1332-407f-b725-4983f2e710eb
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9bb57f55751cbf29f7bef5c704da3aae363976db
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201861"
---
# <a name="sp_validname-transact-sql"></a>sp_validname (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Comprueba la validez de los nombres de identificadores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Todos los datos no binarios y distintos de cero, incluidos los datos Unicode que se pueden almacenar con los tipos de datos **nchar**, **nvarchar** o **ntext** , se aceptan como caracteres válidos para los nombres de identificador.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_validname [@name =] 'name'   
     [, [@raise_error =] raise_error]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @name = ] 'name'` Es el nombre de los [identificadores](../../relational-databases/databases/database-identifiers.md) cuya validez se va a comprobar. *Name* es de **tipo sysname** y no tiene ningún valor predeterminado. *el nombre* no puede ser null, no puede ser una cadena vacía y no puede contener un carácter cero binario.  
  
`[ @raise_error = ] raise_error` Especifica si se va a producir un error. *raise_error* es de **bit** y su valor predeterminado es 1. Esto significa que aparecerán errores. 0 hace que no aparezca ningún mensaje de error.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** .  
  
## <a name="see-also"></a>Consulte también  
 [Motor de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [NCHAR &#40;Transact-SQL&#41;](../../t-sql/functions/nchar-transact-sql.md)   
 [nchar y nvarchar &#40;Transact-SQL&#41;](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md)   
 [ntext, Text e Image &#40;Transact-SQL&#41;](../../t-sql/data-types/ntext-text-and-image-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
