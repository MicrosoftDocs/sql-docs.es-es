---
description: CREATE SECURITY POLICY (Transact-SQL)
title: CREATE SECURITY POLICY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SECURITY_POLICY_TSQL
- CREATE SECURITY
- SECURITY
- CREATE_SECURITY_POLICY_TSQL
- CREATE_SECURITY_TSQL
- SECURITY POLICY
- SECURITY_TSQL
- CREATE SECURITY POLICY
dev_langs:
- TSQL
helpviewer_keywords:
- RLS
- CREATE SECURITY POLICY statement
- Row-Level Security
ms.assetid: d6ab70ee-0fa2-469c-96f6-a3c16d673bc8
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: ddebebc1a204af546734425ffd76781948cd82de
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188488"
---
# <a name="create-security-policy-transact-sql"></a>CREATE SECURITY POLICY (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Crea una directiva de seguridad para la seguridad de nivel de filas.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CREATE SECURITY POLICY [schema_name. ] security_policy_name    
    { ADD [ FILTER | BLOCK ] } PREDICATE tvf_schema_name.security_predicate_function_name   
      ( { column_name | expression } [ , ...n] ) ON table_schema_name. table_name    
      [ <block_dml_operation> ] , [ , ...n] 
    [ WITH ( STATE = { ON | OFF }  [,] [ SCHEMABINDING = { ON | OFF } ] ) ]  
    [ NOT FOR REPLICATION ] 
[;]  
  
<block_dml_operation>  
    [ { AFTER { INSERT | UPDATE } }   
    | { BEFORE { UPDATE | DELETE } } ]  
