---
description: Determinar si los datos modificados están preparados
title: Determinar si los datos modificados están preparados | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- incremental load [Integration Services],determining readiness
ms.assetid: 04935f35-96cc-4d70-a250-0fd326f8daff
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9fd82f56cb1edb459e662e31778fcb8341a6887c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353037"
---
# <a name="determine-whether-the-change-data-is-ready"></a>Determinar si los datos modificados están preparados

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  En el flujo de control de un paquete de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que realiza una carga incremental de los datos modificados, la segunda tarea consiste en asegurarse de que éstos están listos para el intervalo seleccionado. Este paso es necesario porque el proceso de captura asincrónico podría no haber procesado todavía todos los cambios hasta el extremo seleccionado.  
  
> [!NOTE]  
>  La primera tarea para el flujo de control consiste en calcular los extremos del intervalo de cambios. Para más información sobre esta tarea, vea [Especificar un intervalo de datos modificados](../../integration-services/change-data-capture/specify-an-interval-of-change-data.md). Para obtener una descripción del proceso general de diseño del flujo de control, vea [Captura de datos modificados &#40;SSIS&#41;](../../integration-services/change-data-capture/change-data-capture-ssis.md).  
  
## <a name="understanding-the-components-of-the-solution"></a>Entender los componentes de la solución  
 La solución descrita en este tema usa 4 componentes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] :  
  
-   Un contenedor de bucles For que evalúa repetidamente la salida de una tarea Ejecutar SQL.  
  
-   Una tarea Ejecutar SQL que consulta tablas especiales mantenidas por el proceso de captura de datos modificados y, a continuación, utiliza esta información para determinar si éstos están listos.  
  
-   Un componente que implementa un retraso en el procesamiento cuando los datos no están listos. Puede ser una tarea Script o una tarea Ejecutar SQL.  
  
-   Opcionalmente, un componente que informa de un error o de que se ha superado el tiempo de espera cuando la tarea Ejecutar SQL devuelve un valor que indica una de esas condiciones.  
  
 Estos componentes establecen o leen los valores de varias variables del paquete para controlar el flujo de ejecución dentro del bucle y después en el paquete.  
  
#### <a name="to-set-up-package-variables"></a>Para establecer las variables de paquetes  
  
1.  En [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], en la ventana **Variables** , cree las variables siguientes:  
  
    1.  Cree una variable con un tipo de datos entero para almacenar el valor de estado devuelto por la tarea Ejecutar SQL.  
  
         En este ejemplo se utiliza el nombre de variable DataReady con un valor inicial de 0.  
  
    2.  Cree una variable para almacenar el período de tiempo de retraso cuando los datos no están listos. Si desea utilizar una tarea Script para implementar el retraso, la variable debería tener un tipo de datos entero. Si desea utilizar una tarea Ejecutar SQL con una instrucción WAITFOR, la variable debería tener un tipo de datos de cadena para aceptar valores como "00:00:10".  
  
         En este ejemplo se utiliza el nombre de variable DelaySeconds con un valor inicial de 10.  
  
    3.  Cree una variable con un tipo de datos entero para almacenar la iteración actual del bucle.  
  
         En este ejemplo se utiliza el nombre de variable TimeoutCount con un valor inicial de 0.  
  
    4.  Cree una variable con un tipo de datos entero para especificar el número de veces que el bucle debería comprobar la existencia de datos antes de informar de un error de tiempo de espera.  
  
         En este ejemplo se utiliza el nombre de variable TimeoutCeiling con un valor inicial de 20.  
  
    5.  (Opcional) Cree una variable con un tipo de datos entero que podrá utilizar para indicar la primera carga de datos modificados.  
  
         En este ejemplo se utiliza el nombre de variable IntervalID y solamente se comprueba que exista un valor de 0 que indique la carga inicial.  
  
## <a name="configuring-a-for-loop-container"></a>Configurar un contenedor de bucles For  
 Con las variables establecidas, el contenedor de bucles For es el primer componente que se va a agregar.  
  
