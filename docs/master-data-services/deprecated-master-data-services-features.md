---
description: Características en desuso de Master Data Services
title: Características en desuso de Master Data Services
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: d8506bda-66dd-45a4-bfc9-3a10fa665acc
author: lrtoyou1223
ms.author: lle
manager: erikre
ms.openlocfilehash: d8cded1c88278ca67426eaf40df7bdd87474312c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350178"
---
# <a name="deprecated-master-data-services-features"></a>Características en desuso de Master Data Services

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Este tema describe las características desusadas de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] que siguen estando disponibles en [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)]. Está previsto quitar estas características en una futura versión de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Las características en desuso no se deben usar en nuevas aplicaciones.  
  
## <a name="explicit-hierarchies-collections-and-related-components"></a>Jerarquías explícitas, colecciones y componentes relacionados  
 Las jerarquías explícitas, las colecciones y los componentes relacionados están en desuso. Los miembros que antes se modelaban como tipos de miembro consolidados (primario de jerarquía explícita) y tipos de miembro de colección se modelarán como miembros hoja en jerarquías derivadas. Las siguientes características nuevas permiten que las jerarquías derivadas ocupen el lugar de las jerarquías explícitas.  
  
-   Las jerarquías derivadas recursivas ahora se pueden usar para asignar permisos de seguridad de miembro.  
  
     Una jerarquía explícita es análoga a una jerarquía derivada recursiva con un único nivel no recursivo por debajo del nivel recursivo. Una jerarquía derivada recursiva puede ser compleja mediante la inclusión de niveles por debajo y/o por encima de un nivel recursivo.  
  
-   En el Explorador, la página de jerarquía derivada muestra ahora miembros sin asignar (sin usar) para cada nivel de jerarquía. Los nodos sin usar se agrupan por nivel de la jerarquía. Los miembros se pueden mover entre los nodos sin usar y raíz, arrastrando y colocando o cortando y pegando.  
  
     En Administración del sistema, están visibles en los nodos sin usar el panel **Vista previa** . En Seguridad, los nodos sin usar se muestran en el panel **Permisos de los miembros de la jerarquía** . Se puede asignar un permiso a cualquier miembro, ya sea bajo el nodo **Raíz** o **Sin usar** . También se puede asignar permisos a los seudomiembros **Raíz**, **Sin usar** y **Sin usar** .  
  
-   El procedimiento almacenado, mdm.udpConvertCollectionAndConsolidatedMembersToLeaf, convierte jerarquías explícitas en jerarquías derivadas recursivas, y convierte miembros consolidados y de colección en miembros hoja.  
  
     Las jerarquías explícitas y los tipos de miembro consolidados y de colección se siguen admitiendo, por lo que la ejecución del procedimiento almacenado es opcional. Sin embargo, si utiliza estos elementos, se recomienda ejecutar el procedimiento almacenado porque ya están en desuso.  
  
 Para obtener información sobre jerarquías explícitas, colecciones y miembros consolidados, vea los temas siguientes.  
  
-   [Jerarquías explícitas &#40;Master Data Services&#41;](../master-data-services/explicit-hierarchies-master-data-services.md)  
  
-   [Colecciones &#40;Master Data Services&#41;](../master-data-services/collections-master-data-services.md)  
  
-   [Miembros &#40;Master Data Services&#41;](../master-data-services/members-master-data-services.md)  
  
## <a name="attribute-entity-transaction-log-type"></a>Tipo de registro de transacciones de entidad Attribute  
El tipo de registro de transacciones de entidad "Attribute" está en desuso, migre al tipo de registro de transacciones de entidad "Member". Para obtener información sobre los tipos de registro de transacciones de entidad, consulte el tema siguiente:
* [Cambio del tipo de registro de transacciones de entidad (Master Data Services)](../master-data-services/change-the-entity-transaction-log-type-master-data-services.md)
* [Historial de revisiones de miembro](../master-data-services/member-revision-history-master-data-services.md)
  
## <a name="external-resources"></a>Recursos externos  
 Entrada de blog, [Deprecated: Explicit Hierarchies and Collections](https://techcommunity.microsoft.com/t5/sql-server-integration-services/deprecated-explicit-hierarchies-and-collections/ba-p/388221)(En desuso: jerarquías explícitas y colecciones), en msdn.com.  
  
## <a name="see-also"></a>Consulte también  
 [Características descontinuadas de Master Data Services](../master-data-services/discontinued-master-data-services-features.md)  
  
  
