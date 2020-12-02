---
title: Conexión con Microsoft Azure Storage (Restauración) | Microsoft Docs
description: En SQL Server, el cuadro de diálogo Cuenta de Azure Storage le permite especificar una conexión a la información de la cuenta de almacenamiento de Azure para obtener almacenamiento de archivos en una cuenta de Azure.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
f1_keywords:
- sql13.swb.restore.connectstorage.f1
ms.assetid: c0b7d7c8-b878-4b7f-8120-d0c6917b583f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 48bfc198e7e02a6382d149db144a0537d3e830a7
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/23/2020
ms.locfileid: "96130455"
---
# <a name="connect-to-microsoft-azure-storage-restore"></a>Conectar con Azure Storage (Restauración)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  El cuadro de diálogo permite especificar la conexión a la información de la cuenta de Azure Storage para recuperar el almacenamiento de archivos en la cuenta de Azure Storage. Después de especificar la información necesaria, haga clic en **Conectar** para establecer la conexión a Azure Storage.  
  
## <a name="azure-storage-account"></a>Cuenta de Azure Storage  
 **Cuenta de almacenamiento**  
 Seleccione, escriba o pegue el nombre de la cuenta de Azure Storage que desee utilizar. El cuadro desplegable muestra las cuentas utilizadas previamente.  
  
 **Clave de cuenta**  
 Especifique la clave de acceso de la cuenta de Azure Storage.  
  
 Casilla **Usar puntos de conexión seguros (HTTPS)**  
 Seleccione esta opción para establecer una conexión segura con Azure Storage (recomendado).  
  
 Casilla **Guardar clave de cuenta**  
 Active esta casilla si desea que SQL Server recuerde la tecla de acceso para esta cuenta de almacenamiento.  
  
### <a name="sql-credential"></a>Credencial SQL  
 **Seleccionar una credencial existente**  
 Seleccione una credencial existente de SQL que coincida con la información de clave de cuenta y de cuenta de almacenamiento.  
  
 **Crear una nueva credencial**  
 Seleccione esta opción para crear una nueva credencial utilizando la información de clave de cuenta y de cuenta de almacenamiento. Especifique el nombre de la nueva credencial en el campo **Nombre de credencial** .  
  
  
