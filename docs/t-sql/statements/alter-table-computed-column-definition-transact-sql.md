---
description: ALTER TABLE computed_column_definition (Transact-SQL)
title: computed_column_definition (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/05/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER TABLE statement
ms.assetid: 746eabda-3b4f-4940-b0b5-1c379f5cf7a5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: bee0490c99746365998caa1723c6ccf53377dc94
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185958"
---
# <a name="alter-table-computed_column_definition-transact-sql"></a>ALTER TABLE computed_column_definition (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Especifica las propiedades de una columna calculada que se agrega a una tabla por medio de [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
column_name AS computed_column_expression  
[ PERSISTED [ NOT NULL ] ]  
[   
    [ CONSTRAINT constraint_name ]  
    { PRIMARY KEY | UNIQUE }  
        [ CLUSTERED | NONCLUSTERED ]  
        [ WITH FILLFACTOR = fillfactor ]  
        [ WITH ( <index_option> [, ...n ] ) ]  
        [ ON { partition_scheme_name ( partition_column_name ) | filegroup   
            | "default" } ]  
    | [ FOREIGN KEY ]   
        REFERENCES ref_table [ ( ref_column ) ]   
        [ ON DELETE { NO ACTION | CASCADE } ]   
        [ ON UPDATE { NO ACTION } ]   
        [ NOT FOR REPLICATION ]   
    | CHECK [ NOT FOR REPLICATION ] ( logical_expression )  
]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*column_name*  
 Es el nombre de la columna que se va a modificar, agregar o quitar. *column_name* puede tener entre 1 y 128 caracteres. Si se trata de columnas nuevas, *column_name* se puede omitir para las columnas creadas con un tipo de datos **timestamp**. Si no se especifica el argumento *column_name* en una columna con un tipo de datos **timestamp**, se usa el nombre **timestamp**.  
  
*computed_column_expression*  
 Es una expresión que define el valor de una columna calculada. Una columna calculada es una columna virtual que no está almacenada físicamente en la tabla, sino que se calcula a partir de una expresión que utiliza otras columnas de la misma tabla. Una expresión debe proporcionar un valor. Por ejemplo, una columna calculada podría tener la definición: cost AS price * qty. Otro ejemplo con operadores bit a bit: is_finalised COMO is_checked | is_approved. La expresión puede ser un nombre de columna no calculada, una constante, una función, una variable o cualquier combinación de estos elementos conectados mediante uno o más operadores. La expresión no puede ser una condición de búsqueda, una subconsulta ni incluir un tipo de datos alias.  
  
 Las columnas calculadas se pueden utilizar en listas de selección, cláusulas WHERE, cláusulas ORDER BY o cualquier otra ubicación donde se puedan emplear expresiones regulares, pero con las siguientes excepciones:  
  
-   Una columna calculada no puede utilizarse como definición de restricción DEFAULT o FOREIGN KEY ni como NOT NULL. No obstante, si el valor de columna calculada lo define una expresión determinista y se permite el tipo de datos del resultado en columnas de índice, se puede utilizar una columna calculada como columna de clave en un índice o como parte de cualquier restricción PRIMARY KEY o UNIQUE.  
  
     Por ejemplo, si la tabla tiene las columnas de tipo entero a y b, la columna calculada a + b se puede indizar, pero la columna calculada a + DATEPART(dd, GETDATE()) no, porque el valor podría variar en llamadas posteriores.  
  
-   Una columna calculada no puede ser el destino de una instrucción INSERT o UPDATE.  
  
    > [!NOTE]  
    >  Debido a que cada fila de una tabla puede tener distintos valores para las columnas implicadas en una columna calculada, esta columna puede no tener el mismo resultado en cada fila.  
  
PERSISTED  
 Especifica que [!INCLUDE[ssDE](../../includes/ssde-md.md)] almacena físicamente los valores calculados en la tabla y actualiza los valores cuando se actualizan las columnas de las que depende la columna calculada. Al marcar una columna calculada como PERSISTED, se permite la creación de un índice en una columna calculada que sea determinista, pero no precisa. Para obtener más información, vea [Indexes on Computed Columns](../../relational-databases/indexes/indexes-on-computed-columns.md). Las columnas calculadas que se usan como columnas de partición de una tabla con particiones deben marcarse explícitamente como PERSISTED. *computed_column_expression* debe ser determinista cuando se especifique PERSISTED. 

NULL | NOT NULL  
 Especifica si se permiten valores NULL en la columna. NULL no es exactamente una restricción, pero puede especificarse como NOT NULL. NOT NULL solo puede especificarse para las columnas si también se especifica PERSISTED.  
  
CONSTRAINT  
 Especifica el inicio de la definición de una restricción PRIMARY KEY o UNIQUE.  
  
*constraint_name*  
 Es la nueva restricción. Los nombres de restricción deben seguir las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md), excepto en que el nombre no puede empezar por un signo de número (#). Si *constraint_name* no se proporciona, se asigna a la restricción un nombre generado por el sistema.  
  
PRIMARY KEY  
 Es una restricción que aplica la integridad de entidad para una columna o columnas especificadas mediante un índice único. Solo se puede crear una restricción PRIMARY KEY por cada tabla.  
  
UNIQUE  
 Es una restricción que proporciona integridad de entidad para una columna o columnas específicas utilizando un índice exclusivo.  
  
CLUSTERED | NONCLUSTERED  
 Especifica que se ha creado un índice clúster o no clúster para la restricción PRIMARY KEY o UNIQUE. Las restricciones PRIMARY KEY tienen el valor predeterminado CLUSTERED. Las restricciones UNIQUE tienen el valor predeterminado NONCLUSTERED.  
  
 Si en una tabla ya existe una restricción o índice agrupado, no se puede especificar CLUSTERED. Si en una tabla ya existe una restricción o índice agrupado, el valor predeterminado de la restricción PRIMARY KEY es NONCLUSTERED.  
  
WITH FILLFACTOR =*fillfactor*  
 Especifica cuánto se debe llenar cada página de índice del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] utilizada para almacenar los datos de índice. Los valores de *fillfactor* especificados por el usuario pueden estar comprendidos entre 1 y 100. Si no se especifica un valor, el valor predeterminado es 0.  
  
