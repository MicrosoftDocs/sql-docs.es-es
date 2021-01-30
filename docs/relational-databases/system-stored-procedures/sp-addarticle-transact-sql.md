---
title: sp_addarticle (Transact-SQL) | Microsoft Docs
description: Crea un artículo y lo agrega a una publicación. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.
ms.custom: ''
ms.date: 10/28/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addarticle
- sp_addarticle_TSQL
helpviewer_keywords:
- sp_addarticle
ms.assetid: 0483a157-e403-4fdb-b943-23c1b487bef0
author: mashamsft
ms.author: mathoma
ms.openlocfilehash: 5ee845eac866c3954ba03080c037179fa76ad519
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182837"
---
# <a name="sp_addarticle-transact-sql"></a>sp_addarticle (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea un artículo y lo agrega a una publicación. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_addarticle [ @publication = ] 'publication'   
        , [ @article = ] 'article'   
    [ , [ @source_table = ] 'source_table' ]  
    [ , [ @destination_table = ] 'destination_table' ]   
    [ , [ @vertical_partition = ] 'vertical_partition' ]   
    [ , [ @type = ] 'type' ]   
    [ , [ @filter = ] 'filter' ]   
    [ , [ @sync_object= ] 'sync_object' ]   
        [ , [ @ins_cmd = ] 'ins_cmd' ]   
    [ , [ @del_cmd = ] 'del_cmd' ]   
        [ , [ @upd_cmd = ] 'upd_cmd' ]   
    [ , [ @creation_script = ] 'creation_script' ]   
    [ , [ @description = ] 'description' ]   
    [ , [ @pre_creation_cmd = ] 'pre_creation_cmd' ]   
    [ , [ @filter_clause = ] 'filter_clause' ]   
    [ , [ @schema_option = ] schema_option ]   
    [ , [ @destination_owner = ] 'destination_owner' ]   
    [ , [ @status = ] status ]   
    [ , [ @source_owner = ] 'source_owner' ]   
    [ , [ @sync_object_owner = ] 'sync_object_owner' ]   
    [ , [ @filter_owner = ] 'filter_owner' ]   
    [ , [ @source_object = ] 'source_object' ]   
    [ , [ @artid = ] article_ID  OUTPUT ]   
    [ , [ @auto_identity_range = ] 'auto_identity_range' ]   
    [ , [ @pub_identity_range = ] pub_identity_range ]   
    [ , [ @identity_range = ] identity_range ]   
    [ , [ @threshold = ] threshold ]   
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @use_default_datatypes = ] use_default_datatypes  
    [ , [ @identityrangemanagementoption = ] identityrangemanagementoption ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @fire_triggers_on_snapshot = ] 'fire_triggers_on_snapshot' ]   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación que contiene el artículo. El nombre debe ser único en la base de datos. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @article = ] 'article'` Es el nombre del artículo. El nombre debe ser único en la publicación. *article* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @source_table = ] 'source_table'` Este parámetro está en desuso; en su lugar, use *source_object* .  
  
 *Este parámetro no es compatible con los publicadores de Oracle.*  
  
`[ @destination_table = ] 'destination_table'` Es el nombre de la tabla de destino (suscripción), si es diferente de *source_table* o del procedimiento almacenado. *destination_table* es de **tipo sysname y su** valor predeterminado es null, lo que significa que *source_table* es igual a *destination_table * *.*  
  
`[ @vertical_partition = ] 'vertical_partition'` Habilita y deshabilita el filtrado de columnas en un artículo de tabla. *vertical_partition* es **NCHAR (5)** y su valor predeterminado es false.  
  
 **false** indica que no hay filtrado vertical y publica todas las columnas.  
  
 **true** borra todas las columnas excepto la clave principal declarada, las columnas que aceptan valores NULL y las columnas de clave Unique no predeterminadas. Las columnas se agregan mediante [sp_articlecolumn](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md).  
  
`[ @type = ] 'type'` Es el tipo de artículo. *Type* es de tipo **sysname** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**aggregate schema only**|Función de agregado con solo esquema.|  
|**func schema only**|Función con solo esquema.|  
|**indexed view logbased**|Artículo de vista indizada basado en registro. No es compatible con publicadores de Oracle. En este tipo de artículo, no es necesario que la tabla base se publique por separado.|  
|**indexed view logbased manualboth**|Artículo de vista indizada basado en registro con filtro manual y vista manual. Esta opción requiere que especifique los parámetros *sync_object* y *Filter* . En este tipo de artículo, no es necesario que la tabla base se publique por separado. No es compatible con publicadores de Oracle.|  
|**indexed view logbased manualfilter**|Artículo de vista indizada basado en registro con filtro manual. Esta opción requiere que especifique los parámetros *sync_object* y *Filter* . En este tipo de artículo, no es necesario que la tabla base se publique por separado. No es compatible con publicadores de Oracle.|  
|**indexed view logbased manualview**|Artículo de vista indizada basado en registro con vista manual. Esta opción requiere que se especifique el parámetro *sync_object* . En este tipo de artículo, no es necesario que la tabla base se publique por separado. No es compatible con publicadores de Oracle.|  
|**indexed view schema only**|Vista indizada con solo esquema. En este tipo de artículo también debe publicarse la tabla base.|  
|**logbased** (valor predeterminado)|Artículo basado en registro.|  
|**logbased manualboth**|Artículo basado en registro con filtro manual y vista manual. Esta opción requiere que especifique los parámetros *sync_object* y *Filter* . No es compatible con publicadores de Oracle.|  
|**logbased manualfilter**|Artículo basado en registro con filtro manual. Esta opción requiere que especifique los parámetros *sync_object* y *Filter* . No es compatible con publicadores de Oracle.|  
|**logbased manualview**|Artículo basado en registro con vista manual. Esta opción requiere que se especifique el parámetro *sync_object* . No es compatible con publicadores de Oracle.|  
|**proc exec**|Replica la ejecución del procedimiento almacenado a todos los suscriptores del artículo. No es compatible con publicadores de Oracle. Se recomienda usar la opción **serializable proc exec** en lugar de **proc exec**. Para obtener más información, vea la sección "tipos de artículos de ejecución de procedimientos almacenados" en publicación de la [ejecución de procedimientos almacenados en la replicación transaccional](../../relational-databases/replication/transactional/publishing-stored-procedure-execution-in-transactional-replication.md). No disponible cuando la captura de datos modificados está habilitada.|  
|**proc schema only**|Procedimiento con solo esquema. No es compatible con publicadores de Oracle.|  
|**serializable proc exec**|Replica la ejecución del procedimiento almacenado solo si éste se ejecuta dentro del contexto de una transacción serializable. No es compatible con publicadores de Oracle.<br /><br /> El procedimiento también debe ejecutarse dentro de una transacción explícita para que se replique la ejecución del procedimiento.|  
|**view schema only**|Vista con solo esquema. No es compatible con publicadores de Oracle. Si se utiliza esta opción, también debe publicarse la tabla base.|  
  
