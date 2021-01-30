---
description: sp_update_operator (Transact-SQL)
title: sp_update_operator (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_update_operator_TSQL
- sp_update_operator
dev_langs:
- TSQL
helpviewer_keywords:
- sp_update_operator
ms.assetid: 231750a6-4828-4d03-afe6-b91d38c42ed3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 2d664bbfc012ee262528397f735525138e91e82b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187301"
---
# <a name="sp_update_operator-transact-sql"></a>sp_update_operator (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Actualiza información acerca de un operador (destinatario de la notificación) para utilizarla con las alertas y los trabajos.  
  
   ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_update_operator   
     [ @name =] 'name'   
     [ , [ @new_name = ] 'new_name' ]   
     [ , [ @enabled = ] enabled]   
     [ , [ @email_address = ] 'email_address' ]  
     [ , [ @pager_address = ] 'pager_number']   
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
 [ @name =] '*nombre*'  
 El nombre del operador que se va a modificar. *Name* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
 [ @new_name =] '*new_name*'  
 Nuevo nombre del operador. El nombre debe ser único. *new_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
 [ @enabled =] *habilitado*  
 Número que indica el estado actual del operador (**1** si está habilitado actualmente; **0** si no lo está). *Enabled* es de **tinyint** y su valor predeterminado es NULL. Si no está habilitado, un operador no recibirá notificaciones de alertas.  
  
 [ @email_address =] '*EMAIL_ADDRESS*'  
 Dirección de correo electrónico del operador. Esta cadena se pasa directamente al sistema de correo electrónico. *EMAIL_ADDRESS* es de tipo **nvarchar (100)** y su valor predeterminado es NULL.  
  
 [ @pager_address =] '*pager_number*'  
 Dirección del buscapersonas del operador. Esta cadena se pasa directamente al sistema de correo electrónico. *pager_number* es de tipo **nvarchar (100)** y su valor predeterminado es NULL.  
  
 [ @weekday_pager_start_time =] *weekday_pager_start_time*  
 Especifica la hora a partir de la cual puede enviarse una notificación por buscapersonas a este operador, de lunes a viernes. *weekday_pager_start_time* es de **tipo int**, su valor predeterminado es NULL y debe especificarse en el formato HHMMSS para utilizarse con un reloj de 24 horas.  
  
 [ @weekday_pager_end_time =] *weekday_pager_end_time*  
 Especifica la hora a partir de la cual no puede enviarse una notificación por buscapersonas al operador especificado, de lunes a viernes. *weekday_pager_end_time* es de **tipo int**, su valor predeterminado es NULL y debe especificarse en el formato HHMMSS para utilizarse con un reloj de 24 horas.  
  
 [ @saturday_pager_start_time =] *saturday_pager_start_time*  
 Especifica la hora a partir de la cual puede enviarse una notificación por buscapersonas los sábados al operador especificado. *saturday_pager_start_time* es de **tipo int**, su valor predeterminado es NULL y debe especificarse en el formato HHMMSS para utilizarse con un reloj de 24 horas.  
  
 [ @saturday_pager_end_time =] *saturday_pager_end_time*  
 Especifica la hora a partir de la cual no puede enviarse una notificación por buscapersonas los sábados al operador especificado. *saturday_pager_end_time* es de **tipo int**, su valor predeterminado es NULL y debe especificarse en el formato HHMMSS para utilizarse con un reloj de 24 horas.  
  
 [ @sunday_pager_start_time =] *sunday_pager_start_time*  
 Especifica la hora a partir de la cual puede enviarse una notificación por buscapersonas los domingos al operador especificado. *sunday_pager_start_time* es de **tipo int**, su valor predeterminado es NULL y debe especificarse en el formato HHMMSS para utilizarse con un reloj de 24 horas.  
  
 [ @sunday_pager_end_time =] *sunday_pager_end_time*  
 Especifica la hora a partir de la cual no puede enviarse una notificación por buscapersonas los domingos al operador especificado. *sunday_pager_end_time* es de **tipo int**, su valor predeterminado es NULL y debe especificarse en el formato HHMMSS para utilizarse con un reloj de 24 horas.  
  
 [ @pager_days =] *pager_days*  
 Especifica los días en que el operador está disponible para recibir mensajes por buscapersonas (de acuerdo con las horas inicial y final especificadas). *pager_days* es de **tinyint**, su valor predeterminado es NULL y debe ser un valor comprendido entre **0** y **127**. *pager_days* se calcula agregando los valores individuales de los días necesarios. Por ejemplo, de lunes a viernes, **2** + **4** + **8** + **16** + **32**  =  **64**.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1**|Domingo|  
|**2**|Lunes|  
|**4**|Martes|  
|**8**|Miércoles|  
|**16**|Jueves|  
|**32**|Viernes|  
|**64**|Sábado|  
  
 [ @netsend_address =] '*netsend_address*'  
 La dirección de red del operador al que se envía el mensaje de red. *netsend_address* es de tipo **nvarchar (100)** y su valor predeterminado es NULL.  
  
 [ @category_name =] '*categoría*'  
 El nombre de la categoría de esta alerta. *Category* es de **tipo sysname y su** valor predeterminado es NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 sp_update_operator se debe ejecutar desde la base de datos msdb.  
  
## <a name="permissions"></a>Permisos  
 Los permisos de ejecución para este procedimiento corresponden de forma predeterminada a los miembros del rol fijo de servidor sysadmin.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se actualiza el estado del operador a habilitado y se establecen los días (de lunes a viernes, de 8 a.m. a 5 p.m.) en que se le puede enviar una notificación por buscapersonas.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_update_operator   
    @name = N'François Ajenstat',  
    @enabled = 1,  
    @email_address = N'françoisa',  
    @pager_address = N'5551290AW@pager.Adventure-Works.com',  
    @weekday_pager_start_time = 080000,  
    @weekday_pager_end_time = 170000,  
    @pager_days = 64 ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_add_operator &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-operator-transact-sql.md)   
 [sp_delete_operator &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-operator-transact-sql.md)   
 [sp_help_operator &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-operator-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
