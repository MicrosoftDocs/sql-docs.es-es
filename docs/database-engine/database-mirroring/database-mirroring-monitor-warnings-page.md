---
title: Monitor de creación de reflejo de la base de datos (página Advertencias) | Microsoft Docs
description: Use esta página de solo lectura Monitor de creación de reflejo de la base de datos para ver las advertencias que se admiten en los eventos de creación de reflejo de la base de datos, así como los valores de umbral de advertencia especificados, si los hubiera.
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.dbmmonitor.warningsandalerts.f1
ms.assetid: 01936122-961d-436b-ba3c-5f79fefe5469
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: e55294b2bbba96ebe9dda253852d3f1b8280220f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353102"
---
# <a name="database-mirroring-monitor-warnings-page"></a>Monitor de creación de reflejo de la base de datos (página Advertencias)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Muestra una lista de solo lectura de las advertencias que se admiten en los eventos de creación de reflejo de la base de datos, así como los valores de umbral de advertencia especificados (si los hay).  
  
 **Para utilizar SQL Server Management Studio a fin de supervisar la creación de reflejo de la base de datos**  
  
-   [Iniciar el Monitor de creación de reflejo de la base de datos &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
## <a name="columns"></a>Columnas  
 **Warning (ADVERTENCIA)**  
 Entre las advertencias para las que puede definir un umbral se incluyen:  
  
|Advertencia|Umbral|  
|-------------|---------------|  
|**Advierte si el registro sin enviar supera el valor de umbral**|Especifica la cantidad de kilobytes (KB) de registro no enviado que generará una advertencia en la instancia del servidor principal. Esta advertencia ayuda a calcular el potencial de pérdida de datos en KB y resulta especialmente relevante para el modo de rendimiento alto. No obstante, la advertencia también es relevante para el modo de alta seguridad cuando la creación de reflejo se detiene o suspende debido a que los asociados se han desconectado.|  
|**Advertir si el registro sin restaurar sobrepasa el umbral**|Especifica la cantidad de KB de registro sin restaurar que generará una advertencia en la instancia del servidor reflejado. Esta advertencia es útil para calcular el tiempo de conmutación por error en KB. El *tiempo de la conmutación por error* se compone principalmente del tiempo que el servidor reflejado anterior necesita para poner al día los registros pendientes en su cola rehecha, más un breve tiempo adicional.<br /><br /> Nota: En una conmutación automática por error, el tiempo para que el sistema detecte el error es independiente del tiempo de conmutación por error.<br /><br /> Para obtener más información, vea [Calcular la interrupción del servicio durante la conmutación de roles &#40;creación de reflejo de la base de datos&#41;](../../database-engine/database-mirroring/estimate-the-interruption-of-service-during-role-switching-database-mirroring.md).|  
|**Advertir si la transacción sin enviar más antigua sobrepasa el umbral**|Especifica el número de minutos de transacciones que se pueden acumular en la cola de envío antes de que se genere una advertencia en la instancia del servidor principal. Esta advertencia ayuda a calcular el potencial de pérdida de datos en concepto de tiempo y resulta especialmente relevante para el modo de rendimiento alto. No obstante, la advertencia también es relevante para el modo de alta seguridad cuando la creación de reflejo se detiene o suspende debido a que los asociados se han desconectado.|  
|**Advertir si la sobrecarga de confirmación del servidor reflejado sobrepasa el umbral**|Especifica el número de milisengundos de retraso medio por transacción tolerado antes de que se genere una advertencia en el servidor principal. Este retardo es la cantidad de sobrecarga en la que se incurre mientras la instancia del servidor principal espera a la instancia del servidor reflejado para escribir la entrada de registro de la transacción en la cola de puesta al día. Este valor solo es relevante en el modo de alta seguridad.|  
  
 **Umbral en "** _< instancia de servidor_ **"**  
 En cada una de las advertencias, muestra el umbral actual especificado por el usuario (si existe) para una de las instancias del servidor. El nombre completo de la instancia del servidor se indica en el encabezado de la columna correspondiente.  
  
 Para obtener más información, vea la sección "Comentarios" más adelante en este tema.  
  
 **Establecer umbrales**  
 Haga clic en este botón para establecer un umbral para una advertencia en cada asociado de conmutación por error.  
  
 Para obtener más información, vea la sección "Comentarios" más adelante en este tema.  
  
## <a name="remarks"></a>Observaciones  
 Si la información no está disponible actualmente para una instancia del servidor, la columna **Umbral en** muestra un fondo gris y un texto de marca de agua. Si el monitor no está conectado a la instancia de servidor, cada celda de la cuadrícula muestra **No conectado a** _<NOMBRE DE SISTEMA>_ o **No conectado a** _<NOMBRE DE SISTEMA>_ **\\** _<nombre de instancia>_ , en función de si la instancia es la predeterminada o es una instancia con nombre. Si el monitor espera que se devuelva una consulta, la cuadrícula muestra **Esperando datos...** en cada celda.  
  
 Si hay información disponible, la celda de cada advertencia muestra un valor de umbral especificado (y una unidad de medida), o bien **No habilitado**.  
  
 Si se sobrepasa un umbral en el momento en que la tabla de estado se actualiza, se registra un evento en el registro de eventos de Windows cuando se registra la fila de estado. De forma predeterminada, la fila de estado se registra cada minuto si el monitor no está en ejecución. Puede configurar una alerta en cada tipo de evento registrado mediante el Agente SQL Server u otro programa, como Microsoft Management Operations Manager (MOM).  
  
 En un asociado determinado, los eventos registrados dependen de su rol actual, ya sea principal o reflejado. No obstante, es recomendable que establezca un umbral de advertencia para un evento específico en los dos asociados, con lo que podrá asegurarse de que la advertencia persiste si se produce una conmutación por error en la base de datos. El umbral adecuado para cada asociado depende de la capacidad de rendimiento del sistema de dicho asociado.  
  
> [!NOTE]  
>  Además, puede usar el procedimiento almacenado del sistema **sp_dbmmonitorchangealert** para configurar los umbrales de los eventos equivalentes: registro sin enviar, registro sin recuperar, transacción sin enviar más antigua y sobrecarga de confirmación del servidor reflejado. Para obtener más información, vea [sp_dbmmonitorchangealert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangealert-transact-sql.md).  
  
 En la siguiente tabla se muestra el Id. de evento asociado a cada advertencia.  
  
|Advertencia del Monitor de creación de reflejo de la base de datos|Nombre del evento|Id. de evento|  
|----------------------------------------|----------------|--------------|  
|**Advierte si el registro sin enviar supera el valor de umbral**|Registro sin enviar|32042|  
|**Advertir si el registro sin restaurar sobrepasa el umbral**|Registro sin restaurar|32043|  
|**Advertir si la transacción sin enviar más antigua sobrepasa el umbral**|Transacción no enviada más antigua|32044|  
|**Advertir si la sobrecarga de confirmación del servidor reflejado sobrepasa el umbral**|Sobrecarga de confirmación del servidor reflejado|32045|  
  
## <a name="permissions"></a>Permisos  
 Para tener acceso total es preciso pertenecer al rol fijo de servidor **sysadmin** . Solo los miembros de **sysadmin** pueden configurar y ver los umbrales de advertencia de las métricas de rendimiento clave.  
  
 La pertenencia al rol **dbm_monitor** le permite ver solo la fila de estado más reciente de la página **Advertencias** .  
  
## <a name="see-also"></a>Consulte también  
 [Iniciar el Monitor de creación de reflejo de la base de datos &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)   
 [Supervisar la creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [Iniciar el Asistente para la configuración de seguridad de la creación de reflejo de la base de datos &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
  
