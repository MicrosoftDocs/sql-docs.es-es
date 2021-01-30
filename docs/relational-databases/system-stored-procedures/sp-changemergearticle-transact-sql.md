---
description: sp_changemergearticle (Transact-SQL)
title: sp_changemergearticle (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/09/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changemergearticle_TSQL
- sp_changemergearticle
helpviewer_keywords:
- sp_changemergearticle
ms.assetid: 0dc3da5c-4af6-45be-b5f0-074da182def2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b504644f5b96cc631a6352d71f2b387e8040b5a0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159884"
---
# <a name="sp_changemergearticle-transact-sql"></a>sp_changemergearticle (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cambia las propiedades de un artículo de mezcla. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_changemergearticle [ @publication = ] 'publication'  
        , [ @article = ] 'article'  
    [ , [ @property = ] 'property' ]  
    [ , [ @value = ] 'value' ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación en la que existe el artículo. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @article = ] 'article'` Es el nombre del artículo que se va a cambiar. *article* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @property = ] 'property'` Es la propiedad que se va a cambiar para el artículo y la publicación especificados. *Property* es de tipo **nvarchar (30)** y puede ser uno de los valores enumerados en la tabla.  
  
`[ @value = ] 'value'` Es el nuevo valor de la propiedad especificada. *Value* es de tipo **nvarchar (1000)** y puede ser uno de los valores enumerados en la tabla.  
  
 En esta tabla se describen las propiedades de los artículos y los valores de esas propiedades.  
  
|Propiedad|Valores|Descripción|  
|--------------|------------|-----------------|  
|**allow_interactive_resolver**|**true**|Habilita el uso de un solucionador interactivo para el artículo.|  
||**false**|Deshabilita el uso de un solucionador interactivo para el artículo.|  
|**article_resolver**||Solucionador personalizado para el artículo. Solo se aplica a un artículo de tabla.|  
|**check_permissions** (mapa de bits)|**0x00**|Los permisos de tabla no se comprueban.|  
||**0x10**|Los permisos de tabla se comprueban en el publicador antes de que las instrucciones INSERT realizadas en el suscriptor se apliquen en el publicador.|  
||**0x20**|Los permisos de tabla se comprueban en el publicador antes de que las instrucciones UPDATE realizadas en el suscriptor se apliquen en el publicador.|  
||**0x40**|Los permisos de tabla se comprueban en el publicador antes de que las instrucciones DELETE realizadas en el suscriptor se apliquen en el publicador.|  
|**column_tracking**|**true**|Activa el seguimiento por columna. Solo se aplica a un artículo de tabla.<br /><br /> Nota: no se puede usar el seguimiento de nivel de columna al publicar tablas con más de 246 columnas.|  
||**false**|Desactiva el seguimiento por columna y deja la detección de conflictos a nivel de fila. Solo se aplica a un artículo de tabla.|  
|**compensate_for_errors**|**true**|Las acciones de compensación se ejecutan cuando se producen errores durante la sincronización. Para obtener más información, vea [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md).|  
||**false**|Las acciones de compensación no se ejecutan; éste es el comportamiento predeterminado. Para obtener más información, vea [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md).<br /><br /> Importante aunque es posible que parezca que los datos de las filas afectadas no sean de convergencia, tan pronto como se solucionen los errores, se pueden aplicar los cambios y los datos convergerán. **\* \* \* \*** Si la tabla de origen de un artículo ya está publicada en otra publicación, el valor de *compensate_for_errors* debe ser el mismo para ambos artículos.|  
|**creation_script**||Ruta de acceso y nombre de un script opcional del esquema del artículo que se utiliza para crear el artículo en la base de datos de suscripciones.|  
|**delete_tracking**|**true**|Las instrucciones DELETE se replican; éste es el comportamiento predeterminado.|  
||**false**|Las instrucciones DELETE no se replican.<br /><br /> Una configuración **\* \* \* importante \*** **delete_tracking** a **false** da como resultado una no convergencia y las filas eliminadas deben quitarse manualmente.|  
|**description**||Entrada descriptiva del artículo.|  
|**destination_owner**||Nombre del propietario del objeto en la base de datos de suscripciones, si no es **DBO**.|  
|**identity_range**||**BIGINT** que especifica el tamaño del intervalo que se va a utilizar al asignar nuevos valores de identidad si el artículo tiene **identityrangemanagementoption** establecido en **auto** o **auto_identity_range** establecido en **true**. Solamente se aplica en un artículo de la tabla. Para obtener más información, vea la sección "replicación de mezcla" de [replicar columnas de identidad](../../relational-databases/replication/publish/replicate-identity-columns.md).|  
|**identityrangemanagementoption**|**Manual**|Deshabilita la administración automática de intervalos de identidad. Marca las columnas de identidad utilizando NOT FOR REPLICATION para habilitar la administración manual de intervalos de identidad. Para más información, vea [Replicar columnas de identidad](../../relational-databases/replication/publish/replicate-identity-columns.md).|  
||**Ninguna**|Deshabilita toda la administración de intervalos de identidad.|  
|**logical_record_level_conflict_detection**|**true**|Se detecta un conflicto si se realizan cambios en cualquier parte del registro lógico. Requiere que **logical_record_level_conflict_resolution** establecerse en **true**.|  
||**false**|La detección de conflictos predeterminada se usa tal y como se especifica en **column_tracking**.|  
|**logical_record_level_conflict_resolution**|**true**|Todo el registro lógico ganador reemplaza el registro lógico perdedor.|  
||**false**|Las filas ganadoras no se restringen al registro lógico.|  
|**partition_options**|**0**|El filtro para el artículo es estático o no produce un subconjunto de datos único para cada partición, es decir, es una partición "superpuesta".|  
||**1**|Las particiones se superponen y las actualizaciones del lenguaje DML realizadas en el suscriptor no pueden cambiar la partición a la que pertenece una fila.|  
||**2**|El filtro para el artículo produce particiones no superpuestas, pero varios suscriptores pueden recibir la misma partición.|  
||**3**|El filtro para el artículo produce particiones no superpuestas que son exclusivas para cada suscripción.<br /><br /> Nota: Si especifica un valor de **3** para **partition_options**, solo puede haber una suscripción para cada partición de datos de ese artículo. Si se crea una segunda partición en la que el criterio de filtrado de la nueva suscripción se resuelve en la misma partición que la suscripción existente, se quitará la suscripción existente.|  
|**pre_creation_command**|**Ninguna**|Si la tabla ya existe en el suscriptor, no se lleva a cabo ninguna acción.|  
||**delete**|Emite una eliminación basada en la cláusula WHERE del filtro de subconjunto.|  
||**omisiones**|Quita la tabla antes de volver a crearla.|  
||**truncate**|Trunca la tabla de destino.|  
|**processing_order**||**int** que indica el orden de procesamiento de los artículos en una publicación de combinación.|  
|**pub_identity_range**||**BIGINT** que especifica el tamaño de intervalo asignado a un suscriptor con una suscripción de servidor si el artículo tiene el valor **identityrangemanagementoption** establecido en **auto** o **auto_identity_range** establecido en **true**. Este rango de identidad se reserva para que un suscriptor de republicación pueda realizar asignaciones a sus propios suscriptores. Solamente se aplica en un artículo de la tabla. Para obtener más información, vea la sección "replicación de mezcla" de [replicar columnas de identidad](../../relational-databases/replication/publish/replicate-identity-columns.md).|  
|**published_in_tran_pub**|**true**|El artículo también está publicado en una publicación transaccional.|  
||**false**|El artículo no está publicado en una publicación transaccional.|  
|**resolver_info**||Se utiliza para especificar información adicional necesaria para un solucionador personalizado. Algunos de los solucionadores de [!INCLUDE[msCoName](../../includes/msconame-md.md)] requieren que se proporcione una columna como entrada para el solucionador. **resolver_info** es de tipo **nvarchar (255)** y su valor predeterminado es NULL. Para obtener más información, consulte [Solucionadores basados en Microsoft COM](../../relational-databases/replication/merge/advanced-merge-replication-conflict-com-based-resolvers.md).|  
|**schema_option** (mapa de bits)||Para obtener más información, vea la sección Comentarios más adelante en este tema.|  
||**0x00**|Deshabilita el scripting por el Agente de instantáneas y usa el script proporcionado en **creation_script**.|  
||**0x01**|Genera el script de creación del objeto (CREATE TABLE, CREATE PROCEDURE, etc.).|  
||**0x10**|Genera el índice clúster correspondiente.|  
||**0x20**|Convierte los tipos de datos definidos por el usuario en tipos de datos base en el suscriptor. Esta opción no se puede utilizar si existe una restricción CHECK o DEFAULT en una columna de tipo definido por el usuario (UDT), si una columna UDT forma parte de la clave principal o si una columna calculada hace referencia a una columna UDT.|  
||**0x40**|Genera los índices no clúster correspondientes.|  
||**0x80**|Incluye la integridad referencial declarada para las claves principales.|  
||**0x100**|Replica los desencadenadores de usuario en un artículo de tabla, si se han definido.|  
||**0x200**|Replica restricciones FOREIGN KEY. Si la tabla a la que se hace referencia no forma parte de una publicación, no se replica ninguna restricción FOREIGN KEY en una tabla publicada.|  
||**0x400**|Replica las restricciones CHECK.|  
||**0x800**|Replica los valores predeterminados.|  
||**0x1000**|Replica la intercalación de columna.|  
||**0x2000**|Replica las propiedades extendidas asociadas con el objeto de origen del artículo publicado.|  
||**0x4000**|Replica las claves únicas si están definidas en un artículo de tabla.|  
||**0x8000**|Genera instrucciones ALTER TABLE al incluir restricciones en scripting.|  
||**0x10000**|Replica las restricciones CHECK como NOT FOR REPLICATION de manera que no se impongan durante la sincronización.|  
||**0x20000**|Replica las restricciones FOREIGN KEY como NOT FOR REPLICATION de manera que no se impongan durante la sincronización.|  
||**0x40000**|Replica grupos de archivos asociados con un índice o una tabla con particiones.|  
||**0x80000**|Replica el esquema de partición de una tabla con particiones.|  
||**0x100000**|Replica el esquema de partición de un índice con particiones.|  
||**0x200000**|Replica las estadísticas de tabla.|  
||**0x400000**|Replica los enlaces predeterminados|  
||**0x800000**|Replica los enlaces de reglas.|  
||**0x1000000**|Replica el índice de texto completo.|  
||**0x2000000**|Las colecciones de esquemas XML enlazadas a columnas **XML** no se replican.|  
||**0x4000000**|Replica índices en columnas **XML** .|  
||**0x8000000**|Crea esquemas que aún no existen en el suscriptor.|  
||**0x10000000**|Convierte las columnas **XML** en **ntext** en el suscriptor.|  
||**0x20000000**|Convierte los tipos de datos de objetos grandes (**nvarchar (Max)**, **VARCHAR (Max)** y **varbinary (Max)**) que se introdujeron en [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] a tipos de datos que se admiten en [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] .|  
||**0x40000000**|Permisos de replicación.|  
||**0x80000000**|Intenta quitar las dependencias de los objetos que no forman parte de la publicación.|  
||**0x100000000**|Use esta opción para replicar el atributo FILESTREAM si se especifica en columnas **varbinary (Max)** . No especifique esta opción si replica tablas en suscriptores de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. No se admite la replicación de tablas que tienen columnas FILESTREAM en [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] suscriptores, independientemente de cómo se establezca esta opción de esquema. Vea la opción relacionada **0x800000000**.|  
||**0x200000000**|Convierte los tipos de datos de fecha y hora (**Date**, **Time**, **DateTimeOffset** y **datetime2**) introducidos en en [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] tipos de datos que se admiten en versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
||**0x400000000**|Replica la opción de compresión para los datos y los índices. Para obtener más información, consulte [Data Compression](../../relational-databases/data-compression/data-compression.md).|  
||**0x800000000**|Establezca esta opción para almacenar los datos de FILESTREAM en su propio grupo de archivos en el suscriptor. Si no se establece esta opción, los datos de FILESTREAM se almacenan en el grupo de archivos predeterminado. La replicación no crea grupos de archivos; por tanto, si establece esta opción, debe crear el grupo de archivos antes de aplicar la instantánea en el suscriptor. Para obtener más información sobre cómo crear objetos antes de aplicar la instantánea, vea [ejecutar scripts antes y después de aplicar la instantánea](../../relational-databases/replication/snapshot-options.md#execute-scripts-before-and-after-snapshot-is-applied).<br /><br /> Vea la opción relacionada **0x100000000**.|  
||**0x1000000000**|Convierte los tipos definidos por el usuario (UDT) de Common Language Runtime (CLR) a **varbinary (Max)** para que las columnas de tipo UDT se puedan replicar en los suscriptores que ejecutan [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .|  
||**0x2000000000**|Convierte el tipo de datos **hierarchyid** a **varbinary (Max)** para que las columnas de tipo **hierarchyid** se puedan replicar en los suscriptores que ejecutan [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] . Para obtener más información sobre cómo usar las columnas **hierarchyid** en tablas replicadas, vea [hierarchyid &#40;Transact-SQL&#41;](../../t-sql/data-types/hierarchyid-data-type-method-reference.md).|  
||**0x4000000000**|Replica los índices filtrados de la tabla. Para obtener más información acerca de los índices filtrados, vea [crear índices filtrados](../../relational-databases/indexes/create-filtered-indexes.md).|  
||**0x8000000000**|Convierte los tipos de datos **Geography** y **Geometry** en **varbinary (Max)** para que las columnas de estos tipos se puedan replicar en los suscriptores que ejecutan [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .|  
||**0x10000000000**|Replica los índices en columnas de tipo **Geography** y **Geometry**.|  
||NULL|El sistema genera automáticamente una opción de esquema válida para el artículo.|  
|**status**|**active**|Se ejecuta el script inicial de proceso para publicar la tabla.|  
||**unsynced**|El script de procesos inicial para publicar la tabla se ejecuta la próxima vez que se ejecute el Agente de instantáneas.|  
|**stream_blob_columns**|**true**|Se utiliza una optimización del flujo de datos al replicar columnas de objetos binarios grandes. No obstante, ciertas funciones de la replicación de mezcla, como los registros lógicos, pueden impedir que se utilice la optimización de flujos. *stream_blob_columns* se establece en true cuando se habilita FileStream. Esto permite que la replicación de datos FILESTREAM se realice de forma óptima y que se reduzca el uso de memoria. Para forzar que los artículos de tabla FILESTREAM no usen streaming de blobs, establezca *stream_blob_columns* en false.<br /><br /> Importante: habilitar esta optimización de memoria podría perjudicar el rendimiento de la agente de mezcla durante la sincronización. **\* \* \* \*** Esta opción solo se debe utilizar al replicar columnas que contienen megabytes de datos.|  
||**false**|La optimización no se utiliza al replicar columnas de objetos binarios grandes.|  
|**subscriber_upload_options**|**0**|No existen restricciones en actualizaciones realizadas en el suscriptor con una suscripción de cliente; los cambios se cargan en el publicador. Si cambia esta propiedad puede que sea necesaria la reinicialización de suscriptores existentes.|  
||**1**|Se permiten cambios en un suscriptor con una suscripción de cliente, pero no se cargan en el publicador.|  
||**2**|No se permiten cambios en un suscriptor con una suscripción de cliente.|  
|**subset_filterclause**||Cláusula WHERE que especifica el filtrado horizontal. Solo se aplica a un artículo de tabla.<br /><br /> Importante por motivos de rendimiento, se recomienda no aplicar funciones a los nombres de columna en las cláusulas de filtro de fila con parámetros, como. **\* \* \* \*** `LEFT([MyColumn]) = SUSER_SNAME()` Si utiliza [host_name](../../t-sql/functions/host-name-transact-sql.md) en una cláusula de filtro y reemplaza el valor host_name, es posible que tenga que convertir los tipos de datos mediante [Convert](../../t-sql/functions/cast-and-convert-transact-sql.md). Para obtener más información sobre los procedimientos recomendados para este caso, vea la sección "invalidar el valor HOST_NAME ()" en [filtros de fila con parámetros](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).|  
|**threshold**||Valor de porcentaje utilizado para los suscriptores que ejecutan [!INCLUDE[ssEW](../../includes/ssew-md.md)] o versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . **Threshold** controla cuándo el agente de mezcla asigna un nuevo intervalo de identidad. Si se utiliza el porcentaje de valores especificado en el umbral, el Agente de mezcla crea un nuevo intervalo de identidad. Se utiliza cuando el valor de **identityrangemanagementoption** está establecido en **auto** o **auto_identity_range** está establecido en **true**. Solamente se aplica en un artículo de la tabla. Para obtener más información, vea la sección "replicación de mezcla" de [replicar columnas de identidad](../../relational-databases/replication/publish/replicate-identity-columns.md).|  
|**verify_resolver_signature**|**1**|La firma digital en un solucionador personalizado se comprueba para determinar si proviene de una fuente confiable.|  
||**0**|La firma digital en un solucionador personalizado no se comprueba para determinar si proviene de una fuente confiable.|  
|NULL (predeterminado)||Devuelve la lista de valores admitidos para la *propiedad*.|  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` Confirma que la acción realizada por este procedimiento almacenado puede invalidar una instantánea existente. *force_invalidate_snapshot* es de **bit** y su valor predeterminado es **0**.  
  
 **0** especifica que los cambios en el artículo de mezcla no hacen que la instantánea no sea válida. Si el procedimiento almacenado detecta que el cambio requiere una nueva instantánea, se producirá un error y no se realizarán cambios.  
  
 **1** significa que los cambios en el artículo de mezcla pueden hacer que la instantánea no sea válida y, si hay suscripciones existentes que requieran una nueva instantánea, concede permiso para que la instantánea existente se marque como obsoleta y se genere una nueva instantánea.  
  
 Vea en la sección de Notas las propiedades que, si se cambian, requieren que se genere una instantánea nueva.  
  
`[ @force_reinit_subscription = ] force_reinit_subscription` Confirma que la acción realizada por este procedimiento almacenado puede requerir que se reinicialicen las suscripciones existentes. *force_reinit_subscription* es de **bit** y su valor predeterminado es **0**.  
  
 **0** especifica que los cambios en el artículo de mezcla no harán que se reinicialice la suscripción. Si el procedimiento almacenado detecta que el cambio requiere la reinicialización de las suscripciones existentes, se producirá un error y no se realizarán cambios.  
  
 **1** significa que los cambios en el artículo de mezcla hacen que se reinicialicen las suscripciones existentes y concede permiso para que se produzca la reinicialización de la suscripción.  
  
 Vea en la sección de Notas las propiedades que, si se cambian, requieren que se reinicialicen todas las suscripciones existentes.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_changemergearticle** se utiliza en la replicación de mezcla.  
  
 Dado que **sp_changemergearticle** se utiliza para cambiar las propiedades de los artículos que se especificaron inicialmente mediante [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md), consulte [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) para obtener información adicional sobre estas propiedades.  
  
 Para cambiar las siguientes propiedades, es necesario generar una instantánea nueva y especificar el valor **1** para el parámetro *force_invalidate_snapshot* :  
  
-   **check_permissions**  
  
-   **column_tracking**  
  
-   **destination_owner**  
  
-   **pre_creation_command**  
  
-   **schema_options**  
  
-   **subset_filterclause**  
  
 Para cambiar las siguientes propiedades, es necesario reinicializar las suscripciones existentes y debe especificar el valor **1** para el parámetro *force_reinit_subscription* :  
  
-   **check_permissions**  
  
-   **column_tracking**  
  
-   **destination_owner**  
  
-   **pre_creation_command**  
  
-   **identityrangemanagementoption**  
  
-   **subscriber_upload_options**  
  
-   **subset_filterclause**  
  
-   **creation_script**  
  
-   **schema_option**  
  
-   **logical_record_level_conflict_detection**  
  
-   **logical_record_level_conflict_resolution**  
  
 Al especificar un valor de 3 para **partition_options**, los metadatos se limpian cada vez que se ejecuta el agente de mezcla y la instantánea con particiones expira más rápidamente. Al utilizar esta opción, debe considerar la habilitación de la instantánea con particiones solicitada por el suscriptor. Para más información, consulte [Instantáneas para publicaciones de combinación con filtros con parámetros](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  
 Al establecer la propiedad **column_tracking** , si la tabla ya está publicada en otras publicaciones de combinación, el seguimiento de la columna debe ser el mismo que el valor utilizado por los artículos existentes basados en esta tabla. Este parámetro es específico solamente de los artículos de tabla.  
  
 Si varias publicaciones publican artículos basados en la misma tabla subyacente, el cambio de la propiedad **delete_tracking** o la propiedad **compensate_for_errors** de un artículo hace que se realice el mismo cambio en los demás artículos basados en la misma tabla.  
  
 Si la cuenta de inicio de sesión o usuario del publicador que utiliza el proceso de mezcla no dispone de los permisos de tabla correctos, los cambios no válidos se registran como conflictos.  
  
 Al cambiar el valor de **schema_option**, el sistema no realiza una actualización bit a bit. Esto significa que cuando se establece **schema_option** mediante **sp_changemergearticle**, puede que la configuración de bits existente esté desactivada. Para conservar la configuración existente, debe realizar [& (and bit a bit)](../../t-sql/language-elements/bitwise-and-transact-sql.md) entre el valor que está estableciendo y el valor actual de *schema_option*, que se puede determinar mediante la ejecución de [sp_helpmergearticle](../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md).  
  
> [!CAUTION]  
>  Si tiene muchos artículos (quizás cientos) en una publicación y ejecuta **sp_changemergearticle** para uno de los artículos, es posible que se tarde mucho tiempo en finalizar la ejecución.  
  
## <a name="valid-schema-option-table"></a>Tabla Opciones de esquema válidas  
 En la tabla siguiente se describen los valores de *schema_option* permitidos, dependiendo del tipo de artículo.  
  
|Tipo de artículo|Valores de las opciones de esquema|  
|------------------|--------------------------|  
|**func schema only**|**0x01** y **0x2000**|  
|**indexed view schema only**|**0x01**, **0x040**, **0x0100**, **0x2000**, **0x40000**, **0x1000000** y **0x200000**|  
|**proc schema only**|**0x01** y **0x2000**|  
|**table**|Todas las opciones.|  
|**view schema only**|**0x01**, **0x040**, **0x0100**, **0x2000**, **0x40000**, **0x1000000** y **0x200000**|  
  
## <a name="example"></a>Ejemplo  
 [!code-sql[HowTo#sp_changemergearticle](../../relational-databases/replication/codesnippet/tsql/sp-changemergearticle-tr_1.sql)]  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_changemergearticle**.  
  
## <a name="see-also"></a>Consulte también  
 [Ver y modificar las propiedades del artículo](../../relational-databases/replication/publish/view-and-modify-article-properties.md)   
 [Cambiar las propiedades de la publicación y de los artículos](../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [sp_addmergearticle &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)   
 [sp_dropmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergearticle-transact-sql.md)   
 [sp_helpmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md)   
 [Procedimientos almacenados de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
