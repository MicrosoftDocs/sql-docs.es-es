---
description: Códigos de error de objetos DataControl
title: Códigos de error de DataControl | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- errors [ADO], DataControl
- DataControl errors [ADO]
ms.assetid: 293df9d5-e1a2-406d-9107-07bf7cdc6f96
author: rothja
ms.author: jroth
ms.openlocfilehash: dbcf0b6aef651e90c90df1cb49af2dbe6a29c674
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029439"
---
# <a name="datacontrol-object-error-codes"></a>Códigos de error de objetos DataControl
En la tabla siguiente se muestra el [objeto RDS. ](../../reference/rds-api/datacontrol-object-rds.md) Códigos de error del objeto DataControl. La traducción decimal positiva de los dos bytes inferiores, la traducción decimal negativa del código de error completo y los valores hexadecimales se muestran.

|ActiveX. Códigos de error de DataControl|Number|Descripción|
|---------------------------------|------------|-----------------|
|**IDS_AsyncPending**|4107-2146824175 0x800A1011|No se puede realizar la operación mientras la operación asincrónica está pendiente.|
|**IDS_BadInlineTablegram**|4105-2146824183 0x800A1009|Tablegram insertado incorrecto.|
|**IDS_CantConnect**|4099-2146824189 0x800A1003|No se puede conectar al servidor.|
|**IDS_CantCreateObject**|4100-2146824188 0x800A1004|No se puede crear el objeto comercial.|
|**IDS_CantFindDataspace**|4102-2146824186 0x800A1006|La propiedad DataSpace no es válida.|
|**IDS_CantInvokeMethod**|4101-2146824187 0x800A1005|No se puede invocar el método en el objeto comercial.|
|**IDS_CrossDomainWarning**|4112-2146824170 0x800A1016|Esta página obtiene acceso a los datos de otro dominio. ¿Desea permitir esto? Para evitar este mensaje en Internet Explorer, puede Agregar un sitio web seguro a la zona de sitios de confianza en la pestaña **seguridad** del cuadro de diálogo **Opciones de Internet** .|
|**IDS_InvalidADCClientVersion**|4106-2146824176 0x800A1010|Versión de cliente de RDS no válida: el cliente es más reciente que el servidor.|
|**IDS_INVALIDARG**|5376-2147019520 0x80071500|Uno o varios argumentos no son válidos.|
|**IDS_InvalidBindings**|4097-2146824191 0x800A1001|Error en la propiedad de los enlaces.|
|**IDS_InvalidParam**|4110-2146824172 0x800A1014|Uno o varios argumentos no son válidos.|
|**IDS_NOINTERFACE**|5377-2147019519 0x80071501|No se admite esta interfaz.|
|**IDS_NotReentrant**|4111-2146824171 0x800A1015|No se puede ejecutar la solicitud mientras el controlador de eventos todavía se está procesando.|
|**IDS_ObjectNotSafe**|4103-2146824185 0x800A1007|La configuración de seguridad de este equipo prohíbe la creación del objeto comercial.|
|**IDS_RecordsetNotOpen**|4109-2146824173 0x800A1013|El **conjunto de registros** no está abierto.|
|**IDS_ResetInvalidField**|4108-2146824174 0x800A1012|La columna especificada en **SortColumn** o **FilterColumn** no existe.|
|**IDS_RowsetNotUpdateable**|4104-2146824184 0x800A1008|El conjunto de filas no es actualizable.|
|**IDS_UnexpectedError**|4351-2146823937 0x800A10FF|error inesperado.|
|**IDS_UpdatesFailed**|4098-2146824190 0x800A1002|No se puede actualizar la base de datos.|
|**IDS_URLMONNotFound**|4119-2146824169 0x800A1017|La propiedad de **dirección URL** de DataControl requiere el archivo del sistema Urlmon.dll, que no se encuentra.|

## <a name="see-also"></a>Consulte también
 [Objeto DataControl (RDS)](../../reference/rds-api/datacontrol-object-rds.md)