`[ @filter = ] 'filter'` Es el procedimiento almacenado (creado con FOR REPLICAtion) que se usa para filtrar la tabla horizontalmente. *Filter* es de tipo **nvarchar (386)** y su valor predeterminado es NULL. [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md) y [sp_articlefilter](../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md) deben ejecutarse manualmente para crear la vista y el procedimiento almacenado de filtro. Si no es NULL, el procedimiento de filtro no se crea (se presupone que el procedimiento almacenado se crea manualmente).  
  
`[ @sync_object = ] 'sync_object'` Es el nombre de la tabla o vista utilizada para generar el archivo de datos que se usa para representar la instantánea de este artículo. *sync_object* es de tipo **nvarchar (386)** y su valor predeterminado es NULL. Si es NULL, se llama a [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md) para crear automáticamente la vista usada para generar el archivo de salida. Esto sucede después de Agregar columnas con [sp_articlecolumn](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md). Si no es NULL, no se crea la vista (se presupone que la vista se crea manualmente).  
  
`[ @ins_cmd = ] 'ins_cmd'` Es el tipo de comando de replicación utilizado al replicar inserciones para este artículo. *ins_cmd* es **nvarchar (255)** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**NONE**|No se realiza ninguna acción.|  
|**CALL sp_MSins_**<br /> **_tabla_**  (valor predeterminado)<br /><br /> O bien<br /><br /> **CALL custom_stored_procedure_name**|Llama a un procedimiento almacenado que se ejecutará en el suscriptor. Para utilizar este método de replicación, utilice *schema_option* para especificar la creación automática del procedimiento almacenado o cree el procedimiento almacenado especificado en la base de datos de destino de cada suscriptor del artículo. *custom_stored_procedure* es el nombre de un procedimiento almacenado creado por el usuario. <strong>sp_MSins_ *tabla*</strong> contiene el nombre de la tabla de destino en lugar de la *_table* parte del parámetro. Cuando se especifica *destination_owner* , se antepone al nombre de la tabla de destino. Por ejemplo, para la tabla **ProductCategory** propiedad del esquema **Production** en el suscriptor, el parámetro sería `CALL sp_MSins_ProductionProductCategory` . Para un artículo en una topología de replicación punto a punto, *_table* se anexa con un valor GUID. No se puede especificar *custom_stored_procedure* para los suscriptores de actualización.|  
|**SQL** o null|Replica una instrucción INSERT. La instrucción INSERT recibe valores para todas las columnas publicadas en el artículo. Este comando se replica con las inserciones:<br /><br /> `INSERT INTO <table name> VALUES (c1value, c2value, c3value, ..., cnvalue)`|  
  
 Para más información, vea [Especificar cómo se propagan los cambios para los artículos transaccionales](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).  
  
`[ @del_cmd = ] 'del_cmd'` Es el tipo de comando de replicación utilizado al replicar eliminaciones para este artículo. *del_cmd* es **nvarchar (255)** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**NONE**|No se realiza ninguna acción.|  
|**CALLsp_MSdel_**<br /> **_tabla_**  (valor predeterminado)<br /><br /> O bien<br /><br /> **CALL custom_stored_procedure_name**|Llama a un procedimiento almacenado que se ejecutará en el suscriptor. Para utilizar este método de replicación, utilice *schema_option* para especificar la creación automática del procedimiento almacenado o cree el procedimiento almacenado especificado en la base de datos de destino de cada suscriptor del artículo. *custom_stored_procedure* es el nombre de un procedimiento almacenado creado por el usuario. <strong>sp_MSdel_ *tabla*</strong> contiene el nombre de la tabla de destino en lugar de la *_table* parte del parámetro. Cuando se especifica *destination_owner* , se antepone al nombre de la tabla de destino. Por ejemplo, para la tabla **ProductCategory** propiedad del esquema **Production** en el suscriptor, el parámetro sería `CALL sp_MSdel_ProductionProductCategory` . Para un artículo en una topología de replicación punto a punto, *_table* se anexa con un valor GUID. No se puede especificar *custom_stored_procedure* para los suscriptores de actualización.|  
|**XCALL sp_MSdel_**<br /> **_cuadro_**<br /><br /> O bien<br /><br /> **XCALL custom_stored_procedure_name**|Llama a un procedimiento almacenado con parámetros del estilo XCALL. Para utilizar este método de replicación, utilice *schema_option* para especificar la creación automática del procedimiento almacenado o cree el procedimiento almacenado especificado en la base de datos de destino de cada suscriptor del artículo. No se permite especificar un procedimiento almacenado creado por el usuario en suscriptores de actualización.|  
|**SQL** o null|Replica una instrucción DELETE. La instrucción DELETE recibe todos los valores de las columnas de clave principal. Este comando se replica con las eliminaciones:<br /><br /> `DELETE FROM <table name> WHERE pkc1 = pkc1value AND pkc2 = pkc2value AND pkcn = pkcnvalue`|  
  
 Para más información, vea [Especificar cómo se propagan los cambios para los artículos transaccionales](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).  
  
