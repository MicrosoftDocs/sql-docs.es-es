---
description: DataTypeEnum
title: DataTypeEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- DataTypeEnum
helpviewer_keywords:
- DataTypeEnum enumeration [ADO]
ms.assetid: 2c57eca6-9336-4b06-ba10-9fef5926b1d0
author: rothja
ms.author: jroth
ms.openlocfilehash: e815d834ffbab6276f4ee6a9069f1163ac9e4cd5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100025642"
---
# <a name="datatypeenum"></a>DataTypeEnum
Especifica el tipo de datos de un [campo](../../../ado/reference/ado-api/field-object.md), un [parámetro](../../../ado/reference/ado-api/parameter-object.md)o una [propiedad](../../../ado/reference/ado-api/property-object-ado.md). El indicador de tipo de OLE DB correspondiente se muestra entre paréntesis en la columna Descripción de la tabla siguiente.  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|**AdArray**|0x2000|Un valor de marca, siempre combinado con otra constante de tipo de datos, que indica una matriz del otro tipo de datos. No se aplica a ADOX.|  
|**adBigInt**|20|Indica un entero con signo de ocho bytes (DBTYPE_I8).|  
|**adBinary**|128|Indica un valor binario (DBTYPE_BYTES).|  
|**adBoolean**|11|Indica un valor **booleano** (DBTYPE_BOOL).|  
|**adBSTR**|8|Indica una cadena de caracteres terminada en null (Unicode) (DBTYPE_BSTR).|  
|**adChapter**|136|Indica un valor de capítulo de cuatro bytes que identifica las filas de un conjunto de filas secundario (DBTYPE_HCHAPTER).|  
|**adChar**|129|Indica un valor de cadena (DBTYPE_STR).|  
|**adCurrency**|6|Indica un valor de divisa (DBTYPE_CY). Currency es un número de punto fijo con cuatro dígitos a la derecha del separador decimal. Se almacena en un entero con signo de ocho bytes escalado por 10.000.|  
|**adDate**|7|Indica un valor de fecha (DBTYPE_DATE). Una fecha se almacena como un valor de tipo Double, cuya parte entera es el número de días transcurridos desde el 30 de diciembre de 1899 y la parte fraccionaria de la que es la fracción de un día.|  
|**adDBDate**|133|Indica un valor de fecha (AAAAMMDD) (DBTYPE_DBDATE).|  
|**adDBTime**|134|Indica un valor de hora (HHMMSS) (DBTYPE_DBTIME).|  
|**adDBTimeStamp**|135|Indica una marca de fecha y hora (aaaammddhhmmss más una fracción en milmillonésimas) (DBTYPE_DBTIMESTAMP).|  
|**adDecimal**|14|Indica un valor numérico exacto con una precisión y escala fijas (DBTYPE_DECIMAL).|  
|**adDouble**|5|Indica un valor de punto flotante de precisión doble (DBTYPE_R8).|  
|**adEmpty**|0|No especifica ningún valor (DBTYPE_EMPTY).|  
|**adError**|10|Indica un código de error de 32 bits (DBTYPE_ERROR).|  
|**adFileTime**|64|Indica un valor de 64 bits que representa el número de intervalos de 100-nanosegundos desde el 1 de enero de 1601 (DBTYPE_FILETIME).|  
|**adGUID**|72|Indica un identificador único global (GUID) (DBTYPE_GUID).|  
|**adIDispatch**|9|Indica un puntero a una interfaz **IDispatch** en un objeto COM (DBTYPE_IDISPATCH).<br /><br /> **Nota:** Este tipo de datos no es compatible actualmente con ADO. El uso puede producir resultados imprevisibles.|  
|**adInteger**|3|Indica un entero con signo de cuatro bytes (DBTYPE_I4).|  
|**adIUnknown**|13|Indica un puntero a una interfaz **IUnknown** en un objeto COM (DBTYPE_IUNKNOWN).<br /><br /> **Nota:** Este tipo de datos no es compatible actualmente con ADO. El uso puede producir resultados imprevisibles.|  
|**adLongVarBinary**|205|Indica un valor binario largo.|  
|**adLongVarChar**|201|Indica un valor de cadena largo.|  
|**adLongVarWChar**|203|Indica un valor Long de cadena Unicode terminada en NULL.|  
|**adNumeric**|131|Indica un valor numérico exacto con una precisión y escala fijas (DBTYPE_NUMERIC).|  
|**adPropVariant**|138|Indica un PROPVARIANT de automatización (DBTYPE_PROP_VARIANT).|  
|**adSingle**|4|Indica un valor de punto flotante de precisión sencilla (DBTYPE_R4).|  
|**adSmallInt**|2|Indica un entero con signo de dos bytes (DBTYPE_I2).|  
|**adTinyInt**|16|Indica un entero con signo de un byte (DBTYPE_I1).|  
|**adUnsignedBigInt**|21|Indica un entero sin signo de ocho bytes (DBTYPE_UI8).|  
|**adUnsignedInt**|19|Indica un entero sin signo de cuatro bytes (DBTYPE_UI4).|  
|**adUnsignedSmallInt**|18|Indica un entero sin signo de dos bytes (DBTYPE_UI2).|  
|**adUnsignedTinyInt**|17|Indica un entero sin signo de un byte (DBTYPE_UI1).|  
|**adUserDefined**|132|Indica una variable definida por el usuario (DBTYPE_UDT).|  
|**adVarBinary**|204|Indica un valor binario.|  
|**adVarChar**|200|Indica un valor de cadena.|  
|**adVariant**|12|Indica una **variante** de automatización (DBTYPE_VARIANT).<br /><br /> **Nota:** Este tipo de datos no es compatible actualmente con ADO. El uso puede producir resultados imprevisibles.|  
|**adVarNumeric**|139|Indica un valor numérico.|  
|**adVarWChar**|202|Indica una cadena de caracteres Unicode terminada en NULL.|  
|**adWChar**|130|Indica una cadena de caracteres Unicode terminada en null (DBTYPE_WSTR).|  
  
