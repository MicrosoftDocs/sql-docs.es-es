---
description: MSSQL_ENG021331
title: MSSQL_ENG021331 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG021331 error
ms.assetid: 9acd75d9-fda1-44cd-ba17-20295ad53ea0
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: fa13efc56fec37761c236de5daec2ce559ecd6fd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200527"
---
# <a name="mssql_eng021331"></a>MSSQL_ENG021331
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Detalles del mensaje  
  
|Atributo|Value|  
|-|-|  
|Nombre de producto|SQL Server|  
|Id. de evento|21331|  
|Origen de eventos|MSSQLSERVER|  
|Componente|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nombre simbólico||  
|Texto del mensaje|No se pudo copiar el archivo de script de usuario en el distribuidor.(%ls)|  
  
## <a name="explanation"></a>Explicación  
 Este error puede ocurrir cuando se inicializa manualmente una suscripción y no es posible guardar en el directorio especificado los scripts generados por la replicación o especificados en un comando de replicación. Este error puede deberse a un problema de permisos: cuando se inicializa una suscripción sin utilizar una instantánea, la cuenta con la que se ejecuta el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el publicador debe tener permisos de escritura para la carpeta de la instantánea en el distribuidor.  
  
## <a name="user-action"></a>Acción del usuario  
 Compruebe que se ha especificado la ruta de acceso correcta para la carpeta de instantáneas y que la cuenta con la que se ejecuta el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el publicador dispone de permisos suficientes.  
  
## <a name="see-also"></a>Consulte también  
 [Especificación de opciones de instantáneas](../../relational-databases/replication/snapshot-options.md)   
 [Referencia de errores y eventos &#40;replicación&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)   
 [Inicializar una suscripción transaccional sin una instantánea](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md)  
  
  
