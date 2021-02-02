---
description: Contadores de rendimiento
title: Contadores de rendimiento | Microsoft Docs
ms.custom: supportability
ms.date: 08/27/2016
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- logs [Integration Services], performance counters
- performance counters [Integration Services]
- data flow [Integration Services], performance
- counters [Integration Services]
- data flow engine [Integration Services]
ms.assetid: 11e17f4e-72ed-44d7-a71d-a68937a78e4c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7ae8429bb51ec2a58a167784b701b7ae0532bb92
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076933"
---
# <a name="performance-counters"></a>Contadores de rendimiento

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] instala un conjunto de contadores de rendimiento que pueden usarse para supervisar el rendimiento del motor de flujo de datos. Por ejemplo, puede observar el contador "Búferes puestos en cola" para determinar si se están escribiendo búferes de datos en el disco temporalmente mientras se ejecuta un paquete. Este intercambio reduce el rendimiento e indica que el equipo no tiene memoria suficiente.  
  
> **NOTA:** Si instala [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en un equipo que está ejecutando [!INCLUDE[winxpsvr](../../includes/winxpsvr-md.md)]y, a continuación, actualiza el equipo a [!INCLUDE[firstref_longhorn](../../includes/firstref-longhorn-md.md)], el proceso de actualización quita del equipo los contadores de rendimiento de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Para restaurar en el equipo los contadores de rendimiento de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , ejecute el programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en modo de reparación.  
  
 En la tabla siguiente se describen los contadores de rendimiento.  
  
|Contador de rendimiento|Descripción|  
|-------------------------|-----------------|  
|Bytes BLOB leídos|Número de bytes de datos de objetos binarios grandes (BLOB) que ha leído el motor de flujo de datos de todos los orígenes.|  
|Bytes BLOB escritos|Número de bytes de datos BLOB que ha escrito el motor de flujo de datos en todos los destinos.|  
|Archivos BLOB en uso|Número de archivos BLOB que utiliza actualmente el motor de flujo de datos para la puesta en cola.|  
|Memoria de búfer|Cantidad de memoria en uso. Esta cantidad puede incluir tanto la memoria física como virtual. Si el número es mayor que la cantidad de memoria física, el recuento **Búferes puestos en cola** aumenta para indicar que el intercambio de memoria está aumentando. El aumento del intercambio de memoria reduce el rendimiento del motor de flujo de datos.|  
|Búferes en uso|Número de objetos de búfer de cualquier tipo que utilizan actualmente todos los componentes de flujo de datos y el motor de flujo de datos.|  
|Búferes puestos en cola|Número de búferes escritos actualmente en el disco. Si el motor de flujo de datos se queda sin memoria física, los búferes que no se están utilizando se escriben en el disco y se vuelven a cargar cuando se necesitan.|  
|Memoria de búfer plano|Cantidad total de memoria, en bytes, que utilizan todos los búferes planos. Los búferes planos son bloques de memoria que un componente usa para almacenar datos. Un búfer plano es un bloque grande de bytes al que se tiene acceso byte a byte.|  
|Búferes planos en uso|Número de búferes planos que utiliza el motor de flujo de datos. Todos los búferes planos son privados.|  
|Memoria de búfer privado|Cantidad total de memoria en uso por todos los búferes privados. Un búfer no es privado si el motor de flujo de datos lo crea para admitir flujos de datos. Un búfer privado es un búfer que utiliza una transformación para realizar exclusivamente un trabajo temporal. Por ejemplo, la transformación de agregación utiliza búferes privados para realizar su trabajo.|  
|Búferes privados en uso|Número de búferes utilizados por las transformaciones.|  
|Filas leídas|Número de filas generadas por un origen. El número no incluye las filas leídas de las tablas de referencia por la transformación Búsqueda.|  
|Filas escritas|Número de filas ofrecidas a un destino. El número no refleja las filas escritas en el almacenamiento de datos de destino.|  
  
 El complemento de rendimiento de Microsoft Management Console (MMC) se utiliza para crear un registro que capture los contadores de rendimiento.  
  
 Para obtener información sobre cómo mejorar el rendimiento, vea [Características de rendimiento del flujo de datos](../../integration-services/data-flow/data-flow-performance-features.md).  
  
## <a name="obtain-performance-counter-statistics"></a>Obtener estadísticas de contadores de rendimiento  
 Para proyectos [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que se implementan en el servidor [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], puede obtener estadísticas de contadores de rendimiento mediante la función [dm_execution_performance_counters &#40;base de datos de SSISDB&#41;](../../integration-services/functions-dm-execution-performance-counters.md).  
  
 En el ejemplo siguiente, la función devuelve estadísticas para la ejecución actual con el identificador 34.  
  
```sql
select * from [catalog].[dm_execution_performance_counters] (34)  
```  
  
 En el ejemplo siguiente, la función devuelve estadísticas para todas las ejecuciones actuales en el servidor [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
```sql
select * from [catalog].[dm_execution_performance_counters] (NULL)  
  
```  
  
> **IMPORTANTE:** Si es miembro del rol de base de datos **ssis_admin** , se devuelven las estadísticas de rendimiento de todas las ejecuciones actuales.  Si no es miembro del rol de base de datos **ssis_admin** , se devuelven las estadísticas de rendimiento de las ejecuciones actuales para las que tiene permisos de lectura.  
  
## <a name="related-content"></a>Contenido relacionado  
  
-   Herramienta, [SSIS Performance Visualization for Business Intelligence Development Studio (CodePlex Project)](https://go.microsoft.com/fwlink/?LinkId=146626)(Visualización del rendimiento de SSIS para Business Intelligence Development Studio (proyecto CodePlex)), en codeplex.com.  
  
-   Vídeo, [Medir y conocer el rendimiento de los paquetes SSIS en la empresa (vídeo de SQL Server)](/previous-versions/sql/sql-server-2008/dd795223(v=sql.100)), en msdn.microsoft.com.  
  
-   Artículo de soporte técnico, [El contador de rendimiento de SSIS ya no está disponible en el monitor de rendimiento después de actualizar a Windows Server 2008](https://web.archive.org/web/20111112202346/http://support.microsoft.com:80/kb/955632), en support.microsoft.com.  

## <a name="add-a-log-for-data-flow-performance-counters"></a>Agregar un registro para los contadores de rendimiento del flujo de datos
  Este procedimiento describe cómo agregar un registro para los contadores de rendimiento que proporciona el motor de flujo de datos.  
  
> [!NOTE]  
>  Si instala [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en un equipo que está ejecutando [!INCLUDE[winxpsvr](../../includes/winxpsvr-md.md)]y, a continuación, actualiza el equipo a [!INCLUDE[firstref_longhorn](../../includes/firstref-longhorn-md.md)], el proceso de actualización quita del equipo los contadores de rendimiento de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Para restaurar en el equipo los contadores de rendimiento de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , ejecute el programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en modo de reparación.  
  
### <a name="to-add-logging-of-performance-counters"></a>Para agregar un registro de contadores de rendimiento  
  
1.  En el **Panel de control**, si utiliza la Vista clásica, haga clic en **Herramientas administrativas**. Si utiliza la Vista por categorías, haga clic en **Rendimiento y mantenimiento** y, a continuación, en **Herramientas administrativas**.  
  
2.  Haga clic en **Rendimiento**.  
  
3.  En el cuadro de diálogo **Rendimiento** , expanda **Registros y alertas de rendimiento**, haga clic con el botón derecho en **Registros de contador** y, después, haga clic en **Nueva configuración de registro**. Escriba el nombre del registro. Por ejemplo, escriba **miRegistro**.  
  
4.  Haga clic en **OK**.  
  
5.  En el cuadro de diálogo **miRegistro** , haga clic en **Agregar contadores**.  
  
6.  Haga clic en **Usar contadores del equipo local** para registrar los contadores de rendimiento en el equipo local o bien, haga clic en **Seleccionar contadores del equipo** y seleccione un equipo de la lista para registrar los contadores de rendimiento en el equipo especificado.  
  
7.  En el cuadro de diálogo **Agregar contadores** , seleccione **SQL Server:SSIS Pipeline** en la lista **Objeto de rendimiento** .  
  
8.  Para seleccionar los contadores de rendimiento, siga uno de estos procedimientos:  
  
    -   Seleccione **Todos los contadores** para registrar todos los contadores de rendimiento.  
  
    -   Elija **Seleccionar contadores de la lista** y seleccione los contadores de rendimiento que desee utilizar.  
  
9. Haga clic en **Agregar**.  
  
10. Haga clic en **Cerrar**.  
  
11. En el cuadro de diálogo **miRegistro** , revise la lista de contadores de rendimiento del registro en la lista **Contadores** .  
  
12. Para agregar más contadores, repita los pasos 5 a 10.  
  
13. Haga clic en **OK**.  
  
    > [!NOTE]  
    >  Debe iniciar el servicio Registros y alertas de rendimiento con una cuenta local o de dominio que sea miembro del grupo Administradores.  

## <a name="see-also"></a>Consulte también  
 [Ejecución de proyectos y paquetes](../packages/run-integration-services-ssis-packages.md) [Eventos registrados por un paquete de Integration Services](../../integration-services/performance/events-logged-by-an-integration-services-package.md)
