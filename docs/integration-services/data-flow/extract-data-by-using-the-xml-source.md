---
description: Extraer datos mediante el origen de XML
title: Extraer datos mediante el origen de XML | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- extracting data [Integration Services]
- sources [Integration Services], XML
- XML source [Integration Services]
ms.assetid: 5d5be54c-2b7e-4957-9193-c5ea5c5d6d15
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 462ec77dbb8faf87fd8cb53806e3b8169b74317b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339763"
---
# <a name="extract-data-by-using-the-xml-source"></a>Extraer datos mediante el origen de XML

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Para agregar y configurar un origen XML, el paquete ya debe incluir por lo menos una tarea Flujo de datos.  
  
### <a name="to-extract-data-using-an-xml-source"></a>Para extraer datos mediante un origen XML  
  
1.  En [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], abra el proyecto de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que contiene el paquete que desea.  
  
2.  En el Explorador de soluciones, haga doble clic en el paquete para abrirlo.  
  
3.  Haga clic en la pestaña **Flujo de datos** y, a continuación, desde el **cuadro de herramientas**, arrastre el origen XML a la superficie de diseño.  
  
4.  Haga doble clic en el origen XML.  
  
5.  En el **Editor de origen de XML**, en la página **Administrador de conexiones** , seleccione un modo de acceso de datos:  
  
    -   Para ir al modo de acceso **Ubicación del archivo XML** , haga clic en **Examinar** y ubique la carpeta que contiene el archivo XML.  
  
    -   Para ir al modo de acceso **Archivo XML de variable** , seleccione la variable definida por el usuario que contiene la ruta del archivo XML.  
  
    -   Para ir al modo de acceso **Datos XML de variable** , seleccione la variable definida por el usuario que contiene los datos XML.  
  
    > [!NOTE]  
    >  Las variables se deben definir en el ámbito de la tarea Flujo de datos que contiene el origen XML, o en el ámbito del paquete. Además, la variable debe tener un tipo de datos de cadena.  
  
6.  Opcionalmente, seleccione **Utilizar esquema insertado** para indicar que el documento XML incluye información de esquema.  
  
7.  Para especificar un lenguaje externo de definición de Esquema XML (XSD) para el archivo XML, realice una de las siguientes acciones:  
  
    -   Haga clic en **Examinar** para buscar un archivo XSD existente.  
  
    -   Haga clic en **Generar XSD** para crear un XSD a partir del archivo XML.  
  
8.  Para actualizar los nombres de las columnas de salida, haga clic en **Columnas** y modifique los valores en la lista **Columna de salida** .  
  
9. Para configurar la salida de error, haga clic en **Salida de error**. Para más información, consulte [Debugging Data Flow](../../integration-services/troubleshooting/debugging-data-flow.md).  
  
10. Haga clic en **OK**.  
  
11. Para guardar el paquete actualizado, haga clic en **Guardar los elementos seleccionados**, en el menú **Archivo**.  
  
## <a name="see-also"></a>Consulte también  
 [Origen XML](../../integration-services/data-flow/xml-source.md)   
 [Transformaciones de Integration Services](../../integration-services/data-flow/transformations/integration-services-transformations.md)   
 [Rutas de Integration Services](../../integration-services/data-flow/integration-services-paths.md)   
 [Tarea Flujo de datos](../../integration-services/control-flow/data-flow-task.md)  
  
  
