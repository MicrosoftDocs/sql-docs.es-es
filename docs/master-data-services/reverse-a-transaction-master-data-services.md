---
description: Invertir una transacción (Master Data Services)
title: Invertir una transacción
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- transactions [Master Data Services], reversing
ms.assetid: 6f7c3f07-0f64-4283-8c9c-93facd00a046
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 72598e6b0dd5c86cb5f231c4ac01ffd1d499b98b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340242"
---
# <a name="reverse-a-transaction-master-data-services"></a>Invertir una transacción (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], los administradores pueden invertir una transacción cuando sea necesario deshacer una acción. Los ejemplos de transacciones son cambios del valor de atributo, movimientos de la jerarquía o eliminaciones de miembro. Este tema se aplica solo a las transacciones de las entidades con el tipo de registro de transacciones "Attribute". Vaya a la página del explorador de entidades para ver el historial de transacciones de las entidades con el tipo de registro de transacciones "Member".  
  
## <a name="prerequisites"></a>Requisitos previos  
  
-   Debe disponer del permiso para tener acceso al área funcional de **Administración de versiones** .  
  
-   Debe ser administrador de modelo. Para obtener más información, vea [administradores &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-reverse-a-transaction"></a>Para invertir una transacción  
  
1.  En la página principal de [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] , haga clic en **Administración de versiones**.  
  
2.  En la barra de menús, haga clic en **Transacciones**.  
  
3.  En la página **Transacciones** , en la lista **Modelo** , seleccione un modelo.  
  
4.  En la lista **Versión** , seleccione una versión.  
  
5.  Haga clic en la fila de la cuadrícula correspondiente a la transacción que desea invertir.  
  
6.  Haga clic en **Invertir transacción**.  
  
7.  En el cuadro de diálogo de confirmación, haga clic en **Aceptar**. Se agrega otra transacción a la cuadrícula para registrar la transacción invertida.  
  
## <a name="see-also"></a>Consulte también  
 [Transacciones &#40;Master Data Services&#41;](../master-data-services/transactions-master-data-services.md)   
 [Reactivar un miembro o una colección &#40;Master Data Services&#41;](../master-data-services/reactivate-a-member-or-collection-master-data-services.md)  
 [Reversión del historial de revisiones de miembro](../master-data-services/rollback-member-revision-history-master-data-services.md)
  
  
