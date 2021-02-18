---
description: Conectar con Almacenamiento de Microsoft Azure
title: Conectar con Almacenamiento de Microsoft Azure
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.windowsazurestorage.connect.f1
- SQL13.SWB.WINDOWSAZURESTORAGE.CONNECT.F1
ms.assetid: ''
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 07/12/2017
ms.openlocfilehash: 073a2a9597fa05d36ee3fe4724dffd7b38284493
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351661"
---
# <a name="connect-to-microsoft-azure-storage"></a>Conectar con Almacenamiento de Microsoft Azure

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Use el cuadro de diálogo **Conexión de Azure Storage** para especificar una cuenta de almacenamiento y validar la conexión a Azure.  
  
## <a name="options"></a>Opciones  
Especifique la información siguiente sobre la cuenta de Azure y, después, seleccione **Siguiente** para continuar.  
  
1.  **Cuenta de almacenamiento**: especifique el nombre de la cuenta de almacenamiento.

   >[!NOTE]
   > Solo se puede conectar a [Cuentas de almacenamiento de uso general](/azure/storage/common/storage-introduction#azure-storage-services). La conexión a otros tipos de cuentas de almacenamiento puede dar lugar a un error similar al siguiente:
   >
   >  El valor de uno de los encabezados HTTP no tiene el formato correcto. (Microsoft.SqlServer.StorageClient).
   >
   >  El servidor remoto devolvió un error: (400) Solicitud incorrecta. (Sistema)

2.  **Clave de cuenta** : especifique la clave de cuenta para la cuenta de almacenamiento especificada.  
  
3.  **Usar puntos de conexión seguros (HTTPS)** : esta opción usa la comunicación cifrada y el identificador seguro de un servidor web de la red.  
  
4.  **Guardar clave de cuenta** : esta opción guarda su contraseña en un archivo cifrado.  
