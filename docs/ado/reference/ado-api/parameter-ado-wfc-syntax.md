---
description: Parámetro (ADO - sintaxis WFC)
title: Parámetro (ADO-sintaxis WFC) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 02/15/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Parameter collection [ADO], ADO/WFC syntax
ms.assetid: d00d1e1e-14b1-41a2-a00f-2a3cb7396f15
author: rothja
ms.author: jroth
ms.openlocfilehash: 0a8175c2510cfcd977a0144b15f510e1061dc4f7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041175"
---
# <a name="parameter-ado---wfc-syntax"></a>Parámetro (ADO - sintaxis WFC)
## <a name="package-commswfcdata"></a>paquete com. ms. wfc. Data  
  
### <a name="constructor"></a>Constructor  
  
```  
public Parameter()  
public Parameter(String name)  
public Parameter(String name, int type)  
public Parameter(String name, int type, int dir)  
public Parameter(String name, int type, int dir, int size)  
public Parameter(String name, int type, int dir, int size, Object value)  
```  
  
### <a name="methods"></a>Métodos  
  
```  
public void appendChunk(byte[] bytes)  
public void appendChunk(char[] chars)  
public void appendChunk(String chars)  
```  
  
### <a name="properties"></a>Propiedades  
  
```  
public int getAttributes()  
public void setAttributes(int attr)  
public int getDirection()  
public void setDirection(int dir)  
public String getName()  
public void setName(String name)  
public int getNumericScale()  
public void setNumericScale(int scale)  
public int getPrecision()  
public void setPrecision(int prec)  
public int getSize()  
public void setSize(int size)  
public int getType()  
public void setType(int type)  
public com.ms.com.Variant getValue()  
public void setValue(Object v)  
public AdoProperties getProperties()  
```  
  
## <a name="parameter-accessor-methods"></a>Métodos de descriptor de acceso  
 La propiedad [Value](./value-property-ado.md) de un objeto de [parámetro](./parameter-object.md) obtiene o establece el contenido de ese objeto. El contenido se representa como una variante, un tipo de objeto al que se puede asignar un valor y cualquiera de los distintos tipos de datos.  
  
 ADO/WFC implementa la propiedad **Value** con el método **GetValue** , que devuelve un objeto Variant. y el método **SetValue** , que toma una variante como argumento. Las variantes son muy eficaces en algunos lenguajes, como Microsoft Visual Basic.  
  
 Además de la propiedad **Value** , ADO/wfc proporciona métodos de *descriptor de acceso* que utilizan tipos de datos de Java para obtener y establecer el contenido de los objetos de **parámetro** . La mayoría de estos métodos tienen nombres con el formato **Get**_DataType_ o **set**_DataType_.  
  
 Hay una excepción notable: no hay ninguna propiedad **getNull** ; en su lugar, hay una propiedad **IsNull** que devuelve un valor booleano que indica si el campo es NULL.  
  
```  
public boolean getBoolean()  
public void setBoolean(boolean v)  
public byte getByte()  
public void setByte(byte v)  
public double getDouble()  
public void setDouble(double v)  
public float getFloat()  
public void setFloat(float v)  
public int getInt()  
public void setInt(int v)  
public long getLong()  
public void setLong(long v)  
public short getShort()  
public void setShort(short v)  
public String getString()  
public void setString(String v)  
public boolean isNull()  
public void setNull()  
```  
  
## <a name="see-also"></a>Consulte también  
 [Objeto Parameter](./parameter-object.md)