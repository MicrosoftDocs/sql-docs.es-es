---
description: ALTER SECURITY POLICY (Transact-SQL)
title: ALTER SECURITY POLICY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_SECURITY_POLICY_TSQL
- ALTER SECURITY POLICY
- ALTER_SECURITY_TSQL
- ALTER SECURITY
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER SECURITY POLICY statement
ms.assetid: a8efc37e-113d-489c-babc-b914fea2c316
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: e7c0976f6fe428b21817a2577cced14dda66852e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182201"
---
# <a name="alter-security-policy-transact-sql"></a>ALTER SECURITY POLICY (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Modifica una directiva de seguridad.  
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
ALTER SECURITY POLICY schema_name.security_policy_name   
    (  
        { ADD { FILTER | BLOCK } PREDICATE tvf_schema_name.security_predicate_function_name   
           ( { column_name | arguments } [ , ...n ] ) ON table_schema_name.table_name   
           [ <block_dml_operation> ]  }   
        | { ALTER { FILTER | BLOCK } PREDICATE tvf_schema_name.new_security_predicate_function_name   
             ( { column_name | arguments } [ , ...n ] ) ON table_schema_name.table_name   
           [ <block_dml_operation> ] }  
        | { DROP { FILTER | BLOCK } PREDICATE ON table_schema_name.table_name }   
        | [ <additional_add_alter_drop_predicate_statements> [ , ...n ] ]  
    )    [ WITH ( STATE = { ON | OFF } ) ]  
    [ NOT FOR REPLICATION ]  
[;]  
  
<block_dml_operation>  
    [ { AFTER { INSERT | UPDATE } }   
    | { BEFORE { UPDATE | DELETE } } ]  
```  

## <a name="arguments"></a>Argumentos
security_policy_name  
El nombre de la directiva de seguridad. Los nombres de directivas de seguridad deben seguir las reglas de los identificadores y deben ser únicos en la base de datos y para su esquema.  
  
schema_name  
Es el nombre del esquema al que pertenece la directiva de seguridad. *schema_name* es necesario debido a los enlaces de esquema.  
  
[ FILTER | BLOCK ]  
El tipo de predicado de seguridad de la función vinculada a la tabla de destino. Los predicados FILTER filtran en modo silencioso las filas disponibles para leer operaciones. Los predicados BLOCK bloquean explícitamente las operaciones de escritura que infringen la función del predicado.  
  
tvf_schema_name.security_predicate_function_name  
Es la función de valor de tabla insertada que se usará como predicado y que se aplicará en las consultas en una tabla de destino. Se puede definir, como máximo, un predicado de seguridad para una operación DML determinada en una tabla determinada. Cree la función de valor de tabla insertada con la opción SCHEMABINDING.  
  
{ column_name | argumentos }  
El nombre de columna o la expresión que se usan como parámetros de la función de predicado de seguridad. Todas las columnas de la tabla de destino pueden utilizarse como argumentos de la función de predicado. Pueden utilizarse expresiones que incluyen literales, builtins y expresiones que usan operadores aritméticos.  
  
*table_schema_name.table_name*  
Es la tabla de destino del predicado de seguridad. Puede haber varias directivas de seguridad deshabilitadas que tengan como destino una sola tabla para una operación DML concreta, pero no puede haber varias de ellas habilitadas al mismo tiempo.  
  
*\<block_dml_operation>*  
La operación DML determinada a la que se aplicará el predicado de bloqueo. AFTER especifica que el predicado se va a evaluar en función de los valores de las filas después de que se haya realizado la operación DML (INSERT o UPDATE). BEFORE especifica que el predicado se va a evaluar en función de los valores de las filas antes de que se haya realizado la operación DML (UPDATE o DELETE). Si no se especifica ninguna operación, el predicado se aplicará a todas las operaciones.  
  
No puede modificar la operación a la que se va a aplicar un predicado de bloqueo, ya que la operación se utiliza para identificar el predicado de forma única. En cambio, debe quitar el predicado y agregar uno nuevo para la operación nueva.  
  
WITH ( STATE = { ON | OFF } )  
Habilita o deshabilita la aplicación de los predicados de seguridad de la directiva de seguridad en las tablas de destino. Si no se especifica, se habilita la directiva de seguridad que se está creando.  
  
NOT FOR REPLICATION  
Indica que la directiva de seguridad no debe ejecutarse cuando un agente de replicación modifica el objeto de destino. Para obtener más información, vea [Controlar el comportamiento de desencadenadores y restricciones durante la sincronización &#40;programación de la replicación con Transact-SQL&#41;](../../relational-databases/replication/control-behavior-of-triggers-and-constraints-in-synchronization.md).  
  
table_schema_name.table_name  
Es la tabla de destino del predicado de seguridad aplicado. Puede haber varias directivas de seguridad deshabilitadas que tengan como destino una sola tabla, pero no puede haber varias de ellas habilitadas al mismo tiempo.  
  
## <a name="remarks"></a>Observaciones
La instrucción ALTER SECURITY POLICY está en el ámbito de una transacción. Si se revierte la transacción, también se revierte la instrucción.  
  
Al utilizar las funciones de predicado con tablas optimizadas para memoria, las directivas de seguridad deben incluir **SCHEMABINDING** y utilizar la sugerencia de compilación **WITH NATIVE_COMPILATION**. El argumento SCHEMABINDING no se puede intercambiar por la instrucción ALTER porque se aplica a todos los predicados. Para cambiar la vinculación del esquema, debe quitar y volver a crear la directiva de seguridad.  
  
Los predicados de bloqueo se evalúan después de ejecutar la operación DML correspondiente. Por lo tanto, una consulta READ UNCOMMITTED puede mostrar valores transitorios que se revertirán.  
  
## <a name="permissions"></a>Permisos  
Requiere el permiso ALTER ANY SECURITY POLICY.  
  
Además, son necesarios los siguientes permisos para cada predicado que se agrega:  
  
-   Los permisos SELECT y REFERENCES en la función que se utiliza como predicado.  
-   El permiso REFERENCES en la tabla de destino que se enlaza a la directiva.  
-   El permiso REFERENCES en todas las columnas de la tabla de destino que se utilizan como argumentos.  
  
## <a name="examples"></a>Ejemplos  
Los ejemplos siguientes muestran el uso de la sintaxis de **ALTER SECURITY POLICY**. Para ver un ejemplo de un escenario completo de la directiva de seguridad, vea [Seguridad de nivel de fila](../../relational-databases/security/row-level-security.md).  
  
### <a name="a-adding-an-additional-predicate-to-a-policy"></a>A. Agregar un predicado adicional a una directiva  
La siguiente sintaxis modifica una directiva de seguridad y agrega un predicado de filtro a la tabla `mytable`.  
  
```sql  
ALTER SECURITY POLICY pol1   
    ADD FILTER PREDICATE schema_preds.SecPredicate(column1)   
    ON myschema.mytable;  
