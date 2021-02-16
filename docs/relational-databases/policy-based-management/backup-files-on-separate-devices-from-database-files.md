---
title: Los archivos de copia de seguridad deben estar en dispositivos independientes de los archivos de base de datos
description: Obtenga información sobre cómo habilitar una directiva para comprobar la ubicación del archivo de copia de seguridad en comparación con la del archivo de base de datos para la administración basada en directivas con SQL Server.
ms.custom: seo-lt-2019
ms.date: 08/31/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Best Practices [Database Engine]
author: dzsquared
ms.author: drskwier
ms.openlocfilehash: 657b93c4979c838801aee9b5aeab53f31f77c02d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345516"
---
# <a name="backup-files-must-be-on-separate-devices-from-the-database-files"></a>Los archivos de copia de seguridad deben estar en dispositivos independientes de los archivos de base de datos
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Esta regla comprueba si los archivos de base de datos están en dispositivos independientes de los archivos de copia de seguridad. Si los archivos de base de datos y los archivos de copia de seguridad están en el mismo dispositivo y este sufre un error, la base de datos y las copias de seguridad no estarán disponibles. Además, al colocar los archivos de base de datos y los archivos de copia de seguridad en dispositivos independientes, mejora el rendimiento de E/S en el uso en producción de la base de datos y en la escritura de las copias de seguridad.  
  
## <a name="best-practices-recommendations"></a>Procedimientos recomendados  
 Se recomienda que el disco de copia de seguridad no sea el disco de datos o del registro de la base de datos. Esto es necesario para garantizar el acceso a las copias de seguridad si el disco de datos o del registro presenta errores.

Si los archivos de base de datos y los archivos de copia de seguridad están en el mismo dispositivo y este sufre un error, la base de datos y las copias de seguridad no estarán disponibles. Además, al colocar los archivos de base de datos y los archivos de copia de seguridad en dispositivos independientes, mejora el rendimiento de E/S en el uso en producción de la base de datos y en la escritura de las copias de seguridad.
  
## <a name="see-also"></a>Consulte también 
   
  
 [Dispositivos de copia de seguridad](../backup-restore/backup-devices-sql-server.md)  
  
