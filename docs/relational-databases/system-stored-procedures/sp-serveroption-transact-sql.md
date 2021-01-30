---
description: sp_serveroption (Transact-SQL)
title: sp_serveroption (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/11/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_serveroption_TSQL
- sp_serveroption
dev_langs:
- TSQL
helpviewer_keywords:
- 7343 (Database Engine error)
- sp_serveroption
ms.assetid: 47d04a2b-dbf0-4f15-bd9b-81a2efc48131
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ec6dfda1d4e12b190999b82e16c27b5127d5af6d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190773"
---
# <a name="sp_serveroption-transact-sql"></a>sp_serveroption (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Establece las opciones de servidor de los servidores remotos y vinculados.  
  
 
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
sp_serveroption [@server = ] 'server'   
      ,[@optname = ] 'option_name'       
      ,[@optvalue = ] 'option_value' ;  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @server = ] 'server'` Es el nombre del servidor para el que se va a establecer la opción. *server* es de tipo **sysname** y no tiene ningún valor predeterminado.  
  
`[ @optname = ] 'option_name'` Es la opción que se va a establecer para el servidor especificado. *option_name* es de tipo **VARCHAR (** 35 **)** y no tiene ningún valor predeterminado. *option_name* puede ser cualquiera de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**compatible con la intercalación**|Afecta a la ejecución de consultas distribuidas en los servidores vinculados. Si esta opción se establece en **true**, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supone que todos los caracteres del servidor vinculado son compatibles con el servidor local, con respecto al Juego de caracteres y a la secuencia de intercalación (o criterio de ordenación). Esta opción habilita a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para enviar comparaciones en columnas de caracteres al proveedor. Si no se establece esta opción, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] siempre evalúa localmente las comparaciones en las columnas de caracteres.<br /><br /> Esta opción solo se debe establecer si se tiene la certeza de que el origen de datos correspondiente al servidor vinculado tiene el mismo juego de caracteres y criterio de ordenación que el servidor local.|  
|**nombre de intercalación**|Especifica el nombre de la intercalación utilizada por el origen de datos remoto si **usar intercalación remota** es **true** y el origen de datos no es un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] origen de datos. El nombre debe pertenecer a una de las intercalaciones que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]admite.<br /><br /> Utilice esta opción cuando se obtenga acceso a un origen de datos OLE DB que no sea [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], pero que tenga una intercalación que coincida con una de las intercalaciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> El servidor vinculado debe permitir el uso de una única intercalación para todas las columnas de ese servidor. No establezca esta opción si el servidor vinculado admite varias intercalaciones dentro de un único origen de datos o si no se puede determinar si la intercalación del servidor vinculado coincide con alguna de las intercalaciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**tiempo de espera de conexión**|Valor de tiempo de espera en segundos para conectarse a un servidor vinculado.<br /><br /> Si es **0**, use el valor predeterminado de **sp_configure** .|  
|**acceso a datos**|Habilita y deshabilita un servidor vinculado para el acceso a consultas distribuidas. Solo se puede usar para las entradas **Sys. Server** agregadas a través de **sp_addlinkedserver**.|  
|**dist**|Distribuidor.|  
|**lazy schema validation**|Determina si se comprobará el esquema de las tablas remotas.<br /><br /> Si **es true**, se omite la comprobación del esquema de las tablas remotas al principio de la consulta.|  
|**Pub**|Editor.|  
|**tiempo de espera de consulta**|Valor de tiempo de espera para las consultas que se realizan en un servidor vinculado.<br /><br /> Si es **0**, use el valor predeterminado de **sp_configure** .|  
|**RPC**|Habilita RPC desde el servidor dado.|  
|**salida de RPC**|Habilita RPC al servidor dado.|  
|**sub**|Suscriptor.|  
|**sistema**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**usar intercalación remota**|Determina si se utilizará la intercalación de una columna remota o de un servidor local.<br /><br /> Si es **true**, la intercalación de columnas remotas se utiliza para los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] orígenes de datos y la intercalación especificada en **el nombre de intercalación** se utiliza para los orígenes de datos que no son de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> Si **es false**, las consultas distribuidas siempre utilizarán la intercalación predeterminada del servidor local, mientras que el **nombre de intercalación** y la intercalación de columnas remotas se omiten. El valor predeterminado es **false**. (El valor **false** es compatible con la semántica de intercalación que se usa en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7,0).|  
|**remote proc transaction promotion**|Use esta opción para proteger las acciones de un procedimiento entre servidores a través de una transacción del Coordinador de transacciones distribuidas de [!INCLUDE[msCoName](../../includes/msconame-md.md)] (MS DTC). Cuando esta opción es TRUE (u ON) al llamar a un procedimiento almacenado remoto se inicia una transacción distribuida y se da de alta la transacción en MS DTC. La instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que efectúa la llamada al procedimiento almacenado remoto es el originador de la transacción y controla su realización. Posteriormente, cuando se ejecuta para la conexión una instrucción COMMIT TRANSACTION o ROLLBACK TRANSACTION, la instancia que controla la transacción solicita a MS DTC que administre la realización de la transacción distribuida entre los servidores participantes.<br /><br /> Una vez iniciada una transacción distribuida de [!INCLUDE[tsql](../../includes/tsql-md.md)], se pueden realizar llamadas a procedimientos almacenados remotos en otras instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se hayan definido como servidores vinculados. Todos los servidores vinculados se dan de alta en la transacción distribuida de [!INCLUDE[tsql](../../includes/tsql-md.md)] y MS DTC se asegura de que la transacción se complete en cada uno de ellos.<br /><br /> Si esta opción está establecida en FALSE (u OFF), una transacción local no se promoverá a una transacción distribuida mientras se llame a una llamada a procedimiento remoto en un servidor vinculado.<br /><br /> Si antes de realizar una llamada a procedimiento entre servidores, la transacción ya es una transacción distribuida, esta opción no tiene efecto alguno. La llamada a procedimiento en un servidor vinculado se ejecutará en la misma transacción distribuida.<br /><br /> Si antes de realizar una llamada a procedimiento entre servidores, no existe ninguna transacción activa en la conexión, esta opción no tiene efecto alguno. A continuación, el procedimiento se ejecuta en el servidor vinculado sin transacciones activas.<br /><br /> El valor predeterminado para esta opción es TRUE (u ON).|  
  
`[ @optvalue = ] 'option_value'` Especifica si el *option_name* debe estar habilitado (**true** o **activado**) o deshabilitado (**false** u **OFF**). *option_value* es de tipo **VARCHAR (** 10 **)** y no tiene ningún valor predeterminado.  
  
 *option_value* puede ser un entero no negativo para las opciones de tiempo de espera de **conexión** y de **tiempo de espera** de la consulta. En la opción **collation Name** , *option_value* puede ser un nombre de intercalación o null.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 Si la opción **collation compatible** se establece en true, el **nombre de intercalación** se establecerá automáticamente en NULL. Si **el nombre de la intercalación** se establece en un valor que no sea NULL, **collation compatible** se establecerá automáticamente en false.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso ALTER ANY LINKED SERVER en el servidor.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se configura un servidor vinculado que corresponde a otra instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], `SEATTLE3`, para que sea compatible con la intercalación de la instancia local de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql  
USE master;  
EXEC sp_serveroption 'SEATTLE3', 'collation compatible', 'true';  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados de consultas distribuidas &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/distributed-queries-stored-procedures-transact-sql.md)   
 [sp_adddistpublisher &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)   
 [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)   
 [sp_dropdistpublisher &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropdistpublisher-transact-sql.md)   
 [sp_helpserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
