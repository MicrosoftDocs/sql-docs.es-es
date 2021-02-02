---
description: Compatibilidad con alta disponibilidad
title: Compatibilidad con alta disponibilidad | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 2e0f6d3f-0536-46d9-8630-835e199515bf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 225d746b5b86f3e0a4dbd2fb122ecdbfb11bf02f
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813145"
---
# <a name="high-availability-support"></a>Compatibilidad con alta disponibilidad

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  El servicio CDC para Oracle está diseñado para tener alta disponibilidad. Las características siguientes proporcionan parte de la alta disponibilidad:  
  
-   El servicio CDC para Oracle no usa ningún recurso de archivo (local o de otro tipo). Todo su estado se almacena en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de destino. Esto facilita el inicio del servicio en un equipo diferente que use la misma instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si se produce algún error en el equipo en el que se ejecuta el servicio. Para reducir el tiempo de recuperación, las transacciones largas o de ejecución prolongada de Oracle se mantienen una tabla de ensayo en el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]de destino, lo que evita la necesidad de volver a examinar muchos registros de transacciones de Oracle después de que se produzca un error (o un reinicio del servicio).  
  
-   El servicio CDC para Oracle puede usar instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en clúster, por lo que se puede recuperar después de que la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] conmute por error a otro nodo del clúster. El administrador del equipo del servicio CDC de Oracle solo necesita especificar la información de conexión a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en clúster al crear un servicio CDC de Oracle.  
  
-   El servicio CDC para Oracle puede usar la característica de creación de reflejo de la base de datos [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]**AlwaysOn** . Para ello, MSXDBCDC y todas las bases de datos CDC deben estar en el mismo grupo de disponibilidad. Además, el administrador del equipo del servicio CDC de Oracle tiene que especificar la información de conexión adecuada de **AlwaysOn** al grupo de disponibilidad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (por ejemplo, las propiedades de conexión `Failover_Partner and Network=dbmssocn`). Esto permite que el servicio CDC reanude automáticamente el procesamiento en una replicación secundaria de las bases de datos tras una conmutación por error.  
  
-   El servicio CDC para Oracle se puede configurar como un recurso de servicio genérico en un clúster de conmutación por error de Windows (con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]o de forma independiente), de forma que sea fácil conmutar por error y revertir al procesamiento de CDC con el clúster. Para configurar el servicio CDC para Oracle como un recurso de un clúster de conmutación por error, el administrador del sistema debe establecer el servicio como un recurso de servicio genérico en cada nodo del clúster de conmutación por error.  
  
-   El servicio CDC para Oracle admite Oracle RAC, que permite que se comunique con los registros de base de datos y de proceso de Oracle aunque uno de los nodos de Oracle RAC esté inactivo.  
  
  
