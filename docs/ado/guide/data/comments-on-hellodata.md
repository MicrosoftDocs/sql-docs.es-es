---
description: Comentarios en HelloData
title: Comentarios sobre HelloData | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- hellodata sample application [ADO]
ms.assetid: a2831d77-7040-4b73-bbae-fe0bf78107ed
author: rothja
ms.author: jroth
ms.openlocfilehash: e0510d75c69caadd52a595d58900662cc8ef7ad4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037715"
---
# <a name="comments-on-hellodata"></a>Comentarios en HelloData
La aplicación HelloData guía a través de las operaciones básicas de una aplicación ADO típica: obtener, examinar, editar y actualizar datos. Cuando inicie la aplicación, haga clic en el primer botón, **obtener datos**. Se ejecutará la subrutina **GetData** .  
  
## <a name="getdata"></a>GetData  
 **GetData** coloca una cadena de conexión válida en una variable de nivel de módulo *m_sConnStr*. Para obtener más información sobre las cadenas de conexión, vea [crear la cadena de conexión](./creating-a-connection-string.md).  
  
 Asigne un controlador de errores mediante una instrucción Visual Basic **OnError** . Para obtener más información sobre el control de errores en ADO, vea [control de errores](./error-handling.md). Se crea un nuevo objeto de **conexión** y la propiedad **CursorLocation** está establecida en **AdUseClient** porque el ejemplo de HelloData crea un *conjunto de registros desconectado*. Esto significa que, tan pronto como se hayan recuperado los datos del origen de datos, se interrumpirá la conexión física con el origen de datos, pero puede seguir trabajando con los datos almacenados en caché localmente en el objeto de **conjunto de registros** .  
  
 Una vez abierta la conexión, asigne una cadena SQL a una variable (sSQL). A continuación, cree una instancia de un nuevo objeto de **conjunto de registros** , `m_oRecordset1` . En la siguiente línea de código, abra el **conjunto de registros** a través de la **conexión** existente, pasando `sSQL` como el origen del **conjunto de registros**. Ayuda a ADO a tomar la decisión de que la cadena de SQL que ha pasado como origen del **conjunto de registros** es una definición textual de un comando pasando **adCmdText** en el argumento final al método Open de **conjunto de registros** . Esta línea también establece el **LockType** y **CursorType** asociados al **conjunto de registros**.  
  
 La siguiente línea de código establece el valor de la propiedad **MarshalOptions** en **adMarshalModifiedOnly**. **MarshalOptions** indica qué registros deben calcularse en el nivel intermedio (o servidor Web). Para obtener más información sobre el cálculo de referencias, vea la documentación de COM. Cuando se usa **adMarshalModifiedOnly** con un cursor del lado cliente ([CursorLocation](../../reference/ado-api/cursorlocation-property-ado.md)  =  **adUseClient**), solo los registros que se han modificado en el cliente se escriben de nuevo en el nivel intermedio. Establecer **MarshalOptions** en **adMarshalModifiedOnly** puede mejorar el rendimiento porque se calculan las referencias de menos filas.  
  
 A continuación, desconecte el **conjunto de registros** estableciendo su propiedad **ActiveConnection** en **Nothing**. Para obtener más información, vea la sección "desconectar y volver a conectar el conjunto de registros" en [actualización y persistencia de datos](./updating-and-persisting-data.md).  
  
 Cierre la conexión al origen de datos y destruya el objeto de **conexión** existente. Esto libera los recursos consumidos.  
  
 El paso final consiste en establecer el **conjunto de registros** como el **origen** de datos del control DataGrid de Microsoft en el formulario para que pueda mostrar fácilmente los datos del **conjunto de registros** en el formulario.  
  
 Haga clic en el segundo botón, **examinar datos**. Esto ejecuta la subrutina **ExamineData** .  
  