> [!IMPORTANT]  
>  La documentación de WITH FILLFACTOR = *fillfactor* como la única opción de índice que se aplica a las restricciones PRIMARY KEY o UNIQUE se mantiene por compatibilidad con versiones anteriores, pero no se documentará de esta forma en futuras versiones. Es posible especificar otras opciones de índice en la cláusula [index_option &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-index-option-transact-sql.md) de ALTER TABLE.  
  
FOREIGN KEY REFERENCES  
 Es una restricción que proporciona integridad referencial para los datos de la columna o columnas. Las restricciones FOREIGN KEY requieren que cada valor de la columna exista en la columna o columnas de referencia correspondientes de la tabla a la que se hace referencia. Las restricciones FOREIGN KEY pueden hacer referencia solo a columnas que sean restricciones PRIMARY KEY o UNIQUE en la tabla de referencia o a columnas a las que se haga referencia en UNIQUE INDEX en la tabla de referencia. Las claves externas en las columnas calculadas deben marcarse también como PERSISTED.  
  
*ref_table*  
 Es el nombre de la tabla a la que hace referencia la restricción FOREIGN KEY.  
  
(*ref_column*)  
 Es una columna de la tabla a la que hace referencia la restricción FOREIGN KEY.  
  
ON DELETE { **NO ACTION** | CASCADE }  
 Especifica la acción que se produce en las filas de la tabla, si esas filas tienen una relación referencial y la fila a la que se hace referencia se elimina de la tabla principal. El valor predeterminado es NO ACTION.  
  
