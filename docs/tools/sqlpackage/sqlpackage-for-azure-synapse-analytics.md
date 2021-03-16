---
title: SqlPackage para Azure Synapse Analytics
description: Sugerencias para el uso de SqlPackage en escenarios de Azure Synapse Analytics
ms.custom: tools|sos
ms.date: 03/10/2021
ms.prod: sql
ms.reviewer: llali; sstein
ms.prod_service: sql-tools
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.openlocfilehash: 661230edf4deea3d62ceef7d8b400cdb951330a5
ms.sourcegitcommit: 81ee3cd57526d255de93afb84186074a3fb9885f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622916"
---
# <a name="sqlpackage-for-azure-synapse-analytics"></a>SqlPackage para Azure Synapse Analytics

En este artículo se resaltan las características de SqlPackage.exe que se centran en las bases de datos de Azure Synapse Analytics.

## <a name="extract"></a>Extract
Para [extraer](sqlpackage-extract.md) un esquema en Azure Blob Storage, se requieren las propiedades siguientes:
- /p:AzureStorageBlobEndpoint
- /p:AzureStorageContainer
- /p:AzureStorageKey

El acceso para la extracción se autoriza a través de una clave de cuenta de almacenamiento.  Hay un parámetro adicional opcional, que establece la ruta de acceso raíz del almacenamiento en el contenedor:
- /p:AzureStorageRootPath

Sin esta propiedad, el valor predeterminado de la ruta de acceso es `servername/databasename/timestamp/`.

## <a name="publish"></a>Publicar
Para [publicar](sqlpackage-publish.md) un esquema desde un archivo DACPAC en Azure Blob Storage, se requieren las propiedades siguientes:
- /p:AzureStorageBlobEndpoint
- /p:AzureStorageContainer
- /p:AzureStorageRootPath
- /p:AzureStorageKey o /p:AzureSharedAccessSignatureToken

El acceso para la publicación se puede autorizar a través de una clave de cuenta de almacenamiento o un token de Firma de acceso compartido (SAS).

## <a name="next-steps"></a>Pasos siguientes
- Más información sobre [Extract](sqlpackage-extract.md)
- Más información sobre [Publish](sqlpackage-publish.md)
- Más información acerca de [Azure Blob Storage](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction)
- Más información sobre las [claves de cuentas de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-account-keys-manage)