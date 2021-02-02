---
description: El tamaño de paquete de red no debería superar 8060 bytes
title: El tamaño de paquete de red no debería superar 8060 bytes | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Best Practices [Database Engine]
ms.assetid: 86db5da1-afe4-4fbb-8bf8-33cedc7e4361
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 737bad4dd5994bebdf23ea8ebab5f4ed6f788266
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076417"
---
# <a name="network-packet-size-should-not-exceed-8060-bytes"></a>El tamaño de paquete de red no debería superar 8060 bytes
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Si el valor especificado para sp_configure 'network packet size' o si el tamaño de paquete de red de cualquier usuario registrado es mayor que 8060 bytes, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] realiza diferentes operaciones de asignación de memoria. Esto puede producir un aumento en el espacio de direcciones virtuales de proceso que no se reserva para el grupo de búferes.  
  
## <a name="best-practices-recommendations"></a>Prácticas recomendadas  
 El tamaño de paquete de red no debería superar 8060 bytes.  
  
## <a name="for-more-information"></a>Para obtener más información  
 [Artículo 903002 de Microsoft Knowledge Base](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/903002)  
  
## <a name="see-also"></a>Consulte también  
 [Supervisar y aplicar las prácticas recomendadas usando la administración basada en directivas](../../relational-databases/policy-based-management/monitor-and-enforce-best-practices-by-using-policy-based-management.md)  
  
  
