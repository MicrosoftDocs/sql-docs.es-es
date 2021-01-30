---
description: sp_addmergearticle (Transact-SQL)
title: sp_addmergearticle (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addmergearticle
- sp_addmergearticle_TSQL
helpviewer_keywords:
- sp_addmergearticle
ms.assetid: 0df654ea-24e2-4c61-a75a-ecaa7a140a6c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 52e59c1c4c7a75c209be99cbb6caacc425bd39f3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192355"
---
# <a name="sp_addmergearticle-transact-sql"></a>sp_addmergearticle (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Agrega un artículo a una publicación de combinación existente. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_addmergearticle [ @publication = ] 'publication'   
        , [ @article = ] 'article'   
        , [ @source_object = ] 'source_object'   
    [ , [ @type = ] 'type' ]   
    [ , [ @description = ] 'description' ]   
    [ , [ @column_tracking = ] 'column_tracking' ]   
    [ , [ @status = ] 'status' ]   
    [ , [ @pre_creation_cmd = ] 'pre_creation_cmd' ]   
    [ , [ @creation_script = ] 'creation_script' ]   
    [ , [ @schema_option = ] schema_option ]   
    [ , [ @subset_filterclause = ] 'subset_filterclause' ]   
    [ , [ @article_resolver = ] 'article_resolver' ]   
    [ , [ @resolver_info = ] 'resolver_info' ]   
    [ , [ @source_owner = ] 'source_owner' ]   
    [ , [ @destination_owner = ] 'destination_owner' ]   
    [ , [ @vertical_partition = ] 'vertical_partition' ]   
    [ , [ @auto_identity_range = ] 'auto_identity_range' ]   
    [ , [ @pub_identity_range = ] pub_identity_range ]   
    [ , [ @identity_range = ] identity_range ]   
    [ , [ @threshold = ] threshold ]   
    [ , [ @verify_resolver_signature = ] verify_resolver_signature ]   
    [ , [ @destination_object = ] 'destination_object' ]   
    [ , [ @allow_interactive_resolver = ] 'allow_interactive_resolver' ]   
    [ , [ @fast_multicol_updateproc = ] 'fast_multicol_updateproc' ]   
    [ , [ @check_permissions = ] check_permissions ]   
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @published_in_tran_pub = ] 'published_in_tran_pub' ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
    [ , [ @logical_record_level_conflict_detection = ] 'logical_record_level_conflict_detection' ]  
    [ , [ @logical_record_level_conflict_resolution = ] 'logical_record_level_conflict_resolution' ]  
    [ , [ @partition_options = ] partition_options ]  
    [ , [ @processing_order = ] processing_order ]  
    [ , [ @subscriber_upload_options = ] subscriber_upload_options ]  
    [ , [ @identityrangemanagementoption = ] 'identityrangemanagementoption' ]  
    [ , [ @delete_tracking = ] delete_tracking ]  
    [ , [ @compensate_for_errors = ] 'compensate_for_errors' ]   
    [ , [ @stream_blob_columns = ] 'stream_blob_columns' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación que contiene el artículo. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @article = ] 'article'` Es el nombre del artículo. El nombre debe ser único en la publicación. *article* es de **tipo sysname** y no tiene ningún valor predeterminado. el *artículo* debe estar en el equipo local que ejecuta [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y debe ajustarse a las reglas de los identificadores.  
  
`[ @source_object = ] 'source_object'` Es el objeto de base de datos que se va a publicar. *source_object* es de **tipo sysname** y no tiene ningún valor predeterminado. Para obtener más información sobre los tipos de objetos que se pueden publicar mediante la replicación de mezcla, vea [publicar datos y objetos de base de datos](../../relational-databases/replication/publish/publish-data-and-database-objects.md).  
  
`[ @type = ] 'type'` Es el tipo de artículo. *Type* es de tipo **sysname, su** valor predeterminado es **TABLE** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**tabla** (valor predeterminado)|Tabla con esquema y datos. La replicación supervisa la tabla para determinar los datos que se van a replicar.|  
|**func schema only**|Función con solo esquema.|  
|**solo esquema** de **vista indizada**|Vista indizada con solo esquema.|  
|**proc schema only**|Procedimiento almacenado con solo esquema.|  
|**solo esquema de sinónimos**|Sinónimo con solo esquema.|  
|**view schema only**|Vista con solo esquema.|  
  
`[ @description = ] 'description'` Es una descripción del artículo. la *Descripción* es de tipo **nvarchar (255)** y su valor predeterminado es NULL.  
  
`[ @column_tracking = ] 'column_tracking'` Es el valor de seguimiento de nivel de columna. *column_tracking* es de tipo **nvarchar (10)** y su valor predeterminado es false. **true** activa el seguimiento de columnas. **false** desactiva el seguimiento de columnas y deja la detección de conflictos en el nivel de fila. Si la tabla ya está publicada en otras publicaciones de combinación, debe utilizar el mismo valor de seguimiento por columna que utilizan los artículos existentes basados en esta tabla. Este parámetro es específico solamente de los artículos de tabla.  
  
> [!NOTE]  
>  Si se utiliza el seguimiento por fila en la detección de conflictos (valor predeterminado), la tabla base puede incluir un máximo de 1024 columnas, pero en el artículo deben filtrarse las columnas de forma que se publique un máximo de 246 columnas. Si se utiliza el seguimiento por columna, la tabla base puede incluir 246 columnas como máximo.  
  
`[ @status = ] 'status'` Es el estado del artículo. *status* es **nvarchar (10)** y su valor predeterminado es **Unsynced**. Si está **activo**, se ejecuta el script de procesamiento inicial para publicar la tabla. Si no está **sincronizado**, el script de procesamiento inicial para publicar la tabla se ejecutará la próxima vez que se ejecute el agente de instantáneas.  
  
`[ @pre_creation_cmd = ] 'pre_creation_cmd'` Especifica lo que debe hacer el sistema si la tabla existe en el suscriptor al aplicar la instantánea. *pre_creation_cmd* es **nvarchar (10)** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Ninguna**|Si la tabla ya existe en el suscriptor, no se lleva a cabo ninguna acción.|  
|**delete**|Emite una eliminación basada en la cláusula WHERE del filtro de subconjunto.|  
|**Drop** (valor predeterminado)|Quita la tabla antes de volver a crearla. Necesario para admitir [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssEW](../../includes/ssew-md.md)] suscriptores.|  
|**truncate**|Trunca la tabla de destino.|  
  
`[ @creation_script = ] 'creation_script'` Es la ruta de acceso y el nombre de un script de esquema de artículo opcional que se usa para crear el artículo en la base de datos de suscripciones. *creation_script* es de tipo **nvarchar (255)** y su valor predeterminado es NULL.  
  
> [!NOTE]  
>  Los scripts de creación no se ejecutan en suscriptores de [!INCLUDE[ssEW](../../includes/ssew-md.md)].  
  
`[ @schema_option = ] schema_option` Es un mapa de bits de la opción de generación de esquemas para el artículo dado. *schema_option* es **binario (8)** y puede ser el [| (OR bit a bit)](../../t-sql/language-elements/bitwise-or-transact-sql.md) producto de uno o varios de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**0x00**|Deshabilita el scripting por el Agente de instantáneas y usa el script de creación previa de esquemas proporcionado definido en *creation_script*.|  
|**0x01**|Genera la creación del objeto (CREATE TABLE, CREATE PROCEDURE, etc.). Este es el valor predeterminado en los artículos de procedimientos almacenados.|  
|**0x10**|Genera el índice clúster correspondiente. Aunque esta opción no esté establecida, se generarán índices relacionados con claves principales y restricciones UNIQUE si ya están definidos en una tabla publicada.|  
|**0x20**|Convierte los tipos de datos definidos por el usuario (UDT) en tipos de datos base en el suscriptor. Esta opción no se puede utilizar si existe una restricción CHECK o DEFAULT en una columna UDT, si una columna UDT forma parte de la clave principal o si una columna calculada hace referencia a una columna UDT.|  
|**0x40**|Genera los índices no clúster correspondientes. Aunque esta opción no esté establecida, se generarán índices relacionados con claves principales y restricciones UNIQUE si ya están definidos en una tabla publicada.|  
|**0x80**|Replica las restricciones PRIMARY KEY. También se replican los índices relacionados con la restricción, aunque las opciones **0x10** y **0x40** no estén habilitadas.|  
|**0x100**|Replica los desencadenadores de usuario en un artículo de tabla, si se han definido.|  
|**0x200**|Replica restricciones FOREIGN KEY. Si la tabla a la que se hace referencia no forma parte de una publicación, no se replica ninguna restricción FOREIGN KEY en una tabla publicada.|  
|**0x400**|Replica las restricciones CHECK.|  
|**0x800**|Replica los valores predeterminados.|  
|**0x1000**|Replica la intercalación de columna.|  
|**0x2000**|Replica las propiedades extendidas asociadas con el objeto de origen del artículo publicado.|  
|**0x4000**|Replica las restricciones UNIQUE. También se replican los índices relacionados con la restricción, aunque las opciones **0x10** y **0x40** no estén habilitadas.|  
|**0x8000**|Esta opción no es válida para publicadores que ejecutan [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] o versiones posteriores.|  
|**0x10000**|Replica las restricciones CHECK como NOT FOR REPLICATION de manera que no se impongan durante la sincronización.|  
|**0x20000**|Replica las restricciones FOREIGN KEY como NOT FOR REPLICATION de manera que no se impongan durante la sincronización.|  
|**0x40000**|Replica grupos de archivos asociados con un índice o una tabla con particiones.|  
|**0x80000**|Replica el esquema de partición de una tabla con particiones.|  
|**0x100000**|Replica el esquema de partición de un índice con particiones.|  
|**0x200000**|Replica las estadísticas de tabla.|  
|**0x400000**|Replica los enlaces predeterminados.|  
|**0x800000**|Replica los enlaces de reglas.|  
|**0x1000000**|Replica el índice de texto completo.|  
|**0x2000000**|Las colecciones de esquemas XML enlazadas a columnas **XML** no se replican.|  
|**0x4000000**|Replica índices en columnas **XML** .|  
|**0x8000000**|Crea los esquemas que no están ya presentes en el suscriptor.|  
|**0x10000000**|Convierte las columnas **XML** en **ntext** en el suscriptor.|  
|**0x20000000**|Convierte los tipos de datos de objetos grandes (**nvarchar (Max)**, **VARCHAR (Max)** y **varbinary (Max)**) introducidos en en [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] tipos de datos que se admiten en [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] .|  
|**0x40000000**|Replica permisos.|  
|**0x80000000**|Intenta quitar dependencias a objetos que no forman parte de la publicación.|  
|**0x100000000**|Use esta opción para replicar el atributo FILESTREAM si se especifica en columnas **varbinary (Max)** . No especifique esta opción si replica tablas en suscriptores de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. No se admite la replicación de tablas que tienen columnas FILESTREAM en [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] suscriptores, independientemente de cómo se establezca esta opción de esquema. Vea la opción relacionada **0x800000000**.|  
|**0x200000000**|Convierte los tipos de datos de fecha y hora (**Date**, **Time**, **DateTimeOffset** y **datetime2**) introducidos en en [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] tipos de datos que se admiten en versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**0x400000000**|Replica la opción de compresión para los datos y los índices. Para obtener más información, consulte [Data Compression](../../relational-databases/data-compression/data-compression.md).|  
|**0x800000000**|Establezca esta opción para almacenar los datos de FILESTREAM en su propio grupo de archivos en el suscriptor. Si no se establece esta opción, los datos de FILESTREAM se almacenan en el grupo de archivos predeterminado. La replicación no crea grupos de archivos; por tanto, si establece esta opción, debe crear el grupo de archivos antes de aplicar la instantánea en el suscriptor. Para obtener más información sobre cómo crear objetos antes de aplicar la instantánea, vea [ejecutar scripts antes y después de aplicar la instantánea](../../relational-databases/replication/snapshot-options.md#execute-scripts-before-and-after-snapshot-is-applied).<br /><br /> Vea la opción relacionada **0x100000000**.|  
|**0x1000000000**|Convierte los tipos definidos por el usuario (UDT) de Common Language Runtime (CLR) a **varbinary (Max)** para que las columnas de tipo UDT se puedan replicar en los suscriptores que ejecutan [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .|  
|**0x2000000000**|Convierte el tipo de datos **hierarchyid** a **varbinary (Max)** para que las columnas de tipo **hierarchyid** se puedan replicar en los suscriptores que ejecutan [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] . Para obtener más información sobre cómo usar las columnas **hierarchyid** en tablas replicadas, vea [hierarchyid &#40;Transact-SQL&#41;](../../t-sql/data-types/hierarchyid-data-type-method-reference.md).|  
|**0x4000000000**|Replica los índices filtrados de la tabla. Para obtener más información acerca de los índices filtrados, vea [crear índices filtrados](../../relational-databases/indexes/create-filtered-indexes.md).|  
|**0x8000000000**|Convierte los tipos de datos **Geography** y **Geometry** en **varbinary (Max)** para que las columnas de estos tipos se puedan replicar en los suscriptores que ejecutan [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .|  
|**0x10000000000**|Replica los índices en columnas de tipo **Geography** y **Geometry**.|  
  
 Si este valor es NULL, el sistema genera automáticamente una opción de esquema válida para el artículo. La tabla predeterminada de la **opción de esquema** de la sección Comentarios muestra el valor que se elige en función del tipo de artículo. Además, no todos los valores *schema_option* son válidos para cada tipo de replicación y tipo de artículo. La tabla de **Opciones de esquema válidas** que se proporciona en la sección Notas muestra las opciones que se pueden especificar para un tipo de artículo determinado.  
  
> [!NOTE]  
>  El parámetro *schema_option* solo afecta a las opciones de replicación de la instantánea inicial. Una vez que el Agente de instantáneas ha generado el esquema inicial y se ha aplicado en el suscriptor, la replicación de los cambios de esquema de publicación en el suscriptor se realiza en función de las reglas de replicación de cambios de esquema y la configuración del parámetro *replicate_ddl* especificada en [sp_addmergepublication](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md). Para más información, vea [Realizar cambios de esquema en bases de datos de publicaciones](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).  
  
`[ @subset_filterclause = ] 'subset_filterclause'` Es una cláusula WHERE que especifica el filtrado horizontal de un artículo de tabla sin la palabra en la que se incluye. *subset_filterclause* es de tipo **nvarchar (1000)** y su valor predeterminado es una cadena vacía.  
  
> [!IMPORTANT]  
>  Por motivos de rendimiento, se recomienda que no aplique funciones a nombres de columnas en las cláusulas de los filtros de fila con parámetros, como `LEFT([MyColumn]) = SUSER_SNAME()`. Si utiliza [host_name](../../t-sql/functions/host-name-transact-sql.md) en una cláusula de filtro y reemplaza el valor host_name, es posible que tenga que convertir los tipos de datos mediante [Convert](../../t-sql/functions/cast-and-convert-transact-sql.md). Para obtener más información sobre los procedimientos recomendados para este caso, vea la sección "invalidar el valor HOST_NAME ()" en [filtros de fila con parámetros](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
`[ @article_resolver = ] 'article_resolver'` Es el solucionador basado en COM que se usa para resolver conflictos en el artículo de tabla o en el ensamblado de .NET Framework invocado para ejecutar la lógica de negocios personalizada en el artículo de la tabla. *article_resolver* es de tipo **VARCHAR (255)** y su valor predeterminado es NULL. Los valores disponibles para este parámetro se enumeran en los solucionadores personalizados de [!INCLUDE[msCoName](../../includes/msconame-md.md)]. Si el valor proporcionado no es uno de los solucionadores de [!INCLUDE[msCoName](../../includes/msconame-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utiliza el solucionador especificado en lugar del solucionador proporcionado por el sistema. Utilice **sp_enumcustomresolvers** para enumerar la lista de solucionadores personalizados disponibles. Para obtener más información, vea [Ejecutar lógica de negocios durante la sincronización de mezcla](../../relational-databases/replication/merge/execute-business-logic-during-merge-synchronization.md) y [detección y resolución de conflictos de replicación de mezcla avanzada](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md).  
  
`[ @resolver_info = ] 'resolver_info'` Se utiliza para especificar la información adicional requerida por un solucionador personalizado. Algunos de los solucionadores de [!INCLUDE[msCoName](../../includes/msconame-md.md)] requieren que se proporcione una columna como entrada para el solucionador. *resolver_info* es de tipo **nvarchar (255)** y su valor predeterminado es NULL. Para obtener más información, consulte [Solucionadores basados en Microsoft COM](../../relational-databases/replication/merge/advanced-merge-replication-conflict-com-based-resolvers.md).  
  
`[ @source_owner = ] 'source_owner'` Es el nombre del propietario del *source_object*. *source_owner* es de **tipo sysname y su** valor predeterminado es NULL. Si es NULL, se presupone que el usuario actual es el propietario.  
  
`[ @destination_owner = ] 'destination_owner'` Es el propietario del objeto en la base de datos de suscripciones, si no es ' DBO '. *destination_owner* es de **tipo sysname y su** valor predeterminado es NULL. Si es NULL, se presupone que el propietario es 'dbo'.  
  
`[ @vertical_partition = ] 'column_filter'` Habilita y deshabilita el filtrado de columnas en un artículo de tabla. *vertical_partition* es de tipo **nvarchar (5)** y su valor predeterminado es false.  
  
 **false** indica que no hay filtrado vertical y publica todas las columnas.  
  
 **true** borra todas las columnas excepto la clave principal declarada y las columnas ROWGUID. Las columnas se agregan mediante **sp_mergearticlecolumn**.  
  
`[ @auto_identity_range = ] 'automatic_identity_range'` Habilita y deshabilita el control automático de intervalos de identidad para este artículo de tabla en una publicación en el momento en que se crea. *auto_identity_range* es de tipo **nvarchar (5)** y su valor predeterminado es false. **true** habilita el control automático del intervalo de identidad, mientras que **false** lo deshabilita.  
  
> [!NOTE]  
>  *auto_identity_range* ha quedado en desuso y solo se proporciona por compatibilidad con versiones anteriores. Debe usar *identityrangemanagementoption* para especificar las opciones de administración de intervalos de identidad. Para más información, vea [Replicar columnas de identidad](../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
`[ @pub_identity_range = ] pub_identity_range` Controla el tamaño del intervalo de identidad asignado a un suscriptor con una suscripción de servidor cuando se utiliza la administración automática de intervalos de identidad. Este rango de identidad se reserva para que un suscriptor de republicación pueda realizar asignaciones a sus propios suscriptores. *pub_identity_range* es de tipo **BIGINT** y su valor predeterminado es NULL. Debe especificar este parámetro si *identityrangemanagementoption* es **auto** o si *auto_identity_range* es **true**.  
  
`[ @identity_range = ] identity_range` Controla el tamaño del intervalo de identidad asignado al publicador y al suscriptor cuando se utiliza la administración automática del intervalo de identidad. *identity_range* es de tipo **BIGINT** y su valor predeterminado es NULL. Debe especificar este parámetro si *identityrangemanagementoption* es **auto** o si *auto_identity_range* es **true**.  
  
> [!NOTE]  
>  *identity_range* controla el tamaño del intervalo de identidad en los suscriptores de republicación que utilizan versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
`[ @threshold = ] threshold` Valor de porcentaje que controla cuándo el Agente de mezcla asigna un nuevo intervalo de identidad. Cuando se utiliza el porcentaje de valores especificado en *umbral* , el agente de mezcla crea un nuevo intervalo de identidad. *Threshold* es de **tipo int** y su valor predeterminado es NULL. Debe especificar este parámetro si *identityrangemanagementoption* es **auto** o si *auto_identity_range* es **true**.  
  
`[ @verify_resolver_signature = ] verify_resolver_signature` Especifica si se comprueba una firma digital antes de utilizar un solucionador en la replicación de mezcla. *verify_resolver_signature* es de **tipo int** y su valor predeterminado es 1.  
  
 **0** especifica que no se comprobará la firma.  
  
 **1** especifica que se comprobará la firma para ver si procede de un origen de confianza.  
  
`[ @destination_object = ] 'destination_object'` Es el nombre del objeto en la base de datos de suscripciones. *destination_object* es de **tipo sysname y su** valor predeterminado es lo que se encuentra en **\@ source_object**. Este parámetro se puede especificar únicamente si el artículo es de solo esquema, como procedimientos almacenados, vistas y UDF. Si el artículo especificado es un artículo de tabla, el valor de *\@ source_object* invalida el valor de *destination_object*.  
  
`[ @allow_interactive_resolver = ] 'allow_interactive_resolver'` Habilita o deshabilita el uso del solucionador interactivo en un artículo. *allow_interactive_resolver* es de tipo **nvarchar (5)** y su valor predeterminado es false. **true** habilita el uso del solucionador interactivo en el artículo; **false** lo deshabilita.  
  
> [!NOTE]  
>  El Solucionador interactivo no se admite en suscriptores de [!INCLUDE[ssEW](../../includes/ssew-md.md)].  
  
`[ @fast_multicol_updateproc = ] 'fast_multicol_updateproc'` Este parámetro está en desuso y se mantiene por compatibilidad con versiones anteriores de los scripts.  
  
`[ @check_permissions = ] check_permissions` Es un mapa de bits de los permisos de nivel de tabla que se comprueban cuando el Agente de mezcla aplica los cambios al publicador. Si la cuenta de inicio de sesión o usuario del publicador que utiliza el proceso de mezcla no dispone de los permisos de tabla correctos, los cambios no válidos se registran como conflictos. *check_permissions* es de **tipo int** y puede ser el valor de [| (OR bit a bit)](../../t-sql/language-elements/bitwise-or-transact-sql.md) producto de uno o varios de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**0x00** (valor predeterminado)|Los permisos no se comprueban.|  
|**0x10**|Comprueba los permisos en el publicador antes de cargar las operaciones de inserción creadas en un suscriptor.|  
|**0x20**|Comprueba los permisos en el publicador antes de cargar las operaciones de actualización creadas en un suscriptor.|  
|**0x40**|Comprueba los permisos en el publicador antes de cargar las operaciones de eliminación creadas en un suscriptor.|  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` Confirma que la acción realizada por este procedimiento almacenado puede invalidar una instantánea existente. *force_invalidate_snapshot* es de **bit** y su valor predeterminado es 0.  
  
 **0** especifica que al agregar un artículo no se hace que la instantánea no sea válida. Si el procedimiento almacenado detecta que el cambio requiere una nueva instantánea, se producirá un error y no se realizarán cambios.  
  
 **1** especifica que la adición de un artículo puede hacer que la instantánea no sea válida y, si hay suscripciones existentes que requieran una nueva instantánea, concede permiso para que la instantánea existente se marque como obsoleta y se genere una nueva instantánea. *force_invalidate_snapshot* se establece en **1** al agregar un artículo a una publicación con una instantánea existente.  
  
