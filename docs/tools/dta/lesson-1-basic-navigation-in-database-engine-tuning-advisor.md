---
title: Navegación básica en DTA
description: El Asistente para la optimización de motor de base de datos (DTA) ofrece una interfaz gráfica de usuario (GUI) para ver sesiones de optimización e informes de recomendaciones de optimización.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Database Engine Tuning Advisor [SQL Server], tutorials
ms.assetid: ad49b2e0-a5e3-49d2-80fd-9f4eaa3652cb
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-dt-2019
ms.date: 03/01/2017
ms.openlocfilehash: e8c4f32eab8fa7d7791a5f4718d8d771cb1dd6cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354472"
---
# <a name="lesson-1-basic-navigation-in-database-engine-tuning-advisor-dta"></a>Lección 1: Navegación básica en el Asistente para la optimización de motor de base de datos (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

El Asistente para la optimización de motor de base de datos ofrece una interfaz gráfica de usuario (GUI) para ver sesiones de optimización e informes de recomendaciones de optimización. Esta lección muestra cómo iniciar la herramienta y configurar la presentación. Al final de esta lección, habrá aprendido las diferentes formas de iniciar la herramienta y cómo configurar su presentación para proporcionar compatibilidad con las tareas de optimización que realiza regularmente.  

## <a name="prerequisites"></a>Requisitos previos 

Para llevar a cabo este tutorial necesita tener SQL Server Management Studio, acceso a un servidor que ejecute SQL Server y una base de datos de AdventureWorks.

- Instale [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md).
- Instale [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Descargue las [bases de datos de ejemplo de AdventureWorks2017](../../samples/adventureworks-install-configure.md).


Aquí encontrará instrucciones para restaurar bases de datos en SSMS: [Restaurar una base de datos.](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)

  >[!NOTE]
  > Este tutorial está destinado a un usuario familiarizado con el uso de SQL Server Management Studio y las tareas básicas de administración de bases de datos. 
  

## <a name="launch-database-tuning-advisor"></a>Inicio del Asistente para la optimización de motor de base de datos 
Para empezar, abra la interfaz gráfica de usuario (GUI) del Asistente para la optimización de motor de base de datos (DTA). La primera vez que se usa, un miembro del rol fijo de servidor **sysadmin** debe iniciar el Asistente para la optimización de motor de base de datos para inicializar la aplicación. Tras la inicialización, los miembros del rol fijo de base de datos **db_owner** pueden usar el asistente para optimizar bases de datos de su propiedad. Para obtener más información sobre cómo inicializar el Asistente para la optimización de motor de base de datos, consulte [Iniciar y usar el Asistente para la optimización de motor de base de datos](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md).  
  
1. Inicie SQL Server Management Studio (SSMS). En el **Menú Inicio** de Windows, apunte a **Todos los programas** y ubique **SQL Server Management Studio**. 
2. Una vez abierto SSMS, seleccione el menú **Herramientas** y seleccione **Asistente para la optimización de bases de datos**. 

  ![Inicio de DTA desde SSMS](media/dta-tutorials/launch-dta.png)

3. Se inicia el Asistente para la optimización de bases de datos y se abre el cuadro de diálogo **Conectar con el servidor**. Compruebe la configuración predeterminada y, a continuación, seleccione **Conectar** para conectarse a SQL Server.  
  
De manera predeterminada, el Asistente para la optimización de motor de base de datos abre la configuración que muestra la ilustración siguiente:  
  
![Ventana predeterminada del Asistente para la optimización del motor de base de datos](media/dta-tutorials/dta-default-gui.png)
  
> [!NOTE]  
> En la pestaña **Monitor de sesión** se muestra el nombre de la sesión, que es el nombre del usuario conectado y la fecha actual. 
  
Cuando se abre por primera vez, aparecen dos paneles principales en la GUI del Asistente para la optimización de motor de base de datos.  
  
-   El panel izquierdo contiene el Monitor de sesión, que enumera todas las sesiones de optimización que se han realizado en esta instancia de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Al abrir el Asistente para la optimización de motor de base de datos, mostrará una sesión nueva en la parte superior del panel. Puede nombrar esta sesión en el panel adyacente. Inicialmente, solo se muestra una sesión predeterminada. Ésta es la sesión predeterminada que el Asistente para la optimización de motor de base de datos crea automáticamente para el usuario. Después de optimizar las bases de datos, todas las sesiones de optimización para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a las que está conectado se enumerarán debajo de la sesión nueva. Puede hacer clic con el botón secundario en la sesión para cambiarle el nombre, cerrarla, eliminarla o clonarla. Si hace clic con el botón secundario en la lista, podrá ordenar las sesiones por nombre, estado u hora de creación, o bien crear una sesión nueva. En la sección inferior de este panel, se muestran detalles acerca de la sesión de optimización seleccionada. Puede mostrar los detalles organizados por categorías con el botón **Por categorías** , o bien mostrarlos en una lista alfabética usando el botón **Alfabético** . También puede ocultar el Monitor de sesión arrastrando el borde del panel derecho hacia la parte izquierda de la ventana. Para volver a verlo, arrastre el borde del panel hacia la derecha. El Monitor de sesión le permite ver sesiones de optimización previas, o bien utilizarlas para crear sesiones nuevas con definiciones similares. También puede utilizar el Monitor de sesión para evaluar recomendaciones de optimización. Para obtener más información, vea [Ver y trabajar con la salida del Asistente para la optimización de motor de base de datos](../../relational-databases/performance/view-and-work-with-the-output-from-the-database-engine-tuning-advisor.md). Use el botón **Atrás** del explorador para volver a este tutorial.  
  
-   El panel derecho contiene las pestañas **General** y **Opciones de optimización** . Aquí es donde puede definir la sesión de optimización del motor de base de datos. En la pestaña **General** , escriba el nombre de la sesión de optimización, especifique la tabla o el archivo de carga de trabajo que se va a usar y seleccione las bases de datos y tablas que quiere optimizar en esta sesión. Una carga de trabajo es un conjunto de instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] que se ejecuta en una o varias bases de datos que se desean optimizar. El Asistente para la optimización de motor de base de datos utiliza archivos de seguimiento, tablas de seguimiento, scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] o archivos XML como entrada de carga de trabajo a la hora de optimizar bases de datos. En la pestaña **Opciones de optimización** , puede seleccionar las estructuras de diseño físico de base de datos (índices o vistas indizadas) y la estrategia de partición que quiere que el asistente tenga en cuenta durante el análisis. En esta pestaña, también puede especificar el tiempo máximo que el Asistente para la optimización de motor de base de datos empleará en optimizar una carga de trabajo. De forma predeterminada, el asistente emplea una hora en optimizar una carga de trabajo.  
  