```  

## <a name="arguments"></a>Argumentos
 *security_policy_name*  
 El nombre de la directiva de seguridad. Los nombres de directivas de seguridad deben seguir las reglas de los identificadores y deben ser únicos en la base de datos y para su esquema.  
  
 *schema_name*  
 Es el nombre del esquema al que pertenece la directiva de seguridad. *schema_name* es necesario debido a los enlaces de esquema.  
  
 [ FILTER | BLOCK ]  
 El tipo de predicado de seguridad de la función vinculada a la tabla de destino. Los predicados FILTER filtran en modo silencioso las filas disponibles para leer operaciones. Los predicados BLOCK bloquean explícitamente las operaciones de escritura que infringen la función del predicado.  
  
 *tvf_schema_name.security_predicate_function_name*  
 Es la función de valor de tabla insertada que se usará como predicado y que se aplicará en las consultas en una tabla de destino. Se puede definir, como máximo, un predicado de seguridad para una operación DML determinada en una tabla determinada. La función de valor de tabla insertada se debe haber creado con la opción SCHEMABINDING.  
  
 { *nombre_columna* | *expresión* }  
 El nombre de columna o la expresión que se usan como parámetros de la función de predicado de seguridad. Se puede utilizar cualquier columna de la tabla de destino. Una [expresión ](../../t-sql/language-elements/expressions-transact-sql.md) solo puede incluir constantes, funciones escalares incorporadas, operadores y columnas de la tabla de destino. Es necesario especificar un nombre o expresión de columna para cada parámetro de la función.  
  
 *table_schema_name.table_name*  
 Es la tabla de destino a la que se aplicará el predicado de seguridad. Puede haber varias directivas de seguridad deshabilitadas que tengan como destino una sola tabla para una operación DML concreta, pero no puede haber varias de ellas habilitadas al mismo tiempo.  
  
 *\<block_dml_operation>* La operación DML concreta a la que se aplicará el predicado de bloqueo. AFTER especifica que el predicado se va a evaluar en función de los valores de las filas después de que se haya realizado la operación DML (INSERT o UPDATE). BEFORE especifica que el predicado se va a evaluar en función de los valores de las filas antes de que se haya realizado la operación DML (UPDATE o DELETE). Si no se especifica ninguna operación, el predicado se aplicará a todas las operaciones.  
  
 [ STATE = { **ON** | OFF } ]  
 Habilita o deshabilita la aplicación de los predicados de seguridad de la directiva de seguridad en las tablas de destino. Si no se especifica, se habilita la directiva de seguridad que se está creando.  
  
 [ SCHEMABINDING = { **ON** | OFF } ]  
 Indica si todas las funciones de predicado de la directiva se deben crear con la opción SCHEMABINDING. De forma predeterminada, esta configuración es **ON** y todas las funciones se deben crear con SCHEMABINDING.  
  
 NOT FOR REPLICATION  
 Indica que la directiva de seguridad no debe ejecutarse cuando un agente de replicación modifica el objeto de destino. Para obtener más información, vea [Controlar el comportamiento de desencadenadores y restricciones durante la sincronización &#40;programación de la replicación con Transact-SQL&#41;](../../relational-databases/replication/control-behavior-of-triggers-and-constraints-in-synchronization.md).  
  
 [*table_schema_name*.] *table_name*  
 Es la tabla de destino a la que se aplicará el predicado de seguridad. Puede haber varias directivas de seguridad deshabilitadas que tengan como destino una sola tabla, pero no puede haber varias de ellas habilitadas al mismo tiempo.  
  

## <a name="remarks"></a>Observaciones
 Al usar las funciones de predicado con tablas optimizadas para memoria, hay que incluir **SCHEMABINDING** y usar la sugerencia de compilación **WITH NATIVE_COMPILATION**.  
  
 Los predicados de bloqueo se evalúan después de ejecutar la operación DML correspondiente. Por lo tanto, una consulta READ UNCOMMITTED puede mostrar valores transitorios que se revertirán.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso ALTER ANY SECURITY POLICY y el permiso ALTER en el esquema.  
  
 Además, son necesarios los siguientes permisos para cada predicado que se agrega:  
  
-   Los permisos SELECT y REFERENCES en la función que se utiliza como predicado.  
  
-   El permiso REFERENCES en la tabla de destino que se enlaza a la directiva.  
  
-   El permiso REFERENCES en todas las columnas de la tabla de destino que se utilizan como argumentos.  
  
## <a name="examples"></a>Ejemplos  
 Los ejemplos siguientes muestran el uso de la sintaxis de **CREATE SECURITY POLICY** . Para ver un ejemplo de un escenario completo de la directiva de seguridad, vea [Seguridad de nivel de fila](../../relational-databases/security/row-level-security.md).  
  
### <a name="a-creating-a-security-policy"></a>A. Creación de una directiva de seguridad  
 La siguiente sintaxis crea una directiva de seguridad con un predicado de filtro para la tabla Customer y deja deshabilitada la directiva de seguridad.  
  
```  
CREATE SECURITY POLICY [FederatedSecurityPolicy]   
ADD FILTER PREDICATE [rls].[fn_securitypredicate]([CustomerId])   
ON [dbo].[Customer];  
```  
  
### <a name="b-creating-a-policy-that-affects-multiple-tables"></a>B. Creación de una directiva que afecta a varias tablas  
 La siguiente sintaxis crea una directiva de seguridad con tres predicados de filtro en tres tablas diferentes y habilita la directiva de seguridad.  
  
```sql  
CREATE SECURITY POLICY [FederatedSecurityPolicy]   
ADD FILTER PREDICATE [rls].[fn_securitypredicate1]([CustomerId])   
    ON [dbo].[Customer],  
ADD FILTER PREDICATE [rls].[fn_securitypredicate1]([VendorId])   
    ON [dbo].[ Vendor],  
ADD FILTER PREDICATE [rls].[fn_securitypredicate2]([WingId])   
    ON [dbo].[Patient]  
WITH (STATE = ON);  
```  
  
### <a name="c-creating-a-policy-with-multiple-types-of-security-predicates"></a>C. Crear una directiva con varios tipos de predicados de seguridad  
 Agregue un predicado de filtro y un predicado de bloqueo a la tabla Sales.  
  
```sql  
CREATE SECURITY POLICY rls.SecPol  
    ADD FILTER PREDICATE rls.tenantAccessPredicate(TenantId) ON dbo.Sales,  
    ADD BLOCK PREDICATE rls.tenantAccessPredicate(TenantId) ON dbo.Sales AFTER INSERT;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Seguridad de nivel de fila](../../relational-databases/security/row-level-security.md)   
 [ALTER SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-security-policy-transact-sql.md)   
 [DROP SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-security-policy-transact-sql.md)   
 [sys.security_policies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-security-policies-transact-sql.md)   
 [sys.security_predicates &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-security-predicates-transact-sql.md)  
  
  