`[ @upd_cmd = ] 'upd_cmd'` Es el tipo de comando de replicación utilizado al replicar actualizaciones para este artículo. *upd_cmd* es **nvarchar (255)** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**NONE**|No se realiza ninguna acción.|  
|**CALL sp_MSupd_**<br /> **_cuadro_**<br /><br /> O bien<br /><br /> **CALL custom_stored_procedure_name**|Llama a un procedimiento almacenado que se ejecutará en el suscriptor. Para utilizar este método de replicación, utilice *schema_option* para especificar la creación automática del procedimiento almacenado o cree el procedimiento almacenado especificado en la base de datos de destino de cada suscriptor del artículo.|  
|**MCALL sp_MSupd_**<br /> **_cuadro_**<br /><br /> O bien<br /><br /> **MCALL custom_stored_procedure_name**|Llama a un procedimiento almacenado con parámetros del estilo MCALL. Para utilizar este método de replicación, utilice *schema_option* para especificar la creación automática del procedimiento almacenado o cree el procedimiento almacenado especificado en la base de datos de destino de cada suscriptor del artículo. *custom_stored_procedure* es el nombre de un procedimiento almacenado creado por el usuario. <strong>sp_MSupd_ *tabla*</strong> contiene el nombre de la tabla de destino en lugar de la *_table* parte del parámetro. Cuando se especifica *destination_owner* , se antepone al nombre de la tabla de destino. Por ejemplo, para la tabla **ProductCategory** propiedad del esquema **Production** en el suscriptor, el parámetro sería `MCALL sp_MSupd_ProductionProductCategory` . Para un artículo en una topología de replicación punto a punto, *_table* se anexa con un valor GUID. No se permite especificar un procedimiento almacenado creado por el usuario en suscriptores de actualización.|  
|**SCALL sp_MSupd_**<br /> **_tabla_**  (valor predeterminado)<br /><br /> O bien<br /><br /> **SCALL custom_stored_procedure_name**|Llama a un procedimiento almacenado con parámetros del estilo SCALL. Para utilizar este método de replicación, utilice *schema_option* para especificar la creación automática del procedimiento almacenado o cree el procedimiento almacenado especificado en la base de datos de destino de cada suscriptor del artículo. *custom_stored_procedure* es el nombre de un procedimiento almacenado creado por el usuario. <strong>sp_MSupd_ *tabla*</strong> contiene el nombre de la tabla de destino en lugar de la *_table* parte del parámetro. Cuando se especifica *destination_owner* , se antepone al nombre de la tabla de destino. Por ejemplo, para la tabla **ProductCategory** propiedad del esquema **Production** en el suscriptor, el parámetro sería `SCALL sp_MSupd_ProductionProductCategory` . Para un artículo en una topología de replicación punto a punto, *_table* se anexa con un valor GUID. No se permite especificar un procedimiento almacenado creado por el usuario en suscriptores de actualización.|  
|**XCALL sp_MSupd_**<br /> **_cuadro_**<br /><br /> O bien<br /><br /> **XCALL custom_stored_procedure_name**|Llama a un procedimiento almacenado con parámetros del estilo XCALL. Para utilizar este método de replicación, utilice *schema_option* para especificar la creación automática del procedimiento almacenado o cree el procedimiento almacenado especificado en la base de datos de destino de cada suscriptor del artículo. No se permite especificar un procedimiento almacenado creado por el usuario en suscriptores de actualización.|  
|**SQL** o null|Replica una instrucción UPDATE. La instrucción UPDATE está disponible en todos los valores de columna y los valores de las columnas de clave principal. Este comando se replica con las actualizaciones:<br /><br /> `UPDATE <table name> SET c1 = c1value, SET c2 = c2value, SET cn = cnvalue WHERE pkc1 = pkc1value AND pkc2 = pkc2value AND pkcn = pkcnvalue`|  
  
> [!NOTE]  
>  La sintaxis CALL, MCALL, SCALL y XCALL varía la cantidad de datos propagados al suscriptor. La sintaxis CALL pasa todos los valores de todas las columnas insertadas y eliminadas. La sintaxis SCALL solo pasa los valores de las columnas afectadas. La sintaxis XCALL pasa los valores de todas las columnas, tanto si han cambiado como si no, incluido el valor anterior de la columna. Para más información, vea [Especificar cómo se propagan los cambios para los artículos transaccionales](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).  
  
`[ @creation_script = ] 'creation_script'` Es la ruta de acceso y el nombre de un script de esquema de artículo opcional que se usa para crear el artículo en la base de datos de suscripciones. *creation_script* es de tipo **nvarchar (255)** y su valor predeterminado es NULL.  
  
