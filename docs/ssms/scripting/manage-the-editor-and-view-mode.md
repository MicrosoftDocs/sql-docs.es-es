---
title: Administrar el editor y el modo de vista
description: 'Obtenga información sobre cómo seleccionar uno de los dos modos de vista de SQL Server Management Studio: Organización por fichas e Interfaz de múltiples documentos. Obtenga también información sobre las vistas en dos paneles, el ajuste de línea, el modo de espacio virtual, la visualización de números de línea, el modo de pantalla completa y Ocultar todo automáticamente.'
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Query Editor [SQL Server Management Studio], managing window behavior
- workbench view modes [SQL Server Management Studio]
- full screen mode [SQL Server Management Studio]
- fonts [SQL Server Management Studio]
- word wraps [SQL Server Management Studio]
- virtual space mode [SQL Server Management Studio]
- splitting document views
- displaying line numbers
- view modes [SQL Server Management Studio]
ms.assetid: 25c58a14-9f94-4296-9770-7d84c6bc3969
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8b70e9f5a954a3b763d8861afe3be0d83aaf7079
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100270884"
---
# <a name="manage-the-editor-and-view-mode"></a>Administrar el editor y el modo de vista

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

El editor permite controlar la vista del código de diferentes maneras.  

## <a name="changing-the-view-mode"></a>Cambiar el modo de vista  

[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] incluye un modo de vista denominado **Organización por pestañas** que permite abrir varios editores y documentos al mismo tiempo, y tener acceso a ellos mediante pestañas situadas en la parte superior del editor. También puede abrir el entorno de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] en modo Interfaz de múltiples documentos (MDI) para unir las ventanas sin las pestañas y poder organizarlas en mosaico, minimizarlas, etc.  
  
#### <a name="to-switch-between-view-modes"></a>Para cambiar de un modo de vista a otro  
  
1.  En el menú **Herramientas** , haga clic en **Opciones** .  
  
2.  Haga clic en **Entorno**. Haga clic en **General**.  
  
3.  Haga clic en **Organización por pestañas** o **Entorno MDI**.  
  
    > [!NOTE]  
    >  Los cambios no serán efectivos hasta que no se reinicie [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
## <a name="splitting-the-view"></a>Dividir la vista  
 Una ventana del editor también se puede dividir en dos partes independientes para facilitar la edición.  
  
#### <a name="to-split-a-window"></a>Para dividir una ventana  
  
1.  Haga clic en la barra de división, que está situada encima de la barra de desplazamiento.  
  
2.  Arrastre hacia abajo la barra de división.  
  
3.  Para volver a un solo panel, haga doble clic en la barra de división que divide los dos paneles.  
  
 El nuevo panel contiene el mismo documento y cualquier cambio realizado en un panel se refleja en el otro, siempre que muestre el mismo lugar del documento.  
  
## <a name="word-wrap"></a>Ajuste de línea  
 Cuando se activa la opción Ajuste de línea, la barra de desplazamiento horizontal desaparece y las líneas de código que exceden el tamaño de la ventana del editor pasan automáticamente a la línea siguiente en lugar de salirse de la pantalla.  
  
#### <a name="to-activate-word-wrap"></a>Para activar el ajuste de línea  
  
1.  En el menú **Herramientas** , haga clic en **Opciones** .  
  
2.  Haga clic en **Editor de texto**.  
  
3.  Abra la carpeta del idioma correspondiente (o **Todos los idiomas** para seleccionar todos los idiomas).  
  
4.  Seleccione **Ajuste de línea**.  
  
## <a name="enabling-virtual-space-mode"></a>Habilitar el modo de espacio virtual  
 En el modo de **espacio virtual** , el editor actúa como si el espacio que hay al final de cada línea estuviera lleno de un número infinito de espacios, de manera que las líneas de código continuaran fuera del área visible de la pantalla.  
  
#### <a name="to-enable-virtual-space-mode"></a>Para habilitar el modo de espacio virtual  
  
1.  En el menú **Herramientas** , haga clic en **Opciones** .  
  
2.  Haga clic en **Editor de texto**.  
  
3.  Abra la carpeta del idioma correspondiente (o **Todos los idiomas** para seleccionar todos los idiomas).  
  
4.  Seleccione **Habilitar espacio virtual**.  
  
 Cuando el modo de espacio virtual no está habilitado, el cursor salta del final de una línea al primer carácter de la siguiente y viceversa.  
  
## <a name="displaying-line-numbers"></a>mostrar números de línea  
 Se puede activar la numeración de líneas en el código. Los números de línea son muy útiles para navegar por el código. Para obtener más información, vea [Navegar por código y texto](./navigate-code-and-text.md).  
  
> [!NOTE]  
>  Aunque se active la numeración de líneas, el documento no se imprimirá con los números de línea. Para que se impriman, debe activar la casilla **Números de línea** en el comando **Configurar página** del menú **Archivo** .  
  
#### <a name="to-display-line-numbers-in-code"></a>Para mostrar los números de línea en el código  
  
1.  En el menú **Herramientas** , haga clic en **Opciones** .  
  
2.  Haga clic en **Editor de texto**.  
  
3.  Haga clic en **Todos los lenguajes**.  
  
4.  Haga clic en **General**.  
  
5.  Active **Números de línea**.  
  
 Para aplicar la numeración de líneas solo a algunos lenguajes de programación, seleccione **Números de línea** en la carpeta correspondiente.  
  
## <a name="enabling-full-screen-mode"></a>Habilitar el modo de pantalla completa  
 Habilite el modo Pantalla completa para ocultar todas las ventanas de herramienta y ver solamente las ventanas de documento.  
  
#### <a name="to-enable-full-screen-mode"></a>Para habilitar el modo de pantalla completa  
  
1.  Presione ALT+MAYÚS+ENTRAR para alternar el modo Pantalla completa.  
  
## <a name="using-auto-hide-all"></a>Utilizar la opción Ocultar todo automáticamente  
  
#### <a name="to-hide-all-the-tool-windows-at-once"></a>Para ocultar todas las ventanas de herramientas a la vez  
  
1.  En el menú **Ventana** , active **Ocultar todo automáticamente** .  
  
