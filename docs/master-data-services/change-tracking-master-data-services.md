---
description: Seguimiento de cambios [Master Data Services]
title: Seguimiento de cambios
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- change tracking [SQL Server]
ms.assetid: 5e879c65-0d38-454f-9a20-62a6e72c89f7
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 60fd195f5588ef38ab0a6e3c0c7ccbc4162bdc90
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272816"
---
# <a name="change-tracking-master-data-services"></a>Seguimiento de cambios [Master Data Services]

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], puede usar grupos de seguimiento de cambios para realizar una acción cuando cambie un valor de atributo. Use el seguimiento de cambios cuando no conozca cuál será el nuevo valor, pero quiera saber si se produjo algún cambio.  
  
## <a name="configuring-change-tracking"></a>Configurar el seguimiento de cambios  
 Para configurar el seguimiento de cambios, agregue un atributo a un grupo de seguimiento de cambios. El grupo puede contener uno o muchos atributos. A continuación, cree una regla de negocios para definir la acción que se realizará cuando alguno de los atributos del grupo cambie.  
  
> [!NOTE]  
>  Las reglas de negocios del seguimiento de cambios consideran los cambios en lo datos almacenados (importados) provisionalmente.  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Descripción de la tarea|Tema|  
|----------------------|-----------|  
|Agregar atributos a un grupo de seguimiento de cambios.|[Agregar atributos a un grupo de seguimiento de cambios &#40;Master Data Services&#41;](../master-data-services/add-attributes-to-a-change-tracking-group-master-data-services.md)|  
|Crear una regla de negocios para iniciar acciones según los cambios en los atributos.|[Iniciar acciones según los cambios de valores de atributos &#40;Master Data Services&#41;](../master-data-services/initiate-actions-based-on-attribute-value-changes-master-data-services.md)|  
  
## <a name="related-content"></a>Contenido relacionado  
  
-   [Validación &#40;Master Data Services&#41;](../master-data-services/validation-master-data-services.md)  
  
-   [Reglas de negocios &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)  
  
-   [Atributos &#40;Master Data Services&#41;](../master-data-services/attributes-master-data-services.md)  
  
  