`[ @description = ] 'description'` Es una entrada descriptiva del artículo. la *Descripción* es de tipo **nvarchar (255)** y su valor predeterminado es NULL.  
  
`[ @pre_creation_cmd = ] 'pre_creation_cmd'` Especifica lo que debe hacer el sistema si detecta un objeto existente con el mismo nombre en el suscriptor al aplicar la instantánea para este artículo. *pre_creation_cmd* es **nvarchar (10)** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Ninguna**|No usa ningún comando.|  
|**delete**|Elimina datos de la tabla de destino antes de aplicar la instantánea. Cuando el artículo se filtra horizontalmente, solo se eliminan los datos en las columnas especificadas en la cláusula de filtro. No se admite en publicadores de Oracle si se ha definido un filtro horizontal.|  
|**Drop** (valor predeterminado)|Quita la tabla de destino.|  
|**truncate**|Trunca la tabla de destino. No es válido para los suscriptores de ODBC o de OLE DB.|  
  
`[ @filter_clause = ] 'filter_clause'` Es una cláusula de restricción (WHERE) que define un filtro horizontal. Al especificar la cláusula de restricción, omita la palabra clave WHERE. *filter_clause* es **ntext** y su valor predeterminado es NULL. Para obtener más información, vea [Filtrar datos publicados](../../relational-databases/replication/publish/filter-published-data.md).  
  
`[ @schema_option = ] schema_option` Es una máscara de la opción de generación de esquema para el artículo dado. *schema_option* es **binario (8)** y puede ser el [| (OR bit a bit)](../../t-sql/language-elements/bitwise-or-transact-sql.md) producto de uno o varios de estos valores:  
  
