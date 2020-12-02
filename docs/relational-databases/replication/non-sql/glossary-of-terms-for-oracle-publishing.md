---
description: Glosario de términos de publicaciones de Oracle
title: Glosario de términos de publicaciones de Oracle | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Oracle publishing [SQL Server replication], glossary
ms.assetid: e21dfa4b-6144-4be7-9cbf-ca2709b2bd9f
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 2dae5a66a762e53a4748bf43732aed70c5c0edfc
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "88465161"
---
# <a name="glossary-of-terms-for-oracle-publishing"></a>Glosario de términos de publicaciones de Oracle
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Debe estar familiarizado con los siguientes términos de Oracle a la hora de configurar y administrar publicaciones de Oracle. Para obtener una lista completa de términos de Oracle, vea la documentación en pantalla de Oracle.  
  
#### <a name="index-organized-tables-iot"></a>Index Organized Tables (IOT) (Tablas organizadas de índice)  
 Tabla cuyos datos están ordenados físicamente en el disco por orden de índice; es similar a una tabla de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] con un índice agrupado. Una tabla IOT se replica en un suscriptor como una tabla con un índice clúster.  
  
#### <a name="instance"></a>Instancia  
 Una base de datos Oracle está asociada con una instancia. La instancia se compone de la memoria y los procesos en segundo plano que respaldan la base de datos. Una instancia de Oracle se asigna siempre a una única base de datos, mientras que una instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] puede contener varias bases de datos. Existen ocasiones en las que una base de datos Oracle puede tener varias instancias.  
  
#### <a name="oracle-listener"></a>Oracle Listener (Escucha de Oracle)  
 Controla el tráfico de red entrante de una instancia de base de datos Oracle. Al configurar la conectividad de red de una base de datos Oracle, se especifica el protocolo mediante el que se envía el tráfico y el puerto en el que el Listener escucha el tráfico. Normalmente, el Listener se configura para que se ejecute en el mismo equipo que la instancia de base de datos Oracle y se puede configurar para dar servicio a una o más instancias.  
  
#### <a name="rowid"></a>ROWID  
 Puntero que señala la ubicación de una fila concreta de una base de datos. Puesto que recuperar filas usando ROWID es más rápido que utilizar un recorrido de tabla o índice, la replicación utiliza ROWID temporalmente durante el procesamiento de los cambios de la tabla publicada.  
  
#### <a name="sequence"></a>Secuencia  
 Objeto de base de datos que se utiliza para generar números exclusivos. La replicación utiliza secuencias para ordenar los cambios efectuados en las tablas publicadas.  
  
#### <a name="sqlplus"></a>SQL\*Plus  
 Aplicación que se utiliza para obtener acceso y realizar consultas en bases de datos Oracle. Es similar a la utilidad  **sqlcmd** de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
#### <a name="synonym"></a>Synonym (Sinónimo)  
 Alias de un objeto. El sinónimo público especial **MSSQLSERVERDISTRIBUTOR** se crea automáticamente cuando se configura un publicador de Oracle. El sinónimo hace referencia a la tabla **HREPL_Distributor** y proporciona un puntero lógico al distribuidor de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que da servicio al publicador.  
  
 Una vez publicada una base de datos Oracle, los intentos posteriores de configurar este publicador para usar un distribuidor de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] diferente producirán un error porque este sinónimo público identifica al distribuidor concreto que ya se ha configurado para dar servicio al publicador.  
  
#### <a name="tablespace"></a>Tablespace (Espacio de tabla)  
 Unidad de almacenamiento de base de datos que es en líneas generales equivalente a un grupo de archivos en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
#### <a name="tns-service-name"></a>TNS Service Name (Nombre de servicio TNS)  
 TNS (Transparent Network Substrate, Sustrato de red transparente) es una capa de comunicación que utilizan las bases de datos Oracle. TNS Service Name es el nombre por el que se conocen las instancias de una base de datos Oracle en una red. Se asigna un nombre a este servicio cuando se configura la conectividad de la base de datos Oracle. La replicación utiliza el nombre del servicio TNS para identificar al publicador y establecer conexiones.  
  
#### <a name="user-schema"></a>User schema (Esquema de usuario)  
 Un esquema de usuario puede considerarse como un usuario de base de datos que es propietario de un conjunto de objetos de base de datos concreto. El esquema del usuario administrativo de la replicación es propietario de todos los objetos que se crean en el proceso de replicación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en la base de datos Oracle, con la excepción del sinónimo público **MSSQLSERVERDISTRIBUTOR** .  
  
## <a name="see-also"></a>Consulte también  
 [Configurar un publicador de Oracle](../../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md)   
 [Objects Created on the Oracle Publisher](../../../relational-databases/replication/non-sql/objects-created-on-the-oracle-publisher.md)  (Objetos creados en el publicador de Oracle)  
 [Non-SQL Server Publishers](../../../relational-databases/replication/non-sql/non-sql-server-publishers.md)  (Publicadores que no son de SQL Server)  
 [Información general de la publicación de Oracle](../../../relational-databases/replication/non-sql/oracle-publishing-overview.md)  
  
  
