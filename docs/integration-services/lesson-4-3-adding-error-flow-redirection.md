---
description: 'Lección 4-3: Adición de redireccionamiento de flujo de errores'
title: 'Paso 3: Adición de redireccionamiento de flujo de errores | Microsoft Docs'
ms.custom: ''
ms.date: 01/07/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 5683a45d-9e73-4cd5-83ca-fae8b26b488c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ac63d922423bcbb9c0c7f513d34a5834b3f0c1b3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352025"
---
# <a name="lesson-4-3-add-error-flow-redirection"></a>Lección 4-3: Adición de redireccionamiento de flujo de errores

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



En la tarea anterior, la transformación Lookup Currency Key no puede generar una coincidencia cuando la transformación intenta procesar el archivo plano de ejemplo dañado que genera un error. Puesto que la transformación utiliza la configuración de salida de error predeterminada, cualquier error da lugar a un error de la transformación. Cuando se produce un error en la transformación, también se produce un error en el resto del paquete.  
  
En lugar de permitir que se produzca un error en la transformación, puede configurar el componente de modo que la fila que genera el error se redirija a otra ruta de procesamiento mediante la salida de errores. El uso de una ruta de acceso de procesamiento de errores independiente proporciona más opciones. Por ejemplo, podría limpiar los datos y luego volver a procesar la fila con error. O bien, podría guardar la fila con error junto con la información sobre el error para comprobarla y procesarla de nuevo más adelante.  
  
En esta tarea se configura la transformación Lookup Currency Key para redirigir cualquier fila con errores a la salida de errores. En la rama de errores del flujo de datos, [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] escribe estas filas en un archivo.  
  
De forma predeterminada, las dos columnas adicionales en una salida de errores de [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] (**ErrorCode** y **ErrorColumn**) solo contienen un código de error numérico y el identificador de la columna en la que se ha producido el error. En esta tarea, antes de que el paquete escriba las filas con errores en el archivo, se usa un componente de script para acceder a la API [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] y obtener una descripción del error.  
  
## <a name="configure-an-error-output"></a>Configuración de una salida de errores  
  
1.  En el **Cuadro de herramientas de SSIS**, expanda **Comunes** y, a continuación, arrastre **Componente de script** a la superficie de diseño de la pestaña **Flujo de datos** . Coloque **Script** a la derecha de la transformación **Lookup Currency Key** .  
  
2.  En el cuadro de diálogo **Seleccionar el tipo de componente de script**, haga clic en **Transformación** y luego en **Aceptar**.  
  
3.  Para conectar los dos componentes, haga clic en la transformación **Lookup Currency Key** y luego arrastre la flecha de color rojo hasta la nueva transformación **Script**.  
  
    La flecha roja representa la salida de errores de la transformación **Lookup Currency Key** . Con la flecha de color rojo para conectar la transformación con el componente de script, se redirige cualquier error de procesamiento a este componente, que lo procesa y envía al destino.  
  
4.  En el cuadro de diálogo **Configurar la salida de errores**, en la columna **Error**, seleccione **Redirigir fila** y, después, haga clic en **Aceptar**.  
  
5.  En la superficie de diseño **Flujo de datos**, haga clic en el nombre **Componente de script** del nuevo **Componente de script** y cambie el nombre por **Get Error Description**.  
  
6.  Haga doble clic en la transformación **Get Error Description** .  
  
7.  En el cuadro de diálogo **Editor de transformación Script** , en la página **Columnas de entrada** , seleccione la columna **ErrorCode** .  
  
8.  En la página **Entradas y salidas**, expanda **Salida 0**, haga clic en **Columnas de salida** y después en **Agregar columna**.  
  
9. En la propiedad **Nombre**, escriba *ErrorDescription* y establezca la propiedad **DataType** en **Cadena Unicode [DT_WSTR]**.  
  
10. En la página **Script**, compruebe que la propiedad **LocaleID** se haya establecido en **Inglés (Estados Unidos)** .
  
11. Haga clic en **Editar script** para abrir [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] Tools for Applications (VSTA). En el método **Input0_ProcessInputRow**, escriba o pegue el código siguiente:  
  
    [Visual Basic]  
  
    ```vb  
    Row.ErrorDescription =   
      Me.ComponentMetaData.GetErrorDescription(Row.ErrorCode)  
    ```  
  
    [Visual C#]  
  
    ```cs
    Row.ErrorDescription = this.ComponentMetaData.GetErrorDescription(Row.ErrorCode);  
    ```  
  
    La subrutina completada es similar al código siguiente:  
  
    [Visual Basic]  
  
    ```vb
    Public Overrides Sub Input0_ProcessInputRow(ByVal Row As Input0Buffer)  
  
      Row.ErrorDescription =   
        Me.ComponentMetaData.GetErrorDescription(Row.ErrorCode)  
  
    End Sub  
    ```  
  
    [Visual C#]  
  
    ```cs
    public override void Input0_ProcessInputRow(Input0Buffer Row)  
        {  
  
            Row.ErrorDescription = this.ComponentMetaData.GetErrorDescription(Row.ErrorCode);  
  
        }  
    ```  
  
12. En el menú **Compilar**, haga clic en **Compilar solución** para compilar el script y guardar los cambios y, después, cierre VSTA.  
  
13. Haga clic en **Aceptar** para cerrar el cuadro de diálogo **Editor de transformación Script**.  
  
## <a name="go-to-next-task"></a>Ir a la tarea siguiente
[Paso 4: Adición de un destino de archivo plano](../integration-services/lesson-4-4-adding-a-flat-file-destination.md)  
  
  
  
