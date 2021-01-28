---
description: Tarea de descarga de blobs de Azure
title: Tarea de descarga de blobs de Azure | Microsoft Docs
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.afpblobdltask.f1
- sql14.dts.designer.afpblobdltask.f1
ms.assetid: 8a63bf44-71be-456d-9a5c-be7c31aff065
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 21ae3810f5f52ab8d77760cc08a887b74ab65977
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783092"
---
# <a name="azure-blob-download-task"></a>Tarea de descarga de blobs de Azure

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


La tarea de descarga de blobs de Azure permite a un paquete SSIS descargar archivos desde Azure Blob Storage.

Para agregar una **tarea de descarga de blobs de Azure**, arrástrela y suéltela en el Diseñador SSIS y haga doble clic (o haga clic con el botón derecho y, después, haga clic en **Editar** ) para ver el siguiente cuadro de diálogo del **Editor de la tarea de descarga de blobs de Azure** .  
  
 La **tarea de descarga de blobs de Azure** es un componente de [Feature Pack de SQL Server Integration Services (SSIS) para Azure](../../integration-services/azure-feature-pack-for-integration-services-ssis.md).  
  
 En la tabla siguiente se proporciona la descripción de los campos de este cuadro de diálogo.  

|**Campo**|**Descripción**|  
|---|---|
|AzureStorageConnection|Especifique un administrador de conexiones de almacenamiento de Azure existente o cree uno nuevo que haga referencia a una cuenta de almacenamiento de Azure, que esté orientado a la ubicación en la que se encuentran hospedados los archivos de blob.|  
|BlobContainer|Especifica el nombre del contenedor de blobs que contiene los archivos de blob que hay que descargar.|  
|BlobDirectory|Especifica el directorio de blobs que contiene los archivos de blob que hay que descargar. El directorio de blobs es una estructura jerárquica virtual.|  
|SearchRecursively|Especifica si se debe buscar de forma recursiva en subdirectorios.|  
|LocalDirectory|Especifica el directorio local en el que se almacenan los archivos de blob descargados.|  
|FileName|Especifica un nombre de filtro para seleccionar archivos con el patrón de nombre especificado. Por ejemplo, `MySheet*.xls\*` incluye archivos como `MySheet001.xls` y `MySheetABC.xlsx`.|  
|TimeRangeFrom/TimeRangeTo|Especifica un filtro de intervalo de tiempo. Se incluyen los archivos modificados después de **TimeRangeFrom** y antes de **TimeRangeTo**.|  