NO ACTION  
 El [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera un error y se revierte la acción de eliminación de la fila de la tabla primaria.

CASCADE  
 Si esa fila se elimina de la tabla primaria, las filas correspondientes se eliminan de la tabla de referencia.  
  
 Por ejemplo, en la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], la tabla ProductVendor tiene una relación de referencia con la tabla Vendor. La clave externa ProductVendor.BusinessEntityID hace referencia a la clave principal Vendor.BusinessEntityID.  
  
 Si se ejecuta una instrucción DELETE en una fila de la tabla Vendor y se especifica una acción ON DELETE CASCADE para ProductVendor.BusinessEntityID, [!INCLUDE[ssDE](../../includes/ssde-md.md)] comprueba si hay alguna fila dependiente en la tabla ProductVendor. Si las hay, se eliminan tanto las filas dependientes de la tabla ProductVendor como la fila a la que se hace referencia en la tabla Vendor.  
  
 Por el contrario, si se especifica NO ACTION, [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera un error y revierte la acción de eliminación de la fila Vendor si al menos hay una fila en la tabla ProductVendor que haga referencia a dicha fila.  
  
 No especifique CASCADE si la tabla se va a incluir en una publicación de combinación que utiliza registros lógicos. Para obtener más información sobre los registros lógicos, vea [Agrupar cambios en filas relacionadas con registros lógicos](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).  
  
ON UPDATE { **NO ACTION** }  
 Especifica la acción que se produce en las filas de la tabla creada, si esas filas tienen una relación referencial y la fila a la que se hace referencia se actualiza en la tabla principal. Cuando se especifica NO ACTION, [!INCLUDE[ssDE](../../includes/ssde-md.md)] emite un error y revierte la acción de actualización de la fila Vendor si existe al menos una fila de la tabla ProductVendor que hace referencia a ella.  
  
NOT FOR REPLICATION  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Se puede especificar para restricciones FOREIGN KEY y CHECK. Si se especifica esta cláusula para una restricción, la restricción no se aplica cuando los agentes de replicación realizan operaciones de inserción, actualización o eliminación.  
  
CHECK  
 Es una restricción que exige la integridad del dominio al limitar los valores posibles que se pueden escribir en una o varias columnas. Las restricciones CHECK en las columnas calculadas deben marcarse también como PERSISTED.  
  
*logical_expression*  
 Es una expresión lógica que devuelve TRUE o FALSE. La expresión no puede ser una referencia ni incluir un tipo de datos alias.  
  
ON { *partition_scheme_name*(*partition_column_name*) | *filegroup*| "default"}  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Especifica la ubicación de almacenamiento del índice creado para la restricción. Si se especifica *partition_scheme_name*, el índice se divide en particiones y las particiones se asignan a los grupos de archivos que se han especificado con *partition_scheme_name*. Si se especifica *filegroup*, el índice se crea en el grupo de archivos indicado. Si se especifica el parámetro "default" o si no se especifica ON en ningún caso, el índice se crea en el mismo grupo de archivos que la tabla. Si se especifica ON al agregar un índice clúster para una restricción PRIMARY KEY o UNIQUE, la tabla completa se mueve al grupo de archivos especificado cuando se crea el índice clúster.  
  
> [!NOTE]  
>  En este contexto, el valor predeterminado no es una palabra clave. Es un identificador para el grupo de archivos predeterminado y debe delimitarse, como en ON"default" o en ON [default]. Si se especifica "default", la opción QUOTED_IDENTIFIER debe tener el valor ON para la sesión actual. Esta es la configuración predeterminada. Para obtener más información, vea [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
## <a name="remarks"></a>Observaciones  
 Cada restricción PRIMARY KEY y UNIQUE genera un índice. El número de restricciones UNIQUE y PRIMARY KEY no puede hacer que el número de índices de la tabla supere 999 índices no clúster y 1 índice clúster.  
  
## <a name="see-also"></a>Consulte también  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