#### <a name="to-configure-a-for-loop-container-to-wait-until-change-data-is-ready"></a>Para configurar un contenedor de bucles For de tal forma que espere hasta que los datos modificados estén listos  
  
1.  En la pestaña **Flujo de control** del Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] , agregue un contenedor de bucles For al flujo de control.  
  
2.  Conecte la tarea Ejecutar SQL que calcula los extremos del intervalo al contenedor de bucles For.  
  
3.  En el **Editor de bucles For**, seleccione las opciones siguientes:  
  
    1.  Para **InitExpression**, escriba `@DataReady = 0`.  
  
         Esta expresión establece el valor inicial de la variable de bucle.  
  
    2.  Para **EvalExpression**, escriba `@DataReady == 0`.  
  
         Cuando esta expresión se evalúa como **False**, la ejecución se transfiere al bucle y comienza la carga incremental.  
  
## <a name="configuring-the-execute-sql-task-that-queries-for-change-data"></a>Configurar la tarea Ejecutar SQL que consulta los datos modificados  
 Dentro del contenedor de bucles For, agregue una tarea Ejecutar SQL. Esta tarea consulta las tablas que el proceso de captura de datos modificados mantiene en la base de datos. El resultado de esta consulta es un valor de estado que indica si los datos modificados están listos.  
  
 En la tabla siguiente, la primera columna muestra los valores devueltos desde la tarea Ejecutar SQL por la consulta de ejemplo de Transact-SQL. La segunda columna muestra cómo responden los otros componentes a estos valores.  
  
|Valor devuelto|Significado|Response|  
|------------------|-------------|--------------|  
|0|Indica que los datos modificados no están listos.<br /><br /> No hay ningún registro de captura de datos modificados posterior al punto final del intervalo seleccionado.|La ejecución continúa con el componente que implementa un retraso. A continuación, el control vuelve al contenedor de bucles For, que sigue comprobando la tarea Ejecutar SQL mientras el valor devuelto sea 0.|  
|1|Podría indicar que no se han capturado los datos modificados para el intervalo completo, o que se ha eliminado. Esto se trata como una condición de error.<br /><br /> No hay ningún registro de captura de datos modificados anterior al punto inicial del intervalo seleccionado.|La ejecución continúa con el componente opcional que registra el error.|  
|2|Indica que los datos están listos.<br /><br /> Hay registros de captura de datos modificados anteriores al punto inicial y posteriores al punto final del intervalo seleccionado.|La ejecución se transfiere al contenedor de bucles For y comienza la carga incremental.|  
|3|Indica la carga inicial de todos los datos modificados disponibles.<br /><br /> La lógica condicional obtiene este valor de una variable de paquete especial que solamente se utiliza para este propósito.|La ejecución se transfiere al contenedor de bucles For y comienza la carga incremental.|  
|5|Indica que se ha alcanzado el valor de la variable TimeoutCeiling.<br /><br /> El bucle ha comprobado la existencia de datos el número de veces especificado, pero los datos todavía no están disponibles. Sin esta prueba u otra similar, el paquete podría ejecutarse indefinidamente.|La ejecución continúa con el componente opcional que registra el tiempo de espera.|  
  
#### <a name="to-configure-an-execute-sql-task-to-query-whether-change-data-is-ready"></a>Para configurar una tarea Ejecutar SQL de forma que consulte si los datos modificados están listos  
  
1.  Dentro del contenedor de bucles For, agregue una tarea Ejecutar SQL.  
  
