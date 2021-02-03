---
description: DROP ASSEMBLY (Transact-SQL)
title: DROP ASSEMBLY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP ASSEMBLY
- DROP_ASSEMBLY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- removing assemblies
- DROP ASSEMBLY statement
- deleting assemblies
- assemblies [CLR integration], removing
- dropping assemblies
- WITH NO DEPENDENTS option
ms.assetid: 452d181a-a8e6-44a3-975d-29966d01b18d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0dd88564bdd1853c29d08c48605ba3362c8011c9
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237145"
---
# <a name="drop-assembly-transact-sql"></a>DROP ASSEMBLY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Quita un ensamblado y todos sus archivos asociados de la base de datos actual. Los ensamblados se crean con [CREATE ASSEMBLY](../../t-sql/statements/create-assembly-transact-sql.md) y se modifican con [ALTER ASSEMBLY](../../t-sql/statements/alter-assembly-transact-sql.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DROP ASSEMBLY [ IF EXISTS ] assembly_name [ ,...n ]  
[ WITH NO DEPENDENTS ]  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *IF EXISTS*  
 **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (desde [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] hasta la [versión actual](/troubleshoot/sql/general/determine-version-edition-update-level)).  
  
 Quita condicionalmente el ensamblado solo si ya existe.  
  
 *assembly_name*  
 Es el nombre del ensamblado que desea quitar.  
  
 WITH NO DEPENDENTS  
 Si se especifica, solo quita *assembly_name* y ninguno de los ensamblados dependientes a los que hace referencia el ensamblado. Si no se especifica, DROP ASSEMBLY quita *assembly_name* y todos los ensamblados dependientes.  
  
## <a name="remarks"></a>Comentarios  
 Al quitar un ensamblado se quita el propio ensamblado y todos sus archivos asociados, como el código de origen y los archivos de depuración, de la base de datos.  
  
 Si no se especifica WITH NO DEPENDENTS, DROP ASSEMBLY quita *assembly_name* y todos los ensamblados dependientes. Si se generan errores al intentar quitar los ensamblados dependientes, DROP ASSEMBLY devuelve un error.  
  
 DROP ASSEMBLY devuelve un error si otro ensamblado que existe en la base de datos hace referencia al ensamblado o si se utiliza en procedimientos, desencadenadores, tipos definidos por el usuario, agregados o funciones de CLR (Common Language Runtime) en la base de datos actual.  
  
 DROP ASSEMBLY no interfiere con el código al que se hace referencia en el ensamblado que se ejecuta actualmente. No obstante, después de ejecutar DROP ASSEMBLY, los intentos de llamar el código de ensamblado generarán errores.  
  
## <a name="permissions"></a>Permisos  
 Se requiere la propiedad del ensamblado o permiso CONTROL en él.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se asume que el ensamblado `HelloWorld` ya está creado en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql  
DROP ASSEMBLY Helloworld ;  
```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/create-assembly-transact-sql.md)   
 [ALTER ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-assembly-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [Obtener información acerca de los ensamblados](../../relational-databases/clr-integration/assemblies-getting-information.md)  
