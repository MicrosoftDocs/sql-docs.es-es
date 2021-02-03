---
description: DROP DEFAULT (Transact-SQL)
title: DROP DEFAULT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP_DEFAULT_TSQL
- DROP DEFAULT
dev_langs:
- TSQL
helpviewer_keywords:
- DROP DEFAULT statement
- defaults [SQL Server], removing
ms.assetid: d2d3af25-8877-46ba-95d9-1844961d97ee
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: bf1a262a80fdf78eaff96c4f1483403de40b3bf8
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99232912"
---
# <a name="drop-default-transact-sql"></a>DROP DEFAULT (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Quita uno o más valores predeterminados definidos por el usuario de la base de datos actual.  
  
> [!IMPORTANT]
>  DROP DEFAULT se quitará en la siguiente versión de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. No use DROP DEFAULT en un nuevo trabajo de desarrollo y piense en modificar las aplicaciones que actualmente utilizan esta función. En su lugar, use definiciones predeterminadas que pueda crear usando la palabra clave DEFAULT de [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) o [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DROP DEFAULT [ IF EXISTS ] { [ schema_name . ] default_name } [ ,...n ] [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *IF EXISTS*  
 **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (desde [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] hasta la [versión actual](/troubleshoot/sql/general/determine-version-edition-update-level)).  
  
 Quita condicionalmente el valor predeterminado solo si ya existe.  
  
 *schema_name*  
 Es el nombre del esquema al que pertenece el valor predeterminado.  
  
 *default_name*  
 Es el nombre de un valor predeterminado existente. Para ver la lista de valores predeterminados existentes, ejecute **sp_help**. Los valores predeterminados deben seguir las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md). Especificar el nombre del esquema predeterminado es opcional.  
  
## <a name="remarks"></a>Observaciones  
 Antes de quitar un valor predeterminado, cancele el enlace del valor predeterminado; para ello, ejecute **sp_unbindefault** (si el valor predeterminado está enlazado actualmente a una columna o un tipo de datos de alias).  
  
 Después de quitar un valor predeterminado de una columna que permite valores nulos, se inserta NULL en esa posición cuando se agregan filas y no se proporciona un valor explícitamente. Después de quitar un valor predeterminado de una columna NOT NULL, se devuelve un mensaje de error cuando se agregan filas y no se proporciona un valor explícitamente. Estas filas se agregan posteriormente como parte del comportamiento habitual de la instrucción INSERT.  
  
## <a name="permissions"></a>Permisos  
 Para ejecutar DROP DEFAULT, como mínimo, un usuario debe tener permiso ALTER en el esquema al que pertenece el valor predeterminado.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-dropping-a-default"></a>A. Quitar un valor predeterminado  
 Si no se ha enlazado un valor predeterminado a una columna o a un tipo de datos de alias, se puede quitar simplemente con DROP DEFAULT. El siguiente ejemplo quita un valor predeterminado creado por el usuario denominado `datedflt`.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF EXISTS (SELECT name FROM sys.objects  
         WHERE name = 'datedflt'   
            AND type = 'D')  
   DROP DEFAULT datedflt;  
GO  
```  
  
 A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] se puede usar la siguiente sintaxis.  
  
```sql  
DROP DEFAULT IF EXISTS datedflt;  
GO  
```  
  
### <a name="b-dropping-a-default-that-has-been-bound-to-a-column"></a>B. Quitar un valor predeterminado enlazado a una columna  
 El siguiente ejemplo cancela el enlace del valor predeterminado asociado con la columna `EmergencyContactPhone` de la tabla `Contact` y, a continuación, quita el valor predeterminado denominado `phonedflt`.  
  
```sql  
USE AdventureWorks2012;  
GO  
   BEGIN   
      EXEC sp_unbindefault 'Person.Contact.Phone'  
      DROP DEFAULT phonedflt  
   END;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/create-default-transact-sql.md)   
 [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_unbindefault &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-unbindefault-transact-sql.md)  
