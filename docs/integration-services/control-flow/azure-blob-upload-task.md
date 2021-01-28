---
description: Tarea de carga de blobs de Azure
title: Tarea de carga de blobs de Azure | Microsoft Docs
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.afpblobuptask.f1
- sql14.dts.designer.afpblobuptask.f1
ms.assetid: 6ea068b0-4cd8-45b5-b89d-09b8f25040c0
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 588bb693d5bcf8561d7f697dc910c483d63160a7
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783086"
---
# <a name="azure-blob-upload-task"></a>Tarea de carga de blobs de Azure

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


La **tarea de carga de blobs de Azure** permite a un paquete SSIS cargar archivos en Azure Blob Storage.
    
Para agregar una **tarea de carga de blobs de Azure**, arrástrela y colóquela en el Diseñador SSIS, haga doble clic o haga clic con el botón derecho y, luego, haga clic en **Editar** para ver el siguiente cuadro de diálogo del **Azure Blob Upload Task Editor** (Editor de la tarea de carga de blobs de Azure).  
  
 La **tarea de carga de blobs de Azure** es un componente de [Feature Pack de SQL Server Integration Services (SSIS) para Azure](../../integration-services/azure-feature-pack-for-integration-services-ssis.md).
  
 En la tabla siguiente se proporcionan las descripciones de los campos de este cuadro de diálogo.  

|**Campo**|**Descripción**|  
|---|---|  
|AzureStorageConnection|Especifique un administrador de conexiones de almacenamiento de Azure existente o cree uno nuevo que haga referencia a una cuenta de almacenamiento de Azure, que esté orientado a la ubicación en la que se encuentran hospedados los archivos de blob.|  
|BlobContainer|Especifica el nombre del contenedor de blobs que contiene los archivos cargados como blobs.|  
|BlobDirectory|Especifica el directorio de blobs donde se almacena el archivo cargado como un blob en bloques. El directorio de blobs es una estructura jerárquica virtual. Si ya existe el blob, se reemplaza.|  
|LocalDirectory|Especifica el directorio local que contiene los archivos que se van a cargar.|  
|SearchRecursively|Especifique si se debe buscar de forma recursiva en subdirectorios.|  
|FileName|Especifica un nombre de filtro para seleccionar archivos con el patrón de nombre especificado. Por ejemplo, `MySheet*.xls\*` incluye archivos como `MySheet001.xls` y `MySheetABC.xlsx`.|  
|TimeRangeFrom/TimeRangeTo|Especifica un filtro de intervalo de tiempo. Se incluyen los archivos modificados después de **TimeRangeFrom** y antes de **TimeRangeTo**.|  
