---
description: Campo (ADO - sintaxis WFC)
title: Field (ADO-sintaxis WFC) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 02/15/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Field collection [ADO], ADO/WFC syntax
ms.assetid: 7e01cb24-2338-4f92-ad46-8d97248e1a4d
author: rothja
ms.author: jroth
ms.openlocfilehash: 2bfcf230d897274c45621dd404c9ed2816946812
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167384"
---
# <a name="field-ado---wfc-syntax"></a>Campo (ADO - sintaxis WFC)
## <a name="package-commswfcdata"></a>paquete com. ms. wfc. Data  
  
### <a name="methods"></a>Métodos  
  
```  
public void appendChunk(byte[] bytes)  
public void appendChunk(char[] chars)  
public void appendChunk(String chars)  
public byte[] getByteChunk(int len)  
public char[] getCharChunk(int len)  
public String getStringChunk(int len)  
```  
  
### <a name="properties"></a>Propiedades  
  
```  
public int getActualSize()  
public int getAttributes()  
public void setAttributes(int pl)  
public com.ms.com.IUnknown getDataFormat()  
public void setDataFormat(com.ms.com.IUnknown format)  
```  
  
 (Para obtener más información, vea la documentación de la interfaz com. ms. wfc. Data. IDataFormat).  
  
```  
public int getDefinedSize()  
public void setDefinedSize(int pl)  
public String getName()  
public int getNumericScale()  
public void setNumericScale(byte pbNumericScale)  
public Variant getOriginalValue()  
public int getPrecision()  
public void setPrecision(byte pbPrecision)  
public int getType()  
public void setType(int pDataType)  
public Variant getUnderlyingValue()  
public Variant getValue()  
public void setValue(Variant value)  
public AdoProperties getProperties()  
```  
  
### <a name="field-accessor-methods"></a>Métodos descriptores de acceso de campo  
 La propiedad [Value](../../../ado/reference/ado-api/value-property-ado.md) de un objeto de [campo](../../../ado/reference/ado-api/field-object.md) obtiene o establece el contenido de ese objeto. El contenido se representa como una variante, un tipo de objeto al que se puede asignar un valor y cualquiera de los distintos tipos de datos.  
  
 ADO/WFC implementa la propiedad **Value** con el método **GetValue** , que devuelve un objeto Variant. y el método **SetValue** , que toma una variante como argumento. Las variantes son muy eficaces en algunos lenguajes, como Microsoft Visual Basic.  
  
 Además de la propiedad **Value** , ADO/wfc proporciona métodos de *descriptor de acceso* que utilizan tipos de datos de Java para obtener y establecer el contenido de los objetos de **campo** . La mayoría de estos métodos tienen nombres con el formato **Get**_DataType_ o **set**_DataType_.  
  
 Hay dos excepciones destacadas: uno de los métodos **GetObject** devuelve un objeto convertido en una clase especificada. No hay ninguna propiedad **getNull** ; en su lugar, hay una propiedad **IsNull** que devuelve un valor booleano que indica si el campo es NULL.  
  
```  
public native boolean getBoolean();  
public void setBoolean(boolean v)  
public native byte getByte();  
public void setByte(byte v)  
public native byte[] getBytes();  
public void setBytes(byte[] v)  
public native double getDouble();  
public void setDouble(double v)  
public native float getFloat();  
public void setFloat(float v)  
public native int getInt();  
public void setInt(int v)  
public native long getLong();  
public void setLong(long v)  
public native short getShort();  
public void setShort(short v)  
public native String getString();  
public void setString(String v)  
public native boolean isNull();  
public void setNull()  
public Object getObject()  
public Object getObject(Class c)  
public void setObject(Object value)  
```  
  
## <a name="see-also"></a>Consulte también  
 [Objeto Field](../../../ado/reference/ado-api/field-object.md)
