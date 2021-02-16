---
description: Especificar un intervalo de datos modificados
title: Especificar un intervalo de datos modificados | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- incremental load [Integration Services],specifying interval
ms.assetid: 17899078-8ba3-4f40-8769-e9837dc3ec60
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6e3d2ef011a95d2024479656fe083629890186b3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338473"
---
# <a name="specify-an-interval-of-change-data"></a>Especificar un intervalo de datos modificados

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  En el flujo de control de un paquete de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que realiza una carga incremental de datos modificados, la primera tarea consiste en calcular los extremos del intervalo de cambios. Estos extremos son valores **datetime** y se almacenarán en variables de paquete para su uso posterior en el paquete.  
  
> [!NOTE]  
>  Para obtener una descripción del proceso general de diseño del flujo de control, vea [Captura de datos modificados &#40;SSIS&#41;](../../integration-services/change-data-capture/change-data-capture-ssis.md).  
  
## <a name="set-up-package-variables-for-the-endpoints"></a>Configurar variables de paquete para los extremos  
 Antes de configurar la tarea Ejecutar SQL para calcular los extremos, será necesario definir las variables de paquete que almacenarán dichos extremos.  
  
#### <a name="to-set-up-package-variables"></a>Para establecer las variables de paquetes  
  
1.  En [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], abra un nuevo proyecto de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
2.  En la ventana **Variables** , cree las variables siguientes:  
  
    1.  Cree una variable con el tipo de datos **datetime** para almacenar el punto inicial del intervalo.  
  
         En este ejemplo se utiliza el nombre de variable ExtractStartTime.  
  
    2.  Cree otra variable con el tipo de datos **datetime** para almacenar el punto final del intervalo.  
  
         En este ejemplo se utiliza el nombre de variable ExtractEndTime.  
  
 Si calcula los extremos en un paquete primario que ejecuta varios paquetes secundarios, puede utilizar las configuraciones de variables de paquete primario para pasar los valores de estas variables a cada paquete secundario. Para más información, vea [Tarea Ejecutar paquete](../../integration-services/control-flow/execute-package-task.md) y [Usar los valores de variables y parámetros en un paquete secundario](../../integration-services/packages/legacy-package-deployment-ssis.md#child).  
  
## <a name="calculate-a-starting-point-and-an-ending-point-for-change-data"></a>Calcular un punto inicial y un punto final para los datos modificados  
 Después de establecer las variables de paquete para los extremos del intervalo, puede calcular los valores reales para dichos extremos y asignarlos a las variables de paquete correspondientes. Dado que esos extremos son valores **datetime** , deberá usar funciones que puedan calcular o utilizar valores **datetime** . Tanto el lenguaje de expresiones de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] como Transact-SQL tienen funciones que operan con valores **datetime** :  
  
 Funciones del lenguaje de expresiones de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que operan con valores **datetime**  
 -   [DATEADD &#40;expresión de SSIS&#41;](../../integration-services/expressions/dateadd-ssis-expression.md)  
  
-   [DATEDIFF &#40;expresión de SSIS&#41;](../../integration-services/expressions/datediff-ssis-expression.md)  
  
-   [DATEPART &#40;expresión de SSIS&#41;](../../integration-services/expressions/datepart-ssis-expression.md)  
  
-   [DAY &#40;expresión de SSIS&#41;](../../integration-services/expressions/day-ssis-expression.md)  
  
-   [GETDATE &#40;expresión de SSIS&#41;](../../integration-services/expressions/getdate-ssis-expression.md)  
  
-   [GETUTCDATE &#40;expresión de SSIS&#41;](../../integration-services/expressions/getutcdate-ssis-expression.md)  
  
-   [MONTH &#40;expresión de SSIS&#41;](../../integration-services/expressions/month-ssis-expression.md)  
  
-   [YEAR &#40;expresión de SSIS&#41;](../../integration-services/expressions/year-ssis-expression.md)  
  
 Funciones de Transact-SQL que operan con valores **datetime**  
 [Tipos de datos y funciones de fecha y hora &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).  
  
 Antes de utilizar cualquiera de estas funciones de **datetime** para calcular los extremos, deberá determinar si el intervalo es fijo y se produce con regularidad. Por lo general, se prefiere aplicar regularmente en las tablas de destino los cambios que se han producido en las tablas de origen. Por ejemplo, puede aplicar esos cambios cada hora, diariamente o incluso semanalmente.  
  
 Una vez que decida si su intervalo de cambios es fijo o aleatorio, podrá calcular los extremos:  
  
-   **Calcular la fecha y hora de inicio**. Utilice la fecha y hora de finalización de la carga anterior como los valores actuales. Si usa un intervalo fijo para las cargas incrementales, puede calcular este valor con las funciones **datetime** de Transact-SQL o del lenguaje de expresiones de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . De lo contrario, es posible que tenga que conservar los extremos entre las ejecuciones y utilizar una tarea Ejecutar SQL o una tarea Script para cargar el extremo anterior.  
  
-   **Calcular la fecha y hora de finalización**. Si utiliza un intervalo fijo para las cargas incrementales, calcule la fecha y hora de finalización actuales como un desplazamiento de la fecha y hora de inicio. De nuevo, puede calcular este valor con las funciones **datetime** de Transact-SQL o del lenguaje de expresiones de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 En el procedimiento siguiente, el intervalo de cambios utiliza un intervalo fijo y da por supuesto que el paquete de la carga incremental se ejecuta diariamente sin excepciones. De lo contrario, se perderían los datos modificados para los intervalos que faltan. El punto inicial del intervalo es la medianoche de anteayer, es decir, hace unas 24 a 48 horas. El punto final del intervalo es la medianoche de ayer, es decir, la noche anterior, hace unas 0 a 24 horas.  
  
#### <a name="to-calculate-the-starting-point-and-ending-point-for-the-capture-interval"></a>Para calcular el punto inicial y el punto final del intervalo de captura  
  
1.  En la pestaña **Flujo de control** del Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] , agregue una tarea Ejecutar SQL al paquete.  
  
2.  Abra el **Editor de la tarea Ejecutar SQL** y en la página **General** del editor, seleccione las opciones siguientes:  
  
    1.  En **Conjunto de resultados**, seleccione **Fila única**.  
  
    2.  Configure una conexión válida con una base de datos de origen.  
  
    3.  En **SQLSourceType**, seleccione **Entrada directa**.  
  
    4.  En **SQLStatement**, escriba la siguiente instrucción SQL:  
  
        ```sql
        SELECT DATEADD(dd,0, DATEDIFF(dd,0,GETDATE()-1)) AS ExtractStartTime,  
          DATEADD(dd,0, DATEDIFF(dd,0,GETDATE())) AS ExtractEndTime  
  
        ```  
  
3.  En la página **Conjunto de resultados** del **Editor de la tarea Ejecutar SQL**, asigne el resultado de ExtractStartTime a la variable de paquete ExtractStartTime y el resultado de ExtractEndTime a la variable de paquete ExtractEndTime.  
  
    > [!NOTE]  
    >  Al usar una expresión para establecer el valor de una variable de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], dicha expresión se evalúa cada vez que se accede al valor de la variable.  
  
## <a name="next-step"></a>siguiente paso  
 Una vez calculados los puntos inicial y final de un intervalo de cambios, el paso siguiente consiste en determinar si están listos los datos modificados.  
  
 **Tema siguiente:** [Determinar si los datos modificados están preparados](../../integration-services/change-data-capture/determine-whether-the-change-data-is-ready.md)  
  
## <a name="see-also"></a>Vea también  
 [Usar variables en paquetes](../integration-services-ssis-variables.md)   
 [Expresiones de Integration Services &#40;SSIS&#41;](../../integration-services/expressions/integration-services-ssis-expressions.md)   
 [Tarea Ejecutar SQL](../../integration-services/control-flow/execute-sql-task.md)   
 [Tarea Script](../../integration-services/control-flow/script-task.md)  
  
