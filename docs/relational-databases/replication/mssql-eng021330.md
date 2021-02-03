---
description: MSSQL_ENG021330
title: MSSQL_ENG021330 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG021330 error
ms.assetid: e2bb2e21-62a7-4689-b68b-bdfba3fdd985
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: c454c432fe23e56a1a8e3962c4763d3ce68c8e0d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200534"
---
# <a name="mssql_eng021330"></a>MSSQL_ENG021330
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Detalles del mensaje  
  
|Atributo|Value|  
|-|-|  
|Nombre de producto|SQL Server|  
|Id. de evento|21330|  
|Origen de eventos|MSSQLSERVER|  
|Componente|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nombre simbólico||  
|Texto del mensaje|No se pudo crear un subdirectorio bajo el directorio de trabajo de replicación.(%1!)|  
  
## <a name="explanation"></a>Explicación  
 Este error se puede producir cuando una suscripción se inicializa manualmente y hay un problema para crear el directorio en el que se almacenan los scripts de replicación. Este error puede deberse a un problema de permisos: cuando se inicializa una suscripción sin utilizar una instantánea, la cuenta con la que se ejecuta el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el publicador debe tener permisos de escritura para la carpeta de la instantánea en el distribuidor.  
  
## <a name="user-action"></a>Acción del usuario  
 Compruebe que se ha especificado la ruta de acceso correcta para la carpeta de instantáneas y que la cuenta con la que se ejecuta el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el publicador dispone de permisos suficientes.  
  
## <a name="see-also"></a>Consulte también  
 [Modificación de las opciones de instantánea](../../relational-databases/replication/snapshot-options.md)   
 [Referencia de errores y eventos &#40;replicación&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)   
 [Inicializar una suscripción transaccional sin una instantánea](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md)  
  
  
