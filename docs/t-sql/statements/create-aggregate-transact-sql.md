---
description: CREATE AGGREGATE (Transact-SQL)
title: CREATE AGGREGATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_AGGREGATE_TSQL
- CREATE AGGREGATE
- AGGREGATE_TSQL
- AGGREGATE
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE AGGREGATE statement
- aggregate functions [SQL Server], user-defined
- user-defined functions [CLR integration]
ms.assetid: 62eebc19-9f15-4245-94fa-b3fcd64a9d42
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7a616f60459e2b9d2a53789e18b538712aa86425
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207881"
---
# <a name="create-aggregate-transact-sql"></a>CREATE AGGREGATE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea una función de agregado definida por el usuario con la implementación definida en una clase de un ensamblado en [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]. Para que [!INCLUDE[ssDE](../../includes/ssde-md.md)] enlace la función de agregado con su implementación, el ensamblado de [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] que contiene la implementación debe cargarse primero en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con una instrucción CREATE ASSEMBLY.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
  
CREATE AGGREGATE [ schema_name . ] aggregate_name  
        (@param_name <input_sqltype>   
        [ ,...n ] )  
RETURNS <return_sqltype>  
EXTERNAL NAME assembly_name [ .class_name ]  
  
<input_sqltype> ::=  
        system_scalar_type | { [ udt_schema_name. ] udt_type_name }  
  
<return_sqltype> ::=  
        system_scalar_type | { [ udt_schema_name. ] udt_type_name }  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *schema_name*  
 Es el nombre del esquema al que pertenece la función de agregado definida por el usuario.  
  
 *aggregate_name*  
 Es el nombre de la función de agregado que desea crear.  
  
 **@** _param_name_  
 Uno o más parámetros en el agregado definido por el usuario. El usuario debe proporcionar un valor del parámetro cuando se ejecute la función de agregado. Especifique un nombre de parámetro con una arroba ( **@** ) como primer carácter. El nombre del parámetro debe cumplir las mismas reglas para [identifiers](../../relational-databases/databases/database-identifiers.md). Los parámetros son locales para la función.  
  
 *system_scalar_type*  
 Es cualquiera de los tipos de datos escalares del sistema de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que contienen el valor del parámetro de entrada o el valor devuelto. Todos los tipos de datos escalares se pueden usar como parámetros para un agregado definido por el usuario, excepto **text**, **ntext** e **image**. No se pueden especificar tipos no escalares, como **cursor** y **table**.  
  
 *udt_schema_name*  
 Es el nombre del esquema al que pertenece el tipo definido por el usuario CLR. Si no se especifica, [!INCLUDE[ssDE](../../includes/ssde-md.md)] hace referencia a *udt_type_name* en el siguiente orden:  
  
-   El espacio de nombres del tipo de SQL nativo.  
  
-   El esquema predeterminado del usuario actual en la base de datos actual.  
  
-   El esquema **dbo** de la base de datos actual.  
  
 *udt_type_name*  
 Es el nombre de un tipo definido por el usuario CLR que ya está creado en la base de datos actual. Si no se especifica *udt_schema_name*, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da por supuesto que el tipo pertenece al esquema del usuario actual.  
  
 *nombre_del_ensamblado* [ **.** _nombre_de_la_clase_ ]  
 Especifica el ensamblado que se va a vincular con la función de agregado definida por el usuario y, opcionalmente, el nombre del esquema al que pertenece el ensamblado y el nombre de la clase del ensamblado que implementa el agregado definido por el usuario. El ensamblado debe haberse creado con antelación en la base de datos mediante una instrucción CREATE ASSEMBLY. *class_name* debe ser un identificador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] válido y coincidir con el nombre de una clase que exista en el ensamblado. *class_name* puede ser un nombre completo de espacio de nombres si el lenguaje de programación usado para escribir la clase usa espacios de nombres, como C#. Si no se especifica *class_name*, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] considera que es el mismo que *aggregate_name*.  
  
## <a name="remarks"></a>Observaciones  
 De manera predeterminada, la capacidad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de ejecutar código CLR está desactivada. Puede crear, modificar y quitar objetos de base de datos que hacen referencia a módulos de código administrado, pero el código de estos módulos no se ejecutará en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a menos que la opción [clr enabled](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md) esté habilitada con [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
 La clase del ensamblado al que se hace referencia en *assembly_name* y sus métodos deben satisfacer todos los requisitos para implementar una función de agregado definida por el usuario en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para más información, vea [Agregados definidos por el usuario de CLR](../../relational-databases/clr-integration-database-objects-user-defined-functions/clr-user-defined-aggregates.md).  
  
## <a name="permissions"></a>Permisos  
 Requiere premisos CREATE AGGREGATE y REFERENCES en el ensamblado que se especifica en la cláusula EXTERNAL NAME.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se da por supuesto que una aplicación de ejemplo StringUtilities.csproj está compilada. Para más información, vea [Ejemplo de funciones de la utilidad String](/previous-versions/sql/sql-server-2016/ff878119(v=sql.130)).  
  
 En el ejemplo se crea un agregado `Concatenate`. Antes de crear el agregado, el ensamblado `StringUtilities.dll` se registra en la base de datos local.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @SamplesPath nvarchar(1024)  
-- You may have to modify the value of the this variable if you have  
--installed the sample some location other than the default location.  
  
SELECT @SamplesPath = REPLACE(physical_name, 'Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\master.mdf', 'Microsoft SQL Server\130\Samples\Engine\Programmability\CLR\')   
     FROM master.sys.database_files   
     WHERE name = 'master';  
  
CREATE ASSEMBLY StringUtilities FROM @SamplesPath + 'StringUtilities\CS\StringUtilities\bin\debug\StringUtilities.dll'  
WITH PERMISSION_SET=SAFE;  
GO  
  
CREATE AGGREGATE Concatenate(@input nvarchar(4000))  
RETURNS nvarchar(4000)  
EXTERNAL NAME [StringUtilities].[Microsoft.Samples.SqlServer.Concatenate];  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [DROP AGGREGATE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-aggregate-transact-sql.md)  
  
