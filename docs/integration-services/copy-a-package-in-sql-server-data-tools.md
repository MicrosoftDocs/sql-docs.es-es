---
description: Copiar un paquete en herramientas de datos de SQL Server
title: Copiar un paquete de SQL Server Data Tools | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- packages [Integration Services], copying
- copying packages
- regenerating package GUID
- updating package properties
ms.assetid: 03edc659-e76d-48c0-a749-5f1899b6b507
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2bdd78011dc51986691ac31b031735d766dd6f5a
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92194901"
---
# <a name="copy-a-package-in-sql-server-data-tools"></a>Copiar un paquete en herramientas de datos de SQL Server

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


  En este tema se explica cómo crear un nuevo paquete [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] mediante la copia de un paquete existente y cómo actualizar las propiedades **Nombre** y **GUID** del nuevo paquete.  
  
### <a name="to-copy-a-package"></a>Para copiar un paquete  
  
1.  En [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)], abra el proyecto de [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] que contiene el paquete que desea copiar.  
  
2.  En el Explorador de soluciones, haga doble clic en el paquete.  
  
3.  Compruebe que el paquete que desea copiar está seleccionado en el Explorador de soluciones o que la pestaña del Diseñador SSIS que contiene el paquete es una pestaña activa.  
  
4.  En el menú **Archivo** , haga clic en **Guardar \<package name> como**.  
  
    > [!NOTE]  
    >  El paquete se debe abrir en el Diseñador SSIS para que la opción **Guardar como** aparezca en el menú **Archivo** .  
  
5.  También puede examinar una carpeta diferente.  
  
6.  Actualice el nombre del archivo de paquete. Asegúrese de mantener la extensión de archivo .dtsx.  
  
7.  Haga clic en **Save**(Guardar).  
  
8.  En el símbolo del sistema, elija si desea actualizar el nombre del objeto del paquete para que coincida con el nombre de archivo. Si hace clic en **Sí**, se actualiza la propiedad **Name** del paquete. El nuevo paquete se agrega al proyecto de [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] y se abre en el Diseñador [!INCLUDE[ssIS](../includes/ssis-md.md)] .  
  
9. También puede hacer clic en el fondo de la pestaña **Flujo de control** y luego hacer clic en **Propiedades**.  
  
10. En la ventana Propiedades, haga clic en el valor de la propiedad Id. y luego, en la lista desplegable, haga clic en **\<Generate New ID>** .  
  
11. En el menú **Archivo** , haga clic en **Guardar los elementos seleccionados** para guardar el nuevo paquete.  
  
## <a name="see-also"></a>Consulte también  
 [Guardar una copia de un paquete](./save-packages.md)   
 [Crear paquetes en SQL Server Data Tools](../integration-services/create-packages-in-sql-server-data-tools.md)   
 [Paquetes de Integration Services &#40;SSIS&#41;](../integration-services/integration-services-ssis-packages.md)  
  
