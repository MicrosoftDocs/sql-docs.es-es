---
description: Colección de parámetros (ADO)
title: Colección Parameters (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Command15::get_Parameters
- Command15::Parameters
- Command15::GetParameters
helpviewer_keywords:
- Parameters collection [ADO]
ms.assetid: 497cae10-3913-422a-9753-dcbb0a639b1b
author: rothja
ms.author: jroth
ms.openlocfilehash: 93d34581e7ea19ad584161e12375101a06256cbd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041115"
---
# <a name="parameters-collection-ado"></a>Colección de parámetros (ADO)
Contiene todos los objetos de [parámetro](./parameter-object.md) de un objeto [Command](./command-object-ado.md) .  
  
## <a name="remarks"></a>Observaciones  
 Un objeto **Command** tiene una colección **Parameters** formada por objetos **Parameter** .  
  
 El uso del método [Refresh](./refresh-method-ado.md) en la colección de **parámetros** de un objeto **Command** recupera información de parámetros del proveedor para el procedimiento almacenado o la consulta con parámetros especificados en el objeto **Command** . Algunos proveedores no admiten llamadas a procedimientos almacenados o consultas parametrizadas; la llamada al método **Refresh** en la colección **Parameters** cuando se usa este tipo de proveedor devolverá un error.  
  
 Si no ha definido sus propios objetos de **parámetro** y tiene acceso a la colección **Parameters** antes de llamar al método **Refresh** , ADO llamará automáticamente al método y rellenará la colección.  
  
 Puede minimizar las llamadas al proveedor para mejorar el rendimiento si conoce las propiedades de los parámetros asociados al procedimiento almacenado o a la consulta parametrizada a la que desea llamar. Use el método [CreateParameter](./createparameter-method-ado.md) para crear objetos de **parámetro** con los valores de propiedad adecuados y use el método [Append](./append-method-ado.md) para agregarlos a la colección de **parámetros** . Esto le permite establecer y devolver los valores de los parámetros sin tener que llamar al proveedor para obtener la información de los parámetros. Si está escribiendo en un proveedor que no proporciona información de parámetros, debe rellenar manualmente la colección **Parameters** mediante este método para poder usar parámetros en absoluto. Use el método [Delete](./delete-method-ado-parameters-collection.md) para quitar objetos de **parámetro** de la colección **Parameters** si es necesario.  
  
 Los objetos de la colección **Parameters** de un **conjunto de registros** salen del ámbito (por lo que no están disponibles) cuando se cierra el conjunto de **registros** .  
  
 Cuando se llama a un procedimiento almacenado con el **comando**, el parámetro de valor devuelto y de salida de un procedimiento almacenado se recupera de la siguiente manera:  
  
1.  Cuando se llama a un procedimiento almacenado que no tiene parámetros, se debe llamar al método **Refresh** en la colección **Parameters** antes de llamar al método **Execute** en el objeto **Command** .  
  
2.  Cuando se llama a un procedimiento almacenado con parámetros y se anexa explícitamente un parámetro a la colección **Parameters** con **Append**, el valor devuelto/parámetro Output debe anexarse a la colección **Parameters** . El valor devuelto debe anexarse primero a la colección **Parameters** . Use **Append** para agregar los otros parámetros a la colección **Parameters** en el orden de definición. Por ejemplo, el procedimiento almacenado SPWithParam tiene dos parámetros. El primer parámetro, *inparam*, es un parámetro de entrada definido como advarchar (20) y el segundo parámetro, *outParam*, es un parámetro de salida definido como advarchar (20). Puede recuperar el valor devuelto y el parámetro de salida con el código siguiente.  
  
    ```vb
    ' Open Connection Conn  
    set ccmd = CreateObject("ADODB.Command")  
    ccmd.Activeconnection= Conn  
  
    ccmd.CommandText="SPWithParam"  
    ccmd.commandType = 4 'adCmdStoredProc  
  
    ccmd.parameters.Append ccmd.CreateParameter(, adInteger, adParamReturnValue, , NULL)   ' return value  
    ccmd.parameters.Append ccmd.CreateParameter("InParam", adVarChar, adParamInput, 20, "hello world")   ' input parameter  
    ccmd.parameters.Append ccmd.CreateParameter("OutParam", adVarChar, adParamOutput, 20, NULL)   ' output parameter  
  
    ccmd.execute()  
  
    ' Access ccmd.parameters(0) as return value of this stored procedure  
    ' Access ccmd.parameters("OutParam") as the output parameter of this stored procedure.  
  
    ```  
  
3.  Cuando se llama a un procedimiento almacenado con parámetros y se configuran los parámetros mediante una llamada al método **Item** en la colección **Parameters** , el parámetro de valor devuelto y de salida del procedimiento almacenado se puede recuperar de la colección **Parameters** . Por ejemplo, el procedimiento almacenado SPWithParam tiene dos parámetros. El primer parámetro, *inparam*, es un parámetro de entrada definido como advarchar (20) y el segundo parámetro, *outParam*, es un parámetro de salida definido como advarchar (20). Puede recuperar el valor devuelto y el parámetro de salida con el código siguiente.  
  
    ```vb
    ' Open Connection Conn  
    set ccmd = CreateObject("ADODB.Command")  
    ccmd.Activeconnection= Conn  
  
    ccmd.CommandText="SPWithParam"  
    ccmd.commandType = 4 'adCmdStoredProc  
  
    ccmd.parameters.Item("InParam").value = "hello world" ' input parameter  
    ccmd.execute()  
  
    ' Access ccmd.parameters(0) as return value of stored procedure  
    ' Access ccmd.parameters(2) or ccmd.parameters("OutParam") as the output parameter.  
    ```  
  
 Esta sección contiene el siguiente tema.  
  
-   [Propiedades, métodos y eventos de la colección Parameters](./parameters-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Consulte también  
 [Append (método) (ADO)](./append-method-ado.md)   
 [CreateParameter (método) (ADO)](./createparameter-method-ado.md)   
 [Objeto Parameter](./parameter-object.md)