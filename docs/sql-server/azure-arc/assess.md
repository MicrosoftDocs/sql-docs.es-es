---
title: Configuración de SQL Assessment a petición en una la instancia de SQL Server habilitado para Azure Arc
description: Configuración de SQL Assessment a petición en una la instancia de SQL Server habilitado para Azure Arc
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 09/10/2020
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: 2fb25ca30a3b55bfcedb470addad8680f4914e2f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063297"
---
# <a name="configure-sql-assessment-on-an-azure-arc-enabled-sql-server-instance"></a>Configuración de SQL Assessment en una instancia de SQL Server habilitado para Azure Arc

SQL Assessment proporciona un mecanismo para evaluar la configuración de SQL Server. En este artículo se proporcionan instrucciones para usar SQL Assessment en una instancia de SQL Server habilitado para Azure Arc.

## <a name="prerequisites"></a>Requisitos previos

* La instancia de SQL Server debe estar conectada a Azure Arc. Para obtener instrucciones, consulte el artículo [Conexión de la instancia de SQL Server a Azure Arc](connect.md).

* La extensión Microsoft Monitoring Agent (MMA) debe estar instalada y configurada en el equipo. Consulte el artículo [Instalación de MMA](configure-advanced-data-security.md#install-microsoft-monitoring-agent-mma) para obtener instrucciones. También puede obtener más información en el artículo [Agente de Log Analytics](/azure/azure-monitor/platform/log-analytics-agent).

* La instancia de SQL Server debe tener [habilitado el protocolo TCP/IP](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md).

* El [servicio del explorador de SQL Server](../../tools/configuration-manager/sql-server-browser-service.md) se ejecuta en caso de que trabaje con una instancia con nombre de SQL Server.

* Asegúrese de revisar el documento de SQL Server en [Requisitos previos de las evaluaciones a petición del Centro de servicios](/services-hub/health/assessment-prereq-docs#on-demand-assessment-prerequisite-documents).

## <a name="run-on-demand-sql-assessment"></a>Ejecución de SQL Assessment a petición

1. Abra el recurso de SQL Server: Azure Arc y seleccione **Estado del entorno** en el menú izquierdo.

   > [!div class="mx-imgBorder"]
   > [ ![Captura de pantalla que muestra la pantalla Estado del entorno de un recurso de SQL Server-Azure Arc.](media/assess/sql-assessment-heading-sql-server-arc.png) ](media/assess/sql-assessment-heading-sql-server-arc.png#lightbox)

> [!IMPORTANT]
> Si la extensión MMA no está instalada, no podrá iniciar SQL Assessment a petición.

2. Seleccione el tipo de cuenta. Si tiene una cuenta de servicio administrada, le permitirá iniciar SQL Assessment directamente desde el portal. Especifique el nombre de la cuenta.

> [!NOTE]
> Al especificar una *cuenta de servicio administrada*, se activará el botón **Configurar SQL Assessment**, de modo que podrá iniciar la evaluación desde el portal mediante la implementación de una extensión *CustomScriptExtension*. Dado que solo se puede implementar una extensión *CustomScriptExtension* al mismo tiempo, la extensión de script para SQL Assessment se quitará automáticamente después de la ejecución. Si ya tiene otra extensión *CustomScriptExtension* implementada en la máquina host, no se activará el botón **Configurar SQL Assessment**.

3. Si quiere cambiar el valor predeterminado, especifique un directorio de trabajo en la máquina de recopilación de datos. De forma predeterminada, se usa `C:\sql_assessment\work_dir`. Durante la recopilación y el análisis, los datos se almacenan temporalmente en esa carpeta. Si la carpeta no existe, se crea automáticamente.

4. Si inicia SQL Assessment desde el portal haciendo clic en **Configurar SQL Assessment**, se mostrará una burbuja de implementación estándar.

> [!div class="mx-imgBorder"]
   > [ ![Captura de pantalla en la que se muestra la implementación de una extensión CustomScriptExtension.](media/assess/sql-assessment-custom-script-deployment.png) ](media/assess/sql-assessment-custom-script-deployment.png#lightbox)

5. Si prefiere iniciar SQL Assessment desde la máquina de destino, haga clic en **Descargar script de configuración**, copie el script descargado en la máquina de destino y ejecute uno de los siguientes bloques de código en una instancia de administrador de **powershell.exe**:

   * _Cuenta de dominio_:  se le pedirá la cuenta de usuario y la contraseña.

      ```powershell
      Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass -Force
      & '.\AddSqlAssessment.ps1'
      ```

   * _Cuenta de servicio administrada (MSA)_

      ```powershell
      Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass -Force
      & '.\AddSqlAssessment.ps1' -ManagedServiceAccountName <MSA account name>
      ```

> [!NOTE]
> El script programa una tarea denominada *SQLAssessment*, que desencadena la recopilación de datos. Esta tarea se ejecuta en un plazo de una hora después de ejecutar el script. A continuación, se repite cada siete días.

> [!TIP]
> Puede modificar la tarea para que se ejecute en una fecha y hora diferentes, o incluso forzarla para que se ejecute inmediatamente. En la biblioteca del programador de tareas, busque **Microsoft** > **Operations Management Suite** > **AOI\*\*\***  > **Assessments** > **SQLAssessment**.

## <a name="view-sql-assessment-results"></a>Visualización de resultados de SQL Assessment

* En el panel _Estado del entorno_, seleccione el botón **View SQL Assessment results** (Ver resultados de SQL Assessment).

   > [!NOTE]
   > El botón **View SQL Assessment results** (Ver resultados de SQL Assessment) en la hoja Estado del entorno está deshabilitado hasta que los resultados estén listos en Log Analytics. Este proceso puede tardar hasta dos horas después de procesar los archivos de datos en el equipo de destino.

   > [!div class="mx-imgBorder"]
   > [ ![Captura de pantalla que muestra los resultados de SQL Assessment.](media/assess/sql-assessment-results.png) ](media/assess/sql-assessment-results.png#lightbox)

* Puede ver el estado del procesamiento de datos en la máquina de recopilación mediante la comprobación de los archivos en la carpeta de trabajo. Una vez completada la tarea programada, debería ver varios archivos con el prefijo _new._ en el directorio de trabajo.

   > [!div class="mx-imgBorder"]
   > [ ![Captura de pantalla que muestra una ventana del administrador de archivos que muestra nuevos archivos de datos en la carpeta de trabajo.](media/assess/sql-assessment-data-files-ready.png) ](media/assess/sql-assessment-data-files-ready.png#lightbox)

* Microsoft Monitoring Agent examina la carpeta de trabajo cada 15 minutos. Busca archivos _new.*_ y envía los datos al área de trabajo Log Analytics. Una vez que MMA carga el archivo, cambia de prefijo de _new._ a _processed._

   > [!div class="mx-imgBorder"]
   > ![Captura de pantalla que muestra una ventana del administrador de archivos con archivos de datos procesados](media/assess/sql-assessment-data-files-processed.png).

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información, vea el documento de requisitos previos en [Evaluaciones a petición del Centro de servicios](/services-hub/health/assessment-prereq-docs#on-demand-assessment-prerequisite-documents).

* Para obtener soporte técnico completo de la característica SQL Assessment a petición, se requiere una suscripción de soporte técnico de tipo Premier o unificado. Consulte [Soporte técnico Premier de Microsoft Azure](https://azure.microsoft.com/support/plans/premier) para obtener más información.
