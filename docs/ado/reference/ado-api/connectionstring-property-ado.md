---
description: Propiedad ConnectionString (ADO)
title: ConnectionString (propiedad, ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::ConnectionString
helpviewer_keywords:
- ConnectionString property [ADO]
ms.assetid: 3be75b75-4d36-4479-ab64-9a456869252a
author: rothja
ms.author: jroth
ms.openlocfilehash: 3f8fd13b1f00e7ccebf3b334573948d76aeb7242
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034645"
---
# <a name="connectionstring-property-ado"></a>Propiedad ConnectionString (ADO)
Indica la información utilizada para establecer una conexión con un origen de datos.  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 Establece o devuelve un valor de **cadena** .  
  
## <a name="remarks"></a>Observaciones  
 Use la propiedad **ConnectionString** para especificar un origen de datos pasando una cadena de conexión detallada que contenga una serie de instrucciones *argument* *= Value* separadas por punto y coma.  
  
 ADO admite cinco argumentos para la propiedad **ConnectionString** ; cualquier otro argumento pasa directamente al proveedor sin ningún procesamiento por parte de ADO. Los argumentos que admite ADO son los siguientes.  
  
|Argumento|Descripción|  
|--------------|-----------------|  
|*Proveedor =*|Especifica el nombre de un proveedor que se va a utilizar para la conexión.|  
|*Nombre de archivo =*|Especifica el nombre de un archivo específico del proveedor (por ejemplo, un objeto de origen de datos persistente) que contiene información de conexión preestablecida.|  
|*Proveedor remoto =*|Especifica el nombre de un proveedor que se va a utilizar al abrir una conexión del lado cliente. (Solo el servicio de datos remoto).|  
|*Servidor remoto =*|Especifica el nombre de la ruta de acceso del servidor que se va a utilizar al abrir una conexión del lado cliente. (Solo el servicio de datos remoto).|  
|*Dirección URL =*|Especifica la cadena de conexión como una dirección URL absoluta que identifica un recurso, como un archivo o un directorio.|  
  
 Después de establecer la propiedad **ConnectionString** y abrir el objeto de [conexión](./connection-object-ado.md) , el proveedor puede modificar el contenido de la propiedad, por ejemplo, asignando los nombres de argumento definidos por ADO a sus equivalentes para el proveedor específico.  
  
 La propiedad **ConnectionString** hereda automáticamente el valor utilizado para el argumento *ConnectionString* del método [Open](./open-method-ado-connection.md) , por lo que puede invalidar la propiedad **ConnectionString** actual durante la llamada al método **Open** .  
  
 Dado que el argumento de *nombre de archivo* hace que ADO cargue el proveedor asociado, no se pueden pasar los argumentos de *nombre de archivo* y *proveedor* .  
  
 La propiedad **ConnectionString** es de lectura y escritura cuando se cierra la conexión y de solo lectura cuando está abierta.  
  
 Se omiten los duplicados de un argumento en la propiedad **ConnectionString** . Se usa la última instancia de cualquier argumento.  
  
> [!NOTE]
>  **Uso del servicio de datos remotos** Cuando se usa en un objeto de **conexión** del lado cliente, la propiedad **ConnectionString** solo puede incluir los parámetros de *proveedor remoto* y *servidor remoto* .  
  
 En la tabla siguiente se enumeran los proveedores de ADO predeterminados para cada sistema operativo Windows:  
  
|Proveedor de ADO predeterminado|Sistema operativo Windows|  
|--------------------------|------------------------------|  
|MSDASQL<br /><br /> (Para mejorar la legibilidad del código fuente, especifique explícitamente el nombre del proveedor en la cadena de conexión).|Windows 2000 (32 bits)<br /><br /> Windows XP (32 bits)<br /><br /> Windows 2003 Server (32 bits)<br /><br /> Windows Vista (32 bits)<br /><br /> Windows Vista Service Pack 1 o posterior (32 bits y 64 bits)<br /><br /> Versiones de Windows posteriores a Windows Vista (32 bits y 64 bits)|  
|No hay valor predeterminado.<br /><br /> Cuando una aplicación ADO se ejecuta en los siguientes sistemas operativos y no especifica el proveedor explícitamente, ADO devuelve el siguiente error: "ADODB. Conexión: no se ha especificado el proveedor y no hay ningún proveedor predeterminado designado "|Windows 2000 (64 bits)<br /><br /> Windows XP (64 bits)<br /><br /> Windows 2003 Server (64 bits)<br /><br /> Windows Vista (64 bits)|  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conexión (ADO)](./connection-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades ConnectionString, ConnectionTimeout y State (VB)](./connectionstring-connectiontimeout-and-state-properties-example-vb.md)   
 [Ejemplo de las propiedades ConnectionString, ConnectionTimeout y State (VC + +)](./connectionstring-connectiontimeout-and-state-properties-example-vc.md)   
 [Apéndice A: Proveedores](../../guide/appendixes/appendix-a-providers.md)