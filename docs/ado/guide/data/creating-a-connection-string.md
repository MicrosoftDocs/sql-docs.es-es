---
description: Creación de una cadena de conexión
title: Crear una cadena de conexión | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/20/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- connections [ADO]
- connection strings [ADO]
ms.assetid: 14eae122-2d1e-40c8-b88e-b7cb8dfbc93b
author: rothja
ms.author: jroth
ms.openlocfilehash: eda5438d4fbda93b9e8c19602b9236d1812e6826
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033295"
---
# <a name="creating-a-connection-string"></a>Creación de una cadena de conexión
Una cadena de conexión consta de una lista de pares de argumentos y valores (es decir, parámetros), separados por punto y coma. Por ejemplo:  
  
```syntax
"arg1=val1; arg2=val2; ... argN=valN;"  
```  
  
 Todos los parámetros deben ser reconocidos por ADO o por el proveedor especificado.  
  
 ADO reconoce los cinco argumentos siguientes en una cadena de conexión.  
  
|Argumento|Descripción|  
|--------------|-----------------|  
|*Proveedor*|Especifica el nombre de un proveedor que se va a utilizar para la conexión.|  
|*Nombre de archivo*|Especifica el nombre de un archivo específico del proveedor (por ejemplo, un objeto de origen de datos persistente) que contiene información de conexión preestablecida.|  
|*URL*|Especifica la cadena de conexión como una dirección URL absoluta que identifica un recurso, como un archivo o un directorio.|  
|*Proveedor remoto*|Especifica el nombre de un proveedor que se va a utilizar al abrir una conexión del lado cliente. (Solo el servicio de datos remoto).|  
|*Servidor remoto*|Especifica el nombre de la ruta de acceso del servidor que se va a utilizar al abrir una conexión del lado cliente. (Solo el servicio de datos remoto).|  
  
 Otros argumentos se pasan al proveedor denominado en el argumento del *proveedor* , sin ningún procesamiento por parte de ADO.  
  
 La aplicación HelloData en [HelloData: una aplicación simple de ADO](./hellodata-a-simple-ado-application.md) usó la siguiente cadena de conexión:  
  
```vb
m_sConnStr = "Provider=SQLOLEDB;Data Source=MySqlServer;" & _  
             "Initial Catalog=Northwind;Integrated Security='SSPI';"  
```  
  
 En esta cadena de conexión, ADO solo reconoce el `"Provider=SQLOLEDB"` parámetro, que especifica el proveedor de OLE DB de Microsoft para SQL Server como el origen de datos de ADO. El resto de los pares de argumento/valor, `"Data Source=MySqlServer; Initial Catalog=Northwind;Integrated Security='SSPI';"` , se pasan literalmente a este proveedor. El tipo y la validez de estos parámetros son específicos del proveedor. Para obtener información sobre los parámetros válidos que se pueden pasar en la cadena de conexión, consulte la documentación del proveedor individual.  
  
 Según la documentación del proveedor de OLE DB para SQL Server, puede sustituir "servidor" por el parámetro de *origen de datos* y "base de datos" por el parámetro de *catálogo inicial* . Por lo tanto, la siguiente cadena de conexión produciría resultados idénticos a los anteriores:  
  
```vb
m_sConnStr = "Provider=SQLOLEDB;Server=MySqlServer;" & _  
             "Database=Northwind;Integrated Security='SSPI';"  
```