2.  En el **Editor de la tarea Ejecutar SQL**, en la página **General** , seleccione las opciones siguientes:  
  
    1.  En **Conjunto de resultados**, seleccione **Fila única**.  
  
    2.  Configure una conexión válida con una base de datos de origen.  
  
    3.  En **SQLSourceType**, seleccione **Entrada directa**.  
  
    4.  En **SQLStatement**, escriba la siguiente instrucción SQL:  
  
        ```  
        declare @DataReady int, @TimeoutCount int  
  
        if not exists (select tran_end_time from cdc.lsn_time_mapping  
                where tran_end_time > ?  )  
            select @DataReady = 0  
        else  
            if ? = 0  
                select @DataReady = 3   
        else  
            if not exists (select tran_end_time from cdc.lsn_time_mapping  
                    where tran_end_time <= ? )  
                select @DataReady = 1   
        else  
            select @DataReady = 2  
  
        select @TimeoutCount = ?  
        if (@DataReady = 0)  
            select @TimeoutCount = @TimeoutCount + 1  
        else  
            select @TimeoutCount = 0  
  
        if (@TimeoutCount > ?)  
            select @DataReady = 5  
  
        select @DataReady as DataReady, @TimeoutCount as TimeoutCount  
  
        ```  
  
3.  En la página **Asignación de parámetros** del **Editor de la tarea Ejecutar SQL**, realice las asignaciones siguientes:  
  
    1.  Asigne la variable ExtractEndTime al parámetro 0.  
  
    2.  Asigne la variable IntervalID al parámetro 1.  
  
    3.  Asigne la variable ExtractStartTime al parámetro 2.  
  
    4.  Asigne la variable TimeoutCount al parámetro 3.  
  
    5.  Asigne la variable TimeoutCeiling al parámetro 4.  
  
4.  En la página **Conjunto de resultados** del **Editor de la tarea Ejecutar SQL**, asigne el resultado de DataReady a la variable DataReady, y el resultado de TimeoutCount a la variable TimeoutCount.  
  
## <a name="waiting-until-the-change-data-is-ready"></a>Esperar hasta que los datos modificados estén listos  
 Puede utilizar varios métodos para implementar un retraso cuando los datos modificados no estén listos. Los dos procedimientos siguientes muestran cómo utilizar una tarea Script o una tarea Ejecutar SQL para implementar el retraso.  
  
> [!NOTE]  
>  Un script precompilado produce menos sobrecarga que una tarea Ejecutar SQL.  
  
#### <a name="to-implement-a-delay-by-using-a-script-task"></a>Para implementar un retraso mediante una tarea Script  
  
1.  Dentro del contenedor de bucles For, agregue una tarea Script.  
  
2.  Conecte a la nueva tarea Script la tarea Ejecutar SQL que hace la consulta para determinar si los datos modificados están preparados.  
  
3.  Para obtener la restricción de precedencia que conecta la tarea Ejecutar SQL a la tarea Script, abra el **Editor de restricciones de precedencia** y seleccione las opciones siguientes:  
  
    1.  En **Operación de evaluación**, seleccione **Expresión y restricción**.  
  
    2.  En **Valor**, seleccione **Correcto**.  
  
         El valor de restricción **Correcto** se refiere a la tarea anterior. En este caso, la tarea Ejecutar SQL debe haberse realizado correctamente.  
  
    3.  Para **Expresión**, escriba `@DataReady == 0 && @TimeoutCount <= @TimeoutCeiling`.  
  
    4.  Seleccione **AND lógico. Todas las restricciones deben evaluarse como True**, si no está ya seleccionada.  
  
4.  En el **Editor de la tarea Script**, en la página **Script** , para **ReadOnlyVariables**, seleccione la variable de entero **User::DelaySeconds** en la lista.  
  
5.  En el **Editor de la tarea Script**, en la página **Script** , haga clic en **Modificar script** para abrir el entorno de desarrollo de script.  
  
6.  En el procedimiento Main, escriba una de las líneas de código siguientes:  
  
    -   Si está programando en C#, escriba la línea de código siguiente:  
  
        ```  
        System.Threading.Thread.Sleep((int)Dts.Variables["DelaySeconds"].Value * 1000);  
        ```  
  
         \- o -  
  
    -   Si está programando en [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)], escriba la línea de código siguiente:  
  
        ```  
        System.Threading.Thread.Sleep(Ctype(Dts.Variables("DelaySeconds").Value, Integer) * 1000)  
  
        ```  
  
        > [!NOTE]  
        >   El método **Thread.Sleep** espera un argumento especificado en milisegundos.  
  