> [!NOTE]  
>  Si este valor es NULL, el sistema genera automáticamente una opción de esquema válida para el artículo dependiendo de las propiedades del artículo. La tabla de **Opciones de esquema predeterminadas** que se proporciona en la sección Notas muestra el valor que se elegirá en función de la combinación del tipo de artículo y el tipo de replicación.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**0x00**|Deshabilita el scripting por el Agente de instantáneas y usa *creation_script*.|  
|**0x01**|Genera el script de creación del objeto (CREATE TABLE, CREATE PROCEDURE, etc.). Este valor es el predeterminado en los artículos de procedimientos almacenados.|  
|**0x02**|Genera los procedimientos almacenados que propagan los cambios del artículo, si se han definido.|  
|**0x04**|Las columnas de identidad se incluyen en los scripts con la propiedad IDENTITY.|  
|**0x08**|Replicar columnas de **marca** de tiempo. Si no se establece, las columnas de **marca** de tiempo se replican como **binarias**.|  
|**0x10**|Genera el índice clúster correspondiente. Incluso si no se establece esta opción, se generan índices relacionados con las claves principales y las restricciones únicas si ya están definidas en una tabla publicada.|  
|**0x20**|Convierte los tipos de datos definidos por el usuario (UDT) en tipos de datos base en el suscriptor. Esta opción no se puede utilizar si existe una restricción CHECK o DEFAULT en una columna UDT, si una columna UDT forma parte de la clave principal o si una columna calculada hace referencia a una columna UDT. *No se admite para publicadores de Oracle*.|  
|**0x40**|Genera los índices no clúster correspondientes. Incluso si no se establece esta opción, se generan índices relacionados con las claves principales y las restricciones únicas si ya están definidas en una tabla publicada.|  
|**0x80**|Replica las restricciones de clave principal. También se replican los índices relacionados con la restricción, aunque las opciones **0x10** y **0x40** no estén habilitadas.|  
|**0x100**|Replica los desencadenadores de usuario en un artículo de tabla, si se han definido. *No se admite para publicadores de Oracle*.|  
|**0x200**|Replica las restricciones de clave externa. Si la tabla a la que se hace referencia no forma parte de una publicación, no se replica ninguna restricción de clave externa de una tabla publicada. *No se admite para publicadores de Oracle*.|  
|**0x400**|Replica las restricciones CHECK. *No se admite para publicadores de Oracle*.|  
|**0x800**|Replica los valores predeterminados. *No se admite para publicadores de Oracle*.|  
|**0x1000**|Replica la intercalación de columna.<br /><br /> **Nota:** Esta opción debe establecerse para que los publicadores de Oracle habiliten comparaciones que distinguen mayúsculas de minúsculas.|  
|**0x2000**|Replica las propiedades extendidas asociadas con el objeto de origen del artículo publicado. *No se admite para publicadores de Oracle*.|  
|**0x4000**|Replica las restricciones UNIQUE. También se replican los índices relacionados con la restricción, aunque las opciones **0x10** y **0x40** no estén habilitadas.|  
|**0x8000**|Esta opción no es válida para publicadores de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].|  
|**0x10000**|Replica las restricciones CHECK como NOT FOR REPLICATION de manera que no se impongan durante la sincronización.|  
|**0x20000**|Replica las restricciones FOREIGN KEY como NOT FOR REPLICATION de manera que no se impongan durante la sincronización.|  
|**0x40000**|Replica grupos de archivos asociados con un índice o una tabla con particiones.|  
|**0x80000**|Replica el esquema de partición de una tabla con particiones.|  
|**0x100000**|Replica el esquema de partición de un índice con particiones.|  
|**0x200000**|Replica las estadísticas de tabla.|  
|**0x400000**|Enlaces predeterminados|  
|**0x800000**|Enlaces de reglas|  
|**0x1000000**|Índice de texto completo|  
|**0x2000000**|Las colecciones de esquemas XML enlazadas a columnas **XML** no se replican.|  
|**0x4000000**|Replica índices en columnas **XML** .|  
|**0x8000000**|Crea esquemas que aún no existen en el suscriptor.|  
|**0x10000000**|Convierte las columnas **XML** en **ntext** en el suscriptor.|  
|**0x20000000**|Convierte los tipos de datos de objetos grandes (**nvarchar (Max)**, **VARCHAR (Max)** y **varbinary (Max)**) introducidos en en [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] tipos de datos que se admiten en [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] .|  
|**0x40000000**|Permisos de replicación.|  
|**0x80000000**|Intenta quitar las dependencias de los objetos que no forman parte de la publicación.|  
|**0x100000000**|Use esta opción para replicar el atributo FILESTREAM si se especifica en columnas **varbinary (Max)** . No especifique esta opción si replica tablas en suscriptores de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. No se admite la replicación de tablas que tienen columnas FILESTREAM en [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] suscriptores, independientemente de cómo se establezca esta opción de esquema.<br /><br /> Vea la opción relacionada **0x800000000**.|  
|**0x200000000**|Convierte los tipos de datos de fecha y hora (**Date**, **Time**, **DateTimeOffset** y **datetime2**) introducidos en en [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] tipos de datos que se admiten en versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**0x400000000**|Replica la opción de compresión para los datos y los índices. Para obtener más información, consulte [Data Compression](../../relational-databases/data-compression/data-compression.md).|  
|**0x800000000**|Establezca esta opción para almacenar los datos de FILESTREAM en su propio grupo de archivos en el suscriptor. Si no se establece esta opción, los datos de FILESTREAM se almacenan en el grupo de archivos predeterminado. La replicación no crea grupos de archivos; por tanto, si establece esta opción, debe crear el grupo de archivos antes de aplicar la instantánea en el suscriptor. Para obtener más información sobre cómo crear objetos antes de aplicar la instantánea, vea [ejecutar scripts antes y después de aplicar la instantánea](../../relational-databases/replication/snapshot-options.md#execute-scripts-before-and-after-snapshot-is-applied).<br /><br /> Vea la opción relacionada **0x100000000**.|  
|**0x1000000000**|Convierte los tipos definidos por el usuario (UDT) de Common Language Runtime (CLR) mayores de 8000 bytes a **varbinary (Max)** para que las columnas de tipo UDT se puedan replicar en los suscriptores que ejecutan [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .|  
|**0x2000000000**|Convierte el tipo de datos **hierarchyid** a **varbinary (Max)** para que las columnas de tipo **hierarchyid** se puedan replicar en los suscriptores que ejecutan [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] . Para obtener más información sobre cómo usar las columnas **hierarchyid** en tablas replicadas, vea [hierarchyid &#40;Transact-SQL&#41;](../../t-sql/data-types/hierarchyid-data-type-method-reference.md).|  
|**0x4000000000**|Replica los índices filtrados de la tabla. Para obtener más información acerca de los índices filtrados, vea [crear índices filtrados](../../relational-databases/indexes/create-filtered-indexes.md).|  
|**0x8000000000**|Convierte los tipos de datos **Geography** y **Geometry** en **varbinary (Max)** para que las columnas de estos tipos se puedan replicar en los suscriptores que ejecutan [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .|  
|**0x10000000000**|Replica los índices en columnas de tipo **Geography** y **Geometry**.|  
|**0x20000000000**|Replica el atributo SPARSE para las columnas. Para obtener más información sobre este atributo, vea [usar columnas dispersas](../../relational-databases/tables/use-sparse-columns.md).|  
|**0x40000000000**|Habilite el scripting del agente de instantáneas para crear una tabla optimizada para memoria en el suscriptor.|  
|**0x80000000000**|Convierte el índice clúster en un índice no clúster para los artículos con optimización para memoria.|  
|**0x400000000000**|Replica los índices de almacén de columnas no agrupados de las tablas|  
|**0x800000000000**|Replica todos los índices de almacén de columnas no agrupados de flitered en las tablas.|  
|NULL|La replicación establece automáticamente *schema_option* en un valor predeterminado, cuyo valor depende de otras propiedades de artículo. La tabla "Opciones de esquema predeterminadas" de la sección Notas muestra las opciones de esquema predeterminadas basadas en los tipos de artículo y de replicación.<br /><br /> El valor predeterminado para las publicaciones que no son de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es **0x050D3**.|  
  
 No todos los valores *schema_option* son válidos para todos los tipos de replicación y tipo de artículo. La tabla **Opciones de esquema válidas** de la sección Notas muestra las opciones de esquema válidas que se pueden elegir según la combinación del tipo de artículo y el tipo de replicación.  
  
`[ @destination_owner = ] 'destination_owner'` Es el nombre del propietario del objeto de destino. *destination_owner* es de **tipo sysname y su** valor predeterminado es NULL. Cuando no se especifica *destination_owner* , el propietario se especifica automáticamente según las siguientes reglas:  
  
|Condición|Propietario del objeto de destino|  
|---------------|------------------------------|  
|La publicación utiliza una copia masiva en modo nativo para generar la instantánea inicial, que solo admite suscriptores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|Tiene como valor predeterminado el valor de *source_owner*.|  
|Publicado desde un publicador que no es de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|El valor predeterminado es el propietario de la base de datos de destino.|  
|La publicación utiliza una copia masiva en modo de carácter para generar la instantánea inicial, que admite suscriptores que no son de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|No asignado.|  
  
 Para admitir suscriptores que no sean de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , *destination_owner* debe ser null.  
  
`[ @status = ] status` Especifica si el artículo está activo y opciones adicionales sobre cómo se propagan los cambios. *status* es **tinyint** y puede ser el [carácter | (OR bit a bit)](../../t-sql/language-elements/bitwise-or-transact-sql.md) producto de uno o varios de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1**|El artículo está activo.|  
|**8**|Incluye el nombre de la columna en las instrucciones INSERT.|  
|**16** (valor predeterminado)|Usa instrucciones con parámetros.|  
|**24**|Incluye el nombre de la columna en las instrucciones INSERT y usa instrucciones con parámetros.|  
|**64**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
  
 Por ejemplo, un artículo activo que utilice instrucciones con parámetros tendrá un valor de 17 en esta columna. Un valor de **0** significa que el artículo está inactivo y no se han definido propiedades adicionales.  
  
`[ @source_owner = ] 'source_owner'` Es el propietario del objeto de origen. *source_owner* es de **tipo sysname y su** valor predeterminado es NULL. se debe especificar *source_owner* para los publicadores de Oracle.  
  
`[ @sync_object_owner = ] 'sync_object_owner'` Es el propietario de la vista que define el artículo publicado. *sync_object_owner* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @filter_owner = ] 'filter_owner'` Es el propietario del filtro. *filter_owner* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @source_object = ] 'source_object'` Es el objeto de base de datos que se va a publicar. *source_object* es de **tipo sysname y su** valor predeterminado es NULL. Si *source_table* es null, *source_object* no puede ser null. *source_object* debe usarse en lugar de *source_table*. Para obtener más información sobre los tipos de objetos que se pueden publicar mediante la replicación transaccional o de instantáneas, vea [publicar datos y objetos de base de datos](../../relational-databases/replication/publish/publish-data-and-database-objects.md).  
  
`[ @artid = ] _article_ID_ OUTPUT` Es el identificador de artículo del nuevo artículo. *article_ID* es de **tipo int** y su valor predeterminado es null, y es un parámetro de salida.  
  
`[ @auto_identity_range = ] 'auto_identity_range'` Habilita y deshabilita el control automático del intervalo de identidad en una publicación en el momento en que se crea. *auto_identity_range* es de tipo **nvarchar (5)** y puede tener uno de los valores siguientes:  
  
|Value|Descripción|  
|-----------|-----------------|  
|**true**|Habilita la administración automática de intervalos de identidad|  
|**false**|Deshabilita la administración automática de intervalos de identidad|  
|NULL (valor predeterminado)|El control de intervalo de identidad se establece mediante *identityrangemanagementoption*.|  
  
> [!NOTE]  
>  *auto_identity_range* ha quedado en desuso y solo se proporciona por compatibilidad con versiones anteriores. Debe usar *identityrangemanagementoption* para especificar las opciones de administración de intervalos de identidad. Para más información, vea [Replicar columnas de identidad](../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
`[ @pub_identity_range = ] pub_identity_range` Controla el tamaño del intervalo en el publicador si el artículo tiene *identityrangemanagementoption* establecido en **auto** o *auto_identity_range* establecido en **true**. *pub_identity_range* es de tipo **BIGINT** y su valor predeterminado es NULL. *No se admite para publicadores de Oracle*.  
  
`[ @identity_range = ] identity_range` Controla el tamaño del intervalo en el suscriptor si el artículo tiene *identityrangemanagementoption* establecido en **auto** o *auto_identity_range* establecido en **true**. *identity_range* es de tipo **BIGINT** y su valor predeterminado es NULL. Se utiliza cuando *auto_identity_range* está establecido en **true**. *No se admite para publicadores de Oracle*.  
  
`[ @threshold = ] threshold` Es el valor de porcentaje que controla cuándo el Agente de distribución asigna un nuevo intervalo de identidad. Cuando se utiliza el porcentaje de valores especificado en *umbral* , el agente de distribución crea un nuevo intervalo de identidad. *Threshold* es de tipo **BIGINT** y su valor predeterminado es NULL. Se utiliza cuando el valor de *identityrangemanagementoption* está establecido en **auto** o *auto_identity_range* está establecido en **true**. *No se admite para publicadores de Oracle*.  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` Confirma que la acción realizada por este procedimiento almacenado puede invalidar una instantánea existente. *force_invalidate_snapshot* es de **bit** y su valor predeterminado es 0.  
  
 **0** especifica que al agregar un artículo no se hace que la instantánea no sea válida. Si el procedimiento almacenado detecta que el cambio requiere una nueva instantánea, se produce un error y no se realizan cambios.  
  
 **1** especifica que la adición de un artículo puede hacer que la instantánea no sea válida y, si existen suscripciones que requieran una nueva instantánea, concede permiso para que la instantánea existente se marque como obsoleta y se genere una nueva instantánea.  
  
`[ @use_default_datatypes = ] use_default_datatypes` Indica si se utilizan las asignaciones de tipos de datos de columna predeterminadas al publicar un artículo desde un publicador de Oracle. *use_default_datatypes* es de bit y su valor predeterminado es 1.  
  
 **1** = se usan las asignaciones de columnas de artículo predeterminadas. Las asignaciones de tipos de datos predeterminadas se pueden mostrar ejecutando [sp_getdefaultdatatypemapping](../../relational-databases/system-stored-procedures/sp-getdefaultdatatypemapping-transact-sql.md).  
  
 **0** = las asignaciones de columnas de artículo personalizadas están definidas y, por lo tanto, **sp_addarticle** no llama a [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md) .  
  
 Cuando *use_default_datatypes* se establece en **0**, debe ejecutar [sp_changearticlecolumndatatype](../../relational-databases/system-stored-procedures/sp-changearticlecolumndatatype-transact-sql.md) una vez por cada asignación de columna que se cambie de la predeterminada. Una vez definidas todas las asignaciones de columnas personalizadas, debe ejecutar [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md).  
  
> [!NOTE]  
>  Este parámetro solo debe utilizarse en publicadores de Oracle. Si se establece *use_default_datatypes* en **0** para un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publicador, se genera un error.  
  
`[ @identityrangemanagementoption = ] identityrangemanagementoption` Especifica cómo se controla la administración del intervalo de identidad para el artículo. *identityrangemanagementoption* es de tipo **nvarchar (10)** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Ninguna**|La replicación no realiza una administración de intervalos de identidad explícita. Se recomienda esta opción solo por compatibilidad con las versiones anteriores de SQL Server. No se permite en la replicación del mismo nivel.|  
|**Manual**|Marca la columna de identidad utilizando NOT FOR REPLICATION para habilitar la administración manual de intervalos de identidad.|  
|**auto**|Especifica la administración automática de intervalos de identidad.|  
|NULL (valor predeterminado)|Tiene como valor predeterminado **None** cuando el valor de *auto_identity_range* no es **true**. El valor predeterminado es **manual** en una topología punto a punto predeterminada (se omite *auto_identity_range* ).|  
  
 Por compatibilidad con versiones anteriores, cuando el valor de *identityrangemanagementoption* es null, se comprueba el valor de *auto_identity_range* . Sin embargo, cuando el valor de *identityrangemanagementoption* no es null, se omite el valor de *auto_identity_range* .  
  
 Para más información, vea [Replicar columnas de identidad](../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
`[ @publisher = ] 'publisher'` Especifica un publicador que no es de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  no se debe usar el *publicador* al agregar un artículo a un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publicador.  
  
`[ @fire_triggers_on_snapshot = ] 'fire_triggers_on_snapshot'` Es si se ejecutan desencadenadores de usuario replicados cuando se aplica la instantánea inicial. *fire_triggers_on_snapshot* es de tipo **nvarchar (5)** y su valor predeterminado es false. **true** significa que los desencadenadores de usuario en una tabla replicada se ejecutan cuando se aplica la instantánea. Para que se repliquen los desencadenadores, el valor de la máscara de la *schema_option* debe incluir el valor **0x100**.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_addarticle** se utiliza en la replicación de instantáneas o transaccional.  
  
 De forma predeterminada, la replicación no publica columnas en la tabla de origen cuando el tipo de datos de columna no se admite en la replicación. Si necesita publicar este tipo de columna, debe ejecutar [sp_articlecolumn](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md) para agregar la columna.  
  
 Al agregar un artículo a una publicación que admite la replicación transaccional punto a punto, se aplican las restricciones siguientes:  
  
-   Deben especificarse instrucciones con parámetros para todos los artículos de tipo logbased. Debe incluir **16** en el valor de *Estado* .  
  
-   El nombre y el propietario de la tabla de destino deben coincidir con los de la tabla de origen.  
  
-   El artículo no puede filtrarse horizontal ni verticalmente.  
  
-   No se admite la administración automática de intervalos de identidad. Debe especificar un valor de manual para *identityrangemanagementoption*.  
  
-   Si una columna de **marca** de tiempo existe en la tabla, debe incluir 0x08 en *schema_option* para replicar la columna como **marca** de tiempo.  
  
-   No se puede especificar un valor de **SQL** para *ins_cmd*, *upd_cmd* y *del_cmd*.  
  
 Para obtener más información, consulte [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md).  
  
 Al publicar objetos, sus definiciones se copian en los suscriptores. Si va a publicar un objeto de base de datos que depende de uno o varios objetos, debe publicar todos los objetos a los que hace referencia. Por ejemplo, si publica una vista que depende de una tabla, también debe publicar la tabla.  
  
 Si *vertical_partition* se establece en **true**, **sp_addarticle** aplaza la creación de la vista hasta que se llame a [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md) (después de que se haya agregado el último [sp_articlecolumn](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md) ).  
  
 Si la publicación permite suscripciones de actualización y la tabla publicada no tiene una columna **uniqueidentifier** , **sp_addarticle** agrega automáticamente una columna **uniqueidentifier** a la tabla.  
  
 Al replicar en un suscriptor que no es una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (replicación heterogénea), solo [!INCLUDE[tsql](../../includes/tsql-md.md)] se admiten instrucciones para los comandos **Insert**, **Update** y **Delete** .  
  
 Cuando el agente de registro del LOG se está ejecutando, al agregar un artículo a una publicación punto a punto se puede producir un interbloqueo entre el agente de registro del LOG y el proceso que agrega el artículo. Para evitar este problema, antes de agregar un artículo a la publicación punto a punto, use el Monitor de replicación para detener el agente de registro del LOG en el nodo donde se agrega el artículo. Reinicie el agente de registro del LOG después de agregar el artículo.  
  
 Al establecer `@del_cmd = 'NONE'` o `@ins_cmd = 'NONE'` , la propagación de comandos de **actualización** también podría verse afectada si no se envían los comandos cuando se produce una actualización enlazada. (Una actualización enlazada es el tipo de instrucción UPDATE del publicador que se replica como un par DELETE/INSERT en el suscriptor).  
  
## <a name="default-schema-options"></a>Opciones de esquema predeterminadas  
 En esta tabla se describe el valor predeterminado establecido por la replicación si el usuario no especifica *schema_options* , donde este valor depende del tipo de replicación (que se muestra en la parte superior) y del tipo de artículo (que se muestra en la primera columna).  
  
|Tipo de artículo|Replicación transaccional|Replicación de instantáneas|  
|------------------|----------------------|------|  
|**aggregate schema only**|**0x01**|**0x01**|  
|**func schema only**|**0x01**|**0x01**|  
|**indexed view schema only**|**0x01**|**0x01**|  
|**indexed view logbased**|**0x30F3**|**0x3071**|  
|**indexed view logbase manualboth**|**0x30F3**|**0x3071**|  
|**indexed view logbased manualfilter**|**0x30F3**|**0x3071**|  
|**indexed view logbased manualview**|**0x30F3**|**0x3071**|  
|**logbased**|**0x30F3**|**0x3071**|  
|**logbased manualfilter**|**0x30F3**|**0x3071**|  
|**logbased manualview**|**0x30F3**|**0x3071**|  
|**proc exec**|**0x01**|**0x01**|  
|**proc schema only**|**0x01**|**0x01**|  
|**serializable proc exec**|**0x01**|**0x01**|  
|**view schema only**|**0x01**|**0x01**|  
  
> [!NOTE]
>  Si una publicación está habilitada para la actualización en cola, se agrega un valor *schema_option* de **0x80** al valor predeterminado que se muestra en la tabla. La *schema_option* predeterminada para una publicación que no [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es de es **0x050D3**.  
  
## <a name="valid-schema-options"></a>Opciones de esquema válidas  
 En esta tabla se describen los valores permitidos de *schema_option* en función del tipo de replicación (que se muestra en la parte superior) y el tipo de artículo (que se muestra en la primera columna).  
  
|Tipo de artículo|Replicación transaccional|Replicación de instantáneas|  
|------------------|----------------------|------|  
|**logbased**|Todas las opciones|Todas las opciones pero **0x02**|  
|**logbased manualfilter**|Todas las opciones|Todas las opciones pero **0x02**|  
|**logbased manualview**|Todas las opciones|Todas las opciones pero **0x02**|  
|**indexed view logbased**|Todas las opciones|Todas las opciones pero **0x02**|  
|**indexed view logbased manualfilter**|Todas las opciones|Todas las opciones pero **0x02**|  
|**indexed view logbased manualview**|Todas las opciones|Todas las opciones pero **0x02**|  
|**indexed view logbase manualboth**|Todas las opciones|Todas las opciones pero **0x02**|  
|**proc exec**|**0x01**, **0x20**, **0x2000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x10000000**, **0x20000000**, **0x40000000** y **0x80000000**|**0x01**, **0x20**, **0x2000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x10000000**, **0x20000000**, **0x40000000** y **0x80000000**|  
|**serializable proc exec**|**0x01**, **0x20**, **0x2000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x10000000**, **0x20000000**, **0x40000000** y **0x80000000**|**0x01**, **0x20**, **0x2000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x10000000**, **0x20000000**, **0x40000000** y **0x80000000**|  
|**proc schema only**|**0x01**, **0x20**, **0x2000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x10000000**, **0x20000000**, **0x40000000** y **0x80000000**|**0x01**, **0x20**, **0x2000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x10000000**, **0x20000000**, **0x40000000** y **0x80000000**|  
|**view schema only**|**0x01**, **0x010**, **0x020**, **0x040**, **0x0100**, **0x2000**, **0x40000**, **0x100000**, **0x200000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x40000000** y **0x80000000**|**0x01**, **0x010**, **0x020**, **0x040**, **0x0100**, **0x2000**, **0x40000**, **0x100000**, **0x200000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x40000000** y **0x80000000**|  
|**func schema only**|**0x01**, **0x20**, **0x2000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x10000000**, **0x20000000**, **0x40000000** y **0x80000000**|**0x01**, **0x20**, **0x2000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x10000000**, **0x20000000**, **0x40000000** y **0x80000000**|  
|**indexed view schema only**|**0x01**, **0x010**, **0x020**, **0x040**, **0x0100**, **0x2000**, **0x40000**, **0x100000**, **0x200000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x40000000** y **0x80000000**|**0x01**, **0x010**, **0x020**, **0x040**, **0x0100**, **0x2000**, **0x40000**, **0x100000**, **0x200000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x40000000** y **0x80000000**|  
  
> [!NOTE]
>  En el caso de las publicaciones de actualización en cola, los valores de *schema_option* de **0x8000** y **0x80** deben estar habilitados. Los valores de *schema_option* admitidos para las publicaciones que no [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] son de son: **0x01**, **0x02**, **0x10**, **0x40**, **0x80**, **0x1000**, **0x4000** y **0x8000**.  
  
## <a name="example"></a>Ejemplo  
 [!code-sql[HowTo#sp_AddTranArticle](../../relational-databases/replication/codesnippet/tsql/sp-addarticle-transact-sql_1.sql)]  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_addarticle**.  
  
## <a name="see-also"></a>Consulte también  
 [Define an Article](../../relational-databases/replication/publish/define-an-article.md)   
 [sp_articlecolumn &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md)   
 [sp_articlefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md)   
 [sp_articleview &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md)   
 [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_droparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md)   
 [sp_helparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)   
 [sp_helparticlecolumns &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helparticlecolumns-transact-sql.md)   
 [Procedimientos almacenados de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [Publicar datos y objetos de base de datos](../../relational-databases/replication/publish/publish-data-and-database-objects.md)  
  
  