## <a name="adowfc-equivalent"></a>Equivalente de ADO/WFC  
 Paquete: **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums. DataType. ARRAY|  
|AdoEnums. DataType. BIGINT|  
|AdoEnums. DataType. BINARY|  
|AdoEnums. DataType. BOOLEAN|  
|AdoEnums.DataType.BSTR|  
|AdoEnums. DataType. CHAPTER|  
|AdoEnums. DataType. CHAR|  
|AdoEnums. DataType. CURRENCY|  
|AdoEnums. DataType. DATE|  
|AdoEnums. DataType. DBDATE|  
|AdoEnums. DataType. DBTIME|  
|AdoEnums. DataType. DBTIMESTAMP|  
|AdoEnums. DataType. DECIMAL|  
|AdoEnums. DataType. DOUBLE|  
|AdoEnums. DataType. EMPTY|  
|AdoEnums. DataType. ERROR|  
|AdoEnums. DataType. FILETIME|  
|AdoEnums. DataType. GUID|  
|AdoEnums. DataType. IDISPATCH|  
|AdoEnums. DataType. Integer|  
|AdoEnums. DataType. IUNKNOWN|  
|AdoEnums. DataType. LONGVARBINARY|  
|AdoEnums. DataType. LONGVARCHAR|  
|AdoEnums. DataType. LONGVARWCHAR|  
|AdoEnums. DataType. NUMERIC|  
|AdoEnums. DataType. PROPVARIANT|  
|AdoEnums. DataType. SINGLE|  
|AdoEnums. DataType. SMALLINT|  
|AdoEnums. DataType. TINYINT|  
|AdoEnums. DataType. UNSIGNEDBIGINT|  
|AdoEnums. DataType. UNSIGNEDINT|  
|AdoEnums. DataType. UNSIGNEDSMALLINT|  
|AdoEnums. DataType. UNSIGNEDTINYINT|  
|AdoEnums. DataType. USERDEFINED|  
|AdoEnums. DataType. VARBINARY|  
|AdoEnums. DataType. VARCHAR|  
|AdoEnums. DataType. VARIANT|  
|AdoEnums. DataType. VARNUMERIC|  
|AdoEnums. DataType. VARWCHAR|  
|AdoEnums. DataType. WCHAR|  
  
## <a name="applies-to"></a>Se aplica a  

:::row:::
    :::column:::
        [Append (método) (ADO)](../../../ado/reference/ado-api/append-method-ado.md)  
        [Método CreateParameter (ADO)](../../../ado/reference/ado-api/createparameter-method-ado.md)  
    :::column-end:::
    :::column:::
        [Ejemplo del método CreateRecordset (RDS)](../../../ado/reference/rds-api/createrecordset-method-rds.md)  
        [Tipo (propiedad, ADO)](../../../ado/reference/ado-api/type-property-ado.md)  
    :::column-end:::
:::row-end:::
