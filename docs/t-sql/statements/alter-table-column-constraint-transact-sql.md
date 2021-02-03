---
description: ALTER TABLE column_constraint (Transact-SQL)
title: column_constraint (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/05/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- column_constraint
- column_constraint_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER TABLE statement
- constraints [SQL Server], properties
- constraints [SQL Server], definitions
- column_constraint
ms.assetid: 8119b7c7-e93b-4de5-8f71-c3b7c70b993c
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 085ac66991ba4e6bbc5778bbcbfd36cb77a7fa4c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172500"
---
# <a name="alter-table-column_constraint-transact-sql"></a>ALTER TABLE column_constraint (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Especifica las propiedades de una restricción PRIMARY KEY, FOREIGN KEY, UNIQUE o CHECK que forma parte de una nueva definición de columna agregada a una tabla mediante [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
[ CONSTRAINT constraint_name ]   
{   
    [ NULL | NOT NULL ]   
    { PRIMARY KEY | UNIQUE }   
        [ CLUSTERED | NONCLUSTERED ]   
        [ WITH FILLFACTOR = fillfactor ]   
        [ WITH ( index_option [, ...n ] ) ]  
        [ ON { partition_scheme_name (partition_column_name)   
            | filegroup | "default" } ]   
    | [ FOREIGN KEY ]   
        REFERENCES [ schema_name . ] referenced_table_name   
            [ ( ref_column ) ]   
        [ ON DELETE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]   
        [ ON UPDATE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]   
        [ NOT FOR REPLICATION ]   
    | CHECK [ NOT FOR REPLICATION ] ( logical_expression )  
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 CONSTRAINT  
 Especifica el inicio de la definición de una restricción PRIMARY KEY, UNIQUE, FOREIGN KEY o CHECK.  
  
 *constraint_name*  
 Es el nombre de la restricción. Los nombres de restricción deben seguir las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md), excepto en que el nombre no puede empezar por un signo de número (#). Si *constraint_name* no se proporciona, se asigna a la restricción un nombre generado por el sistema.  
  
 NULL | NOT NULL  
 Especifica si la columna puede aceptar valores NULL. Las columnas que no admiten valores NULL solo se pueden agregar si tienen especificado un valor predeterminado. Si la columna nueva admite valores NULL y no se especifica un valor predeterminado, la columna nueva contendrá un valor NULL en cada fila de la tabla. Si la nueva columna permite valores NULL y se agrega una definición predeterminada con la nueva columna, se puede usar la opción WITH VALUES para almacenar el valor predeterminado en la nueva columna para cada fila existente en la tabla.  
  
 Si la columna nueva no admite valores NULL, se deberá agregar una definición DEFAULT con la columna nueva. La columna nueva se carga automáticamente con el valor predeterminado en cada fila existente de las columnas nuevas.  
  
 Cuando la adición de una columna obliga a realizar cambios físicos en las filas de datos de una tabla, como agregar valores DEFAULT a cada fila, se mantienen bloqueos en la tabla mientras se ejecuta ALTER TABLE. Esto afecta a la capacidad de cambiar el contenido de la tabla mientras el bloqueo está activado. En cambio, agregar una columna que admite valores NULL y no especifica un valor predeterminado es solo una operación de metadatos y no activa ningún bloqueo.  
  
 Cuando se utiliza CREATE TABLE o ALTER TABLE, la configuración de la base de datos y de la sesión afecta a (y puede anular) la nulabilidad del tipo de datos que se utiliza en una definición de columna. Se recomienda que defina siempre explícitamente las columnas no calculadas como NULL o NOT NULL, o bien, si utiliza un tipo de datos definido por el usuario, que permita que la columna utilice la nulabilidad predeterminada del tipo de datos. Para obtener más información, vea [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).  
  
 PRIMARY KEY  
 Es una restricción que aplica la integridad de entidad para una columna o columnas especificadas mediante un índice único. Solo se puede crear una restricción PRIMARY KEY por cada tabla.  
  
 UNIQUE  
 Es una restricción que proporciona integridad de entidad para una columna o columnas especificadas mediante un índice único.  
  
 CLUSTERED | NONCLUSTERED  
 Especifica que se ha creado un índice clúster o no clúster para la restricción PRIMARY KEY o UNIQUE. Las restricciones PRIMARY KEY tienen el valor predeterminado CLUSTERED. Las restricciones UNIQUE tienen el valor predeterminado NONCLUSTERED.  
  
 Si en una tabla ya existe una restricción o índice agrupado, no se puede especificar CLUSTERED. Si en una tabla ya existe una restricción o índice agrupado, el valor predeterminado de la restricción PRIMARY KEY es NONCLUSTERED.  
  
 Las columnas de los tipos de datos **ntext**, **text**, **varchar(max)** , **nvarchar(max)** , **varbinary(max)** , **xml** o **image** no pueden especificarse como columnas para un índice.  
  
 WITH FILLFACTOR **=** _fillfactor_  
 Especifica cuánto se debe llenar cada página de índice del [!INCLUDE[ssDE](../../includes/ssde-md.md)] utilizada para almacenar los datos de índice. Los valores de factor de relleno especificados por el usuario pueden estar comprendidos entre 1 y 100. Si no se especifica un valor, el valor predeterminado es 0.  
  
> [!IMPORTANT]  
>  La documentación de WITH FILLFACTOR = *fillfactor* como la única opción de índice que se aplica a las restricciones PRIMARY KEY o UNIQUE se mantiene por compatibilidad con versiones anteriores, pero no se documentará de esta forma en futuras versiones. Es posible especificar otras opciones de índice en la cláusula [index_option](../../t-sql/statements/alter-table-index-option-transact-sql.md) de ALTER TABLE.  
  
 ON { _partition_scheme_name_ **(** _partition_column_name_ **)**  | *filegroup* |  **"** default **"** } **Se aplica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores  
  
 Especifica la ubicación de almacenamiento del índice creado para la restricción. Si se especifica *partition_scheme_name*, el índice se divide en particiones y las particiones se asignan a los grupos de archivos que se han especificado con *partition_scheme_name*. Si se especifica *filegroup*, el índice se crea en el grupo de archivos indicado. Si se especifica **"** default **"** o si no se especifica ON en ningún caso, el índice se crea en el mismo grupo de archivos que la tabla. Si se especifica ON al agregar un índice clúster para una restricción PRIMARY KEY o UNIQUE, la tabla completa se mueve al grupo de archivos especificado cuando se crea el índice clúster.  
  
 En este contexto, default no es una palabra clave. Es un identificador para el grupo de archivos predeterminado y debe delimitarse, como en ON **"** default **"** u ON **[** default **]** . Si se especifica **"** default **"** , la opción QUOTED_IDENTIFIER debe ser ON para la sesión actual. Esta es la configuración predeterminada. Para obtener más información, vea [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
 FOREIGN KEY REFERENCES  
 Es una restricción que proporciona integridad referencial para los datos de la columna. Las restricciones FOREIGN KEY exigen que cada valor de la columna exista en la columna especificada de la tabla a la que se hace referencia.  
  
 *schema_name*  
 Es el nombre del esquema al que pertenece la tabla a la que hace referencia la restricción FOREIGN KEY.  
  
 *referenced_table_name*  
 Es la tabla a la que hace referencia la restricción FOREIGN KEY.  
  
 *ref_column*  
 Es una columna entre paréntesis a la que hace referencia la nueva restricción FOREIGN KEY.  
  
 ON DELETE { **NO ACTION** \| CASCADE \| SET NULL \| SET DEFAULT }  
 Especifica la acción que se produce en las filas de la tabla modificada, si esas filas tienen una relación referencial y la fila a la que se hace referencia se elimina de la tabla primaria. El valor predeterminado es NO ACTION.  
  
 NO ACTION  
 El [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] genera un error y se revierte la acción de eliminación de la fila de la tabla primaria.  
  
 CASCADE  
 Si esa fila se elimina de la tabla primaria, las filas correspondientes se eliminan de la tabla de referencia.  
  
 SET NULL  
 Cuando se elimina la fila correspondiente de la tabla primaria, todos los valores que componen la clave externa se establecen en NULL. Para que esta restricción se ejecute, las columnas de clave externa deben aceptar valores NULL.  
  
 SET DEFAULT  
 Cuando se elimina la fila correspondiente de la tabla primaria, todos los valores que componen la clave externa se establecen en sus valores predeterminados. Para que esta restricción se ejecute, todas las columnas de clave externa deben tener definiciones predeterminadas. Si una columna acepta valores NULL y no se ha establecido un valor predeterminado explícito, NULL se convierte en el valor predeterminado explícito de dicha columna.  
  
 No especifique CASCADE si la tabla se va a incluir en una publicación de combinación que utiliza registros lógicos. Para obtener más información sobre los registros lógicos, vea [Agrupar cambios en filas relacionadas con registros lógicos](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).  
  
 No se puede definir ON DELETE CASCADE si ya existe un desencadenador INSTEAD OF en ON DELETE en la tabla que se va a modificar.  
  
 Por ejemplo, en la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], la tabla **ProductVendor** tiene una relación referencial con la tabla **Vendor**. La clave externa **ProductVendor.VendorID** hace referencia a la clave principal **Vendor.VendorID**.  
  
 Si se ejecuta una instrucción DELETE en una fila de la tabla **Vendor** y se especifica una acción ON DELETE CASCADE para **ProductVendor.VendorID**, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] comprueba una o más filas dependientes de la tabla **ProductVendor**. Si existe alguna, se eliminarán las filas dependientes de la tabla **ProductVendor**, así como la fila a la que se hace referencia de la tabla **Vendor**.  
  
 Por el contrario, si se especifica NO ACTION, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera un error y revierte la acción de eliminación de la fila **Vendor** si al menos hay una fila en la tabla **ProductVendor** que haga referencia a dicha fila.  
  
 ON UPDATE { **NO ACTION** \| CASCADE \| SET NULL \| SET DEFAULT }  
 Especifica la acción que se produce en las filas de la tabla modificada cuando esas filas tienen una relación referencial y la fila a la que se hace referencia se actualiza en la tabla primaria. El valor predeterminado es NO ACTION.  
  
 NO ACTION  
 El [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera un error y se revierte la acción de actualización de la fila de la tabla primaria.  
  
 CASCADE  
 Si esa fila se actualiza en la tabla primaria, las filas correspondientes se actualizan en la tabla de referencia.  
  
 SET NULL  
 Cuando se actualiza la fila correspondiente en la tabla primaria, todos los valores que componen la clave externa se establecen en NULL. Para que esta restricción se ejecute, las columnas de clave externa deben aceptar valores NULL.  
  
 SET DEFAULT  
 Cuando se actualiza la fila correspondiente en la tabla primaria, todos los valores que componen la clave externa se establecen en sus valores predeterminados. Para que esta restricción se ejecute, todas las columnas de clave externa deben tener definiciones predeterminadas. Si una columna acepta valores NULL y no se ha establecido un valor predeterminado explícito, NULL se convierte en el valor predeterminado explícito de dicha columna.  
  
 No especifique CASCADE si la tabla se va a incluir en una publicación de combinación que utiliza registros lógicos. Para obtener más información sobre los registros lógicos, vea [Agrupar cambios en filas relacionadas con registros lógicos](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).  
  
 No se puede definir ON UPDATE CASCADE, SET NULL o SET DEFAULT si ya existe un desencadenador INSTEAD OF en ON UPDATE en la tabla que se va a modificar.  
  
 Por ejemplo, en la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], la tabla **ProductVendor** tiene una relación referencial con la tabla **Vendor**. La clave externa **ProductVendor.VendorID** hace referencia a la clave principal **Vendor.VendorID**.  
  
 Si se ejecuta una instrucción UPDATE en una fila de la tabla **Vendor** y se especifica una acción ON UPDATE CASCADE para **ProductVendor.VendorID**, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] comprueba una o más filas dependientes de la tabla **ProductVendor**. Si existe alguna, se actualiza la fila dependiente de la tabla **ProductVendor**, así como la fila a la que se hace referencia de la tabla **Vendor**.  
  
 Por el contrario, si se especifica NO ACTION, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera un error y revierte la acción de actualización en la fila **Vendor** si al menos hay una fila en la tabla **ProductVendor** que haga referencia a dicha fila.  
  
 NOT FOR REPLICATION  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Se puede especificar para restricciones FOREIGN KEY y CHECK. Si se especifica esta cláusula para una restricción, la restricción no se aplica cuando los agentes de replicación realizan operaciones de inserción, actualización o eliminación.  
  
 CHECK  
 Es una restricción que exige la integridad del dominio al limitar los valores posibles que se pueden escribir en una o varias columnas.  
  
 *logical_expression*  
 Es una expresión lógica empleada en una restricción CHECK que devuelve TRUE o FALSE. El parámetro *logical_expression* usado con restricciones CHECK no puede hacer referencia a otra tabla, aunque sí a otras columnas de la misma tabla para la misma fila. La expresión no puede hacer referencia a un tipo de datos de alias.  
  