```  
  
### <a name="b-enabling-an-existing-policy"></a>B. Habilitar una directiva existente  
En el ejemplo siguiente, se utiliza la sintaxis de ALTER para habilitar una directiva de seguridad.  
  
```sql  
ALTER SECURITY POLICY pol1 WITH ( STATE = ON );  
```  
  
### <a name="c-adding-and-dropping-multiple-predicates"></a>C. Agregar y anular varios predicados  
La sintaxis siguiente modifica una directiva de seguridad, agrega predicados de filtro a las tablas `mytable1` y `mytable3` y quita el predicado de filtro de la tabla `mytable2`.  
  
```sql  
ALTER SECURITY POLICY pol1  
ADD FILTER PREDICATE schema_preds.SecPredicate1(column1)   
    ON myschema.mytable1,  
DROP FILTER PREDICATE   
    ON myschema.mytable2,  
ADD FILTER PREDICATE schema_preds.SecPredicate2(column2, 1)   
    ON myschema.mytable3;  
```  
  
### <a name="d-changing-the-predicate-on-a-table"></a>D. Cambiar el predicado en una tabla  
La sintaxis siguiente cambia el predicado de filtro existente en la tabla mytable para que sea la función SecPredicate2.  
  
```sql  
ALTER SECURITY POLICY pol1  
    ALTER FILTER PREDICATE schema_preds.SecPredicate2(column1)  
        ON myschema.mytable;  
```  
  
### <a name="e-changing-a-block-predicate"></a>E. Cambiar un predicado de bloqueo  
Cambiar la función de predicado de bloqueo por una operación en una tabla.  
  
```sql 
ALTER SECURITY POLICY rls.SecPol  
    ALTER BLOCK PREDICATE rls.tenantAccessPredicate_v2(TenantId) 
    ON dbo.Sales AFTER INSERT;  
```  
  
## <a name="see-also"></a>Consulte también  
[Seguridad de nivel de fila](../../relational-databases/security/row-level-security.md)   
[CREATE SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/create-security-policy-transact-sql.md)   
[DROP SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-security-policy-transact-sql.md)   
[sys.security_policies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-security-policies-transact-sql.md)   
[sys.security_predicates &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-security-predicates-transact-sql.md)  
  
  