7.  Deje la línea de código predeterminada que devuelve **DtsExecResult.Success** de la ejecución del script.  
  
8.  Cierre el entorno de desarrollo de script y el **Editor de la tarea Script**.  
  
#### <a name="to-implement-a-delay-by-using-an-execute-sql-task"></a>Para implementar un retraso mediante una tarea Ejecutar SQL  
  
1.  Dentro del contenedor de bucles For, agregue una tarea Ejecutar SQL.  
  
2.  Conecte a la nueva tarea Ejecutar SQL la tarea Ejecutar SQL que hace la consulta para determinar si los datos modificados están preparados.  
  
3.  Para obtener la restricción de precedencia que conecta las dos tareas Ejecutar SQL, abra el **Editor de restricciones de precedencia** y seleccione las opciones siguientes:  
  
    1.  En **Operación de evaluación**, seleccione **Expresión y restricción**.  
  
    2.  En **Valor**, seleccione **Correcto**.  
  
         El valor de restricción **Correcto** se refiere a la tarea Ejecutar SQL anterior.  
  
    3.  Para **Expresión**, escriba `@DataReady == 0`.  
  
    4.  Seleccione **AND lógico. Todas las restricciones deben evaluarse como True**, si no está ya seleccionada.  
  
         Esta selección requiere que ambas condiciones, la restricción y la expresión, sean true.  
  
4.  En el **Editor de la tarea Ejecutar SQL**, en la página **General** , seleccione las opciones siguientes:  
  
    1.  En **Conjunto de resultados**, seleccione **Fila única**.  
  
    2.  Configure una conexión válida con una base de datos de origen.  
  
    3.  En **SQLSourceType**, seleccione **Entrada directa**.  
  
    4.  En **SQLStatement**, escriba la siguiente instrucción SQL:  
  
        ```  
        WAITFOR DELAY ?  
  
        ```  
  
5.  En la página **Asignación de parámetros** del editor, asigne la variable de cadena DelaySeconds al parámetro 0.  
  
## <a name="handling-an-error-condition"></a>Controlar una condición de error  
 Si lo desea, puede configurar un componente adicional dentro del bucle para registrar un error o que se ha superado el tiempo de espera:  
  
-   Este componente puede registrar una condición de error cuando el valor de la variable DataReady es igual a 1. Este valor indica que no hay datos modificados disponibles antes del inicio del intervalo seleccionado.  
  
-   Este componente también puede registrar un error de tiempo de espera cuando se alcanza el valor de la variable TimeoutCeiling. Este valor indica que el bucle ha comprobado la existencia de datos el número de veces especificado pero los datos aún no están disponibles. Sin esta prueba u otra similar, el paquete podría ejecutarse indefinidamente.  
  
#### <a name="to-configure-an-optional-script-task-to-log-an-error-condition"></a>Para configurar una tarea Script opcional de forma que registre una condición de error  
  
