---
title: Establecer umbrales de advertencia | Microsoft Docs
description: Obtenga información sobre cómo usar el cuadro de diálogo Establecer umbrales de advertencia para habilitar y configurar umbrales de advertencia para bases de datos reflejadas concretas en SQL Server.
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.dbmmonitor.setwarningthreshold.f1
ms.assetid: 17f93147-e7d9-4092-b4c2-c11b38051171
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: c5433b64027311fbf98b8c62bd8d25d1fbc3fdfb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352253"
---
# <a name="set-warning-thresholds"></a>Establecer umbrales de advertencia
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Utilice este cuadro de diálogo para habilitar y configurar uno o varios umbrales de advertencia para la base de datos seleccionada en el árbol de navegación del cuadro de diálogo **Monitor de creación de reflejo de la base de datos** .  
  
 El cuadro de diálogo intenta conectarse a las dos instancias del servidor. Estas conexiones se establecen de forma asincrónica. En el cuadro de diálogo se muestra el estado de conexión de cada asociado. Si el asociado no está conectado, haga clic en **Conectar**.  
  
 **Para utilizar SQL Server Management Studio a fin de supervisar la creación de reflejo de la base de datos**  
  
-   [Iniciar el Monitor de creación de reflejo de la base de datos &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
## <a name="options"></a>Opciones  
 *Instancia del servidor y su estado de conexión*  
 Nombre de una instancia del servidor asociado con el formato *SYSTEM* **\\** _INSTANCE_NAME_. En el caso de una instancia de servidor predeterminada, solo se muestra el nombre del sistema.  
  
 Este campo también indica si el monitor está conectado actualmente a esta instancia del servidor. Los estados de conexión posibles son:  
  
-   **No conectado a** *nombre de instancia de servidor*  
  
-   **Intentando conectar a** *nombre de instancia de servidor*  
  
-   **Conectado a** *nombre de instancia de servidor*  
  
    > [!NOTE]  
    >  Si no es miembro del rol fijo de servidor **sysadmin**, este estado es **Conectado a** *nombre de instancia de servidor*  **(permisos limitados)** .  
  
 El nombre de cada una de las instancias del servidor asociado se muestra en otro campo de *instancia del servidor y su estado de conexión* . En el campo superior se muestra el servidor principal cuando el monitor inició su ejecución.  
  
 **Conectar**/**Cancelar**  
 Un botón **Conectar**/**Cancelar** está asociado a cada campo de la *instancia del servidor y su estado de conexión* . El estado del botón depende del estado de la conexión:  
  
-   Si no hay conexión con la instancia del servidor, el texto del botón es **Conectar**. Haga clic en el botón para conectarse a la instancia del servidor.  
  
-   Cuando hay un intento de conexión en curso, el texto del botón es **Cancelar**. Haga clic en el botón para cancelar el intento de conexión.  
  
-   Si el servidor está conectado, el texto del botón es **Conectado** y éste aparece atenuado.  
  
 **Umbrales**  
 En la cuadrícula **Umbrales** se muestra la configuración de advertencias para las dos instancias del servidor.  
  
> [!NOTE]  
>  Si una instancia del servidor no está conectada, sus columnas se muestran con celdas vacías y un fondo gris. Cuando la conexión se abre, la cuadrícula muestra automáticamente el contenido de la instancia.  
  
 La cuadrícula contiene las columnas siguientes:  
  
 **Advertencias**  
 Muestra las advertencias admitidas:  
  
|Advertencia|Descripción|  
|-------------|-----------------|  
|**Advierte si el registro sin enviar supera el valor de umbral**|El umbral indica el número de kilobytes (KB) del registro sin enviar en la cola de envío del servidor principal.|  
|**Advertir si el registro sin restaurar sobrepasa el umbral**|El umbral indica el número de KB de la cola rehecha en la instancia del servidor reflejado.|  
|**Advertir si la transacción sin enviar más antigua sobrepasa el umbral**|El umbral indica el número de minutos de transacciones que no se han enviado desde la cola de envío a la instancia del servidor reflejado. Este valor ayuda a medir la posibilidad de pérdida de datos con respecto a la hora.|  
|**Advertir si la sobrecarga de confirmación del servidor reflejado sobrepasa el umbral**|El umbral indica el número de milisegundos de retraso por transacción (solo relevante en el modo de seguridad alta). Este retardo es la cantidad de sobrecarga en la que se incurre mientras la instancia del servidor principal espera a la instancia del servidor reflejado para escribir la entrada de registro de la transacción en la cola de puesta al día.|  
  
 **Habilitado en "** *\<server instance>* **"**  
 Una casilla en blanco indica que la advertencia está deshabilitada actualmente en la instancia del servidor. Para habilitar una advertencia, haga clic en su casilla.  
  
 **Umbral en "** *\<server instance>* **"**  
 Al habilitar una advertencia, establezca el umbral en la parte izquierda de esta columna. Se produce un evento si se ha alcanzado el umbral especificado al actualizarse la tabla de estado. Si deshabilita un umbral tras la configuración de un valor, su valor permanece en este campo y se usará si vuelve a habilitar la advertencia.  
  
 Cuando no se habilita una advertencia, este campo está inactivo.  
  
 **OK (CORRECTO)**  
 Al hacer clic en **Aceptar** , se cierra este cuadro de diálogo y se muestran los valores especificados actualmente de los umbrales de advertencia de la cuadrícula **Umbrales** en la página con pestañas **Advertencias**.  
  
## <a name="remarks"></a>Observaciones  
 Un umbral solo se puede aplicar a un asociado a la vez, aunque es recomendable que establezca un umbral para un evento específico en los dos asociados, con lo que podrá asegurarse de que la advertencia persiste si se produce una conmutación por error en la base de datos. El umbral adecuado para cada asociado depende de la capacidad de rendimiento del sistema de dicho asociado.  
  
 Un evento solo se escribe en el registro de eventos para un rendimiento si su valor se encuentra en el umbral, o por encima de éste, cuando se actualiza la tabla de estado. Si un valor máximo alcanza el umbral momentáneamente entre las actualizaciones de estado, se pierde dicho máximo.  
  
## <a name="see-also"></a>Consulte también  
 [Iniciar el Monitor de creación de reflejo de la base de datos &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)   
 [Supervisar la creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [Iniciar el Asistente para la configuración de seguridad de la creación de reflejo de la base de datos &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
  