## <a name="remarks"></a>Observaciones  
 Cuando se agregan restricciones FOREIGN KEY o CHECK, se comprueba si hay infracciones de restricción en todos los datos existentes a menos que se especifique la opción WITH NOCHECK. Si se produce alguna infracción, ALTER TABLE no se ejecuta correctamente y se devuelve un error. Cuando se agrega una nueva restricción PRIMARY KEY o UNIQUE a una columna existente, los datos de las columnas deben ser únicos. Si se detectan valores duplicados, ALTER TABLE no se ejecuta correctamente. La opción WITH NOCHECK no tiene efecto cuando se agregan restricciones PRIMARY KEY o UNIQUE.  
  
 Cada restricción PRIMARY KEY y UNIQUE genera un índice. El número de restricciones UNIQUE y PRIMARY KEY no puede hacer que el número de índices de la tabla supere 999 índices no clúster y 1 índice clúster. Las restricciones de clave externa no generan automáticamente un índice. Sin embargo, las columnas de clave externa suelen emplearse en los criterios de combinación en consultas mediante la correspondencia de la columna o columnas de la restricción de clave externa de una tabla y la columna o columnas de la clave única o principal de la otra tabla. Un índice de las columnas de clave externa permite al [!INCLUDE[ssDE](../../includes/ssde-md.md)] buscar con rapidez datos relacionados en la tabla de clave externa.  
  
## <a name="examples"></a>Ejemplos  
 Para consultar otros ejemplos, vea [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md).  
  
## <a name="see-also"></a>Consulte también  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [column_definition &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-column-definition-transact-sql.md)  
  
  
