---
description: Cuadro de diálogo Configurar
title: Cuadro de diálogo Configurar | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.configure.f1
- sql13.SSIS.SSMS.ISPROJECTPROP.REFERENCES.F1
- sql13.SSIS.SSMS.ISPROJECTPROP.PARAMETERS.F1
ms.assetid: 10183c8d-b1be-420f-972a-96ea97d4f4d8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: cdbcc9b0a03b0afefe56152dc6b29c7ae59ea4cc
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96130688"
---
# <a name="configure-dialog-box"></a>Cuadro de diálogo Configurar

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Utilice el cuadro de diálogo **Configurar** para configurar parámetros, administradores de conexión y referencias a los entornos, para los paquetes y los proyectos.  
  
 **¿Qué desea hacer?**  
  
-   [Abrir el cuadro de diálogo Configurar.](#open_dialog)  
  
-   [Establecer las opciones de la página Parámetros](#parameter)  
  
-   [Establecer las opciones de la página Referencias](#references)  
  
##  <a name="open-the-configure-dialog-box"></a><a name="open_dialog"></a> Abrir el cuadro de diálogo Configurar.  
  
1.  En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], conéctese al servidor de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
     Se conectará a la instancia del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] que hospeda la base de datos SSISDB.  
  
2.  En el Explorador de objetos, expanda el árbol para que se muestre el nodo **Catálogos de Integration Services** .  
  
3.  Expanda el nodo **SSISDB** .  
  
4.  Expanda la carpeta que contiene el paquete o proyecto que desea configurar.  
  
5.  Haga clic con el botón derecho en el paquete o proyecto y, después, haga clic en **Configurar**.  
  
##  <a name="set-the-options-on-the-parameters-page"></a><a name="parameter"></a> Establecer las opciones de la página Parámetros  
 Use la página **Parámetros** para ver los nombres de los parámetros y valores, así como para modificar los valores.  
  
 Seleccione el ámbito de los parámetros que aparecen en las pestañas **Parámetros** y **Administradores de conexiones** , en la lista desplegable **Ámbito** .  
  
 La siguiente es una lista de las opciones de la pestaña **Parámetros** .  
  
 **Contenedor**  
 Muestra el objeto que contiene el parámetro.  
  
 **Nombre**  
 Muestra el nombre de parámetro.  
  
 **Valor**  
 Muestra el valor del parámetro. Haga clic en los puntos suspensivos para cambiar el valor del cuadro de diálogo **Establecer valor de parámetro** .  
  
 La siguiente es una lista de las opciones de la pestaña de **Administradores de conexiones** . Use esta pestaña para cambiar los valores de las propiedades del administrador de conexiones. En el servidor SSIS se generan automáticamente parámetros para las propiedades.  
  
 **Contenedor**  
 Muestra el objeto que contiene el administrador de conexiones.  
  
 **Nombre**  
 Muestra el nombre del administrador de conexiones.  
  
 **Nombre de propiedad**  
 Muestra el nombre de la propiedad del administrador de conexiones.  
  
 **Valor**  
 Muestra el valor asignado a la propiedad del administrador de conexiones. Haga clic en los puntos suspensivos para cambiar el valor del cuadro de diálogo **Establecer valor de parámetro** . Puede especificar un valor literal, asignar una variable de entorno que contiene el valor que desea usar o emplear el valor predeterminado del paquete.  
  
##  <a name="set-the-options-on-the-references-page"></a><a name="references"></a> Establecer las opciones de la página Referencias  
 Use la página **Referencias** para agregar y quitar las referencias a los entornos y a las propiedades del entorno de acceso.  
  
 Un entorno especifica los valores en tiempo de ejecución para los paquetes contenidos en los proyectos que ha implementado en el servidor [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 **Entorno**  
 Muestra el entorno.  
  
 **Carpeta del entorno**  
 Muestra la carpeta que contiene el entorno.  
  
 **Abrir**  
 Haga clic para abrir el cuadro de diálogo de **Propiedades del entorno** .  
  
 **Add (Agregar)**  
 Haga clic para agregar una referencia a un entorno. En el cuadro de diálogo **Examinar entornos** , haga clic en un entorno y después en **Aceptar**.  
  
 Puede seleccionar un entorno contenido en cualquier carpeta del proyecto bajo el nodo de **SSISDB** .  
  
 **Remove**  
 Haga clic en un entorno que se muestra en el área de **Referencias** y, después, haga clic **Quitar**.  
  
  