1.  Si desea informar del error o de que se ha superado el tiempo de espera escribiendo un mensaje en el registro, configure el registro para el paquete. Para más información, vea [Habilitar el registro de paquetes en SQL Server Data Tools](../../integration-services/performance/integration-services-ssis-logging.md#ssdt).  
  
2.  Dentro del contenedor de bucles For, agregue una tarea Script.  
  
3.  Conecte a la nueva tarea Script la tarea Ejecutar SQL que hace la consulta para determinar si los datos modificados están preparados.  
  
4.  Para obtener la restricción de precedencia que conecta la tarea Ejecutar SQL a la tarea Script, abra el **Editor de restricciones de precedencia** y seleccione las opciones siguientes:  
  
    1.  En **Operación de evaluación**, seleccione **Expresión y restricción**.  
  
    2.  En **Valor**, seleccione **Correcto**.  
  
         El valor de restricción **Correcto** se refiere a la tarea anterior. En este caso, la tarea Ejecutar SQL debe haberse realizado correctamente.  
  
    3.  Para **Expresión**, escriba `@DataReady == 1 || @DataReady == 5`.  
  
    4.  Seleccione **AND lógico. Todas las restricciones deben evaluarse como True**, si no está ya seleccionada.  
  
         Esta selección requiere que ambas condiciones, la restricción y la expresión, sean true.  
  
5.  En el **Editor de la tarea Script**, en la página **Script** del editor, para **ReadOnlyVariables**, seleccione **User::DataReady** y **User::ExtractStartTime** en la lista para que sus valores estén disponibles para el script.  
  
     Si desea incluir datos de ciertas variables del sistema (por ejemplo, System::PackageName) en la información que escribe en el registro, seleccione también dichas variables.  
  
6.  En el **Editor de la tarea Script**, en la página **Script** , haga clic en **Modificar script** para abrir el entorno de desarrollo de script.  
  
7.  En el procedimiento Main, escriba el código para registrar un error llamando al método **Dts.Log** o el código para provocar un evento llamando a uno de los métodos de la interfaz **Dts.Events** . Para informar del error al paquete, devuelva `Dts.TaskResult = Dts.Results.Failure`.  
  
     El ejemplo siguiente muestra cómo escribir un mensaje en el registro. Para obtener más información, consulte [Logging in the Script Task](../../integration-services/extending-packages-scripting/task/logging-in-the-script-task.md), [Raising Events in the Script Task](../../integration-services/extending-packages-scripting/task/raising-events-in-the-script-task.md)y [Returning Results from the Script Task](../../integration-services/extending-packages-scripting/task/returning-results-from-the-script-task.md).  
  
    ```  
    ' User variables.  
    Dim dataReady As Integer = _  
      CType(Dts.Variables("DataReady").Value, Integer)  
    Dim extractStartTime As Date = _  
      CType(Dts.Variables("ExtractStartTime").Value, DateTime)  
  
    ' System variables.  
    Dim packageName As String = _  
      Dts.Variables("PackageName").Value.ToString()  
    Dim executionStartTime As Date = _  
      CType(Dts.Variables("StartTime").Value, DateTime)  
  
    Dim eventMessage As New System.Text.StringBuilder()  
  
    If dataReady = 1 OrElse dataReady = 5 Then  
  
      If dataReady = 1 Then  
        eventMessage.AppendLine("Start Time Error")  
      Else  
        eventMessage.AppendLine("Timeout Error")  
      End If  
  
      With eventMessage  
        .Append("The package ")  
        .Append(packageName)  
        .Append(" started at ")  
        .Append(executionStartTime.ToString())  
        .Append(" and ended at ")  
        .AppendLine(DateTime.Now().ToString())  
        If dataReady = 1 Then  
          .Append("The specified ExtractStartTime was ")  
          .AppendLine(extractStartTime.ToString())  
        End If  
      End With  
  
      System.Windows.Forms.MessageBox.Show(eventMessage.ToString())  
  
      Dts.Log(eventMessage.ToString(), 0, Nothing)  
  
      Dts.TaskResult = Dts.Results.Failure  
  
    Else  
  
      Dts.TaskResult = Dts.Results.Success  
  
    End If  
  
    ```  
  
8.  Cierre el entorno de desarrollo de script y el **Editor de la tarea Script**.  
  
## <a name="next-step"></a>siguiente paso  
 Después de determinar que los datos modificados están listos, el paso siguiente consiste en preparar la consulta de los mismos.  
  
 **Siguiente tema:** [Preparar para consultar datos modificados](../../integration-services/change-data-capture/prepare-to-query-for-the-change-data.md)  
  
  
