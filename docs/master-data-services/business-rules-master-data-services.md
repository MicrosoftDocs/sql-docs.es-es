---
title: Reglas de negocios
description: Obtenga información sobre las reglas de negocios en Master Data Services, que pueden actualizar datos, enviar correo electrónico o iniciar un proceso de negocio o un flujo de trabajo.
ms.custom: ''
ms.date: 03/18/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- business rules [Master Data Services], about business rules
- business rules [Master Data Services]
ms.assetid: a9f9e41a-2461-4845-b947-58b3a205543f
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 4869875867f7b1cad99780b1ec82664c9d569201
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341486"
---
# <a name="business-rules-master-data-services"></a>Reglas de negocios (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], una regla de negocios es aquella que se usa para asegurarse de la calidad y la exactitud de los datos maestros. Puede usar una regla de negocios para actualizar datos automáticamente, enviar mensajes de correo electrónico, o iniciar un proceso de negocio o un flujo de trabajo.  
  
 Para ver ejemplos de reglas de negocios, vea [Ejemplos de reglas de negocios &#40;Master Data Services&#41;](../master-data-services/business-rule-examples-master-data-services.md).  
  
## <a name="create-and-publish-business-rules"></a>Crear y publicar reglas de negocios  
 Las reglas de negocios son instrucciones **If/Then/Else** que se crean en [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]. Si un valor de atributo cumple una condición especificada, la acción se realiza; de lo contrario, se realiza una acción Else. Entre las posibles acciones se incluyen establecer un valor predeterminado o cambiar un valor. Estas acciones se pueden combinar con el envío de una notificación de correo electrónico.  
  
 Las reglas de negocios se pueden basar en valores de atributo concretos (por ejemplo, realizar una acción si Color=Azul) o cuando los valores de atributo cambian (por ejemplo, realizar una acción si el valor del atributo Color cambia). Para más información sobre el seguimiento de cambios no específicos, vea [Seguimiento de cambios &#40;Master Data Services&#41;](../master-data-services/change-tracking-master-data-services.md).  
  
 Para utilizar reglas de negocios, primero debe crear y publicar las reglas, y a continuación aplicar las reglas publicadas a los datos. Puede aplicar reglas a los subconjuntos de datos o a todos los datos de una versión validando la versión. No se puede confirmar una versión hasta que todos los atributos pasen la validación de la regla de negocios.  
  
 Si un usuario intenta agregar un valor de atributo que no pasa la validación de una regla de negocios, el valor aún se puede guardar. Puede revisar y corregir los problemas de validación, que se muestran en [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)].  
  
 Al crear un paquete de implementación de un modelo, si desea incluir reglas de negocios, debe incluir los datos de la versión en el paquete.  
  
 Si crea una regla de negocios que use el operador **OR** , debe crear una regla distinta para cada instrucción condicional que se pueda evaluar independientemente. A continuación, puede excluir las reglas según sea necesario, proporcionando más flexibilidad y facilitando la solución de problemas.  
  
## <a name="how-business-rules-are-applied"></a>Cómo se aplican reglas de negocios  
 Puede establecer el orden de prioridad de ejecución de las reglas de negocio desplazándolas hacia arriba y hacia abajo. Sin embargo, antes de tener en cuenta la prioridad, las reglas de negocios se aplican en función del tipo de acción que emprende la regla. El orden es el siguiente:  
  
1.  **Valor predeterminado**  
  
2.  **Cambiar valor**  
  
3.  **Validación**  
  
4.  **Acción externa**  
  
5.  **Script de acción definida por el usuario**  
  
 Dentro de estos grupos, las acciones se aplican en el orden de prioridad, de menor a mayor. De esta forma, por ejemplo, es posible que haya cuatro reglas independientes que tengan acciones **Valor predeterminado** . La acción **Valor predeterminado** que se produce en primer lugar depende del orden de prioridad especificado en la interfaz de usuario web.  
  
 Otras notas importantes sobre la aplicación de reglas:  
  
-   Si una regla de negocios se excluye o no se publica con el estado **Activa**, sigue estando disponible, pero no se incluye cuando se aplican las reglas de negocios.  
  
-   Las reglas de negocios se aplican a los valores de atributo para todos los miembros consolidados o todos los miembros hoja, pero no para ambos.  
  
-   Las reglas de negocios se pueden aplicar a cualquier versión de un modelo cuyo estado sea **Abierta** o **Bloqueada**.  
  
-   Los cambios realizados en los datos cuando se aplican reglas de negocios no se registran como transacciones.  
  
-   Una regla de negocios no puede contener más de una acción **iniciar flujo de trabajo** .  
  
## <a name="system-settings"></a>Configuración del sistema  
 Hay dos valores de [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] que afectan a las reglas de negocios. Puede ajustar estos valores en [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] o directamente en la tabla Configuración del sistema. Para obtener más información, vea [Configuración del sistema &#40;Master Data Services&#41;](../master-data-services/system-settings-master-data-services.md).  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Descripción de la tarea|Tema|  
|----------------------|-----------|  
|Crear y publicar una nueva regla de negocios.|[Crear y publicar una regla de negocios &#40;Master Data Services&#41;](../master-data-services/create-and-publish-a-business-rule-master-data-services.md)|  
|Agregar varias condiciones a una regla de negocios.|[Agregar varias condiciones a una regla de negocios &#40;Master Data Services&#41;](../master-data-services/add-multiple-conditions-to-a-business-rule-master-data-services.md)|  
|Crear una regla de negocios para exigir que los atributos tengan valores.|[Requerir valores de atributo &#40;Master Data Services&#41;](../master-data-services/require-attribute-values-master-data-services.md)|  
|Crear una regla de negocios para realizar una acción según los cambios de los valores de atributos.|[Iniciar acciones según los cambios de valores de atributos &#40;Master Data Services&#41;](../master-data-services/initiate-actions-based-on-attribute-value-changes-master-data-services.md)|  
|Crear una regla de negocio para establecer el script definido por el usuario como una condición|[Extensión de reglas de negocios &#40;Master Data Services&#41;](../master-data-services/business-rules-extension-master-data-services.md)|  
|Crear una regla de negocio para establecer un script definido por el usuario como una acción|[Extensión de reglas de negocios &#40;Master Data Services&#41;](../master-data-services/business-rules-extension-master-data-services.md)|  
|Cambiar el nombre de una regla de negocios existente.|[Cambiar el nombre de una regla de negocios &#40;Master Data Services&#41;](../master-data-services/change-a-business-rule-name-master-data-services.md)|  
|Configurar [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] para enviar notificaciones cuando se aplican las reglas de negocios.|[Configurar reglas de negocios para enviar notificaciones &#40;Master Data Services&#41;](../master-data-services/configure-business-rules-to-send-notifications-master-data-services.md)|  
|Aplicar reglas de negocios a determinados miembros.|[Validar miembros específicos con las reglas de negocios &#40;Master Data Services&#41;](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)|  
|Excluir una regla de negocios para que no se utilice.|[Excluir una regla de negocios &#40;Master Data Services&#41;](../master-data-services/exclude-a-business-rule-master-data-services.md)|  
|Eliminar una regla de negocios existente.|[Eliminar una regla de negocios &#40;Master Data Services&#41;](../master-data-services/delete-a-business-rule-master-data-services.md)|  
  
## <a name="related-content"></a>Contenido relacionado  
  
-   [Introducción a Master Data Services &#40;MDS&#41;](../master-data-services/master-data-services-overview-mds.md)  
  
-   [Versiones &#40;Master Data Services&#41;](../master-data-services/versions-master-data-services.md)  
  
-   [Validación &#40;Master Data Services&#41;](../master-data-services/validation-master-data-services.md)  
  
-   [Seguimiento de cambios &#40;Master Data Services&#41;](../master-data-services/change-tracking-master-data-services.md)  
  
  
