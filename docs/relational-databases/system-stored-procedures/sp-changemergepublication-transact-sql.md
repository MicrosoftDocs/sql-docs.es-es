---
description: sp_changemergepublication (Transact-SQL)
title: sp_changemergepublication (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changemergepublication_TSQL
- sp_changemergepublication
helpviewer_keywords:
- sp_changemergepublication
ms.assetid: 81fe1994-7678-4852-980b-e02fedf1e796
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e7807a445de3ddf919679b09002646c87b313c5d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159862"
---
# <a name="sp_changemergepublication-transact-sql"></a>sp_changemergepublication (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cambia las propiedades de una publicación de combinación. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_changemergepublication [ @publication= ] 'publication'  
    [ , [ @property= ] 'property' ]  
    [ , [ @value= ] 'value' ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Nombre de la publicación. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @property = ] 'property'` Propiedad que se va a cambiar para la publicación especificada. *Property* es de **tipo sysname** y puede tener uno de los valores enumerados en la tabla siguiente.  
  
`[ @value = ] 'value'` Nuevo valor de la propiedad especificada. el *valor* es **nvarchar (255)** y puede ser uno de los valores enumerados en la tabla siguiente.  
  
 Esta tabla describe las propiedades de la publicación que se pueden cambiar, así como las restricciones de los valores de esas propiedades.  
  
|Propiedad|Value|Descripción|  
|--------------|-----------|-----------------|  
|**allow_anonymous**|**true**|Se admiten las suscripciones anónimas.|  
||**false**|No se admiten las suscripciones anónimas.|  
|**allow_partition_realignment**|**true**|Las eliminaciones se envían al suscriptor para reflejar los resultados de un cambio en la partición mediante la eliminación de los datos que han dejado de formar parte de la partición del suscriptor. Éste es el comportamiento predeterminado.|  
||**false**|Los datos de la partición antigua se dejan en el suscriptor, donde los cambios realizados en estos datos del publicador no se replican en este suscriptor. En cambio, los cambios realizados en el suscriptor se replican en el publicador. Esto sirve para conservar los datos de una partición antigua en una suscripción cuando es necesario que los datos estén accesibles con fines históricos.|  
|**allow_pull**|**true**|Se permiten suscripciones de extracción para la publicación indicada.|  
||**false**|No se permiten suscripciones de extracción para la publicación indicada.|  
|**allow_push**|**true**|Se permiten suscripciones de inserción para la publicación indicada.|  
||**false**|No se permiten suscripciones de inserción para la publicación indicada.|  
|**allow_subscriber_initiated_snapshot**|**true**|El suscriptor puede iniciar el proceso de instantáneas.|  
||**false**|El suscriptor no puede iniciar el proceso de instantáneas.|  
|**allow_subscription_copy**|**true**|Puede copiar las bases de datos de suscripciones suscritas a esta publicación.|  
||**false**|No puede copiar las bases de datos de suscripciones suscritas a esta publicación.|  
|**allow_synctoalternate**|**true**|Permite que un asociado de sincronización alternativo se sincronice con este publicador.|  
||**false**|No permite que un modelo de sincronización alternativo se sincronice con este publicador.|  
|**allow_web_synchronization**|**true**|Las suscripciones se pueden sincronizar por HTTPS.|  
||**false**|Las suscripciones no se pueden sincronizar por HTTPS.|  
|**alt_snapshot_folder**||Especifica la ubicación de la carpeta alternativa de la instantánea.|  
|**automatic_reinitialization_policy**|**1**|Los cambios se cargan desde el suscriptor antes de reinicializar la suscripción.|  
||**0**|La suscripción se reinicializa sin cargar primero los cambios.|  
|**centralized_conflicts**|**true**|Todos los registros de conflictos se almacenan en el publicador. Si cambia esta propiedad, se deben reinicializar los suscriptores existentes.|  
||**false**|Los registros de conflictos se almacenan en el servidor que perdió en la resolución de conflictos. Si cambia esta propiedad, se deben reinicializar los suscriptores existentes.|  
|**compress_snapshot**|**true**|La instantánea de una carpeta de instantáneas alternativa se comprime en formato CAB. La instantánea de la carpeta de instantáneas predeterminada no se puede comprimir. Para cambiar esta propiedad, se requiere una instantánea nueva.|  
||**false**|De forma predeterminada, no se comprime la instantánea. Para cambiar esta propiedad, se requiere una instantánea nueva.|  
|**conflict_logging**|**publisher**|Los registros de conflictos se almacenan en el publicador.|  
||**suscriptor**|Los registros de conflictos se almacenan en el suscriptor que causó el conflicto. No se admite para los [!INCLUDE[ssEW](../../includes/ssew-md.md)] suscriptores de *.*|  
||**ambos**|Los registros de conflictos se almacenan tanto en el publicador como en el suscriptor.|  
|**conflict_retention**||Un valor **int** que especifica el período de retención, en días, durante el que se conservan los conflictos. Establecer *conflict_retention* en **0** significa que no se necesita ninguna limpieza de conflictos.|  
|**description**||Descripción de la publicación.|  
|**dynamic_filters**|**true**|La publicación se filtra según una cláusula dinámica.|  
||**false**|La publicación no se filtra dinámicamente.|  
|**enabled_for_internet**|**true**|La publicación para Internet está habilitada. El protocolo de transferencia de archivos (FTP) se puede utilizar para transferir los archivos de instantáneas a un suscriptor. Los archivos de sincronización de la publicación se colocan en el directorio C:\Archivos de Programa\microsoft SQL Server\MSSQL\Repldata\ftp|  
||**false**|No se habilita la publicación para Internet.|  
|**ftp_address**||Dirección de red del servicio FTP para el distribuidor. Especifica la ubicación de los archivos de instantáneas de la publicación.|  
|**ftp_login**||Nombre de usuario que se usa para conectarse al servicio FTP.|  
|**ftp_password**||Contraseña de usuario que se usa para conectarse al servicio FTP.|  
|**ftp_port**||El número de puerto del servicio FTP para el distribuidor. Especifica el número del puerto TCP del sitio FTP donde se almacenan los archivos de instantáneas de la publicación.|  
|**ftp_subdirectory**||Especifica dónde se crean los archivos de instantáneas si la publicación admite la propagación de instantáneas mediante FTP.|  
|**generation_leveling_threshold**|**int**|Especifica el número de cambios contenidos en una generación. Una generación es una colección de cambios que se entregan a un publicador o a un suscriptor.|  
|**keep_partition_changes**|**true**|La sincronización se optimiza y solo se ven afectados los suscriptores que tienen filas en las particiones que han cambiado. Para cambiar esta propiedad, se requiere una instantánea nueva.|  
||**false**|La sincronización no se optimiza y las particiones que se envían a todos los suscriptores se comprueban cuando los datos cambian en una partición. Para cambiar esta propiedad, se requiere una instantánea nueva.|  
|**max_concurrent_merge**||Es un valor **int** que representa el número máximo de procesos de mezcla simultáneos que se pueden ejecutar en una publicación. Si es 0, no hay límite. Si se programa un número de procesos de mezcla superior a éste para que se ejecuten a la vez, el exceso de trabajos se coloca en una cola y se espera hasta que termine de procesarse la mezcla que se está ejecutando.|  
|**max_concurrent_dynamic_snapshots**||Es un valor **int** que representa el número máximo de sesiones de instantáneas para generar una instantánea de datos filtrados que se puede ejecutar simultáneamente en una publicación de combinación que utiliza filtros de fila con parámetros. Si es **0**, no hay ningún límite. Si se programa un número de procesos de instantáneas superior a éste para que se ejecuten a la vez, el exceso de trabajos se coloca en una cola y se espera hasta que termine de procesarse la mezcla que se está ejecutando.|  
|**post_snapshot_script**||Especifica un puntero a una ubicación de archivos **. SQL** . El Agente de distribución o el Agente de mezcla ejecutan el script posterior a la instantánea después de que se aplique el resto de scripts de objetos replicados y datos durante la sincronización inicial. Para cambiar esta propiedad, se requiere una instantánea nueva.|  
|**pre_snapshot_script**||Especifica un puntero a una ubicación de archivos **. SQL** . El Agente de mezcla ejecuta el script previo a la instantánea antes que cualquiera de los scripts de objetos replicados al aplicar la instantánea en un suscriptor. Para cambiar esta propiedad, se requiere una instantánea nueva.|  
|**publication_compatibility_level**|**100RTM**|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|  
||**90RTM**|[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|  
|**publish_to_activedirectory**|**true**|Este parámetro ha quedado desusado y solo se admite para la compatibilidad de scripts con versiones anteriores. Ya no es posible agregar información de publicación a Active Directory.|  
||**false**|Quita la información de publicaciones de Active Directory.|  
|**replicate_ddl**|**1**|Las instrucciones de lenguaje de definición de datos (DDL) que se ejecutan en el publicador se replican.|  
||**0**|Las instrucciones de DDL no se replican.|  
|**políticas**||Es un valor **int** que representa el número de unidades de *retention_period_unit* para las que se van a guardar los cambios de la publicación especificada. Si la suscripción no está sincronizada en el período de retención y se han quitado, por medio de una operación de limpieza en el distribuidor, los cambios pendientes que podía haber recibido, la suscripción expira y es necesario reinicializarla. El período de retención máximo admitido es el número de días entre el 31 de diciembre de 9999 y la fecha actual.<br /><br /> Nota: el período de retención de las publicaciones de combinación tiene un período de gracia de 24 horas para dar cabida a los suscriptores en diferentes zonas horarias.|  
|**retention_period_unit**|**day**|El período de retención se especifica en días.|  
||**week**|El período de retención se especifica en semanas.|  
||**month**|El período de retención se especifica en meses.|  
||**year**|El período de retención se especifica en años.|  
|**snapshot_in_defaultfolder**|**true**|Los archivos de instantánea se almacenan en la carpeta de instantáneas predeterminada.|  
||**false**|Los archivos de instantáneas se almacenan en la ubicación alternativa especificada por *alt_snapshot_folder*. Esta combinación especifica que los archivos de instantáneas se almacenan tanto en la ubicación predeterminada como en la alternativa.|  
|**snapshot_ready**|**true**|Está disponible la instantánea para la publicación.|  
||**false**|No está disponible la instantánea para la publicación.|  
|**status**|**active**|La publicación está en estado activo.|  
||**inactive**|La publicación está en estado inactivo.|  
|**sync_mode**|**nativo** o<br /><br /> **BCP nativo**|La salida del programa de copia masiva de todas las tablas en modo nativo se utiliza para la instantánea inicial.|  
||**carácter**<br /><br /> o **carácter BCP**|La salida del programa de copia masiva de todas las tablas en modo de carácter se utiliza para la instantánea inicial, que se necesita para todos los suscriptores que no lo son de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**use_partition_groups**<br /><br /> Nota: después de usar partition_groups, si revierte a usar **setupbelongs** y establece **use_partition_groups = false** en **changemergearticle**, es posible que no se refleje correctamente después de tomar una instantánea. Los desencadenadores que genera una instantánea son conformes con los grupos de particiones.<br /><br /> La solución a este escenario es establecer el estado en inactivo, modificar el **use_partition_groups** y, a continuación, establecer el estado en activo.|**true**|La publicación utiliza particiones previamente calculadas.|  
||**false**|La publicación no utiliza particiones previamente calculadas.|  
|**validate_subscriber_info**||Muestra las funciones que se utilizan para recuperar información del suscriptor. Después, valida los criterios de filtro dinámico que se están utilizando para que el suscriptor compruebe que la información se está dividiendo de un modo coherente.|  
|**web_synchronization_url**||Valor predeterminado de la URL de Internet utilizada para la sincronización web.|  
|NULL (predeterminado)||Devuelve la lista de valores admitidos para la *propiedad*.|  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` Confirma que la acción realizada por este procedimiento almacenado podría invalidar una instantánea existente. *force_invalidate_snapshot* es de **bit** y su valor predeterminado es **0**.  
  
 **0** especifica que el cambio de la publicación no invalidará la instantánea. Si el procedimiento almacenado detecta que el cambio requiere una nueva instantánea, se producirá un error y no se realizarán cambios.  
  
 **1** especifica que el cambio de la publicación puede invalidar la instantánea. Si hay suscripciones existentes que requieran una nueva instantánea, se da permiso para que la instantánea existente se marque como obsoleta y se genere otra nueva.  
  
 Vea en la sección de Observaciones las propiedades que, si se cambian, requieren que se genere una nueva instantánea.  
  
`[ @force_reinit_subscription = ] force_reinit_subscription` Confirma que la acción realizada por este procedimiento almacenado puede requerir que se reinicialicen las suscripciones existentes. *force_reinit_subscription* es un **bit** con un valor predeterminado de **0**.  
  
 **0** especifica que, al cambiar la publicación, no es necesario reinicializar las suscripciones. Si el procedimiento almacenado detecta que el cambio requiere la reinicialización de las suscripciones existentes, se producirá un error y no se realizarán cambios.  
  
 **1** especifica que, al cambiar la publicación, se reinicializan las suscripciones existentes y concede permiso para que se produzca la reinicialización de la suscripción.  
  
 Vea en la sección de Notas las propiedades que, si se cambian, requieren que se reinicialicen todas las suscripciones existentes.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_changemergepublication** se utiliza en la replicación de mezcla.  
  
 Si se cambian las propiedades siguientes, se requiere que se genere una nueva instantánea. Debe especificar el valor **1** para el parámetro *force_invalidate_snapshot* .  
  
-   **alt_snapshot_folder**  
  
-   **compress_snapshot**  
  
-   **dynamic_filters**  
  
-   **ftp_address**  
  
-   **ftp_login**  
  
-   **ftp_password**  
  
-   **ftp_port**  
  
-   **ftp_subdirectory**  
  
-   **post_snapshot_script**  
  
-   **publication_compatibility_level** (solo en **80SP3** )  
  
-   **pre_snapshot_script**  
  
-   **snapshot_in_defaultfolder**  
  
-   **sync_mode**  
  
-   **use_partition_groups**  
  
 Al cambiar las propiedades siguientes, se requiere que se reinicialicen las suscripciones existentes. Debe especificar el valor **1** para el parámetro *force_reinit_subscription* .  
  
-   **dynamic_filters**  
  
-   **validate_subscriber_info**  
  
 Para enumerar los objetos de publicación para Active Directory mediante el *publish_to_Active_Directory*, el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objeto ya debe estar creado en Active Directory.  
  
## <a name="example"></a>Ejemplo  
 [!code-sql[HowTo#sp_changemergepublication](../../relational-databases/replication/codesnippet/tsql/sp-changemergepublicatio_1.sql)]  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_changemergepublication**.  
  
## <a name="see-also"></a>Consulte también  
 [Ver y modificar propiedades de publicación](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [Cambiar las propiedades de la publicación y de los artículos](../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [sp_addmergepublication &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md)   
 [sp_dropmergepublication &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropmergepublication-transact-sql.md)   
 [sp_helpmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md)   
 [Procedimientos almacenados de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
