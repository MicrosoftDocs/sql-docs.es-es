---
title: Asociar extensiones de archivo a un editor de código
description: Obtenga información sobre cómo asociar una extensión de archivo a un editor de código concreto para que, al hacer doble clic en un archivo con la extensión, lo abra el editor asociado.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- file extensions [SQL Server]
- associating file extensions [SQL Server]
- Query Editor [SQL Server Management Studio], associating file extensions
ms.assetid: 193630f4-93de-4950-8f36-68702531f925
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4196519b2e743449864cd171d6da3c7863475157
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354800"
---
# <a name="associate-file-extensions-to-a-code-editor"></a>Asociar extensiones de archivo a un editor de código

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

La asociación de extensiones de archivo a un editor de código específico permite abrir un archivo con el editor de código de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] apropiado al hacer doble clic en un archivo con el Explorador de Windows. Las extensiones que usa normalmente [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], como .sql y .mdx, se asocian durante la instalación. Las nuevas extensiones de archivo también deben estar asociadas a [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] en el sistema de archivos. Puede utilizar esta característica para abrir archivos creados con otros editores o de los cuales ha cambiado el nombre, como las copias de seguridad de archivos .sql cuya extensión se ha cambiado por .bak.  
  
 Este proceso incluye dos pasos. En primer lugar, se asocia la extensión con [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]y, a continuación, con un editor de código determinado.  
  
### <a name="to-associate-a-new-file-extension-with-sql-server-management-studio"></a>Para asociar una nueva extensión de archivo con SQL Server Management Studio  
  
1.  En el menú **Inicio** , seleccione **Todos los programas**, **Accesorios** y, a continuación, haga clic en **Explorador de Windows**.  
  
2.  En el menú **Herramientas** del Explorador de Windows, haga clic en **Opciones de carpeta**.  
  
3.  En la pestaña **Tipos de archivo** del cuadro de diálogo **Opciones de carpeta** , haga clic en **Nuevo**.  
  
4.  En el cuadro **Extensión de archivo** del cuadro de diálogo **Crear nueva extensión** , escriba la nueva extensión de archivo que desea asociar y, a continuación, haga clic en **Aceptar**. No escriba el nombre de la extensión con un punto.  
  
5.  En el cuadro **Tipos de archivo registrados** , haga clic en la nueva extensión y, a continuación, haga clic en **Cambiar**.  
  
6.  En el cuadro de diálogo **Abrir con**, haga clic en **SSMS - SQL Server Management Studio** y después en **Aceptar**.  
  
7.  Haga clic en **Cerrar** para cerrar el cuadro de diálogo **Opciones de carpeta** y, a continuación, cierre el Explorador de Windows.  
  
### <a name="to-associate-a-new-file-extension-with-a-code-editor-in-sql-server-management-studio"></a>Para asociar una nueva extensión de archivo con un editor de código en SQL Server Management Studio  
  
1.  En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], en el menú **Herramientas** , haga clic en **Opciones**.  
  
2.  En el cuadro de diálogo **Opciones** , haga clic en **Editor de texto** y, a continuación, en **Extensión de archivo**.  
  
3.  En el cuadro **Extensión** , escriba la nueva extensión de archivo.  
  
4.  En el cuadro **Editor** , haga clic en el editor de código que desea utilizar para abrir este tipo de archivo, haga clic en **Agregar** y, a continuación, en **Aceptar**.  
  
## <a name="see-also"></a>Consulte también  
 [Ssms (Utilidad)](../ssms-utility.md)  
  
