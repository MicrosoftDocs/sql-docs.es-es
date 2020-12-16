---
description: 'Reinicializar suscripciones: Todas las suscripciones'
title: 'Reinicialización de suscripciones: todas las suscripciones | Microsoft Docs'
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.reinit.all.f1
helpviewer_keywords:
- Reinitialize Subscription(s) dialog box
ms.assetid: e1122018-9f74-43e3-8489-7eae33ff23d9
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 377fc6d4a501bac57f1bf0cdfc2748387e39a88c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479826"
---
# <a name="reinitialize-subscriptions---all-subscriptions"></a>Reinicializar suscripciones: Todas las suscripciones
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
   El cuadro de diálogo **Reinicializar suscripciones** permite marcar todas las suscripciones a una publicación para reiniciarlas. La reinicialización implica aplicar una instantánea a cada suscriptor; la realiza el Agente de distribución para las suscripciones a publicaciones transaccionales y el Agente de mezcla para las suscripciones a publicaciones de combinación.  
  
## <a name="options"></a>Opciones  
 **Utilizar la instantánea actual**  
 Seleccione esta opción para aplicar la instantánea actual a cada suscriptor la próxima vez que ejecute el Agente de distribución o el Agente de mezcla para la suscripción. Si no hay ninguna instantánea válida disponible, esta opción no puede seleccionarse.  
  
 **Utilizar una instantánea nueva**  
 Seleccione esta opción para reinicializar todas las suscripciones con una instantánea nueva. La instantánea solo puede aplicarse a cada suscriptor tras haber sido generada por el Agente de instantáneas. Si el Agente de instantáneas está configurado para ejecutarse de forma programada, las suscripciones no se reinicializarán hasta la siguiente ejecución programada del mismo.  
  
 Seleccione **Generar la nueva instantánea ahora** para iniciar el Agente de instantáneas de forma inmediata.  
  
 **Cargar los cambios no sincronizados antes de reinicializar**  
 Solo replicación de mezcla. Seleccione esta opción para cargar los cambios pendientes de las bases de datos de suscripciones antes de sobrescribir los datos de los suscriptores con una instantánea.  
  
 Si se agrega, quita o modifica un filtro con parámetros, los cambios pendientes en el suscriptor no se pueden cargar en el publicador durante la reinicialización. Si desea cargar los cambios pendientes, sincronice todas las suscripciones antes de cambiar el filtro.  
  
 **Marcar para reinicializar**  
 Haga clic en esta opción para marcar cada suscripción para reinicializarla. Una vez que hay una instantánea disponible, se aplicará dicha instantánea al suscriptor la próxima vez que se ejecute el Agente de distribución o el Agente de mezcla para la suscripción.  
  
## <a name="see-also"></a>Consulte también  
 [Reinicializar suscripciones](../../relational-databases/replication/reinitialize-subscriptions.md)  
  
  
