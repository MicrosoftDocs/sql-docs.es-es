---
description: sys.bandwidth_usage (Azure SQL Database)
title: sys.bandwidth_usage (Azure SQL Database) | Microsoft Docs
ms.custom: ''
ms.date: 01/28/2019
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- bandwidth_usage
- sys.bandwidth_usage
- bandwidth_usage_TSQL
- sys.bandwidth_usage_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.bandwidth_usage
- bandwidth_usage
ms.assetid: 43ed8435-f059-4907-b5c0-193a258b394a
author: julieMSFT
ms.author: jrasnick
monikerRange: = azuresqldb-current
ms.openlocfilehash: 9af11212533337e575976261995161b222e73efa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198570"
---
# <a name="sysbandwidth_usage-azure-sql-database"></a>sys.bandwidth_usage (Azure SQL Database)

[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

> [!NOTE]
> Esto solo se aplica a [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] v11. * *  
  
 Devuelve información sobre el ancho de banda de red usado por cada base de datos en un **[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] servidor de base de datos de V11**,. Cada fila devuelta para una base de datos determinada resume una única dirección y clase de uso durante un período de una hora.  
  
 **Esto está en desuso en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] .**  
  
 La vista **Sys.bandwidth_usage** contiene las columnas siguientes.  
  
|Nombre de columna|Descripción|  
|-----------------|-----------------|  
|**time**|La hora a la que se utilizó el ancho de banda. Las filas de esta vista ofrecen información por horas. Por ejemplo, 2009-09-19 02:00:00.000 significa que el ancho de banda se consumió el 19 de septiembre de 2009 entre las 2:00 a. m. y las 3:00 a. m.|  
|**database_name**|El nombre de la base de datos que utilizó ancho de banda.|  
|**direction**|El tipo de ancho banda utilizado, que puede ser:<br /><br /> Entrada: datos que se mueven al [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] .<br /><br /> Salida: datos que se mueven fuera de [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] .|  
|**class**|La clase de ancho banda utilizada, que puede ser:<br />Interno: datos que se mueven dentro de la plataforma Azure.<br />External: datos que se mueven fuera de la plataforma Azure.<br /><br /> Se devuelve esta clase solamente si la base de datos está ocupada en una relación continua de copias entre las regiones ([!INCLUDE[ssGeoDR](../../includes/ssgeodr-md.md)]). Si una base de datos determinada no participa en ninguna relación de copia continua, no se devuelven las filas "Interlink". Para obtener más información, vea la sección “Comentarios” más adelante en este tema.|  
|**time_period**|El período de tiempo de uso es Pico o Fuera de horas pico. El valor Peak va en función de la región en la que se creó el servidor. Por ejemplo, si se creó un servidor en la región "US_Northwest”, las horas pico se definen como una hora comprendida entre las 10:00 a. m. y a las 06:00:00 p. m. PST.|  
|**quantity**|La cantidad de ancho banda consumido, en kilobytes (KB).|  
  
## <a name="permissions"></a>Permisos

 Esta vista solo está disponible en la base de datos **maestra** para el inicio de sesión principal de nivel de servidor.  
  
## <a name="remarks"></a>Observaciones  
  
### <a name="external-and-internal-classes"></a>Clases externas e internas

 Para cada base de datos que se utiliza en un momento dado, la vista **Sys.bandwidth_usage** devuelve filas que muestran la clase y la dirección del uso del ancho de banda. En el ejemplo siguiente se muestran los datos que se pueden exponer para una base de datos determinada. En este ejemplo, el tiempo son las 17:00 2012-04-21: 00, que se produce durante el período de tiempo máximo. El nombre de base de datos es Db1. En este ejemplo, **Sys.bandwidth_usage** ha devuelto una fila para las cuatro combinaciones de las direcciones de entrada y salida, y las clases externas e internas, como se indica a continuación:  
  
|time|database_name|direction|class|time_period|quantity|  
|----------|--------------------|---------------|-----------|------------------|--------------|  
|2012-04-21 17:00:00|Db1|Entrada|Externo|Peak|66|  
|2012-04-21 17:00:00|Db1|Salida|Externo|Peak|741|  
|2012-04-21 17:00:00|Db1|Entrada|Interno|Peak|1052|  
|2012-04-21 17:00:00|Db1|Salida|Interno|Peak|3525|  
  
### <a name="interpreting-data-direction-for-ssgeodr"></a>Interpretar la dirección de los datos para [!INCLUDE[ssGeoDR](../../includes/ssgeodr-md.md)]

 Para [!INCLUDE[ssGeoDR](../../includes/ssgeodr-md.md)], los datos de uso del ancho banda son visibles en la base de datos maestra lógica en ambos lados de una relación continua de copia. Por lo tanto, debe interpretar los indicadores de dirección de entrada y salida desde la perspectiva de las bases de datos que está consultando. Por ejemplo, considere un flujo de replicación que transfiere 1 MB de datos del servidor de origen al de destino. En este caso, en el servidor de origen, 1 MB cuenta el número total de datos enviados y en el servidor de destino, el 1 MB se registra como datos recibidos.  
  
> [!NOTE]  
> La importación masiva de datos transferidos es del servidor de origen al de destino, en la dirección del flujo de datos de usuario. Sin embargo, algunas transferencias de datos se requieren en la otra dirección.  
