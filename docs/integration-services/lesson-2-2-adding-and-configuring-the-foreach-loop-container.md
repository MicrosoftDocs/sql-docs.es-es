---
description: 'Lección 2-2: Incorporación y configuración del contenedor de bucles Foreach'
title: 'Paso 2: Incorporación y configuración del contenedor de bucles Foreach | Microsoft Docs'
ms.custom: ''
ms.date: 01/03/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 88a973cc-0f23-4ecf-adb6-5b06279c2df6
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8c63288019fa6938f4481bbded2e5b29c29198d3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354309"
---
# <a name="lesson-2-2-add-and-configure-the-foreach-loop-container"></a>Lección 2-2: Incorporación y configuración del contenedor de bucles Foreach

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



En esta tarea, agregará la capacidad de recorrer en bucle una carpeta de archivos planos y aplicará la misma transformación de flujo de datos de la lección 1 a cada uno de esos archivos. Para ello, agregará y configurará un contenedor de bucles Foreach para el flujo de control.  
  
El contenedor de bucles Foreach que agregue debe poder conectarse a cada uno de los archivos planos de la carpeta. Puesto que todos los archivos de la carpeta tienen el mismo formato, el contenedor de bucles Foreach puede utilizar el mismo administrador de conexiones de archivos planos para conectarse a cada uno de estos archivos. El administrador de conexiones de archivos planos que usa el contenedor es el que creó en la lección 1.  
  
Actualmente, el administrador de conexiones de archivos planos de la lección 1 solo se conecta a un archivo plano específico. Para conectarse de forma iterativa a cada uno de los archivos planos de la carpeta, debe configurar el contenedor de bucles Foreach y el administrador de conexiones de archivos planos de este modo:  
  
-   **Contenedor de bucles Foreach:** asignará el valor enumerado del contenedor a una variable de paquete definida por el usuario. El contenedor usa esta variable para modificar de forma dinámica la propiedad **ConnectionString** del administrador de conexiones de archivos planos y conectar de forma iterativa con cada uno de los archivos planos de la carpeta.  
  
-   **Administrador de conexiones de archivos planos:** modificará el administrador de conexiones creado en la lección 1 mediante una variable definida por el usuario para rellenar la propiedad **ConnectionString** del administrador de conexiones.  
  
En los procedimientos de esta tarea se muestra cómo crear y modificar el contenedor de bucles Foreach para usar una variable de paquete definida por el usuario y agregar la tarea Flujo de datos al bucle. Aprenderá a modificar el administrador de conexiones de archivos planos para usar esa variable definida por el usuario en la siguiente tarea.  
  
Una vez realizadas estas modificaciones en el paquete y, cuando este se ejecute, el contenedor de bucles Foreach recorrerá en iteración todos los archivos de la carpeta Datos de ejemplo. Cada vez que se encuentra un archivo que coincida con los criterios, el contenedor de bucles Foreach rellena la nueva variable con el nombre de archivo, asigna esa variable a la propiedad **ConnectionString** del administrador de conexiones de archivos planos Sample Currency Data y, después, ejecuta el flujo de datos en ese archivo. Por consiguiente, en cada iteración del bucle Foreach la tarea Flujo de datos usa un archivo plano distinto.  
  
> [!NOTE]  
> Como [!INCLUDE[msCoName](../includes/msconame-md.md)][!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] separa el flujo de control del flujo de datos, los bucles que agregue al flujo de control no precisarán ninguna modificación en el flujo de datos. Por lo tanto, el flujo de datos de la lección 1 no tiene que cambiarse.  
  
## <a name="add-a-foreach-loop-container"></a>Incorporación de un contenedor de bucles Foreach  
  
1.  En **SQL Server Data Tools**, seleccione la pestaña **Flujo de control**.  
  
2.  En el **Cuadro de herramientas de SSIS**, expanda **Contenedores** y arrastre un **Contenedor de bucles Foreach** a la superficie de diseño de la pestaña **Flujo de control** .  
  
3.  Haga clic con el botón derecho en el nuevo **contenedor de bucles Foreach** y seleccione **Editar**.  
  
4.  En el cuadro de diálogo **Editor de bucles Foreach**, en la página **General**, en **Nombre**, escriba **Foreach File in Folder**. Seleccione **Aceptar**.  
  
5.  Haga clic con el botón derecho en el contenedor de bucles Foreach, seleccione **Propiedades** y, en la ventana **Propiedades**, compruebe que la propiedad **LocaleID** esté establecida en **Inglés (Estados Unidos)** .  
  
## <a name="configure-the-enumerator-for-the-foreach-loop-container"></a>Configuración del enumerador para el contenedor de bucles Foreach  
  
1.  Haga doble clic en **Foreach File in Folder** para volver a abrir el **Editor de bucles Foreach**.  
  
2.  Seleccione **Colección**.  
  
3.  En la página **Colección** , seleccione **Enumerador de archivos Foreach**.  
  
4.  En el grupo **Configuración de enumerador**, haga clic en **Examinar**.  
  
5.  En el cuadro de diálogo **Buscar carpeta**, busque la carpeta en la máquina que contenga los archivos Currency_*.txt incluidos con los datos de ejemplo.

6.  En el cuadro **Archivos**, escriba **Currency_\*.txt**.  
  
## <a name="map-the-enumerator-to-a-user-defined-variable"></a>Asignación del enumerador a una variable definida por el usuario  
  
1.  Seleccione **Asignaciones de variables**.  
  
2.  En la página **Asignaciones de variables**, en la columna **Variable**, seleccione la celda vacía y elija **\<New Variable...>** .  
  
3.  En el cuadro de diálogo **Agregar variable**, en **Nombre**, escriba **varFileName**.  
  
    > [!NOTE]  
    > Los nombres de variables distinguen entre mayúsculas y minúsculas.  
  
4.  Seleccione **Aceptar**.  
  
5.  Seleccione de nuevo **Aceptar** para salir del cuadro de diálogo **Editor de bucles Foreach**.  
  
## <a name="add-the-data-flow-task-to-the-loop"></a>Incorporación de la tarea Flujo de datos al bucle  
  
-   Arrastre la tarea Flujo de datos **Extract Sample Currency Data** al contenedor de bucles Foreach **Foreach File in Folder**.  
  
## <a name="go-to-next-task"></a>Ir a la tarea siguiente  
[Paso 3: Modificación del administrador de conexiones de archivos planos](../integration-services/lesson-2-3-modifying-the-flat-file-connection-manager.md)  
  
## <a name="see-also"></a>Consulte también  
[Configuración de un contenedor de bucles Foreach](./control-flow/foreach-loop-container.md)  
[Uso de variables en paquetes](./integration-services-ssis-variables.md)  
  
  
