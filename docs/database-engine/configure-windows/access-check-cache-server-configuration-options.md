---
title: access check cache (opciones de configuración del servidor) | Microsoft Docs
description: Obtenga información sobre la caché de resultados de comprobación de acceso y las opciones que controlan el comportamiento de la caché. Vea cuándo se deben cambiar estas opciones en SQL Server.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- access check cache option
- access check cache bucket count
- access check cache quota
ms.assetid: 0a992ea8-3ec6-4a4d-97b5-460ae7326247
author: markingmyname
ms.author: maghan
ms.openlocfilehash: dc1ff18d464c03d1eb5e96834f4d753bfead5559
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99233979"
---
# <a name="access-check-cache-server-configuration-options"></a>access check cache (opciones de configuración del servidor)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]tiene acceso a los objetos de base de datos, la comprobación del acceso se almacena en memoria caché en una estructura interna denominada **memoria caché de resultados de comprobación de acceso**. 
  
La opción **access check cache bucket count** controla el número de cubos de hash que se usan para la caché de resultados de comprobación de acceso. 

La opción **access check cache quota** controla el número de entradas que se almacenan en la caché de resultados de comprobación de acceso. Cuando se alcanza el número máximo de entradas, las más antiguas se quitan de la caché de resultados de comprobación de acceso.
  
Los valores predeterminados de 0 indican que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] está administrando estas opciones. A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], los valores predeterminados se traducen a las configuraciones internas siguientes:
-   En el caso de "check cache bucket count", el valor 0 establece un valor predeterminado de 256 cubos.
-   En el caso de "access check cache quota", el valor 0 establece un valor predeterminado de 1024 entradas.

En circunstancias excepcionales, el rendimiento se puede mejorar cambiando estas opciones. Por ejemplo, puede que quiera reducir el tamaño de la caché de resultados de la comprobación de acceso si se usa demasiada memoria. También puede que quiera aumentar el tamaño de la caché de resultados de comprobación de acceso si experimenta un uso intensivo de la CPU cuando se vuelven a calcular los permisos.
 
> [!IMPORTANT]
> Microsoft solo recomienda cambiar estas opciones cuando lo indiquen los servicios de soporte al cliente de Microsoft. Si tiene que cambiar los valores de las opciones "access check cache bucket count" y "access check cache quota", utilice una relación de 1:4. Por ejemplo, si cambia el valor de "access check cache bucket count" a 512, debe cambiar el valor de "access check cache quota" a 2048. 
  
## <a name="see-also"></a>Consulte también  
 [Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
