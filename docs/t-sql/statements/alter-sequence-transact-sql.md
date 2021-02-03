---
description: ALTER SEQUENCE (Transact-SQL)
title: ALTER SEQUENCE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/08/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_SEQUENCE_TSQL
- ALTER SEQUENCE
dev_langs:
- TSQL
helpviewer_keywords:
- sequence number object, altering
- ALTER SEQUENCE statement
ms.assetid: decc0760-029e-4baf-96c9-4a64073df1c2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 44baafa6b239a2eff1a62d6c5194535d9a87ecbd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182215"
---
# <a name="alter-sequence-transact-sql"></a>ALTER SEQUENCE (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Modifica los argumentos de un objeto de secuencia existente. Si la secuencia se creara con la opción **CACHE**, al modificar la secuencia se volverá a crear la memoria caché.  
  
 Los objetos de secuencia se crean con la instrucción [CREATE SEQUENCE](../../t-sql/statements/create-sequence-transact-sql.md). Las secuencias son valores enteros y pueden ser de cualquier tipo de datos que devuelva un entero. El tipo de datos no se puede cambiar mediante la instrucción ALTER SEQUENCE. Para cambiar el tipo de datos, quite y cree el objeto de secuencia.  
  
 Una secuencia es un objeto enlazado a un esquema definido por el usuario que genera una secuencia de valores numéricos según una especificación. Se generan nuevos valores de una secuencia llamando a la función NEXT VALUE FOR. Use **sp_sequence_get_range** para obtener varios números de secuencia a la vez. Para obtener información y escenarios en los que se usa CREATE SEQUENCE, **sp_sequence_get_range** y la función NEXT VALUE FOR, vea [Números de secuencia](../../relational-databases/sequence-numbers/sequence-numbers.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
ALTER SEQUENCE [schema_name. ] sequence_name  
    [ RESTART [ WITH <constant> ] ]  
    [ INCREMENT BY <constant> ]  
    [ { MINVALUE <constant> } | { NO MINVALUE } ]  
    [ { MAXVALUE <constant> } | { NO MAXVALUE } ]  
    [ CYCLE | { NO CYCLE } ]  
    [ { CACHE [ <constant> ] } | { NO CACHE } ]  
    [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *sequence_name*  
 Especifica el nombre exclusivo por el que se conoce la secuencia en la base de datos. El tipo es **sysname**.  
  
 RESTART [ WITH \<constant> ]  
 El valor siguiente que el objeto de secuencia devolverá. Si se proporciona, el valor RESTART WITH debe ser un entero menor o igual que el valor máximo, y mayor o igual que el valor mínimo del objeto de secuencia. Si se omite el valor WITH, la numeración de la secuencia se reinicia basándose en las opciones de CREATE SEQUENCE iniciales.  
  
 INCREMENT BY \<constant>  
 El valor que se usa para aumentar (o disminuir si es negativo) el valor base del objeto de secuencia para cada llamada a la función NEXT VALUE FOR. Si el incremento es un valor negativo el objeto de secuencia es descendente, de lo contrario, es ascendente. El incremento no puede ser 0.  
  
 [ MINVALUE \<constant> | NO MINVALUE ]  
 Especifica los límites del objeto de secuencia. Si se especifica NO MINVALUE, se usa el valor mínimo posible del tipo de datos de la secuencia.  
  
 [ MAXVALUE \<constant> | NO MAXVALUE  
 Especifica los límites del objeto de secuencia. Si se especifica NO MAXVALUE, se usa el valor máximo posible del tipo de datos de la secuencia.  
  
 [ CYCLE | NO CYCLE ]  
 Esta propiedad especifica si el objeto de secuencia se debe reiniciar desde el valor mínimo (o el valor máximo para los objetos de secuencia descendente) o se debe producir una excepción cuando se supera el valor mínimo o máximo.  
  
> [!NOTE]  
>  Después del recorrido, el valor siguiente es el valor mínimo o máximo, no el valor START VALUE de la secuencia.  
  
 [ CACHE [\<constant> ] | NO CACHE ]  
 Aumenta el rendimiento de las aplicaciones que usan los objetos de secuencia minimizando el número de operaciones de E/S necesarias para conservar los valores generados en las tablas del sistema.  
  
 Para obtener más información sobre el comportamiento de la memoria caché, vea [CREATE SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-sequence-transact-sql.md).  
  
## <a name="remarks"></a>Observaciones  
 Para obtener información sobre cómo se crean las secuencias y cómo se administra la memoria caché de secuencias, vea [CREATE SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-sequence-transact-sql.md).  
  
 Los valores MINVALUE de las secuencias ascendentes y MAXVALUE de las secuencias descendentes no se pueden modificar con un valor que no permite el valor START WITH de la secuencia. Para cambiar el valor MINVALUE de una secuencia ascendente a un número mayor que el valor START WITH o cambiar el valor MAXVALUE de una secuencia descendente a un número menor que el valor START WITH, incluya el argumento RESTART WITH para reiniciar la secuencia en un punto deseado que esté dentro del rango mínimo y máximo.  
  
## <a name="metadata"></a>Metadatos  
 Para obtener información sobre las secuencias, consulte [sys.sequences](../../relational-databases/system-catalog-views/sys-sequences-transact-sql.md).  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 Requiere permiso **ALTER** en la secuencia o permiso **ALTER** en el esquema. Para otorgar el permiso **ALTER** en la secuencia, use **ALTER ON OBJECT** en el siguiente formato:  
  
```sql  
GRANT ALTER ON OBJECT::Test.TinySeq TO [AdventureWorks\Larry]  
```  
  
 La propiedad de un objeto de secuencia se puede transferir mediante la instrucción **ALTER AUTHORIZATION**.  
  
### <a name="audit"></a>Auditoría  
 Para auditar **ALTER SEQUENCE**, supervise el **SCHEMA_OBJECT_CHANGE_GROUP**.  
  
## <a name="examples"></a>Ejemplos  
 Para ver ejemplos de cómo crear secuencias y cómo usar la función **NEXT VALUE FOR** para generar números de secuencia, vea [Números de secuencia](../../relational-databases/sequence-numbers/sequence-numbers.md).  
  
### <a name="a-altering-a-sequence"></a>A. Modificar una secuencia  
 En el siguiente ejemplo se crea un esquema denominado Test y una secuencia denominada TestSeq usando el tipo de datos **int**, con un rango de 100 a 200. La secuencia se inicia con 125 y se incrementa en 25 cada vez que se genera un número. Dado que la secuencia está configurada para recorrerla en ciclo, cuando el valor supera el valor máximo de 200, se reinicia en el valor mínimo de 100.  
  
```sql  
CREATE SCHEMA Test ;  
GO  
  
CREATE SEQUENCE Test.TestSeq  
    AS int   
    START WITH 125  
    INCREMENT BY 25  
    MINVALUE 100  
    MAXVALUE 200  
    CYCLE  
    CACHE 3  
;  
GO  
```  
  
 En el siguiente ejemplo se modifica la secuencia TestSeq para que tenga un rango de 50 a 200. La secuencia reinicia la serie de la numeración con 100 y se incrementa en 50 cada vez que se genera un número.  
  
```sql  
ALTER SEQUENCE Test. TestSeq  
    RESTART WITH 100  
    INCREMENT BY 50  
    MINVALUE 50  
    MAXVALUE 200  
    NO CYCLE  
    NO CACHE  
;  
GO  
```  
  
 Dado que la secuencia no se recorrerá en ciclo, la función **NEXT VALUE FOR** producirá un error cuando la secuencia supere el valor 200.  
  
### <a name="b-restarting-a-sequence"></a>B. Reiniciar una secuencia  
 En el siguiente ejemplo se crea una secuencia llamada CountBy1. La secuencia usa los valores predeterminados.  
  
```sql  
CREATE SEQUENCE Test.CountBy1 ;  
```  
  
 Para generar un valor de secuencia, el propietario ejecuta la siguiente instrucción:  
  
```sql  
SELECT NEXT VALUE FOR Test.CountBy1  
```  
  
 El valor devuelto de -9.223.372.036.854.775.808 es el más bajo posible para el tipo de datos **bigint**. El propietario se da cuenta de que deseaba que la secuencia comenzase con 1, pero no indicó la cláusula **START WITH** al crear la secuencia. Para corregir este error, el propietario ejecuta la siguiente instrucción.  
  
```sql  
ALTER SEQUENCE Test.CountBy1 RESTART WITH 1 ;  
```  
  
 A continuación, el propietario ejecuta de nuevo la siguiente instrucción para generar un número de secuencia.  
  
```sql  
SELECT NEXT VALUE FOR Test.CountBy1;  
```  
  
 El número es ahora 1, tal y como se esperaba.  
  
 La secuencia CountBy1 se creó usando el valor predeterminado de NO CYCLE de modo que dejara de funcionar después de generar el número 9.223.372.036.854.775.807. Las llamadas subsiguientes al objeto de secuencia devolverán el error 11728. La siguiente instrucción cambia el objeto de secuencia para que se recorra en ciclo y establece una memoria caché de 20.  
  
```sql  
ALTER SEQUENCE Test.CountBy1  
    CYCLE  
    CACHE 20 ; 
```  
  
 Ahora, cuando el objeto de secuencia alcance 9.223.372.036.854.775.807 se recorrerá en ciclo y el número siguiente al recorrido del ciclo será el mínimo del tipo de datos, -9.223.372.036.854.775.808.  
  
 El propietario se da cuenta de que el tipo de datos **bigint** usa 8 bytes cada vez que se utiliza. El tipo de datos **int** que usa 4 bytes es suficiente. Sin embargo, no se puede modificar el tipo de datos de un objeto de secuencia. Para cambiar a un tipo de datos **int**, el propietario debe quitar el objeto de secuencia y volver a crear el objeto con el tipo de datos correcto.  
  
## <a name="see-also"></a>Consulte también  
 [CREATE SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-sequence-transact-sql.md)   
 [DROP SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-sequence-transact-sql.md)   
 [NEXT VALUE FOR &#40;Transact-SQL&#41;](../../t-sql/functions/next-value-for-transact-sql.md)   
 [Números de secuencia](../../relational-databases/sequence-numbers/sequence-numbers.md)   
 [sp_sequence_get_range &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-sequence-get-range-transact-sql.md)  
  
  