`[ @published_in_tran_pub = ] 'published_in_tran_pub'` Indica que un artículo de una publicación de combinación también se publica en una publicación transaccional. *published_in_tran_pub* es de tipo **nvarchar (5)** y su valor predeterminado es false. **true** especifica que el artículo también se publica en una publicación transaccional.  
  
`[ @force_reinit_subscription = ] force_reinit_subscription` Confirma que la acción realizada por este procedimiento almacenado puede requerir que se reinicialicen las suscripciones existentes. *force_reinit_subscription* es de **bit** y su valor predeterminado es 0.  
  
 **0** especifica que al agregar un artículo no se reinicializa la suscripción. Si el procedimiento almacenado detecta que el cambio requiere la reinicialización de las suscripciones existentes, se producirá un error y no se realizarán cambios.  
  
 **1** significa que los cambios en el artículo de mezcla hacen que se reinicialicen las suscripciones existentes y concede permiso para que se produzca la reinicialización de la suscripción. *force_reinit_subscription* se establece en **1** cuando *subset_filterclause* especifica un filtro de fila con parámetros.  
  
`[ @logical_record_level_conflict_detection = ] 'logical_record_level_conflict_detection'` Especifica el nivel de detección de conflictos para un artículo que es miembro de un registro lógico. *logical_record_level_conflict_detection* es de tipo **nvarchar (5)** y su valor predeterminado es false.  
  
 **true** especifica que se detectará un conflicto si se realizan cambios en cualquier lugar del registro lógico.  
  
 **false** especifica que la detección de conflictos predeterminada se usa tal y como se especifica en *column_tracking*. Para más información, vea [Agrupar cambios en filas relacionadas con registros lógicos](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).  
  
