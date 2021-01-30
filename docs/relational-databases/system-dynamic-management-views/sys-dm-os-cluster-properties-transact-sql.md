---
description: sys.dm_os_cluster_properties (Transact-SQL)
title: sys.dm_os_cluster_properties (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_cluster_properties_TSQL
- sys.dm_os_cluster_properties
- dm_os_cluster_properties_TSQL
- dm_os_cluster_properties
dev_langs:
- TSQL
helpviewer_keywords:
- dm_os_cluster_properties
- sys.dm_os_cluster_properties
ms.assetid: 6d82e770-fba7-49e0-9a0c-3b34b393e4a7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: cc832c3e5e89e0b78761e680ce53b0f1884e1f92
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184943"
---
# <a name="sysdm_os_cluster_properties-transact-sql"></a>sys.dm_os_cluster_properties (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una fila con los valores actuales de las propiedades de recurso de clúster de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] identificadas en este tema. No se devolverá ningún dato si esta vista se ejecuta en una instancia independiente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Estas propiedades se usan para establecer los valores que afectan a la detección de errores, el tiempo de respuesta de errores y el registro para supervisar el estado de mantenimiento de la instancia en clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  

|Nombre de columna|Propiedad|Descripción|  
|-----------------|--------------|-----------------|  
|VerboseLogging|bigint|Nivel de registro para el clúster de conmutación por error de SQL Server. Se puede activar el registro detallado para proporcionar detalles adicionales en los registros de errores para solucionar problemas. Uno de los siguientes valores:<br /><br /> 0: el registro está desactivado (valor predeterminado)<br /><br /> 1: solo errores<br /><br /> 2: errores y advertencias<br /><br /> Para obtener más información, vea [ALTER Server CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-configuration-transact-sql.md).|  
|SqlDumperDumpFlags|bigint|Las marcas de volcado de SQLDumper determinan el tipo de archivos de volcado generados por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El valor predeterminado es 0.|  
|SqlDumperDumpPath|nvarchar(260)|Ubicación donde la utilidad SQLDumper genera los archivos de volcado.|  
|SqlDumperDumpTimeOut|bigint|Valor de tiempo de espera en milisegundos para que la utilidad SQLDumper genere un volcado en caso de un error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El valor predeterminado es 0.|  
|FailureConditionLevel|bigint|Establece las condiciones en que el clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe producir la conmutación por error o reiniciarse. El valor predeterminado es 3. Para obtener una explicación detallada o cambiar la configuración de las propiedades, vea [Configure FailureConditionLevel Property Settings](../../sql-server/failover-clusters/windows/configure-failureconditionlevel-property-settings.md).|  
|HealthCheckTimeout|bigint|Valor de tiempo de espera que establece cuánto tiempo debe la DLL del recurso del motor de base de datos de SQL Server esperar la información de estado del servidor antes de considerar que la instancia de SQL Server no responde. El valor de tiempo de espera se expresa en milisegundos. El valor predeterminado es 60000. Para obtener más información o cambiar la configuración de esta propiedad, vea [Configure HealthCheckTimeout Property Settings](../../sql-server/failover-clusters/windows/configure-healthchecktimeout-property-settings.md).|  
  
## <a name="permissions"></a>Permisos  
 Necesita permisos VIEW SERVER STATE en la instancia en clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se usa sys.dm_os_cluster_properties para devolver los valores de propiedad para el recurso de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```  
SELECT VerboseLogging, SqlDumperDumpFlags, SqlDumperDumpPath, SqlDumperDumpTimeOut, FailureConditionLevel, HealthCheckTimeout  
FROM sys.dm_os_cluster_properties;  
```  
  
 A continuación se muestra un conjunto de resultados de ejemplo.  
  
|VerboseLogging|SqlDumperDumpFlags|SqlDumperDumpPath|SqlDumperDumpTimeOut|FailureConditionLevel|HealthCheckTimeout|  
|--------------------|------------------------|-----------------------|--------------------------|---------------------------|------------------------|  
|0|0|NULL|0|3|60000|  
  
  
