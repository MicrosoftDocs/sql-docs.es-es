---
description: Método ConvertToString (RDS)
title: Método ConvertToString (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- ConvertToString method [ADO]
ms.assetid: b3f36bc8-6f69-49b0-83cd-2ccd3afebfbe
author: rothja
ms.author: jroth
ms.openlocfilehash: ce472fa758c479a02a43ce8ddd640219e1f7919e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049515"
---
# <a name="converttostring-method-rds"></a>Método ConvertToString (RDS)
Convierte un [conjunto de registros](../ado-api/recordset-object-ado.md) en una cadena MIME que representa los datos del conjunto de registros.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
DataFactory.ConvertToString(Recordset)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *DataFactory*  
 Variable de objeto que representa un objeto [RDSServer. DataFactory](./datafactory-object-rdsserver.md) .  
  
 *DataRecordsets*  
 Variable de objeto que representa un objeto de **conjunto de registros** .  
  
## <a name="remarks"></a>Observaciones  
 Con los archivos. asp, use **ConvertToString** para incrustar el **conjunto de registros** en una página HTML generada en el servidor para transportarlo a un equipo cliente.  
  
 **ConvertToString** carga primero el **conjunto de registros** en las tablas de servicio de cursor y, a continuación, genera una secuencia en formato MIME.  
  
 En el cliente, el servicio de datos remotos puede volver a convertir la cadena MIME en un **conjunto de registros** totalmente funcional. Funciona bien para controlar menos de 400 filas de datos con un ancho de más de 1024 bytes por fila. No debe usarlo para la transmisión por secuencias de datos de BLOBs y grandes conjuntos de resultados a través de HTTP. No se realiza ninguna compresión de conexión en la cadena, por lo que los conjuntos de datos muy grandes tardarán mucho tiempo en transportarse a través de HTTP cuando se comparan con el formato tablegram optimizado para conexión que el servicio de datos remotos define e implementa como su formato de protocolo de transporte nativo.  
  
> [!NOTE]
>  Si está utilizando Active Server páginas para insertar la cadena MIME resultante en una página HTML de cliente, tenga en cuenta que las versiones de VBScript anteriores a la versión 2,0 limitan el tamaño de la cadena a 32 KB. Si se supera este límite, se devuelve un error. Mantenga el ámbito de la consulta relativamente pequeño al utilizar la incrustación MIME a través de archivos. asp. Para solucionarlo, descargue la versión más reciente de VBScript del sitio web Microsoft Windows Script Technologies.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto DataFactory (RDSServer)](./datafactory-object-rdsserver.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo del método ConvertToString (VB)](../ado-api/converttostring-method-example-vb.md)   
 [Ejemplo del método ConvertToString (VBScript)](./converttostring-method-example-vbscript.md)