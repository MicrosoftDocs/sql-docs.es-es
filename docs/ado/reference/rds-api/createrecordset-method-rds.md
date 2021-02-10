---
description: Ejemplo del método CreateRecordset (RDS)
title: Método CreateRecordset (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- DataControl::CreateRecordset
- RDS.DataControl::CreateRecordset
- CreateRecordset
- RDSServer.DataFactory::CreateRecordset
- DataFactory::CreateRecordset
helpviewer_keywords:
- CreateRecordset method [RDS]
ms.assetid: 6840b1e5-c04d-4d3e-9dcc-42128c83492f
author: rothja
ms.author: jroth
ms.openlocfilehash: 6349438e415f3f3d4b2f28820a23bbb1a70720cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100053266"
---
# <a name="createrecordset-method-rds"></a>Ejemplo del método CreateRecordset (RDS)
Crea un [conjunto de registros](../ado-api/recordset-object-ado.md)vacío y desconectado.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
object.CreateRecordset(ColumnInfos)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *Object*  
 Variable de objeto que representa un objeto [RDSServer. DataFactory](./datafactory-object-rdsserver.md) o [RDS. Objeto DataControl](./datacontrol-object-rds.md) .  
  
 *ColumnsInfos*  
 Matriz de atributos **Variant** que define cada columna del conjunto de **registros** creado. Cada definición de columna contiene una matriz de cuatro atributos necesarios y un atributo opcional.  
  
|Atributo|Descripción|  
|---------------|-----------------|  
|Nombre|Nombre del encabezado de columna.|  
|Tipo|Entero del tipo de datos.|  
|Size|Entero del ancho en caracteres, independientemente del tipo de datos.|  
|Nulabilidad|Valor booleano.|  
|Escala (opcional)|Este atributo opcional define la escala de los campos numéricos. Si no se especifica este valor, los valores numéricos se truncarán en una escala de tres. La precisión no se ve afectada, pero el número de dígitos que siguen al separador decimal se truncará en tres.|  
  
 Después, el conjunto de matrices de columnas se agrupa en una matriz, que define el **conjunto de registros**.  
  
## <a name="remarks"></a>Observaciones  
 El objeto comercial del lado servidor puede rellenar el **conjunto de registros** resultante con datos de un proveedor de datos no OLE DB, como un archivo del sistema operativo que contiene cotizaciones bursátiles.  
  
 En la tabla siguiente se enumeran los valores de [DataTypeEnum](../ado-api/datatypeenum.md) admitidos por el método **CreateRecordset** . El número mostrado es el número de referencia que se usa para definir los campos.  
  
 Cada uno de los tipos de datos tiene una longitud fija o una longitud variable. Los tipos de longitud fija deben definirse con un tamaño de-1, ya que el tamaño está predeterminado y todavía se requiere una definición de tamaño. Los tipos de datos de longitud variable permiten un tamaño de 1 a 32767.  
  
 En algunos de los tipos de datos de variable, el tipo se puede convertir al tipo indicado en la columna de sustitución. No verá las sustituciones hasta que el conjunto de **registros** se haya creado y rellenado. A continuación, puede comprobar el tipo de datos real, si es necesario.  
  
|Length|Constante|Number|Sustitución|  
|------------|--------------|------------|------------------|  
|Fijo|**adTinyInt**|16||  
|Fijo|**adSmallInt**|2||  
|Fijo|**adInteger**|3||  
|Fijo|**adBigInt**|20||  
|Fijo|**adUnsignedTinyInt**|17||  
|Fijo|**adUnsignedSmallInt**|18||  
|Fijo|**adUnsignedInt**|19||  
|Fijo|**adUnsignedBigInt**|21||  
|Fijo|**adSingle**|4||  
|Fijo|**adDouble**|5||  
|Fijo|**adCurrency**|6||  
|Fijo|**adDecimal**|14||  
|Fijo|**adNumeric**|131||  
|Fijo|**adBoolean**|11||  
|Fijo|**adError**|10||  
|Fijo|**adGuid**|72||  
|Fijo|**adDate**|7||  
|Fijo|**adDBDate**|133||  
|Fijo|**adDBTime**|134||  
|Fijo|**adDBTimestamp**|135|7|  
|Variable|**adBSTR**|8|130|  
|Variable|**adChar**|129|200|  
|Variable|**adVarChar**|200||  
|Variable|**adLongVarChar**|201|200|  
|Variable|**adWChar**|130||  
|Variable|**adVarWChar**|202|130|  
|Variable|**adLongVarWChar**|203|130|  
|Variable|**adBinary**|128||  
|Variable|**adVarBinary**|204||  
|Variable|**adLongVarBinary**|205|204|  
  
## <a name="applies-to"></a>Se aplica a  

:::row:::
    :::column:::
        [Objeto DataControl (RDS)](./datacontrol-object-rds.md)  
    :::column-end:::
    :::column:::
        [Objeto DataFactory (RDSServer)](./datafactory-object-rdsserver.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Consulte también  
 [Ejemplo del método CreateRecordset (VB)](../ado-api/createrecordset-method-example-vb.md)   
 [Ejemplo del método CreateRecordset (VBScript)](./createrecordset-method-example-vbscript.md)   
 [CreateObject (método) (RDS)](./createobject-method-rds.md)