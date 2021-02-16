---
description: Ver la lista de paquetes en el servidor de Integration Services
title: Ver la lista de paquetes en el servidor de Integration Services | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 67a992d1-7524-4f4b-b3d8-ebd9e5ea82a8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a63c6b374377029d09df340f6888c1535d5b2012
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339258"
---
# <a name="view-the-list-of-packages-on-the-integration-services-server"></a>Ver la lista de paquetes en el servidor de Integration Services

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Puede ver la lista de paquetes que están almacenados en el servidor [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] de una de dos maneras.  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] access  
 Para ver la lista de paquetes que están almacenados en el servidor, consulte la vista [catalog.packages &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-packages-ssisdb-database.md).  
  
 En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]  
 Para ver los paquetes almacenados en el servidor con el Explorador de objetos en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], siga el procedimiento siguiente.  
  
### <a name="to-view-packages-using-ssmanstudiofull"></a>Para ver los paquetes con [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]  
  
1.  En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], conéctese al servidor de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Es decir, conéctese a la instancia de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] que hospeda la base de datos de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
2.  En el Explorador de objetos, expanda el árbol para que se muestre el nodo **Catálogos de Integration Services** .  
  
3.  Expanda el nodo **Catálogos de Integration Services** para mostrar el nodo de **SSISDB** .  
  
4.  Expanda el nodo **SSISDB** para mostrar una lista de una o varias carpetas. Cada carpeta contiene uno o más proyectos en la carpeta **Proyectos** y cada proyecto contiene uno varios paquetes en la carpeta **Paquetes** .  
  
  
