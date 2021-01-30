---
description: sys.configurations (Transact-SQL)
title: sys.configurations (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.configurations_TSQL
- configurations
- sys.configurations
- configurations_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.configurations catalog view
ms.assetid: c4709ed1-bf88-4458-9e98-8e9b78150441
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: bca35c6287a88c1439ca64c6f7cfd431de0269ea
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159112"
---
# <a name="sysconfigurations-transact-sql"></a>sys.configurations (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila para cada valor de opción de configuración de todo el servidor en el sistema.  

|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**configuration_id**|**int**|Id. exclusivo del valor de configuración.|  
|**name**|**nvarchar(35)**|Nombre de la opción de configuración.|  
|**value**|**sql_variant**|Valor configurado para esta opción.|  
|**Mínimo**|**sql_variant**|Valor mínimo de la opción de configuración.|  
|**Máximo**|**sql_variant**|Valor máximo de la opción de configuración.|  
|**value_in_use**|**sql_variant**|Valor actual de esta opción.|  
|**description**|**nvarchar(255)**|Descripción de la opción de configuración.|  
|**is_dynamic**|**bit**|1 = La variable que surte surte efecto cuando se ejecuta la instrucción RECONFIGURE.|  
|**is_advanced**|**bit**|1 = la variable solo se muestra cuando se establece **Show advancedoption** .|  
  
 ## <a name="remarks"></a>Observaciones
  Para obtener una lista de todas las opciones de configuración del servidor, consulte [Opciones de configuración del servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
> [!NOTE]  
>  Para las opciones de configuración de nivel de base de datos, vea [ALTER DATABASE scoped configuration &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md). Para configurar la configuración de Soft-NUMA, consulte [&#40;de Soft-numa SQL Server&#41;](../../database-engine/configure-windows/soft-numa-sql-server.md).  
 
La vista de catálogo sys.configurations se puede usar para determinar el config_value (la columna de valor), el run_value (la columna de value_in_use) y si la opción de configuración es dinámica (no requiere un reinicio del motor de servidor ni la columna is_dynamic).

> [!NOTE]
> El config_value del conjunto de resultados de sp_configure es equivalente a la columna **sys.configurations. Value** . La **run_value** es equivalente a la columna de **urations.value_in_usesys.config** .

La siguiente consulta se puede utilizar para determinar si no se han instalado valores configurados:

```SQL
select * from sys.configurations where value != value_in_use
```

Si el valor es igual al cambio de la opción de configuración realizada pero el **value_in_use** no es el mismo, el comando reconfigure no se ejecutó o se produjo un error o se debe reiniciar el motor del servidor.

Hay opciones de configuración en las que el valor y el value_in_use no pueden ser iguales y este comportamiento es el esperado. Por ejemplo:

"Max Server Memory (MB)": el valor predeterminado configurado 0 se muestra como **value_in_use** = 2147483647<br>

"memoria de servidor mínima (MB)": el valor predeterminado configurado de 0 puede aparecer como **value_in_use** = 8 (32 bits) o 16 (64 bits). En algunos casos, el **value_in_use** es 0. En esta situación, el **value_in_use** "true" es 8 (32 bits) o 16 (64 bits).


La columna **is_dynamic** se puede utilizar para determinar si la opción de configuración requiere un reinicio. is_dynamic = 1 significa que cuando se ejecuta la comandos reconfigure (T-SQL), el nuevo valor surtirá efecto "inmediatamente" (en algunos casos, es posible que el motor del servidor no evalúe el nuevo valor inmediatamente, pero lo hará en el curso normal de su ejecución). is_dynamic = 0 significa que el valor de configuración cambiado no surtirá efecto hasta que se reinicie el servidor, aunque se haya ejecutado el comando reconfigure (T-SQL).

En el caso de una opción de configuración que no sea dinámica, no hay ninguna manera de saber si se ha ejecutado el comando reconfigure (T-SQL) para realizar el primer paso de la instalación del cambio de configuración. Antes de reiniciar SQL Server para instalar un cambio de configuración, ejecute el comando reconfigure (T-SQL) para asegurarse de que todos los cambios de configuración se aplicarán después de un reinicio de SQL Server. 
 
 
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** . Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo de configuración de todo el servidor &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/server-wide-configuration-catalog-views-transact-sql.md)   
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
