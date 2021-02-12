---
description: Explorador de dependencias de entidad
title: Explorador de dependencias de entidad
ms.custom: ''
ms.date: 04/06/2016
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
keywords:
- master data services
ms.assetid: 9d922118-1412-4a9d-9c02-70d6c48d6c0d
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 46fceca874105ee036815d2149638be75a53b687
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272576"
---
# <a name="entity-dependencies-explorer"></a>Explorador de dependencias de entidad

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  
[!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] 2016 agrega una nueva página del explorador, Dependencias de entidades, que proporciona una forma alternativa de visualizar las relaciones entre miembros de la entidad dentro de un modelo, según lo especificado por sus valores de atributo basados en dominio (DBA), pero sin tener que definir primero una jerarquía derivada.   
  
Ayuda a responder a la pregunta "¿Quién consume mi entidad y cómo?". La vista es similar a la de la página del explorador de la jerarquía derivada, pero es más inclusiva. Muestra todas las relaciones de DBA, no solo las que se definen como parte de una jerarquía determinada. No se necesita una definición de jerarquía porque la estructura jerárquica mostrada se deduce simplemente de los DBA existentes.  
  
En el menú de página del explorador, el elemento de menú de las dependencias de la entidad enumera todas las entidades en el modelo que dependen de, al menos, una entidad (es decir, al menos una entidad tiene un DBA que hace referencia a la entidad de la lista). El número de dependencias (directas e indirectas) se muestra junto al nombre de la entidad y la lista está ordenada por este número con las entidades a las que se les hace más referencia en la parte superior. La siguiente captura de pantalla, tomada del modelo de cliente de los [datos de ejemplo](./sql-server-samples-model-deployment-packages-mds.md), muestra que 7 entidades hacen referencia a la entidad BigArea (directa o indirectamente):  
  
![MDS_EntityDependencies_Menu.jpg](../master-data-services/media/mds-entitydependencies-menu-jpg.jpg)  
    
Al hacer clic en este elemento de menú se abre la nueva página de explorador de dependencias de la entidad para la entidad BigArea, que muestra cómo se consumen los miembros de la entidad. Muestra una vista jerárquica con los miembros de BigArea en la parte superior del árbol, con los miembros de las 7 entidades de consumo anidadas debajo:  
  
![MDS_EntityDependencies_Tree.jpg](../master-data-services/media/mds-entitydependencies-tree-jpg.jpg)  
    
Más de una entidad puede consumir directamente a una entidad. En el ejemplo anterior, las entidades SubRegion y RegionClimate consumen (tienen referencias DBA a) la entidad Region. Los miembros de cada entidad de consumo se agrupan bajo un nodo primario que lleva el nombre de la entidad:   
  
![MDS_EntityDependencies_Entity_Node.jpg](../master-data-services/media/mds-entitydependencies-entity-node-jpg.jpg)  
  
Estos nodos de árbol contenedores tienen un icono de cuadrícula a la izquierda del nombre de entidad y el texto está coloreado según la profundidad de nivel de la jerarquía. En el ejemplo anterior se muestra que SubRegion "CDSR {Canada}" tiene una referencia DBA a Region "CDR {Canada}", que hace referencia a Area "CDA {Canada}", que hace referencia a BigArea "NAm {N. America}".  
  
La vista se puede modificar por completo, al igual que en la página del explorador de la jerarquía. Se pueden modificar las relaciones primarias y secundarias en el árbol al cortar y pegar o arrastrar y soltar miembros secundarios de un elemento primario a otro. Se pueden modificar otros valores de atributo de miembro en el panel de detalles a la derecha del árbol.   
  
  
  
