---
description: Objeto DataControl (RDS)
title: Objeto DataControl (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- DataControl
- RDS.DataControl
helpviewer_keywords:
- DataControl object [ADO]
ms.assetid: d85ea4fc-451c-436e-97b8-58f92b149dd0
author: rothja
ms.author: jroth
ms.openlocfilehash: 5ecd88f80d5d29db4e27fa99315a1e5cb2489541
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049465"
---
# <a name="datacontrol-object-rds"></a>Objeto DataControl (RDS)
Enlaza un [conjunto de registros](../ado-api/recordset-object-ado.md) de consulta de datos a uno o varios controles (por ejemplo, un cuadro de texto, un control de cuadrícula o un cuadro combinado) para mostrar los datos del **conjunto de registros** en una página web.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
<OBJECT CLASSID="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33" ID="DataControl"  
   <PARAM NAME="Connect" VALUE="DSN=DSNName;UID=MyUserID;PWD=MyPassword;">  
   <PARAM NAME="Server" VALUE="https://awebsrvr">  
   <PARAM NAME="SQL" VALUE="QueryText">  
</OBJECT>  
```  
  
## <a name="remarks"></a>Observaciones  
 IDENTIFICADOR de clase del **objeto RDS.** El objeto DataControl es BD96C556-65A3-11D0-983A-00C04FC29E33.  
  
> [!NOTE]
>  Si recibe un error que hace que un [objeto RDS. DataSpace](./dataspace-object-rds.md) o **RDS.** El objeto DataControl no se carga, asegúrese de que está usando el identificador de clase correcto. Los identificadores de clase de estos objetos han cambiado respecto a la versión 1,0 y 1,1. Además, tenga en cuenta que incluso las columnas que aceptan valores NULL se deben establecer cuando se usa el objeto **DataControl de RDS** .  
  
 En un escenario básico, solo tiene que establecer las propiedades **SQL**, **Connect** y **Server** de **RDS. Objeto DataControl** , que llamará automáticamente al objeto comercial predeterminado, [RDSServer. DataFactory](./datafactory-object-rdsserver.md).  
  
 Todas las propiedades de **RDS. DataControl** son opcionales porque los objetos comerciales personalizados pueden reemplazar su funcionalidad.  
  
> [!NOTE]
>  Si consulta varios resultados, solo se devuelve el primer [conjunto de registros](../ado-api/recordset-object-ado.md) . Si se necesitan varios conjuntos de resultados, asigne cada uno a su propio **control de DataControl**. Un ejemplo de una consulta para varios resultados podría ser el siguiente: `"Select * from Authors, Select * from Topics"`  
  
 Agregar "DFMode = 20;" a la cadena de conexión cuando se utiliza **RDS.** El objeto DataControl puede mejorar el rendimiento del servidor cuando se actualizan datos. Con esta configuración, el objeto **RDSServer. DataFactory** en el servidor utiliza un modo con menos recursos. Sin embargo, las siguientes características no están disponibles en esta configuración:  
  
-   Uso de consultas con parámetros.  
  
-   Obtención de información de parámetros o columnas antes de llamar al método **Execute** .  
  
-   Establecer **Transact updates** en **true**.  
  
-   Obteniendo el estado de la fila.  
  
-   Llamada al método [Resync](../ado-api/resync-method.md) .  
  
-   Actualizar (explícita o automáticamente) a través de la propiedad de [resincronización](../ado-api/update-resync-property-dynamic-ado.md) de la actualización.  
  
-   Establecer propiedades de **comando** o [conjunto de registros](./recordset-sourcerecordset-properties-rds.md) .  
  
-   Usar **adCmdTableDirect**.  
  
 **Objeto RDS.** El objeto DataControl se ejecuta de forma predeterminada en modo asincrónico. Si necesita ejecutar la aplicación sincrónicamente, establezca el parámetro [ExecuteOptions](./executeoptions-property-rds.md) en **adcExecSync** y el parámetro [FetchOptions](./fetchoptions-property-rds.md) en **adcFetchUpFront**, como se muestra en el ejemplo siguiente.  
  
```  
<OBJECT CLASSID="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33"   
    ID="DataControl"  
   <PARAM NAME="Connect" VALUE="DSN=DSNName;UID=MyUserID;PWD=MyPassword;">  
   <PARAM NAME="Server" VALUE="https://awebsrvr">  
   <PARAM NAME="SQL" VALUE="QueryText">  
   <PARAM NAME="ExecuteOptions" VALUE="1">   <PARAM NAME="FetchOptions" VALUE="1">  
</OBJECT>  
```  
  
 Use un **objeto RDS. Objeto DataControl** para vincular los resultados de una consulta única a uno o varios controles visuales. Por ejemplo, supongamos que codifica una consulta que solicita datos de clientes como el nombre, la residencia, el lugar de nacimiento, la edad y el estado de los clientes prioritarios. Puede usar un solo **RDS. Objeto DataControl** para mostrar el nombre, la edad y la región de un cliente en tres cuadros de texto independientes; Estado de cliente prioritario en una casilla; y todos los datos de un control de cuadrícula.  
  
 Usar **RDS diferente. Objetos DataControl** para vincular los resultados de varias consultas a diferentes controles visuales. Por ejemplo, supongamos que usa una consulta para obtener información sobre un cliente y una segunda consulta para obtener información sobre los productos que ha adquirido el cliente. Desea mostrar los resultados de la primera consulta en tres cuadros de texto y una casilla, y los resultados de la segunda consulta en un control de cuadrícula. Si utiliza el objeto comercial predeterminado (**RDSServer. DataFactory**), debe hacer lo siguiente:  
  
-   Agregue dos **RDS. Objetos DataControl** en la Página Web.  
  
-   Escriba dos consultas, una para cada propiedad **SQL** de los dos **RDS. Objetos DataControl** . Un **objeto RDS.** El objeto DataControl contendrá una consulta SQL que solicita información del cliente; el segundo contendrá una consulta que solicita una lista de productos que el cliente ha adquirido.  
  
-   En las etiquetas de objeto de cada control enlazado, especifique el valor de DATAFLD para establecer los valores de los datos que desea mostrar en cada control visual.  
  
 No hay ninguna restricción de recuento en el número de **RDS. Objetos DataControl** que se pueden insertar mediante etiquetas de objeto en una sola página web.  
  
 Al definir **RDS. Objeto DataControl** en una página web, use valores de **alto** y **ancho** distintos de cero, como 1 (para evitar la inclusión de espacio adicional).  
  
 Los componentes de cliente del servicio de datos remotos ya están incluidos como parte de Internet Explorer 4,0; por lo tanto, no es necesario incluir un parámetro CODEBASE en el **objeto RDS.** Etiqueta de objeto DataControl.  
  
 Con Internet Explorer 4,0 o posterior, puede enlazar a datos utilizando controles HTML y controles de® ActiveX solo si están marcados como controles de modelo de apartamento.  
  
> [!NOTE]
>  **Usuarios de Microsoft Visual Basic** **Objeto RDS. DataControl** es seguro para scripting y solo se usa en aplicaciones basadas en Web. Una aplicación cliente Visual Basic no lo necesita.  
  
 Esta sección contiene el siguiente tema.  
  
-   [Eventos, métodos y propiedades de objeto DataControl (RDS)](./datacontrol-object-rds-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo del objeto DataControl (VBScript)](./datacontrol-object-example-vbscript.md)