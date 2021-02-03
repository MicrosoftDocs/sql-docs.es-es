---
description: CREATE SEQUENCE (Transact-SQL)
title: CREATE SEQUENCE (Transact-SQL)
ms.custom: ''
ms.date: 04/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SEQUENCE
- CREATE SEQUENCE
- SEQUENCE_TSQL
- CREATE_SEQUENCE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE SEQUENCE statement
- sequence number object, creating
- sequence object
- number, sequence
ms.assetid: 419f907b-8a72-4d6c-80cb-301df44c24c1
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0fb687f42e65bb1f1ec74046927282dc90563ccd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188496"
---
# <a name="create-sequence-transact-sql"></a>CREATE SEQUENCE (Transact-SQL)

[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Crea un objeto de secuencia y especifica sus propiedades. Una secuencia es un objeto enlazado a un esquema definido por el usuario que genera una secuencia de valores numéricos según la especificación con la que se creó la secuencia. La secuencia de valores numéricos se genera en orden ascendente o descendente en un intervalo definido y se puede configurar para reiniciarse (en un ciclo) cuando se agota. Las secuencias, a diferencia de las columnas de identidad, no se asocian a tablas concretas. Las aplicaciones hacen referencia a un objeto de secuencia para recuperar su valor siguiente. La aplicación controla la relación entre las secuencias y tablas. Las aplicaciones de usuario pueden hacer referencia un objeto de secuencia y coordinar los valores a través de varias filas y tablas.  
  
 A diferencia de los valores de columnas de identidad que se generan cuando se insertan filas, una aplicación puede obtener el número de secuencia siguiente sin insertar la fila llamando a la función [NEXT VALUE FOR](../../t-sql/functions/next-value-for-transact-sql.md). Use [sp_sequence_get_range](../../relational-databases/system-stored-procedures/sp-sequence-get-range-transact-sql.md) para obtener varios números de secuencia a la vez.  
  
 Para obtener información sobre las funciones **CREATE SEQUENCE** y **NEXT VALUE FOR** y escenarios en los que se usan, vea [Números de secuencia](../../relational-databases/sequence-numbers/sequence-numbers.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CREATE SEQUENCE [schema_name . ] sequence_name  
    [ AS [ built_in_integer_type | user-defined_integer_type ] ]  
    [ START WITH <constant> ]  
    [ INCREMENT BY <constant> ]  
    [ { MINVALUE [ <constant> ] } | { NO MINVALUE } ]  
    [ { MAXVALUE [ <constant> ] } | { NO MAXVALUE } ]  
    [ CYCLE | { NO CYCLE } ]  
    [ { CACHE [ <constant> ] } | { NO CACHE } ]  
    [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*sequence_name*  
Especifica el nombre exclusivo por el que se conoce la secuencia en la base de datos. El tipo es **sysname**.  
  
[ built_in_integer_type | user-defined_integer_type  
Una secuencia se puede definir como de cualquier tipo entero. Están permitidos los siguientes tipos  
  
-   **tinyint**: abarca de 0 a 255  
-   **smallint**: abarca de -32 768 a 32 767  
-   **int**: abarca de -2 147 483 648 a 2 147 483 647  
-   **bigint**: abarca de -9 223 372 036 854 775 808 a 9 223 372 036 854 775 807  
-   **decimal** y **numeric** con una escala de 0.  
-   Cualquier tipo de datos definido por el usuario (tipo de alias) que esté basado en uno de los tipos permitidos.  
  
Si no se proporciona ningún tipo de datos, el tipo de datos **bigint** se usa como valor predeterminado.  
  
START WITH \<constant>  
Primer valor devuelto por el objeto de secuencia. El valor **START** debe ser menor o igual que el máximo, y mayor o igual que el valor mínimo del objeto de secuencia. El valor inicial predeterminado para un nuevo objeto de secuencia es el valor mínimo para un objeto de secuencia ascendente y el valor máximo para uno descendente.  
  
INCREMENT BY \<constant>  
Valor usado para incrementar (o disminuir si es negativo) el valor del objeto de secuencia para cada llamada a la función **NEXT VALUE FOR**. Si el incremento es un valor negativo, el objeto de secuencia es descendente; de lo contrario, es ascendente. El incremento no puede ser 0. El incremento predeterminado para un nuevo objeto de secuencia es 1.  
  
[ MINVALUE \<constant> | **NO MINVALUE** ]  
Especifica los límites del objeto de secuencia. El valor mínimo predeterminado para un nuevo objeto de secuencia es el valor mínimo del tipo de datos del objeto de secuencia. Es cero para el tipo de datos **tinyint** y un número negativo para todos los demás.  
  
[ MAXVALUE \<constant> | **NO MAXVALUE**  
Especifica los límites del objeto de secuencia. El valor máximo predeterminado para un nuevo objeto de secuencia es el valor máximo del tipo de datos del objeto de secuencia.  
  
[ CYCLE | **NO CYCLE** ]  
Propiedad especifica si el objeto de secuencia se debería reiniciar desde el valor mínimo (o el máximo para los objetos de secuencia descendente) o producir una excepción cuando se supera el valor mínimo o máximo. La opción de ciclo predeterminado para los nuevos objetos de secuencia es NO CYCLE.  
  
> [!NOTE]
> El ciclo de SEQUENCE se reinicia a partir del valor mínimo o máximo, no a partir del valor inicial.  
  
[ **CACHE** [\<constant> ] | NO CACHE ]  
Aumenta el rendimiento de las aplicaciones que utilizan objetos de secuencia al reducir el número de E/S de disco que se necesitan para generar números de secuencia. El valor predeterminado es CACHÉ.  
  
Por ejemplo, si se elige un tamaño de caché de 50, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no mantiene 50 valores individuales almacenados en memoria caché. Solo almacena en memoria caché el valor actual y el número de valores que se quedan en la memoria caché. Esto significa que la cantidad de memoria necesaria para almacenar la memoria caché siempre es dos veces la del tipo de datos del objeto de secuencia.  
  
> [!NOTE]  
> Si la opción de memoria caché se habilita sin especificar un tamaño de caché, el Motor de base de datos seleccionará un tamaño. Sin embargo, los usuarios no deben confiar en que la selección sea coherente. [!INCLUDE[msCoName](../../includes/msconame-md.md)] puede cambiar el método de cálculo del tamaño de la memoria caché sin previo aviso.  
  
Cuando se crean con la opción **CACHE**, un apagado inesperado (como un corte de suministro eléctrico) puede provocar la pérdida de los números de secuencia que permanecen en la memoria caché.  
  
## <a name="general-remarks"></a>Notas generales  
 Los números de secuencia se generan fuera del ámbito de la transacción actual. Se utilizan tanto si la transacción que usa el número de secuencia se confirma como si se revierte. La validación de duplicados solo se produce una vez que un registro está totalmente relleno. Esto puede dar lugar a casos en que se use el mismo número para más de un registro durante la creación, pero luego se identifique como un duplicado. Si ocurre esto y se han aplicado otros valores autonuméricos a registros posteriores, esto puede dar lugar a una discrepancia entre los valores autonuméricos y su comportamiento esperado.
  
### <a name="cache-management"></a>Administración de la memoria caché  
 Para mejorar el rendimiento, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] asigna con antelación el número de números de secuencia especificados por el argumento **CACHE**.  
  
 Por ejemplo, imagine que se crea una secuencia nueva con el valor inicial 1 y 15 como tamaño de caché. Cuando se necesita el primer valor, están disponibles los valores 1 a 15 de la memoria. El último valor almacenado en memoria caché (15) se escribe en las tablas del sistema del disco. Cuando se utilizan los 15 números, la solicitud siguiente (la del número 16) hará que la memoria caché sea asignada de nuevo. El nuevo último valor almacenado en memoria caché (30) se escribirá en las tablas del sistema.  
  
 Si el [!INCLUDE[ssDE](../../includes/ssde-md.md)] se detiene después de utilizar 22 números, el siguiente número de secuencia en memoria (23) se escribe en las tablas del sistema, reemplazando el número almacenado previamente.  
  
 Una vez que SQL Server se reinicia y se necesita un número de secuencia, el número inicial se lee en las tablas del sistema (23). La cantidad de memoria caché de 15 números (23-38) se asigna a la memoria y el siguiente número que no está en memoria caché (39) se escribe en las tablas del sistema.  
  
 Si el [!INCLUDE[ssDE](../../includes/ssde-md.md)] se detiene de modo anómalo por un suceso como un error de alimentación, la secuencia se reinicia con el número leído de las tablas del sistema (39). Se pierden los números de secuencia asignados a la memoria (que nunca fueran solicitados por un usuario o aplicación). Esta característica puede dejar huecos, pero garantiza que nunca se emita el mismo valor dos veces para un solo objeto de secuencia, a menos que se defina como **CYCLE** o se reinicie manualmente.  
  
 La memoria caché se mantiene en memoria realizando el seguimiento del valor actual (el último valor emitido) y el número de valores que se dejan en la memoria caché. Por consiguiente, la cantidad de memoria que la memoria caché usa siempre es dos veces la del tipo de datos del objeto de secuencia.  
  
 Al establecer el argumento de la memoria caché en **CYCLE**, el valor de la secuencia actual se escribe en las tablas del sistema cada vez que se usa una secuencia. Esto puede ralentizar el rendimiento al aumentar el acceso al disco, pero reduce la posibilidad de que se produzcan huecos imprevistos. Es posible que se sigan produciendo huecos si los números se solicitan con las funciones **NEXT VALUE FOR** o **sp_sequence_get_range**, pero entonces los números no se usan o se usan en transacciones no confirmadas.  
  
 Cuando un objeto de secuencia usa la opción **CACHE**, si reinicia el objeto de secuencia o altera las propiedades del tamaño de caché, **INCREMENT**, **CYCLE**, **MINVALUE** o **MAXVALUE**, ello hará que la memoria caché se escriba en las tablas del sistema antes de que el cambio se produzca. A continuación, la memoria caché vuelve a cargarse comenzando con el valor actual (es decir, no se salta ningún número). El cambio realizado en el tamaño de la memoria caché surte efecto de forma inmediata.  
  
 **Opción CACHE cuando hay disponibles valores almacenados en memoria caché**  
  
 El siguiente proceso se produce cada vez que se solicita un objeto de secuencia para generar el valor siguiente para la opción **CACHE** si hay disponibles valores sin usar en la memoria caché en memoria para el objeto de secuencia.  
  
1.  Se calcula el valor siguiente para el objeto de secuencia.  
  
2.  El nuevo valor actual para el objeto de secuencia se actualiza en la memoria.  
  
3.  El valor calculado se devuelve a la instrucción que realiza la llamada.  
  
**Opción CACHE cuando se agota la memoria caché**  
  
 Cada vez que se solicita un objeto de secuencia para generar el valor siguiente para la opción **CACHE**, si se ha agotado la memoria caché, tiene lugar el siguiente proceso:  
  
1.  Se calcula el valor siguiente para el objeto de secuencia.  
  
2.  Se calcula el último valor para la nueva memoria caché.  
  
3.  Se bloquea la fila de la tabla del sistema para el objeto de secuencia y el valor calculado en el paso 2 (el último valor) se escribe en la tabla del sistema. Se activa un xevent agotado en caché para notificar al usuario el nuevo valor conservado.  
  
**Opción NO CACHE**  
  
 Cada vez que se solicita que un objeto de secuencia genere el valor siguiente para la opción **NO CACHE**, tiene lugar el siguiente proceso:  
  
1.  Se calcula el valor siguiente para el objeto de secuencia.  
  
2.  El nuevo valor actual para el objeto de secuencia se escribe en la tabla del sistema.  
  
3.  El valor calculado se devuelve a la instrucción que realiza la llamada.  
  
## <a name="metadata"></a>Metadatos  
 Para obtener información sobre las secuencias, consulte [sys.sequences](../../relational-databases/system-catalog-views/sys-sequences-transact-sql.md).  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 Requiere el permiso **CREATE SEQUENCE**, **ALTER** o **CONTROL** en el SCHEMA.  
  
-   Los miembros de los roles fijos de base de datos db_owner y db_ddladmin pueden crear, alterar y quitar los objetos de secuencia.  
  
-   Los miembros de los roles fijos de base de datos db_owner y db_datawriter pueden actualizar los objetos de secuencia haciendo que generen los números.  
  
 En este ejemplo se concede al usuario el permiso AdventureWorks\Larry para crear secuencias en el esquema Test.  
  
```sql  
GRANT CREATE SEQUENCE ON SCHEMA::Test TO [AdventureWorks\Larry]  
```  
  
 La propiedad de un objeto de secuencia se puede transferir usando la instrucción **ALTER AUTHORIZATION**.  
  
 Si una secuencia utiliza un tipo de datos definido por el usuario, el creador de la secuencia debe tener el permiso REFERENCES en el tipo.  
  
### <a name="audit"></a>Auditoría  
 Para auditar **CREATE SEQUENCE**, supervise el **SCHEMA_OBJECT_CHANGE_GROUP**.  
  
## <a name="examples"></a>Ejemplos  
 Para obtener ejemplos de cómo crear secuencias y cómo usar la función **NEXT VALUE FOR** para generar los números de secuencia, vea [Números de secuencia](../../relational-databases/sequence-numbers/sequence-numbers.md).  
  
 En la mayoría de estos ejemplos se crean objetos de secuencia en un esquema denominado Test.  
  
 Para crear el esquema Test, ejecute la siguiente instrucción.  
  
```sql  
CREATE SCHEMA Test ;  
GO  
```  
  
### <a name="a-creating-a-sequence-that-increases-by-1"></a>A. Crear una secuencia que se incremente en 1  
 En este ejemplo, Thierry crea una secuencia denominada CountBy1 que se incrementa en uno cada vez que se usa.  
  
```sql  
CREATE SEQUENCE Test.CountBy1  
    START WITH 1  
    INCREMENT BY 1 ;  
GO  
```  
  
### <a name="b-creating-a-sequence-that-decreases-by-1"></a>B. Crear una secuencia que se disminuya en 1  
 El ejemplo siguiente comienza en 0 y resta uno cada vez que se utiliza.  
  
```sql  
CREATE SEQUENCE Test.CountByNeg1  
    START WITH 0  
    INCREMENT BY -1 ;  
GO  
```  
  
### <a name="c-creating-a-sequence-that-increases-by-5"></a>C. Crear una secuencia que se incremente en 5  
 El siguiente ejemplo crea una secuencia que se incrementa en 5 cada vez que se utiliza.  
  
```sql  
CREATE SEQUENCE Test.CountBy1  
    START WITH 5  
    INCREMENT BY 5 ;  
GO  
```  
  
### <a name="d-creating-a-sequence-that-starts-with-a-designated-number"></a>D. Crear una secuencia que se inicia con un número designado  
 Después de importar una tabla, Thierry observa que el mayor número de identificación utilizado es 24.328. Thierry necesita una secuencia que generará números a partir de 24.329. El siguiente código crea una secuencia que comienza en 24.329 y se incrementa en 1.  
  
```sql  
CREATE SEQUENCE Test.ID_Seq  
    START WITH 24329  
    INCREMENT BY 1 ;  
GO  
```  
  
### <a name="e-creating-a-sequence-using-default-values"></a>E. Crear una secuencia utilizando los valores predeterminados  
 En el siguiente ejemplo se crea una secuencia mediante los valores predeterminados.  
  
```sql  
CREATE SEQUENCE Test.TestSequence ;  
```  
  
 Ejecute la siguiente instrucción para ver las propiedades de la secuencia.  
  
```sql  
SELECT * FROM sys.sequences WHERE name = 'TestSequence' ;  
```  
  
 Una lista parcial del resultado demuestra los valores predeterminados.  
  
| Output | Valor predeterminado|  
|-|-|  
|`start_value`|`-9223372036854775808`|  
|`increment`|`1`|  
|`mimimum_value`|`-9223372036854775808`|  
|`maximum_value`|`9223372036854775807`|  
|`is_cycling`|`0`|  
|`is_cached`|`1`|  
|`current_value`|`-9223372036854775808`|  
  
### <a name="f-creating-a-sequence-with-a-specific-data-type"></a>F. Crear una secuencia con un tipo de datos concreto

En este ejemplo se crea una secuencia usando el tipo de datos **smallint**, que abarca de -32 768 a 32 767.  
  
```sql  
CREATE SEQUENCE SmallSeq 
    AS smallint ;  
```  
  
### <a name="g-creating-a-sequence-using-all-arguments"></a>G. Crear una secuencia utilizando todos los argumentos  
 En este ejemplo se crea una secuencia denominada DecSeq mediante el tipo de datos **decimal**, que abarca de 0 a 255. La secuencia se inicia con 125 y se incrementa en 25 cada vez que se genera un número. Dado que la secuencia se configura para recorrer en ciclo cuando el valor supera el valor máximo de 200, la secuencia se reinicia en el valor mínimo de 100.  
  
```sql  
CREATE SEQUENCE Test.DecSeq  
    AS decimal(3,0)   
    START WITH 125  
    INCREMENT BY 25  
    MINVALUE 100  
    MAXVALUE 200  
    CYCLE  
    CACHE 3  
;  
```  
  
 Ejecute la siguiente instrucción para ver el primer valor; la opción `START WITH` 125.  
  
```sql  
SELECT NEXT VALUE FOR Test.DecSeq;  
```  
  
 Ejecute la instrucción tres veces más para devolver 150, 175 y 200.  
  
 Ejecute la instrucción para ver de nuevo cómo vuelve el valor inicial a la opción `MINVALUE` 100.  
  
 Ejecute el siguiente código para confirmar el tamaño de caché y ver el valor actual.  
  
```sql  
SELECT cache_size, current_value   
FROM sys.sequences  
WHERE name = 'DecSeq' ;  
```  
  
## <a name="see-also"></a>Consulte también  
 [ALTER SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-sequence-transact-sql.md)   
 [DROP SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-sequence-transact-sql.md)   
 [NEXT VALUE FOR &#40;Transact-SQL&#41;](../../t-sql/functions/next-value-for-transact-sql.md)   
 [Números de secuencia](../../relational-databases/sequence-numbers/sequence-numbers.md)  
  
  
