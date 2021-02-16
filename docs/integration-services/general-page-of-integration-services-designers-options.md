---
description: Opciones de diseñadores de página general de la integración de servicios
title: Opciones de la página General de Diseñadores de Integration Services | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- VS.ToolsOptionsPages.Business_Intelligence_Designers.Data_Transformation_Designers.General
ms.assetid: d695690a-923b-4036-945e-7621e8651deb
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 76be801b071c85d26b8b18ed23e2c478641fa0cb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347806"
---
# <a name="general-page-of-integration-services-designers-options"></a>Opciones de diseñadores de página general de la integración de servicios

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


  Utilice la página **General** de la página **Diseñadores de Integration Services** en el cuadro de diálogo **Opciones** con el fin de especificar las opciones para cargar, mostrar y actualizar los paquetes.  
  
 Para abrir la página **General** , en [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)], en el menú **Herramientas** , haga clic en **Opciones**, expanda **Diseñadores de Business Intelligence** y seleccione **Diseñadores de Integration Services**.  
  
## <a name="options"></a>Opciones  
 **Comprobar la firma digital al cargar un paquete**  
 Seleccione para que [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] compruebe la firma digital al cargar un paquete. [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] solo realizará la comprobación si la firma digital está presente, es válida y procede de un origen de confianza. [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] no comprobará si el paquete ha cambiado desde que se firmó.  
  
 Si establece el valor del Registro **BlockedSignatureStates** , este valor del Registro invalida la opción **Comprobar la firma digital al cargar un paquete** . Para más información, vea [Implementar una directiva de firma estableciendo un valor del Registro](./security/identify-the-source-of-packages-with-digital-signatures.md).  
  
 Para más información sobre los paquetes y los certificados digitales, vea [Identificar el origen de paquetes con firmas digitales](../integration-services/security/identify-the-source-of-packages-with-digital-signatures.md).  
  
 **Mostrar una advertencia si un paquete está sin firmar**  
 Seleccione esta opción para mostrar una advertencia cuando se carga un paquete que está sin firmar.  
  
 **Mostrar etiquetas de restricción de precedencia**  
 Seleccione la etiqueta que quiere que se muestre (Correcto, Error o Conclusión) en restricciones de precedencia al visualizar paquetes en [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)].  
  
 **Lenguaje de scripting**  
 Seleccione el lenguaje de scripting predeterminado para las nuevas tareas Script y los componentes Script.  
  
 **Actualizar las cadenas de conexión para reflejar los nuevos nombres de proveedor**  
 Al abrir o actualizar paquetes de [!INCLUDE[ssISversion2005](../includes/ssisversion2005-md.md)] , actualice las cadenas de conexión para que usen los nombres de los proveedores siguientes para la versión actual de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]:  
  
-   [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] Proveedor OLE DB  
  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Native Client  
  
 El Asistente para actualizar paquetes de [!INCLUDE[ssIS](../includes/ssis-md.md)] solo actualiza las cadenas de conexión que se almacenan en administradores de conexiones. No actualiza las cadenas de conexión que se construyen dinámicamente utilizando el lenguaje de expresiones de [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] o código en una tarea Script.  
  
 **Crear el identificador del nuevo paquete**  
 Al actualizar paquetes de [!INCLUDE[ssISversion2005](../includes/ssisversion2005-md.md)] , cree los identificadores de los paquetes nuevos para las versiones actualizadas de los paquetes.  
  
## <a name="see-also"></a>Vea también  
 [Información general sobre seguridad &#40;Integration Services&#41;](../integration-services/security/security-overview-integration-services.md)   
 [Ampliar paquetes con scripting](../integration-services/extending-packages-scripting/extending-packages-with-scripting.md)  
  
