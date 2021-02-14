---
description: Historial de revisiones de miembro (Master Data Services)
title: Historial de revisiones de miembro
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 113069c5-12e6-48ec-b443-b42e14f77308
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 148d2d815680ef0e8c03cf2dc4d68f43864872f4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341415"
---
# <a name="member-revision-history-master-data-services"></a>Historial de revisiones de miembro (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Cada vez que un miembro cambia, si el tipo de registro de transacciones de la entidad es miembro, se registra un historial de revisiones de miembro.  
  
 Para obtener información sobre los tipos de registros de transacciones, consulte [Cambio del tipo de registro de transacciones de entidad &#40;Master Data Services&#41;](../master-data-services/change-the-entity-transaction-log-type-master-data-services.md).  
  
 Los historiales de revisiones de miembro se registran cuando se producen los siguientes cambios:  
  
-   Se crean, eliminan, reactivar o purgan miembros.  
  
-   Se cambian los valores de atributo.  
  
-   Los miembros se mueven en una jerarquía o colección.  
  
## <a name="view-and-manage-revision-history-by-entity"></a>Visualización y administración del historial de revisiones por entidad  
 En el área funcional del explorador, puede ver las revisiones de todos los miembros de la entidad. Si tiene permisos de actualización, puede revertir un miembro a una versión anterior.  
  
 **Para ver y administrar el historial de revisiones**  
  
1.  En [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], seleccione el modelo y la versión y luego haga clic en **Explorador**.  
  
2.  Seleccione la entidad en el menú **Entidades** .  
  
3.  Haga clic en **Ver historial** para ver todos los datos históricos de la entidad.  
  
4.  Haga clic en **Filtar** para filtrar los datos.  
  
5.  Haga clic en el encabezado de columna para ordenar los datos.  
  
6.  Si tiene permisos de actualización, haga clic en **Revert Member** (Revertir miembro) para revertir a la versión seleccionada.  
  
## <a name="view-and-manage-revision-history-by-member"></a>Visualización y administración del historial de revisiones por miembro  
 En el área funcional del explorador, puede ver las revisiones de un miembro si tiene permisos de lectura para el miembro. Si tiene permisos de actualización, puede revertir el miembro a una revisión anterior o agregar anotaciones a la revisión.  
  
1.  En [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], seleccione el modelo y la versión y luego haga clic en **Explorador**.  
  
2.  Seleccione la entidad en el menú **Entidades** .  
  
3.  Seleccione el miembro.  
  
4.  Haga clic en **Ver historial** en el panel derecho.  
  
## <a name="log-retention-setting"></a>Configuración de la retención de registros  
 Puede configurar el tiempo que se conservan los datos históricos; para ello, establezca la propiedad **Retención de registro en días** en la configuración del sistema para la base de datos [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] , y establezca **Log Retention Days** (Días de retención del registro) al crear o editar un modelo.  
  
## <a name="related-task"></a>Tarea relacionada  
  
|Descripción de la tarea|Tema|  
|----------------------|-----------|  
|Revertir el historial de revisiones de miembro|[Reversión del historial de revisiones de miembro &#40;Master Data Services&#41;](../master-data-services/rollback-member-revision-history-master-data-services.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Cree un modelo &#40;Master Data Services&#41;](../master-data-services/create-a-model-master-data-services.md)   
 [Configuración del sistema &#40;Master Data Services&#41;](../master-data-services/system-settings-master-data-services.md)  
  
  
