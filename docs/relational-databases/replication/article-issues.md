---
description: Problemas de los artículos
title: Problemas de los artículos | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.newpubwizard.articleissues.f1
ms.assetid: bde57da2-dd47-412f-9df7-9224968b2448
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 53df3848ffac7b7408d90b48d74a27410cd2a155
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467276"
---
# <a name="article-issues"></a>Problemas de los artículos
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  La página **Problemas de los artículos** muestra los problemas encontrados en los artículos y los cambios necesarios para solucionar dichos problemas. La siguiente tabla muestra los posibles problemas y las acciones necesarias para garantizar que la replicación y las aplicaciones existentes funcionen correctamente.  
  
|Problema de los artículos|Detalles|Acción requerida|  
|-------------------|-------------|---------------------|  
|Se agregarán columnas uniqueidentifier a las tablas.|La replicación requiere una columna de tipo de datos **uniqueidentifier** para todos los artículos de una publicación de combinación o una publicación transaccional que permite suscripciones de actualización.|La replicación agrega automáticamente una columna de tipo de datos **uniqueidentifier** a las tablas publicadas que no tengan una durante la generación de la primera instantánea. Deberá comprobar que las instrucciones INSERT y UPDATE que hacen referencia a dichas tablas utilizan listas de columnas. Compruebe también que existe suficiente espacio en el disco para la columna adicional.|  
|Las columnas IDENTITY requieren la opción NOT FOR REPLICATION.|La replicación requiere que todas las columnas IDENTITY utilicen la opción NOT FOR REPLICATION. Si una columna IDENTITY publicada no utiliza esta opción, es posible que los comandos INSERT no se repliquen correctamente.|Se aplica a las publicaciones creadas en publicadores que ejecutan [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] y versiones anteriores. Deberá especificar la propiedad NOT FOR REPLICATION para todas las columnas IDENTITY.|  
|No se ha transferido la propiedad IDENTITY a los suscriptores.|Esta publicación no permite actualizaciones a los suscriptores. Si se transfieren las columnas IDENTITY al suscriptor, no se transferirá la propiedad IDENTITY. Por ejemplo, una columna definida como INT IDENTITY en el publicador, se definirá como INT en el suscriptor.|Se aplica a las publicaciones creadas en publicadores que ejecutan [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] y versiones anteriores. No se requiere ninguna acción.|  
|Son necesarias las tablas a las que hacen referencia las vistas.|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] requiere que todas las tablas a las que hacen referencia las vistas y las vistas indexadas publicadas estén disponibles en el suscriptor. Si las tablas a las que se hace referencia no se publican como artículos en esta publicación, se deben crear manualmente en el suscriptor.|Use el botón **Atrás** para navegar a la página **Artículos** . Agregue los objetos necesarios.|  
|Se requieren los objetos a los que hacen referencia los procedimientos almacenados.|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] requiere que todos los objetos a los que hacen referencia los procedimientos almacenados publicados, como tablas y funciones definidas por el usuario, estén disponibles en el suscriptor. Si los objetos a los que se hace referencia no se publican como artículos en esta publicación, se deben crear manualmente en el suscriptor.|Use el botón **Atrás** para navegar a la página **Artículos** . Agregue los objetos necesarios.|  
  
## <a name="see-also"></a>Consulte también  
 [Publicar datos y objetos de base de datos](../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md) (Creación de una publicación)  
  
  