## <a name="examinedata"></a>ExamineData  
 ExamineData usa varios métodos y propiedades del objeto de **conjunto de registros** para mostrar información sobre los datos del conjunto de **registros**. Informa del número de registros mediante la propiedad **RecordCount** . Recorre en bucle el **conjunto de registros** e imprime el valor de la propiedad **AbsolutePosition** en el cuadro de texto para mostrar del formulario. Además, en el bucle, el valor de la propiedad **Bookmark** del tercer registro se coloca en una variable Variant, *vBookmark*, para su uso posterior.  
  
 La rutina navega directamente al tercer registro mediante la variable de marcador que almacenó anteriormente. La rutina llama a la subrutina **WalkFields** , que recorre en bucle la colección **Fields** del **conjunto de registros** y muestra los detalles de cada **campo** de la colección.  
  
 Por último, **ExamineData** usa la propiedad **Filter** del **conjunto de registros** para buscar solo los registros cuyo **CategoryID** sea igual a 2. El resultado de aplicar este filtro es visible inmediatamente en la cuadrícula de presentación del formulario.  
  
 Para obtener más información acerca de la funcionalidad que se muestra en la subrutina **ExamineData** , consulte [examinar datos](./examining-data.md).  
  
 A continuación, haga clic en el tercer botón, **Editar datos**. Se ejecutará la subrutina **EditData** .  
  
## <a name="editdata"></a>EditData  
 Cuando el código entra en la subrutina **EditData** , el **conjunto de registros** sigue estando filtrado por **CategoryID** igual a 2, de modo que solo estén visibles los elementos que cumplan los criterios de filtro. En primer lugar, recorre en iteración el **conjunto de registros** y aumenta el precio de cada elemento visible en el **conjunto de registros** en un 10 por ciento. El valor del campo **Price** se cambia estableciendo la propiedad **Value** de ese campo en una cantidad nueva y válida.  
  
 Recuerde que el **conjunto de registros** está desconectado del origen de datos. Los cambios que se realizaron en **EditData** solo se realizan en la copia almacenada localmente en caché de los datos. Para obtener más información, vea [Editar datos](./editing-data.md).  
  
 Los cambios no se realizarán en el origen de datos hasta que haga clic en el cuarto botón, **actualizar datos**. Se ejecutará la subrutina **UpdateData** .  
  
## <a name="updatedata"></a>UpdateData  
 En primer lugar, UpdateData quita el filtro que se ha aplicado al **conjunto de registros**. El código quita y restablece `m_oRecordset1` como el **origen** de los elementos DataGrid de Microsoft en el formulario para que el **conjunto de registros** sin filtrar aparezca en la cuadrícula.  
  
 A continuación, el código comprueba para ver si puede moverse hacia atrás en el **conjunto de registros** mediante el método **Supports** con el argumento **adMovePrevious** .  
  
 La rutina se mueve al primer registro mediante el método **MoveFirst** y muestra los valores originales y actuales del campo, utilizando las propiedades **OriginalValue** y **Value** del objeto de **campo** . Estas propiedades, junto con la propiedad **UnderlyingValue** (no utilizadas aquí), se describen en [actualizar y conservar datos](./updating-and-persisting-data.md).  
  
 A continuación, se crea un nuevo objeto de **conexión** y se utiliza para restablecer una conexión con el origen de datos. Para volver a conectar el **conjunto de registros** al origen de datos, establezca la nueva **conexión** como el objeto **ActiveConnection** del **conjunto de registros**. Para enviar las actualizaciones al servidor, el código llama a **UpdateBatch** en el **conjunto de registros**.  
  
 Si la actualización por lotes se realiza correctamente, una variable de marcador de nivel de módulo, `m_flgPriceUpdated` , se establece en true. Esto le avisará más adelante para limpiar todos los cambios que se realizaron en la base de datos.  
  
 Por último, el código vuelve al primer registro del conjunto de **registros** y muestra los valores originales y actuales. Los valores son los mismos después de la llamada a **UpdateBatch**.  
  
 Para obtener información detallada sobre cómo actualizar los datos, incluido qué hacer cuando cambian los datos en el servidor mientras el **conjunto de registros** está desconectado, vea [actualizar y conservar datos](./updating-and-persisting-data.md).  
  
## <a name="form_unload"></a>Form_Unload  
 La subrutina **Form_Unload** es importante por varias razones. En primer lugar, dado que se trata de una aplicación de ejemplo, Form_Unload limpia los cambios que se realizaron en la base de datos antes de salir de la aplicación. En segundo lugar, el código muestra cómo un comando se puede ejecutar directamente desde un objeto de **conexión** abierto mediante el método **Execute** . Por último, se muestra un ejemplo de ejecución de una consulta que no devuelve filas (una consulta UPDATE) en el origen de datos.