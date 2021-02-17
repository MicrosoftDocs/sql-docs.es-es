---
description: Crear una jerarquía derivada (Master Data Services)
title: Crear una jerarquía derivada
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- derived hierarchies, creating
- creating derived hierarchies [Master Data Services]
ms.assetid: fec653c4-11cc-46a2-8dd8-b605341ebb40
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 3b69064a3adb5b9e13bd37e9d46bf6a41f6df992
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272706"
---
# <a name="create-a-derived-hierarchy-master-data-services"></a>Crear una jerarquía derivada (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], cree una jerarquía derivada cuando desee una jerarquía basada en nivel que garantice que los miembros existen en el nivel correcto. Las jerarquías derivadas se basan en las relaciones de atributo basados en dominios que existen en un modelo.  
  
> [!NOTE]  
>  Si un valor de atributo basado en dominios no existe para un miembro, este no se incluye en la jerarquía derivada. Consulte [Requerir valores de atributo &#40;Master Data Services&#41;](../master-data-services/require-attribute-values-master-data-services.md) para requerir un valor de atributo basado en dominio para todos los miembros.  
  
## <a name="prerequisites"></a>Prerrequisitos  
 Para realizar este procedimiento:  
  
-   Debe disponer de permiso para tener acceso al área funcional de **Administración del sistema** .  
  
-   Debe ser administrador de modelo. Para obtener más información, vea [administradores &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-create-a-derived-hierarchy"></a>Crear una jerarquía derivada  
  
1.  En [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], haga clic en **Administración del sistema**.  
  
2.  En la barra de menús, seleccione **Administrar** y haga clic en **Jerarquías derivadas**.  
  
3.  En la página **Mantenimiento de jerarquías derivadas** , en la lista **Modelo** , seleccione un modelo.  
  
4.  Haga clic en **Agregar**.  
  
5.  En la página **Agregar jerarquía derivada** , en el cuadro **Nombre de jerarquía derivada** , escriba un nombre para la jerarquía.  
  
    > [!TIP]  
    >  Use un nombre que describa los niveles de la jerarquía, por ejemplo **Producto de subcategoría a categoría**.  
  
6.  Haga clic en **Guardar jerarquía derivada**.  
  
7.  En la página **Editar jerarquía derivada** , en el panel **Entidades y jerarquías disponibles** , haga clic en una entidad o jerarquía, y arrástrela hasta **Colocar principal aquí** en el panel **Niveles actuales** .  
  
8.  Siga arrastrando entidades o jerarquías hasta que la jerarquía esté completa.  
  
9. Haga clic en **Atrás**.  
  
## <a name="see-also"></a>Consulte también  
 [Jerarquías derivadas &#40;Master Data Services&#41;](../master-data-services/derived-hierarchies-master-data-services.md)   
 [Jerarquías derivadas con límites explícitos &#40;Master Data Services&#41;](../master-data-services/derived-hierarchies-with-explicit-caps-master-data-services.md)   
 [Atributos basados en dominios &#40;Master Data Services&#41;](../master-data-services/domain-based-attributes-master-data-services.md)  
  
  