> [!NOTE]
> El Asistente para la optimización de motor de base de datos admite archivos XML como entrada cuando se importa un script de [!INCLUDE[tsql](../../includes/tsql-md.md)] desde el Editor de consultas de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] . Para obtener más información, consulte la sección sobre cómo iniciar el Asistente para la optimización de motor de base de datos desde el Editor de consultas de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] en [Iniciar y usar el Asistente para la optimización de motor de base de datos](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md).  
  
## <a name="configure-tool-options-and-layout"></a>Configuración de las opciones de herramientas y el diseño 

1.  En el menú **Herramientas** , haga clic en **Opciones**.  

   ![Opciones de DTA](media/dta-tutorials/dta-settings.png) 
  
2.  En el cuadro de diálogo **Opciones** , observe las opciones siguientes:  
  
    -   Expanda la lista **Al iniciar** para ver lo que muestra el Asistente para la optimización de motor de base de datos cuando se inicia. De forma predeterminada, está seleccionada la opción **Mostrar una nueva sesión** .  
  
    -   Haga clic en **Cambiar fuente** para ver las fuentes que puede elegir para las listas de bases de datos y tablas en la pestaña **General** . Las fuentes que elija para esta opción también se utilizarán en las cuadrículas de recomendación y los informes del Asistente para la optimización de motor de base de datos después de realizar la optimización. De forma predeterminada, el asistente utiliza las fuentes del sistema.  
  
    -   La opción **Número de elementos en las listas usadas más recientemente** puede establecerse entre **1** y **10**. De esta manera, se establece el número máximo de elementos en las listas que se muestran cuando hace clic en **Sesiones recientes** o **Archivos recientes** del menú **Archivo** . De forma predeterminada, el valor de esta opción es **4**.  
  
    -   Cuando la opción **Recordar mis últimas opciones de optimización** está activada, el Asistente para la optimización de motor de base de datos utiliza de forma predeterminada las opciones de optimización que especificó en la última sesión de optimización y las aplica a la sesión actual. Desactive esta casilla para utilizar las opciones de optimización predeterminadas del asistente. Esta opción está seleccionada de forma predeterminada.  
  
    -   La opción **Preguntar antes de eliminar permanentemente las sesiones** está activada de forma predeterminada para impedir la eliminación accidental de sesiones de optimización.  
  
    -   La opción **Preguntar antes de detener análisis de sesión** está activada de forma predeterminada para impedir la detención accidental de la sesión de optimización antes de que el Asistente para la optimización de motor de base de datos haya terminado de analizar la carga de trabajo.  
  
## <a name="next-lesson"></a>Lección siguiente  
[Lección 2: Usar el Asistente para la optimización de motor de base de datos](../../tools/dta/lesson-2-using-database-engine-tuning-advisor.md)  
  
  