> [!NOTE]  
>  Dado que los suscriptores no admiten registros lógicos [!INCLUDE[ssEW](../../includes/ssew-md.md)] , debe especificar un valor de **false** para que *logical_record_level_conflict_detection* admita estos suscriptores.  
  
`[ @logical_record_level_conflict_resolution = ] 'logical_record_level_conflict_resolution'` Especifica el nivel de resolución de conflictos para un artículo que es miembro de un registro lógico. *logical_record_level_conflict_resolution* es de tipo **nvarchar (5)** y su valor predeterminado es false.  
  
 **true** especifica que todo el registro lógico ganador sobrescribe el registro lógico perdedor.  
  
 **false** especifica que las filas ganadoras no se restringen al registro lógico. Si *logical_record_level_conflict_detection* es **true**, *logical_record_level_conflict_resolution* también se debe establecer en **true**. Para más información, vea [Agrupar cambios en filas relacionadas con registros lógicos](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).  
  
> [!NOTE]  
>  Dado que los suscriptores no admiten registros lógicos [!INCLUDE[ssEW](../../includes/ssew-md.md)] , debe especificar un valor de **false** para que *logical_record_level_conflict_resolution* admita estos suscriptores.  
  
`[ @partition_options = ] partition_options` Define la manera en que se crean las particiones de los datos en el artículo, lo que permite optimizar el rendimiento cuando todas las filas pertenecen solo a una partición o a una sola suscripción. *partition_options* es **tinyint** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**0** (valor predeterminado)|El filtrado para el artículo es estático o no produce un subconjunto único de datos para cada partición, es decir, una partición "superpuesta".|  
|**1**|Las particiones se superponen y las actualizaciones del lenguaje de manipulación de datos (DML) realizadas en el suscriptor no pueden cambiar la partición a la que pertenece la fila.|  
|**2**|El filtro para el artículo produce particiones no superpuestas, pero varios suscriptores pueden recibir la misma partición.|  
|**3**|El filtro para el artículo produce particiones no superpuestas que son exclusivas para cada suscripción.|  
  
