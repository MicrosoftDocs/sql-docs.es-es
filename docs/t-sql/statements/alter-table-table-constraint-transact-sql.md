---
description: ALTER TABLE table_constraint (Transact-SQL)
title: table_constraint (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CONSTRAINT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- table_constraint
ms.assetid: ac2a11e0-cc77-4e27-b107-4fe5bc6f5195
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: a2ebbdee1d7e56a06136ec41384782ded0fb5556
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193634"
---
# <a name="alter-table-table_constraint-transact-sql"></a>ALTER TABLE table_constraint (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Especifica las propiedades de una restricción PRIMARY KEY, UNIQUE, FOREIGN KEY o CHECK, o una definición DEFAULT agregada a una tabla mediante [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
[ CONSTRAINT constraint_name ]   
{   
    { PRIMARY KEY | UNIQUE }   
        [ CLUSTERED | NONCLUSTERED ]   
        (column [ ASC | DESC ] [ ,...n ] )  
        [ WITH FILLFACTOR = fillfactor   
        [ WITH ( <index_option>[ , ...n ] ) ]  
        [ ON { partition_scheme_name ( partition_column_name ... )  
          | filegroup | "default" } ]   
    | FOREIGN KEY   
        ( column [ ,...n ] )  
        REFERENCES referenced_table_name [ ( ref_column [ ,...n ] ) ]   
        [ ON DELETE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]   
        [ ON UPDATE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]   
        [ NOT FOR REPLICATION ]   
    | CONNECTION
        ( { node_table TO node_table } 
          [ , {node_table TO node_table }]
          [ , ...n ]
        )
        [ ON DELETE { NO ACTION | CASCADE } ]
    | DEFAULT constant_expression FOR column [ WITH VALUES ]   
    | CHECK [ NOT FOR REPLICATION ] ( logical_expression )  
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 CONSTRAINT  
 Especifica el inicio de una definición para una restricción PRIMARY KEY, UNIQUE, FOREIGN KEY o CHECK o una definición DEFAULT.  
  
 *constraint_name*  
 Es el nombre de la restricción. Los nombres de restricción deben seguir las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md), excepto en que el nombre no puede empezar por un signo de número (#). Si no se proporciona el parámetro constraint_name, se asigna a la restricción un nombre generado por el sistema.  
  
 PRIMARY KEY  
 Es una restricción que aplica la integridad de entidad para una columna o columnas especificadas mediante un índice único. Solo se puede crear una restricción PRIMARY KEY por cada tabla.  
  
 UNIQUE  
 Es una restricción que proporciona integridad de entidad para una columna o columnas especificadas mediante un índice único.  
  
 CLUSTERED | NONCLUSTERED  
 Especifica que se ha creado un índice clúster o no clúster para la restricción PRIMARY KEY o UNIQUE. Las restricciones PRIMARY KEY tienen el valor predeterminado CLUSTERED. Las restricciones UNIQUE tienen el valor predeterminado NONCLUSTERED.  
  
 Si en una tabla ya existe una restricción o índice agrupado, no se puede especificar CLUSTERED. Si en una tabla ya existe una restricción o índice agrupado, el valor predeterminado de la restricción PRIMARY KEY es NONCLUSTERED.  
  
 Las columnas de los tipos de datos **ntext**, **text**, **varchar(max)** , **nvarchar(max)** , **varbinary(max)** , **xml** o **image** no pueden especificarse como columnas para un índice.  
  
 *column*  
 Es una columna o lista de columnas especificadas entre paréntesis que se utilizan en una nueva restricción.  
  
 [ **ASC** | DESC ]  
 Especifica cómo se ordenan la columna o las columnas que participan en las restricciones de la tabla. El valor predeterminado es ASC.  
  
 WITH FILLFACTOR **=** _fillfactor_  
 Especifica cuánto se debe llenar cada página de índice del [!INCLUDE[ssDE](../../includes/ssde-md.md)] utilizada para almacenar los datos de índice. Los valores de *fillfactor* especificados por el usuario pueden estar comprendidos entre 1 y 100. Si no se especifica un valor, el valor predeterminado es 0.  
  
> [!IMPORTANT]  
>  La documentación de WITH FILLFACTOR = *fillfactor* como la única opción de índice que se aplica a las restricciones PRIMARY KEY o UNIQUE se mantiene por compatibilidad con versiones anteriores, pero no se documentará de esta forma en futuras versiones. Es posible especificar otras opciones de índice en la cláusula [index_option](../../t-sql/statements/alter-table-index-option-transact-sql.md) de ALTER TABLE.  
  
 ON { _partition\_scheme\_name_ **(** _partition\_column\_name_ **)**  | _filegroup_|  **"** default **"** }  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Especifica la ubicación de almacenamiento del índice creado para la restricción. Si se especifica *partition_scheme_name*, el índice se divide en particiones y las particiones se asignan a los grupos de archivos que se han especificado con *partition_scheme_name*. Si se especifica *filegroup*, el índice se crea en el grupo de archivos indicado. Si se especifica **"** default **"** o si no se especifica ON en ningún caso, el índice se crea en el mismo grupo de archivos que la tabla. Si se especifica ON al agregar un índice clúster para una restricción PRIMARY KEY o UNIQUE, la tabla completa se mueve al grupo de archivos especificado cuando se crea el índice clúster.  
  
 En este contexto, default no es una palabra clave, sino un identificador del grupo de archivos predeterminado y debe delimitarse, como en ON **"** default **"** u ON **[** default **]** . Si se especifica **"** default **"** , la opción QUOTED_IDENTIFIER debe ser ON para la sesión actual. Esta es la configuración predeterminada.  
  
 FOREIGN KEY REFERENCES  
 Es una restricción que proporciona integridad referencial para los datos de la columna. Las restricciones FOREIGN KEY exigen que cada valor de la columna exista en la columna especificada de la tabla a la que se hace referencia.  
  
 *referenced_table_name*  
 Es la tabla a la que hace referencia la restricción FOREIGN KEY.  
  
 *ref_column*  
 Es una columna o lista de columnas entre paréntesis a la que hace referencia la nueva restricción FOREIGN KEY.  
  
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
  
 No se puede definir la acción ON DELETE CASCADE si ya existe un desencadenador INSTEAD OF en ON DELETE en la tabla que se está modificando.  
  
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
  
 Si se ejecuta una instrucción UPDATE en una fila de la tabla **Vendor** y se especifica una acción ON UPDATE CASCADE para **ProductVendor.VendorID**, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] comprueba una o más filas dependientes de la tabla **ProductVendor**. Si existe alguna, se actualizarán la fila dependiente de la tabla **ProductVendor**, así como la fila a la que se hace referencia de la tabla **Vendor**.  
  
 Por el contrario, si se especifica NO ACTION, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera un error y revierte la acción de actualización en la fila **Vendor** si al menos hay una fila en la tabla **ProductVendor** que haga referencia a dicha fila.  
  
 NOT FOR REPLICATION  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Se puede especificar para restricciones FOREIGN KEY y CHECK. Si se especifica esta cláusula para una restricción, la restricción no se aplica cuando los agentes de replicación realizan operaciones de inserción, actualización o eliminación.  

 CONNECTION Especifica el par de tablas de nodo a la que puede conectarse la restricción perimetral determinada. ON DELETE especifica lo que sucede con las filas de la tabla perimetral cuando se eliminan los nodos que se han conectado a través de esta tabla perimetral. 
 
 DEFAULT  
 Especifica el valor predeterminado de la columna. Las definiciones DEFAULT se pueden utilizar para proporcionar valores a una nueva columna de las filas existentes de datos. Las definiciones DEFAULT no se pueden agregar a las columnas que tengan un tipo de datos **timestamp**, una propiedad IDENTITY, una definición DEFAULT existente o un valor predeterminado enlazado. Si la columna tiene un valor predeterminado existente, éste debe quitarse para poder agregar uno nuevo. Si se especifica un valor predeterminado para una columna de un tipo definido por el usuario, dicho tipo debe admitir la conversión implícita de *constant_expression* al tipo definido por el usuario. Para mantener la compatibilidad con las versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se puede asignar un nombre de restricción a DEFAULT.  
  
 *constant_expression*  
 Es un valor literal, un valor NULL o una función del sistema que se utiliza como valor predeterminado de la columna. Si *constant_expression* se usa junto con una columna definida como de[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]tipo definido por el usuario, la implementación del tipo debe ser compatible con la conversión implícita de *constant_expression* en el tipo definido por el usuario.  
  
 FOR *column*  
 Especifica la columna asociada a una definición DEFAULT de nivel de tabla.  
  
 WITH VALUES  
 Al agregar una columna Y una restricción DEFAULT, si la columna permite valores NULL con WITH VALUES, para las filas existentes, establecerá el valor de la columna nueva en el valor determinado en la expresión *constant_expression* DEFAULT. Si la columna que se agrega no permite valores NULL, para las filas existentes, el valor de la columna siempre se establecerá en el valor determinado en la *expresión constante* DEFAULT. Desde SQL Server 2012, está puede ser una operación de metadatos [adding-not-null-columns-as-an-online-operation](alter-table-transact-sql.md#adding-not-null-columns-as-an-online-operation).
Si se usa cuando no se está agregando también la columna relacionada, no tiene ningún impacto. 
  
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
  
  
