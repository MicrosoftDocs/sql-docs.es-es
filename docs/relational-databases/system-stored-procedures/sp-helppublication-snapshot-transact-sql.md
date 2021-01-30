---
description: sp_helppublication_snapshot (Transact-SQL)
title: sp_helppublication_snapshot (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helppublication_snapshot
- sp_helppublication_snapshot_TSQL
helpviewer_keywords:
- sp_helppublication_snapshot
ms.assetid: 97b4a7ae-40a5-4328-88f1-ff5d105bbb34
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4ae7357cd8bc8f03805c6de948095ff2dbc4d541
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210868"
---
# <a name="sp_helppublication_snapshot-transact-sql"></a>sp_helppublication_snapshot (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Devuelve información acerca del Agente de instantáneas de una publicación específica. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helppublication_snapshot [ @publication = ] 'publication'  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @publisher = ] 'publisher'` Especifica un publicador que no es de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  no se debe usar el *publicador* al agregar un artículo a un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publicador.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**id**|**int**|IDENTIFICADOR del Agente de instantáneas.|  
|**name**|**nvarchar(100**|Nombre del Agente de instantáneas.|  
|**publisher_security_mode**|**smallint**|Modo de seguridad utilizado por el agente al conectarse al publicador, que puede ser uno de los siguientes:<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación<br /><br /> **1** = autenticación de Windows.|  
|**publisher_login**|**sysname**|Inicio de sesión utilizado para conectarse al publicador.|  
|**publisher_password**|**nvarchar (524)**|Por motivos de seguridad, **\*\*\*\*\*\*\*\*\*\*** siempre se devuelve un valor de.|  
|**job_id**|**uniqueidentifier**|Id. único del trabajo del agente.|  
|**job_login**|**nvarchar(512)**|Es la cuenta de Windows con la que se ejecuta el agente de instantáneas, que se devuelve con el formato *dominio* \\ *nombreDeUsuario*.|  
|**job_password**|**sysname**|Por motivos de seguridad, **\*\*\*\*\*\*\*\*\*\*** siempre se devuelve un valor de.|  
|**schedule_name**|**sysname**|Nombre de la programación utilizada para este trabajo de agente.|  
|**frequency_type**|**int**|Se trata de la frecuencia de ejecución programada del agente, que puede ser uno de estos valores.<br /><br /> **1** = una vez<br /><br /> **2** = a petición<br /><br /> **4** = diariamente<br /><br /> **8** = semanalmente<br /><br /> **16** = mensualmente<br /><br /> **32** = mensualmente relativo<br /><br /> **64** = AutoStart<br /><br /> **128** = recurrente|  
|**frequency_interval**|**int**|Los días en los que se ejecuta el agente; puede tener los valores siguientes.<br /><br /> **1** = Domingo<br /><br /> **2** = lunes<br /><br /> **3** = martes<br /><br /> **4** = miércoles<br /><br /> **5** = jueves<br /><br /> **6** = viernes<br /><br /> **7** = sábado<br /><br /> **8** = día<br /><br /> **9** = días de la semana<br /><br /> **10** = días del fin de semana|  
|**frequency_subday_type**|**int**|Es el tipo que define la frecuencia con que se ejecuta el agente cuando *frequency_type* es **4** (diariamente) y puede tener uno de estos valores.<br /><br /> **1** = en el momento especificado<br /><br /> **2** = segundos<br /><br /> **4** = minutos<br /><br /> **8** = horas|  
|**frequency_subday_interval**|**int**|Número de intervalos de *frequency_subday_type* que se producen entre la ejecución programada del agente.|  
|**frequency_relative_interval**|**int**|Es la semana en la que el agente se ejecuta en un mes determinado cuando *frequency_type* es **32** (relativo mensual) y puede tener uno de estos valores.<br /><br /> **1** = primero<br /><br /> **2** = segundo<br /><br /> **4** = tercero<br /><br /> **8** = cuarto<br /><br /> **16** = último|  
|**frequency_recurrence_factor**|**int**|Número de semanas o meses entre las ejecuciones programadas del agente.|  
|**active_start_date**|**int**|Fecha en que se programa la primera ejecución del agente, con el formato AAAAMMDD.|  
|**active_end_date**|**int**|Fecha en que se programa la última ejecución del agente, con el formato AAAAMMDD.|  
|**active_start_time**|**int**|Hora en que se programa la primera ejecución del agente, con el formato HHMMSS.|  
|**active_end_time**|**int**|Hora en que se programa la última ejecución del agente, con el formato HHMMSS.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_help_publication_snapshot** se utiliza en todos los tipos de replicación.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** en el publicador o los miembros del rol fijo de base de datos **db_owner** en la base de datos de publicación pueden ejecutar **sp_help_publication_snapshot**.  
  
## <a name="see-also"></a>Consulte también  
 [Ver y modificar propiedades de publicación](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [sp_addpublication_snapshot &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md)   
 [sp_changepublication_snapshot &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changepublication-snapshot-transact-sql.md)   
 [sp_dropmergepublication &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropmergepublication-transact-sql.md)   
 [sp_droppublication &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-droppublication-transact-sql.md)  
  
  