> [!NOTE]  
>  Si la tabla de origen de un artículo ya está publicada en otra publicación, el valor de *partition_options* debe ser el mismo para ambos artículos.  
  
`[ @processing_order = ] processing_order` Indica el orden de procesamiento de los artículos en una publicación de combinación. *processing_order* es de **tipo int** y su valor predeterminado es 0. **0** especifica que el artículo está desordenado y cualquier otro valor representa el valor ordinal del orden de procesamiento para este artículo. Los artículos se procesan en orden desde el valor menor al mayor. Si dos artículos tienen el mismo valor, el orden en el que se procesa el alias del artículo en la tabla del sistema [sysmergearticles](../../relational-databases/system-tables/sysmergearticles-transact-sql.md) determina el orden de procesamiento. Para más información, vea [Specify merge replication properties](../../relational-databases/replication/merge/specify-merge-replication-properties.md) (Especificación de propiedades de replicación de mezcla).  
  
`[ @subscriber_upload_options = ] subscriber_upload_options` Define las restricciones en las actualizaciones realizadas en un suscriptor con una suscripción de cliente. Para más información, vea [Optimizar el rendimiento de la replicación de mezcla con artículos de solo descarga](../../relational-databases/replication/merge/optimize-merge-replication-performance-with-download-only-articles.md). *subscriber_upload_options* es **tinyint** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**0** (valor predeterminado)|Sin restricciones. Los cambios realizados en el suscriptor se cargan en el publicador.|  
|**1**|Se permiten cambios en el suscriptor, pero éstos no se cargan en el publicador.|  
|**2**|No se permiten cambios en el suscriptor.|  
  
 El cambio de *subscriber_upload_options* requiere que se reinicialice la suscripción mediante una llamada a [sp_reinitmergepullsubscription &#40;&#41;de Transact-SQL](../../relational-databases/system-stored-procedures/sp-reinitmergepullsubscription-transact-sql.md).  
  
> [!NOTE]  
>  Si la tabla de origen de un artículo ya está publicada en otra publicación, el valor de *subscriber_upload_options* debe ser el mismo para ambos artículos.  
  
`[ @identityrangemanagementoption = ] identityrangemanagementoption` Especifica cómo se controla la administración del intervalo de identidad para el artículo. *identityrangemanagementoption* es de tipo **nvarchar (10)** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Ninguna**|Deshabilita la administración de intervalos de identidad.|  
|**Manual**|Marca la columna de identidad utilizando NOT FOR REPLICATION para habilitar la administración manual de intervalos de identidad.|  
|**auto**|Especifica la administración automática de intervalos de identidad.|  
|NULL (valor predeterminado)|Tiene como valor predeterminado **None** cuando el valor de *auto_identity_range* no es **true**.|  
  
 Por compatibilidad con versiones anteriores, cuando el valor de *identityrangemanagementoption* es null, se comprueba el valor de *auto_identity_range* . Sin embargo, cuando el valor de *identityrangemanagementoption* no es null, se omite el valor de *auto_identity_range* . Para más información, vea [Replicar columnas de identidad](../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
`[ @delete_tracking = ] 'delete_tracking'` Indica si se replican las eliminaciones. *delete_tracking* es de tipo **nvarchar (5)** y su valor predeterminado es true. **false** indica que las eliminaciones no se replican y **true** indica que las eliminaciones se replican, que es el comportamiento habitual de la replicación de mezcla. Cuando *delete_tracking* está establecido en **false**, las filas eliminadas en el suscriptor deben quitarse manualmente en el publicador, y las filas eliminadas en el publicador deben quitarse manualmente en el suscriptor.  
  
> [!IMPORTANT]  
>  Si se establece *delete_tracking* en **false** , se produce una no convergencia. Si la tabla de origen de un artículo ya está publicada en otra publicación, el valor de *delete_tracking* debe ser el mismo para ambos artículos.  
  
> [!NOTE]  
>  *delete_tracking* opciones no se pueden establecer mediante el **Asistente para nueva publicación** o el cuadro de diálogo Propiedades de la **publicación** .  
  
`[ @compensate_for_errors = ] 'compensate_for_errors'` Indica si se realizan acciones de compensación cuando se producen errores durante la sincronización. *compensate_for_errors* es de tipo **nvarchar (5)** y su valor predeterminado es false. Cuando se establece en **true**, los cambios que no se pueden aplicar en un suscriptor o publicador durante la sincronización siempre conducen a acciones de compensación para deshacer el cambio; sin embargo, un suscriptor configurado incorrectamente que genera un error puede hacer que se deshagan los cambios en otros suscriptores y publicadores. **false** deshabilita estas acciones de compensación; sin embargo, los errores todavía se registran como con compensación y las combinaciones posteriores continúan intentando aplicar los cambios hasta que se realiza correctamente.  
  
> [!IMPORTANT]  
>  Aunque parezca que los datos de las filas afectadas no tengan convergencia, en cuanto trate los errores, se podrán aplicar los cambios y los datos convergerán. Si la tabla de origen de un artículo ya está publicada en otra publicación, el valor de *compensate_for_errors* debe ser el mismo para ambos artículos.  
  
`[ @stream_blob_columns = ] 'stream_blob_columns'` Especifica que se debe usar una optimización del flujo de datos al replicar columnas de objetos binarios grandes. *stream_blob_columns* es de tipo **nvarchar (5)** y su valor predeterminado es false. **true** significa que se intentará la optimización. *stream_blob_columns* se establece en true cuando se habilita FileStream. Esto permite que la replicación de datos FILESTREAM se realice de forma óptima y que se reduzca el uso de memoria. Para forzar que los artículos de tabla FILESTREAM no usen streaming de blobs, use **sp_changemergearticle** para establecer *stream_blob_columns* en false.  
  
> [!IMPORTANT]  
>  Si se habilita esta optimización de la memoria, el rendimiento del Agente de mezcla podría verse afectado durante la sincronización. Esta opción solo se debe utilizar al replicar columnas que contienen megabytes de datos.  
  
> [!NOTE]  
>  Ciertas funcionalidades de la replicación de mezcla, como los registros lógicos, pueden impedir que se utilice la optimización de la secuencia al replicar objetos binarios grandes incluso con *stream_blob_columns* establecida en **true**.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_addmergearticle** se utiliza en la replicación de mezcla.  
  
 Al publicar objetos, sus definiciones se copian en los suscriptores. Si va a publicar un objeto de base de datos que depende de uno o varios objetos, debe publicar todos los objetos a los que hace referencia. Por ejemplo, si publica una vista que depende de una tabla, también debe publicar la tabla.  
  
 Si especifica un valor de **3** para *partition_options*, solo puede haber una suscripción para cada partición de datos de ese artículo. Si se crea una segunda partición en la que el criterio de filtrado de la nueva suscripción se resuelve en la misma partición que la suscripción existente, se quitará la suscripción existente.  
  
 Al especificar un valor de 3 para *partition_options*, los metadatos se limpian cada vez que se ejecuta el agente de mezcla y la instantánea con particiones expira más rápidamente. Al utilizar esta opción, debe considerar la habilitación de la instantánea con particiones solicitada por el suscriptor. Para más información, consulte [Instantáneas para publicaciones de combinación con filtros con parámetros](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  
 La adición de un artículo con un filtro estático horizontal, con *subset_filterclause*, a una publicación existente con artículos que tienen filtros con parámetros requiere que se reinicialicen las suscripciones.  
  
 Al especificar *processing_order*, se recomienda que se dejen huecos entre los valores de orden del artículo, lo que facilita el establecimiento de nuevos valores en el futuro. Por ejemplo, si tiene tres artículos: artículo1, Artículo2 y Artículo3, establezca *processing_order* en 10, 20 y 30, en lugar de en 1, 2 y 3. Para más información, vea [Specify merge replication properties](../../relational-databases/replication/merge/specify-merge-replication-properties.md) (Especificación de propiedades de replicación de mezcla).  
  
## <a name="default-schema-option-table"></a>Tabla Opciones de esquema predeterminadas  
 En esta tabla se describe el valor predeterminado establecido por el procedimiento almacenado si se especifica un valor NULL para *schema_option*, que depende del tipo de artículo.  
  
|Tipo de artículo|Valor de la opción de esquema|  
|------------------|-------------------------|  
|**func schema only**|**0x01**|  
|**indexed view schema only**|**0x01**|  
|**proc schema only**|**0x01**|  
|**table**|**0x0C034FD1**  -  [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] y publicaciones compatibles con versiones posteriores con una instantánea en modo nativo.<br /><br /> **0x08034FF1**  -  [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] y publicaciones compatibles con versiones posteriores con una instantánea en modo de carácter.|  
|**view schema only**|**0x01**|  
  
> [!NOTE]  
>  Si la publicación admite versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la opción de esquema predeterminada para la **tabla** es **0x30034FF1**.  
  
## <a name="valid-schema-option-table"></a>Tabla Opciones de esquema válidas  
 En la tabla siguiente se describen los valores permitidos *schema_option* en función del tipo de artículo.  
  
|Tipo de artículo|Valores de las opciones de esquema|  
|------------------|--------------------------|  
|**func schema only**|**0x01** y **0x2000**|  
|**indexed view schema only**|**0x01**, **0x040**, **0x0100**, **0x2000**, **0x40000**, **0x1000000** y **0x200000**|  
|**proc schema only**|**0x01** y **0x2000**|  
|**table**|Todas las opciones.|  
|**view schema only**|**0x01**, **0x040**, **0x0100**, **0x2000**, **0x40000**, **0x1000000** y **0x200000**|  
  
## <a name="example"></a>Ejemplo  
 [!code-sql[HowTo#sp_AddMergeArticle](../../relational-databases/replication/codesnippet/tsql/sp-addmergearticle-trans_1.sql)]  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol fijo de servidor **sysadmin** o al rol fijo de base de datos **db_owner** .  
  
## <a name="see-also"></a>Consulte también  
 [Define an Article](../../relational-databases/replication/publish/define-an-article.md)   
 [Publicar datos y objetos de base de datos](../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Replicar columnas de identidad](../../relational-databases/replication/publish/replicate-identity-columns.md)   
 [sp_changemergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)   
 [sp_dropmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergearticle-transact-sql.md)   
 [sp_helpmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md)   
 [Procedimientos almacenados de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
