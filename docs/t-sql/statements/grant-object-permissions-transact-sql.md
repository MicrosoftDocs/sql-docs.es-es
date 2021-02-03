---
description: GRANT (permisos de objeto de Transact-SQL)
title: GRANT (permisos de objeto de Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- granting permissions [SQL Server], objects
- GRANT statement, objects
ms.assetid: c001c2e7-d092-43d4-8fa6-693b3ec4c3ea
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 681cb64aa6ef4874d331d61e2d8c07df2ed06ac1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161235"
---
# <a name="grant-object-permissions-transact-sql"></a>GRANT (permisos de objeto de Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Concede permisos para una tabla, vista, función con valores de tabla, procedimiento almacenado, procedimiento almacenado extendido, función escalar, función de agregado, cola de servicio o sinónimo.  
  

  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
GRANT <permission> [ ,...n ] ON   
    [ OBJECT :: ][ schema_name ]. object_name [ ( column [ ,...n ] ) ]  
    TO <database_principal> [ ,...n ]   
    [ WITH GRANT OPTION ]  
    [ AS <database_principal> ]  
  
<permission> ::=  
    ALL [ PRIVILEGES ] | permission [ ( column [ ,...n ] ) ]  
  
<database_principal> ::=   
        Database_user   
    | Database_role   
    | Application_role   
    | Database_user_mapped_to_Windows_User   
    | Database_user_mapped_to_Windows_Group   
    | Database_user_mapped_to_certificate   
    | Database_user_mapped_to_asymmetric_key   
    | Database_user_with_no_login  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *permission*  
 Especifica un permiso que se puede conceder para un objeto contenido en un esquema. Para obtener una lista de permisos, vea la sección Comentarios que se muestra posteriormente en este tema.  
  
 ALL  
 Si concede ALL no se conceden todos los permisos posibles. Conceder ALL es equivalente a conceder todos los permisos [!INCLUDE[vcpransi](../../includes/vcpransi-md.md)]-92 aplicables al objeto especificado. El significado de ALL varía de la siguiente forma:  
  
- Permisos de la función escalar: EXECUTE, REFERENCES.  
- Permisos de función con valores de tabla: DELETE, INSERT, REFERENCES, SELECT, UPDATE.  
- Permisos de procedimiento almacenado: EXECUTE.  
- Permisos de tabla: DELETE, INSERT, REFERENCES, SELECT, UPDATE.  
- Permisos de vista: DELETE, INSERT, REFERENCES, SELECT, UPDATE.  
  
PRIVILEGES  
 Incluido por compatibilidad con [!INCLUDE[vcpransi](../../includes/vcpransi-md.md)]-92. No cambia el comportamiento de ALL.  
  
*column*  
 Especifica el nombre de una columna en una tabla, vista o función con valores de tabla en la que se concede el permiso. Los paréntesis ( ) son obligatorios. Solo es posible conceder los permisos SELECT, REFERENCES y UPDATE para una columna. Se puede especificar *column* en la cláusula de permisos o después del nombre del elemento protegible.  
  
> [!CAUTION]  
>  Un permiso DENY de nivel de tabla no tiene prioridad sobre uno GRANT de nivel de columna. Se ha conservado esta incoherencia en la jerarquía de permisos para mantener la compatibilidad con versiones anteriores.  
  
 ON [ OBJECT :: ] [ *schema_name* ] . *object_name*  
 Especifica el objeto en el que se va a conceder el permiso. La frase OBJECT es opcional si se especifica *schema_name*. Si se utiliza la frase OBJECT, se requiere el calificador de ámbito (::). Si no se especifica *schema_name*, se usa el esquema predeterminado. Si se especifica *schema_name*, se necesita el calificador de ámbito de esquema (.).  
  
 TO \<database_principal>  
 Especifica la entidad de seguridad para la que se concede el permiso.  
  
 WITH GRANT OPTION  
 Indica que la entidad de seguridad también podrá conceder el permiso especificado a otras entidades de seguridad.  
  
 AS \<database_principal> Especifica una entidad de seguridad desde la que la entidad de seguridad que ejecuta esta consulta deriva su derecho de conceder el permiso.  
  
 *Database_user*  
 Especifica un usuario de base de datos.  
  
 *Database_role*  
 Especifica un rol de base de datos.  
  
 *Application_role*  
 Especifica un rol de aplicación.  
  
 *Database_user_mapped_to_Windows_User*  
 Especifica un usuario de base de datos asignado a un usuario de Windows.  
  
 *Database_user_mapped_to_Windows_Group*  
 Especifica un usuario de base de datos asignado a un grupo de Windows.  
  
 *Database_user_mapped_to_certificate*  
 Especifica un usuario de base de datos asignado a un certificado.  
  
 *Database_user_mapped_to_asymmetric_key*  
 Especifica un usuario de base de datos asignado a una clave asimétrica.  
  
 *Database_user_with_no_login*  
 Especifica un usuario de base de datos sin entidad de seguridad de servidor correspondiente.  
  
## <a name="remarks"></a>Observaciones  
  
> [!IMPORTANT]  
>  En algunos casos, una combinación de los permisos ALTER y REFERENCE podría permitir al receptor ver datos o ejecutar funciones no autorizadas. Por ejemplo: Un usuario con el permiso ALTER en una tabla y el permiso REFERENCE en una función puede crear una columna calculada sobre una función y hacer que se ejecute. En este caso, el usuario también necesitaría el permiso SELECT en la columna calculada.  
  
 Puede ver la información acerca de objetos en varias vistas de catálogo. Para más información, vea [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md) [Vistas de catálogo de objetos &#40;Transact-SQL&#41;].  
  
 Un objeto es un elemento protegible de nivel de esquema que contiene el esquema que es su entidad primaria en la jerarquía de permisos. La mayoría de los permisos limitados y específicos que se pueden conceder para un objeto se muestran en la siguiente tabla, junto con los permisos más generales que los incluyen por implicación.  
  
|Permiso de objeto|Implícito en el permiso de objeto|Implícito en el permiso del esquema|  
|-----------------------|----------------------------------|----------------------------------|  
|ALTER|CONTROL|ALTER|  
|CONTROL|CONTROL|CONTROL|  
|Delete|CONTROL|Delete|  
|Ejecute|CONTROL|Ejecute|  
|INSERT|CONTROL|INSERT|  
|RECEIVE|CONTROL|CONTROL|  
|REFERENCES|CONTROL|REFERENCES|  
|SELECT|RECEIVE|SELECT|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|UPDATE|CONTROL|UPDATE|  
|VIEW CHANGE TRACKING|CONTROL|VIEW CHANGE TRACKING|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Permisos  
 El otorgante del permiso (o la entidad de seguridad especificada con la opción AS) debe tener el permiso con GRANT OPTION, o un permiso superior que implique el permiso que se va a conceder.  
  
 Si utiliza la opción AS, se aplican los siguientes requisitos adicionales.  
  
|AS|Permiso adicional necesario|  
|--------|------------------------------------|  
|Usuario de la base de datos|Permiso IMPERSONATE para el usuario, pertenencia al rol fijo de base de datos db_securityadmin, pertenencia al rol fijo de base de datos db_owner o pertenencia al rol fijo de servidor sysadmin.|  
|Usuario de la base de datos asignado a un inicio de sesión de Windows|Permiso IMPERSONATE para el usuario, pertenencia al rol fijo de base de datos db_securityadmin, pertenencia al rol fijo de base de datos db_owner o pertenencia al rol fijo de servidor sysadmin.|  
|Usuario de la base de datos asignado a un grupo de Windows|Pertenencia al grupo de Windows, pertenencia al rol fijo de base de datos db_securityadmin, pertenencia al rol fijo de base de datos db_owner o pertenencia al rol fijo de servidor sysadmin.|  
|Usuario de la base de datos asignado a un certificado|Pertenencia al rol fijo de base de datos db_securityadmin, pertenencia al rol fijo de base de datos db_owner o pertenencia al rol fijo de servidor sysadmin.|  
|Usuario de la base de datos asignado a una clave asimétrica|Pertenencia al rol fijo de base de datos db_securityadmin, pertenencia al rol fijo de base de datos db_owner o pertenencia al rol fijo de servidor sysadmin.|  
|Usuario de la base de datos no asignado a una entidad de seguridad del servidor|Permiso IMPERSONATE para el usuario, pertenencia al rol fijo de base de datos db_securityadmin, pertenencia al rol fijo de base de datos db_owner o pertenencia al rol fijo de servidor sysadmin.|  
|Rol de base de datos|Permiso ALTER para el rol, pertenencia al rol fijo de base de datos db_securityadmin, pertenencia al rol fijo de base de datos db_owner o pertenencia al rol fijo de servidor sysadmin.|  
|Rol de aplicación|Permiso ALTER para el rol, pertenencia al rol fijo de base de datos db_securityadmin, pertenencia al rol fijo de base de datos db_owner o pertenencia al rol fijo de servidor sysadmin.|  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-granting-select-permission-on-a-table"></a>A. Conceder el permiso SELECT para una tabla  
 En el siguiente ejemplo, se concede el permiso `SELECT` al usuario `RosaQdM` en la tabla `Person.Address` de la base de datos `AdventureWorks2012`.  
  
```sql  
GRANT SELECT ON OBJECT::Person.Address TO RosaQdM;  
GO  
```  
  
### <a name="b-granting-execute-permission-on-a-stored-procedure"></a>B. Conceder el permiso EXECUTE para un procedimiento almacenado  
 En el siguiente ejemplo, se concede el permiso `EXECUTE` para el procedimiento almacenado `HumanResources.uspUpdateEmployeeHireInfo` a un rol de aplicación denominado `Recruiting11`.  
  
```sql  
USE AdventureWorks2012;   
GRANT EXECUTE ON OBJECT::HumanResources.uspUpdateEmployeeHireInfo  
    TO Recruiting11;  
GO   
```  
  
### <a name="c-granting-references-permission-on-a-view-with-grant-option"></a>C. Conceder el permiso REFERENCES para una vista con GRANT OPTION  
 En el siguiente ejemplo, se concede el permiso `REFERENCES` en la columna `BusinessEntityID` de la vista `HumanResources.vEmployee` al usuario `Wanida` con `GRANT OPTION`.  
  
```sql  
GRANT REFERENCES (BusinessEntityID) ON OBJECT::HumanResources.vEmployee   
    TO Wanida WITH GRANT OPTION;  
GO  
```  
  
### <a name="d-granting-select-permission-on-a-table-without-using-the-object-phrase"></a>D. Conceder el permiso SELECT en una tabla sin usar la frase OBJECT  
 En el siguiente ejemplo, se concede el permiso `SELECT` al usuario `RosaQdM` en la tabla `Person.Address` de la base de datos `AdventureWorks2012`.  
  
```sql  
GRANT SELECT ON Person.Address TO RosaQdM;  
GO  
```  
  
### <a name="e-granting-select-permission-on-a-table-to-a-domain-account"></a>E. Conceder el permiso SELECT en una tabla a una cuenta de dominio  
 En el siguiente ejemplo, se concede el permiso `SELECT` al usuario `AdventureWorks2012\RosaQdM` en la tabla `Person.Address` de la base de datos `AdventureWorks2012`.  
  
```sql  
GRANT SELECT ON Person.Address TO [AdventureWorks2012\RosaQdM];  
GO  
```  
  
### <a name="f-granting-execute-permission-on-a-procedure-to-a-role"></a>F. Conceder el permiso EXECUTE en un procedimiento a un rol  
 En el siguiente ejemplo, se crea un rol y, a continuación, se concede el permiso `EXECUTE` al rol en el procedimiento `uspGetBillOfMaterials` de la base de datos `AdventureWorks2012`.  
  
```sql  
CREATE ROLE newrole ;  
GRANT EXECUTE ON dbo.uspGetBillOfMaterials TO newrole ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Permisos de objeto DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md)   
 [REVOKE &#40;permisos de objeto de Transact-SQL&#41;](../../t-sql/statements/revoke-object-permissions-transact-sql.md)   
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  (Vistas de catálogo de objetos [Transact-SQL])  
 [Permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Securables](../../relational-databases/security/securables.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [HAS_PERMS_BY_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/has-perms-by-name-transact-sql.md)   
 [sys.fn_my_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)  
  
  

