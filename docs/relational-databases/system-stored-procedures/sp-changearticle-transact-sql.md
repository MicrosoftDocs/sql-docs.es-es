---
description: sp_changearticle (Transact-SQL)
title: sp_changearticle (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/28/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changearticle
- sp_changearticle_TSQL
helpviewer_keywords:
- sp_changearticle
ms.assetid: 24c33ca5-f03a-4417-a267-131ca5ba6bb5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 22d64091d206161546856e7186958ab60217bcc2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203742"
---
# <a name="sp_changearticle-transact-sql"></a>sp_changearticle (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Cambia las propiedades de un artículo en una publicación transaccional o de instantáneas. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_changearticle [ [@publication= ] 'publication' ]  
    [ , [ @article= ] 'article' ]  
    [ , [ @property= ] 'property' ]  
    [ , [ @value= ] 'value' ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación que contiene el artículo. *Publication* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @article = ] 'article'` Es el nombre del artículo cuya propiedad se va a cambiar. *article* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @property = ] 'property'` Es una propiedad de artículo que se va a cambiar. *Property* es de tipo **nvarchar (100)**.  
  
`[ @value = ] 'value'` Es el nuevo valor de la propiedad del artículo. el *valor* es **nvarchar (255)**.  
  
 En esta tabla se describen las propiedades de los artículos y los valores de esas propiedades.  
  
|Propiedad|Valores|Descripción|  
|--------------|------------|-----------------|  
|**creation_script**||Ruta de acceso y nombre de un script de esquema del artículo que se utiliza para crear tablas de destino. El valor predeterminado es NULL.|  
|**del_cmd**||Instrucción DELETE que se va a ejecutar; de lo contrario, se genera a partir del registro.|  
|**description**||Nueva entrada descriptiva del artículo.|  
|**dest_object**||Se proporciona para mantener la compatibilidad con versiones anteriores. Use **dest_table**.|  
|**dest_table**||Nueva tabla de destino.|  
|**destination_owner**||Nombre del propietario del objeto de destino.|  
|**filter**||Nuevo procedimiento almacenado para filtrar la tabla (filtrado horizontal). El valor predeterminado es NULL. No se puede cambiar para las publicaciones de replicación punto a punto.|  
|**fire_triggers_on_snapshot**|**true**|Los desencadenadores de usuario replicados se ejecutan cuando se aplica la instantánea inicial.<br /><br /> Nota: en el caso de los desencadenadores que se van a replicar, el valor de la máscara de *schema_option* debe incluir el valor **0x100**.|  
||**false**|Los desencadenadores de usuario replicados no se ejecutan cuando se aplica la instantánea inicial.|  
|**identity_range**||Controla el tamaño de los intervalos de identidad asignados en el suscriptor. No se admite para la replicación punto a punto.|  
|**ins_cmd**||Instrucción INSERT que se ejecuta; de lo contrario, se crea a partir del registro.|  
|**pre_creation_cmd**||Comando de creación previa que puede quitar, eliminar o truncar la tabla de destino antes de que se aplique la sincronización.|  
||**Ninguna**|No usa ningún comando.|  
||**omisiones**|Quita la tabla de destino.|  
||**delete**|Elimina la tabla de destino.|  
||**truncate**|Trunca la tabla de destino.|  
|**pub_identity_range**||Controla el tamaño de los intervalos de identidad asignados en el suscriptor. No se admite para la replicación punto a punto.|  
|**schema_option**||Especifica el mapa de bits de la opción de generación del esquema para el artículo especificado. *schema_option* es **binario (8)**. Para obtener más información, vea la sección Comentarios más adelante en este tema.|  
||**0x00**|Deshabilita el scripting del Agente de instantáneas.|  
||**0x01**|Genera la creación del objeto (CREATE TABLE, CREATE PROCEDURE, etc.).|  
||**0x02**|Genera los procedimientos almacenados que propagan los cambios del artículo, si se han definido.|  
||**0x04**|Las columnas de identidad se incluyen en los scripts con la propiedad IDENTITY.|  
||**0x08**|Replicar columnas de **marca** de tiempo. Si no se establece, las columnas de **marca** de tiempo se replican como **binarias**.|  
||**0x10**|Genera el índice clúster correspondiente.|  
||**0x20**|Convierte los tipos de datos definidos por el usuario (UDT) en tipos de datos base en el suscriptor. Esta opción no se puede utilizar si existe una restricción CHECK o DEFAULT en una columna UDT, si una columna UDT forma parte de la clave principal o si una columna calculada hace referencia a una columna UDT. No es compatible con publicadores de Oracle.|  
||**0x40**|Genera los índices no clúster correspondientes.|  
||**0x80**|Incluye la integridad referencial declarada para las claves principales.|  
||**0x100**|Replica los desencadenadores de usuario en un artículo de tabla, si se han definido.|  
||**0x200**|Replica restricciones FOREIGN KEY. Si la tabla a la que se hace referencia no forma parte de una publicación, no se replica ninguna restricción FOREIGN KEY en una tabla publicada.|  
||**0x400**|Replica las restricciones CHECK.|  
||**0x800**|Replica los valores predeterminados.|  
||**0x1000**|Replica la intercalación de columna.|  
||**0x2000**|Replica las propiedades extendidas asociadas con el objeto de origen del artículo publicado.|  
||**0x4000**|Replica las claves únicas si están definidas en un artículo de tabla.|  
||**0x8000**|Replica la clave principal y las claves únicas de un artículo de tabla como restricciones mediante instrucciones ALTER TABLE.<br /><br /> Nota: esta opción está en desuso. Utilice **0x80** y **0x4000** en su lugar.|  
||**0x10000**|Replica las restricciones CHECK como NOT FOR REPLICATION de manera que no se impongan durante la sincronización.|  
||**0x20000**|Replica las restricciones FOREIGN KEY como NOT FOR REPLICATION de manera que no se impongan durante la sincronización.|  
||**0x40000**|Replica grupos de archivos asociados con un índice o una tabla con particiones.|  
||**0x80000**|Replica el esquema de partición de una tabla con particiones.|  
||**0x100000**|Replica el esquema de partición de un índice con particiones.|  
||**0x200000**|Replica las estadísticas de tabla.|  
||**0x400000**|Enlaces predeterminados|  
||**0x800000**|Enlaces de reglas|  
||**0x1000000**|Índice de texto completo|  
||**0x2000000**|Las colecciones de esquemas XML enlazadas a columnas **XML** no se replican.|  
||**0x4000000**|Replica índices en columnas **XML** .|  
||**0x8000000**|Crea esquemas que aún no existen en el suscriptor.|  
||**0x10000000**|Convierte las columnas **XML** en **ntext** en el suscriptor.|  
||**0x20000000**|Convierte los tipos de datos de objetos grandes (**nvarchar (Max)**, **VARCHAR (Max)** y **varbinary (Max)**) que se introdujeron en [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] a tipos de datos que se admiten en [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] .|  
||**0x40000000**|Permisos de replicación.|  
||**0x80000000**|Intenta quitar las dependencias de los objetos que no forman parte de la publicación.|  
||**0x100000000**|Use esta opción para replicar el atributo FILESTREAM si se especifica en columnas **varbinary (Max)** . No especifique esta opción si replica tablas en suscriptores de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. No se admite la replicación de tablas que tienen columnas FILESTREAM en [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] suscriptores, independientemente de cómo se establezca esta opción de esquema.<br /><br /> Vea la opción relacionada **0x800000000**.|  
||**0x200000000**|Convierte los tipos de datos de fecha y hora (**Date**, **Time**, **DateTimeOffset** y **datetime2**) introducidos en en [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] tipos de datos que se admiten en versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
||**0x400000000**|Replica la opción de compresión para los datos y los índices. Para obtener más información, consulte [Data Compression](../../relational-databases/data-compression/data-compression.md).|  
||**0x800000000**|Establezca esta opción para almacenar los datos de FILESTREAM en su propio grupo de archivos en el suscriptor. Si no se establece esta opción, los datos de FILESTREAM se almacenan en el grupo de archivos predeterminado. La replicación no crea grupos de archivos; por tanto, si establece esta opción, debe crear el grupo de archivos antes de aplicar la instantánea en el suscriptor. Para obtener más información sobre cómo crear objetos antes de aplicar la instantánea, vea [ejecutar scripts antes y después de aplicar la instantánea](../../relational-databases/replication/snapshot-options.md#execute-scripts-before-and-after-snapshot-is-applied).<br /><br /> Vea la opción relacionada **0x100000000**.|  
||**0x1000000000**|Convierte los tipos definidos por el usuario (UDT) de Common Language Runtime (CLR) de más de 8000 bytes a **varbinary (Max)** para que las columnas de tipo UDT se puedan replicar en los suscriptores que ejecutan [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .|  
||**0x2000000000**|Convierte el tipo de datos **hierarchyid** a **varbinary (Max)** para que las columnas de tipo **hierarchyid** se puedan replicar en los suscriptores que ejecutan [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] . Para obtener más información sobre cómo usar las columnas **hierarchyid** en tablas replicadas, vea [hierarchyid &#40;Transact-SQL&#41;](../../t-sql/data-types/hierarchyid-data-type-method-reference.md).|  
||**0x4000000000**|Replica los índices filtrados de la tabla. Para obtener más información acerca de los índices filtrados, vea [crear índices filtrados](../../relational-databases/indexes/create-filtered-indexes.md).|  
||**0x8000000000**|Convierte los tipos de datos **Geography** y **Geometry** en **varbinary (Max)** para que las columnas de estos tipos se puedan replicar en los suscriptores que ejecutan [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .|  
||**0x10000000000**|Replica los índices en columnas de tipo **Geography** y **Geometry**.|  
||**0x20000000000**|Replica el atributo SPARSE para las columnas. Para obtener más información sobre este atributo, vea [usar columnas dispersas](../../relational-databases/tables/use-sparse-columns.md).|  
||**0x40000000000**|Habilite el scripting del agente de instantáneas para crear una tabla optimizada en memoria en el suscriptor.|  
||**0x80000000000**|Convierte el índice clúster en un índice no clúster para los artículos con optimización para memoria.|  
|**status**||Especifica el nuevo estado de la propiedad.|  
||**dts horizontal partitions**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
||**include column names**|Los nombres de columnas se incluyen en la instrucción INSERT replicada.|  
||**no column names**|Los nombres de columnas no se incluyen en la instrucción INSERT replicada.|  
||**no dts horizontal partitions**|La partición horizontal del artículo no se define mediante una suscripción transformable.|  
||**Ninguna**|Borra todas las opciones de estado de la tabla [sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md) y marca el artículo como inactivo.|  
||**parameters**|Los cambios se propagan al suscriptor mediante comandos con parámetros. Es el valor predeterminado para los artículos nuevos.|  
||**literales de cadena**|Los cambios se propagan al suscriptor mediante valores literales de cadena.|  
|**sync_object**||Nombre de la tabla o vista utilizada para generar un archivo de salida de sincronización. El valor predeterminado es NULL. No es compatible con publicadores de Oracle.|  
|**Taba**||Identifica el espacio de tablas utilizado por la tabla de registro de un artículo publicado desde una base de datos Oracle. Para más información, vea [Manage Oracle Databases](../../relational-databases/replication/non-sql/manage-oracle-tablespaces.md) (Administrar bases de datos de Oracle).|  
|**threshold**||Valor de porcentaje que controla cuándo el Agente de distribución asigna un nuevo intervalo de identidad. No se admite para la replicación punto a punto.|  
|**type**||No es compatible con publicadores de Oracle.|  
||**logbased**|Artículo basado en registro.|  
||**logbased manualboth**|Artículo basado en registro con filtro manual y vista manual. Esta opción requiere que también se establezcan las propiedades *sync_object* y *Filter* . No es compatible con publicadores de Oracle.|  
||**logbased manualfilter**|Artículo basado en registro con filtro manual. Esta opción requiere que también se establezcan las propiedades *sync_object* y *Filter* . No es compatible con publicadores de Oracle.|  
||**logbased manualview**|Artículo basado en registro con vista manual. Esta opción requiere que también se establezca la propiedad *sync_object* . No es compatible con publicadores de Oracle.|  
||**viewlogbased indizado**|Artículo de vista indizada basado en registro. No es compatible con publicadores de Oracle. En este tipo de artículo, no es necesario que la tabla base se publique por separado.|  
||**indexado de viewlogbased manualboth**|Artículo de vista indizada basado en registro con filtro manual y vista manual. Esta opción requiere que también se establezcan las propiedades *sync_object* y *Filter* . En este tipo de artículo, no es necesario que la tabla base se publique por separado. No es compatible con publicadores de Oracle.|  
||**indexado de viewlogbased manualfilter**|Artículo de vista indizada basado en registro con filtro manual. Esta opción requiere que también se establezcan las propiedades *sync_object* y *Filter* . En este tipo de artículo, no es necesario que la tabla base se publique por separado. No es compatible con publicadores de Oracle.|  
||**indexado de viewlogbased manualview**|Artículo de vista indizada basado en registro con vista manual. Esta opción requiere que también se establezca la propiedad *sync_object* . En este tipo de artículo, no es necesario que la tabla base se publique por separado. No es compatible con publicadores de Oracle.|  
|**upd_cmd**||Instrucción UPDATE que se va a ejecutar; de lo contrario, se genera a partir del registro.|  
|NULL|NULL|Devuelve una lista con las propiedades del artículo que se pueden cambiar.|  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` Confirma que la acción realizada por este procedimiento almacenado puede invalidar una instantánea existente. *force_invalidate_snapshot* es de **bit** y su valor predeterminado es **0**.  
  
 **0** especifica que los cambios en el artículo no hacen que la instantánea no sea válida. Si el procedimiento almacenado detecta que el cambio requiere una nueva instantánea, se producirá un error y no se realizarán cambios.  
  
 **1** especifica que los cambios en el artículo pueden hacer que la instantánea no sea válida y, si hay suscripciones existentes que requieran una nueva instantánea, concede permiso para que la instantánea existente se marque como obsoleta y se genere una nueva instantánea.  
  
 Vea en la sección de Notas las propiedades que, si se cambian, requieren que se genere una instantánea nueva.  
  
`[ @force_reinit_subscription = ]force_reinit_subscription_` Confirma que la acción realizada por este procedimiento almacenado puede requerir que se reinicialicen las suscripciones existentes. *force_reinit_subscription* es un **bit** con un valor predeterminado de **0**.  
  
 **0** especifica que los cambios en el artículo no harán que se reinicialice la suscripción. Si el procedimiento almacenado detecta que el cambio requiere la reinicialización de las suscripciones existentes, se producirá un error y no se realizarán cambios.  
  
 **1** especifica que los cambios en el artículo hacen que se reinicialicen las suscripciones existentes y concede permiso para que se produzca la reinicialización de la suscripción.  
  
 Vea en la sección de Notas las propiedades que, si se cambian, requieren que se reinicialicen todas las suscripciones existentes.  
  
`[ @publisher = ] 'publisher'` Especifica un publicador que no es de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  no se debe usar el *publicador* al cambiar las propiedades de un artículo en un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publicador.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_changearticle** se utiliza en la replicación de instantáneas y en la replicación transaccional.  
  
 Cuando un artículo pertenece a una publicación que admite la replicación transaccional punto a punto, solo se pueden cambiar las propiedades **Description**, **ins_cmd**, **upd_cmd** y **del_cmd** .  
  
 Para cambiar cualquiera de las siguientes propiedades, es necesario generar una nueva instantánea y debe especificar el valor **1** para el parámetro *force_invalidate_snapshot* :  
  
-   **del_cmd**  
  
-   **dest_table**  
  
-   **destination_owner**  
  
-   **ins_cmd**  
  
-   **pre_creation_cmd**  
  
-   **schema_options**  
  
-   **upd_cmd**  
  
 Para cambiar cualquiera de las siguientes propiedades, es necesario reinicializar las suscripciones existentes y debe especificar un valor de **1** para el parámetro *force_reinit_subscription* .  
  
-   **del_cmd**  
  
-   **dest_table**  
  
-   **destination_owner**  
  
-   **filter**  
  
-   **ins_cmd**  
  
-   **status**  
  
-   **upd_cmd**  
  
 En una publicación existente, puede usar **sp_changearticle** para cambiar un artículo sin tener que quitar y volver a crear toda la publicación.  
  
> [!NOTE]  
>  Al cambiar el valor de *schema_option*, el sistema no realiza una actualización bit a bit. Esto significa que cuando se establece *schema_option* mediante **sp_changearticle**, puede que la configuración de bits existente esté desactivada. Para conservar la configuración existente, debe ejecutar [| (OR bit a bit)](../../t-sql/language-elements/bitwise-or-transact-sql.md) entre el valor que está estableciendo y el valor actual de *schema_option*, que se puede determinar mediante la ejecución de [sp_helparticle](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md).  
  
## <a name="valid-schema-options"></a>Opciones de esquema válidas  
 En la tabla siguiente se describen los valores permitidos de *schema_option* en función del tipo de replicación (que se muestra en la parte superior) y el tipo de artículo (que se muestra en la primera columna).  
  
| Tipo de artículo | Tipo de replicación: transaccional | Tipo de replicación: instantánea |
| ------------ | -------------------------------- | --------------------------- |
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
>  En las publicaciones de actualización en cola, se debe habilitar el valor de *schema_option* de **0x80** . Los valores de *schema_option* admitidos para las publicaciones que no [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] son de son: **0x01**, **0x02**, **0x10**, **0x40**, **0x80**, **0x1000** y **0x4000**.  
  
## <a name="example"></a>Ejemplo  
 [!code-sql[HowTo#sp_changetranarticle](../../relational-databases/replication/codesnippet/tsql/sp-changearticle-transac_1.sql)]  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_changearticle**.  
  
## <a name="see-also"></a>Consulte también  
 [Ver y modificar las propiedades del artículo](../../relational-databases/replication/publish/view-and-modify-article-properties.md)   
 [Cambiar las propiedades de la publicación y de los artículos](../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [sp_addarticle &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_articlecolumn &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md)   
 [sp_droparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md)   
 [sp_helparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)   
 [sp_helparticlecolumns &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helparticlecolumns-transact-sql.md)  
  
  
