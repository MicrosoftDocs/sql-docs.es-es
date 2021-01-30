---
description: sp_add_operator (Transact-SQL)
title: sp_add_operator (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_operator
- sp_add_operator_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_operator
ms.assetid: 817cd98a-4dff-4ed8-a546-f336c144d1e0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6cb3aed762c87430e75c14c954c80e58ab70d81e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195891"
---
# <a name="sp_add_operator-transact-sql"></a>sp_add_operator (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea un operador (destinatario de la notificación) para utilizarlo con las alertas y los trabajos.  
  
 
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_add_operator [ @name = ] 'name'   
     [ , [ @enabled = ] enabled ]   
     [ , [ @email_address = ] 'email_address' ]   
     [ , [ @pager_address = ] 'pager_address' ]   
     [ , [ @weekday_pager_start_time = ] weekday_pager_start_time ]   
     [ , [ @weekday_pager_end_time = ] weekday_pager_end_time ]   
     [ , [ @saturday_pager_start_time = ] saturday_pager_start_time ]   
     [ , [ @saturday_pager_end_time = ] saturday_pager_end_time ]   
     [ , [ @sunday_pager_start_time = ] sunday_pager_start_time ]   
     [ , [ @sunday_pager_end_time = ] sunday_pager_end_time ]   
     [ , [ @pager_days = ] pager_days ]   
     [ , [ @netsend_address = ] 'netsend_address' ]   
     [ , [ @category_name = ] 'category' ]   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @name = ] 'name'` El nombre de un operador (destinatario de notificación). Este nombre debe ser único y no puede contener el carácter de porcentaje ( **%** ). *Name* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @enabled = ] enabled` Indica el estado actual del operador. *Enabled* es de **tinyint** y su valor predeterminado es **1** (habilitado). Si es **0**, el operador no está habilitado y no recibe notificaciones.  
  
`[ @email_address = ] 'email_address'` Dirección de correo electrónico del operador. Esta cadena se pasa directamente al sistema de correo electrónico. *EMAIL_ADDRESS* es de tipo **nvarchar (100)** y su valor predeterminado es NULL.  
  
 Puede especificar una dirección de correo electrónico física o un alias para *EMAIL_ADDRESS*. Por ejemplo:  
  
 '**juan_perez**' o '**juan_perez \@ XYZ.com**'  
  
> [!NOTE]  
>  Debe utilizar la dirección de correo electrónico para Correo electrónico de base de datos.  
  
`[ @pager_address = ] 'pager_address'` Dirección del buscapersonas del operador. Esta cadena se pasa directamente al sistema de correo electrónico. *pager_address* es de tipo **nvarchar (100)** y su valor predeterminado es NULL.  
  
`[ @weekday_pager_start_time = ] weekday_pager_start_time` Hora a partir de la cual el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] agente envía una notificación por buscapersonas al operador especificado los días laborables, de lunes a viernes. *weekday_pager_start_time* es de **tipo int** y su valor predeterminado es **090000**, que indica 9:00 A.M. en un reloj de 24 horas. Se debe especificar con el formato HHMMSS.  
  
`[ @weekday_pager_end_time = ] weekday_pager_end_time` Hora a partir de la cual el servicio **SQLServerAgent** ya no envía una notificación por buscapersonas al operador especificado los días laborables, de lunes a viernes. *weekday_pager_end_time* es de **tipo int** y su valor predeterminado es 180000, que indica 6:00 P.M. en un reloj de 24 horas. Se debe especificar con el formato HHMMSS.  
  
`[ @saturday_pager_start_time = ] saturday_pager_start_time` Hora a partir de la cual el servicio **SQLServerAgent** envía una notificación por buscapersonas al operador especificado los sábados. *saturday_pager_start_time* es de **tipo int** y su valor predeterminado es 090000, que indica 9:00 A.M. en un reloj de 24 horas. Se debe especificar con el formato HHMMSS.  
  
`[ @saturday_pager_end_time = ] saturday_pager_end_time` Hora a partir de la cual el servicio **SQLServerAgent** ya no envía una notificación por buscapersonas al operador especificado los sábados. *saturday_pager_end_time* es de **tipo int** y su valor predeterminado es **180000**, que indica 6:00 P.M. en un reloj de 24 horas. Se debe especificar con el formato HHMMSS.  
  
`[ @sunday_pager_start_time = ] sunday_pager_start_time` Hora a partir de la cual el servicio **SQLServerAgent** envía una notificación por buscapersonas al operador especificado los domingos. *sunday_pager_start_time* es de **tipo int** y su valor predeterminado es **090000**, que indica 9:00 A.M. en un reloj de 24 horas. Se debe especificar con el formato HHMMSS.  
  
`[ @sunday_pager_end_time = ] sunday_pager_end_time` Hora a partir de la cual el servicio **SQLServerAgent** ya no envía una notificación por buscapersonas al operador especificado los domingos. *sunday_pager_end_time* es de **tipo int** y su valor predeterminado es **180000**, que indica 6:00 P.M. en un reloj de 24 horas. Se debe especificar con el formato HHMMSS.  
  
`[ @pager_days = ] pager_days` Es un número que indica los días en los que el operador está disponible para las páginas (sujeto a las horas de inicio y finalización especificadas). *pager_days* es de tipo **tinyint** y su valor predeterminado es **0** , lo que indica que el operador nunca está disponible para recibir una página. Los valores válidos son de **0** a **127**. *pager_days* se calcula agregando los valores individuales de los días necesarios. Por ejemplo, de lunes a viernes, **2** + **4** + **8** + **16** + **32**  =  **62**. En la siguiente tabla se incluye el valor para cada día de la semana.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1**|Domingo|  
|**2**|Lunes|  
|**4**|Martes|  
|**8**|Miércoles|  
|**16**|Jueves|  
|**32**|Viernes|  
|**64**|Sábado|  
  
`[ @netsend_address = ] 'netsend_address'` Dirección de red del operador al que se envía el mensaje de red. *netsend_address* es de tipo **nvarchar (100)** y su valor predeterminado es NULL.  
  
`[ @category_name = ] 'category'` Nombre de la categoría de este operador. *Category* es de **tipo sysname y su** valor predeterminado es NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 **sp_add_operator** se debe ejecutar desde la base de datos **msdb** .  
  
 Los avisos de localización son compatibles con el sistema de correo electrónico, que debe disponer de capacidad de correo electrónico a buscapersonas si desea utilizar esta funcionalidad.  
  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ofrece un método gráfico sencillo para administrar trabajos y es el método recomendado para crear y administrar la infraestructura de trabajo.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar **sp_add_operator**.  
  
## <a name="examples"></a>Ejemplos  
 En este ejemplo se configura la información del operador para `danwi`. El operador está habilitado. El Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] envía notificaciones por buscapersonas de lunes a viernes, de 8 a.m. a 5 p.m.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_operator  
    @name = N'Dan Wilson',  
    @enabled = 1,  
    @email_address = N'danwi',  
    @pager_address = N'5551290AW@pager.Adventure-Works.com',  
    @weekday_pager_start_time = 080000,  
    @weekday_pager_end_time = 170000,  
    @pager_days = 62 ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_delete_operator &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-operator-transact-sql.md)   
 [sp_help_operator &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-operator-transact-sql.md)   
 [sp_update_operator &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-operator-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
