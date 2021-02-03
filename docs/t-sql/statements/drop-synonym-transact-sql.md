---
description: DROP SYNONYM (Transact-SQL)
title: DROP SYNONYM (Transact-SQL)
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP SYNONYM
- DROP_SYNONYM_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- deleting synonyms
- synonyms [SQL Server], removing
- removing synonyms
- DROP SYNONYM statement
- dropping synonyms
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a156c4c5bda031b079df53fa3f3796d271b62a1b
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236205"
---
# <a name="drop-synonym-transact-sql"></a>DROP SYNONYM (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Quita un sinónimo de un esquema especificado.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DROP SYNONYM [ IF EXISTS ] [ schema. ] synonym_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *IF EXISTS*  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (desde [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] hasta la [versión actual](/troubleshoot/sql/general/determine-version-edition-update-level)).
  
 Quita condicionalmente el sinónimo solo si ya existe.  
  
 *schema*  
 Especifica el esquema en el que existe el sinónimo. Si no se especifica, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utiliza el esquema predeterminado del usuario actual.  
  
 *synonym_name*  
 Es el nombre del sinónimo que se va a quitar.  
  
## <a name="remarks"></a>Observaciones  
 Las referencias a sinónimos no están enlazadas al esquema, por lo que un sinónimo se puede quitar cuando se desee. Las referencias a sinónimos quitados solo se encontrarán en tiempo de ejecución.  
  
 Es posible crear, quitar y hacer referencia a sinónimos en SQL dinámico.  
  
## <a name="permissions"></a>Permisos  
 Para quitar un sinónimo, un usuario debe cumplir al menos una de las condiciones siguientes. El usuario debe ser:  
  
-   El propietario actual del sinónimo.  
  
-   Receptor del permiso CONTROL en el sinónimo.  
  
-   Receptor del permiso ALTER SCHEMA en el esquema contenedor.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente, primero se crea el sinónimo `MyProduct` y después se quita.  
  
```sql  
USE tempdb;  
GO  
-- Create a synonym for the Product table in AdventureWorks2012.  
CREATE SYNONYM MyProduct  
FOR AdventureWorks2012.Production.Product;  
GO  
-- Drop synonym MyProduct.  
USE tempdb;  
GO  
DROP SYNONYM MyProduct;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE SYNONYM &#40;Transact-SQL&#41;](../../t-sql/statements/create-synonym-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
