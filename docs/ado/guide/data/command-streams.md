---
description: Secuencias de comandos
title: Secuencias de comandos | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- command streams [ADO]
- streams [ADO], command
ms.assetid: 0ac09dbe-2665-411e-8fbb-d1efe6c777be
author: rothja
ms.author: jroth
ms.openlocfilehash: 1c0872f20754d093020ece2830d389de32f09032
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037745"
---
# <a name="command-streams"></a>Secuencias de comandos
ADO siempre admite la entrada de comandos en el formato de cadena especificado por la propiedad **CommandText** . Como alternativa, con ADO 2,7 o posterior, también puede usar un flujo de información para la entrada de comandos mediante la asignación de la secuencia a la propiedad **CommandStream** . Puede asignar un objeto de **secuencia** de ADO o cualquier objeto que admita la interfaz **IStream** de com.  
  
 El contenido de la secuencia de comandos simplemente se pasa de ADO al proveedor, por lo que el proveedor debe admitir la entrada de comando por secuencia para que esta característica funcione. Por ejemplo, SQL Server admite consultas en forma de plantillas XML o extensiones OpenXML en Transact-SQL.  
  
 Dado que el proveedor debe interpretar los detalles de la secuencia, debe especificar el dialecto del comando estableciendo la propiedad de **dialecto** . El valor de **Dialect** es una cadena que contiene un GUID, que es definido por el proveedor. Para obtener información sobre los valores válidos para el **dialecto** compatible con el proveedor, consulte la documentación del proveedor.  
  
## <a name="xml-template-query-example"></a>Ejemplo de consulta de plantilla XML  
 El siguiente ejemplo está escrito en VBScript en la base de datos Northwind.  
  
 En primer lugar, inicialice y abra el objeto de **secuencia** que se utilizará para contener la secuencia de consulta:  
  
```  
Dim adoStreamQuery  
Set adoStreamQuery = Server.CreateObject("ADODB.Stream")  
adoStreamQuery.Open  
```  
  
 El contenido de la secuencia de consulta será una consulta de plantilla XML.  
  
 La consulta de plantilla requiere una referencia al espacio de nombres XML identificado por el prefijo SQL: de la \<sql:query> etiqueta. Se incluye una instrucción SELECT de SQL como contenido de la plantilla XML y se asigna a una variable de cadena de la siguiente manera:  
  
```  
sQuery = "<ROOT xmlns:sql='urn:schemas-microsoft-com:xml-sql'>  
<sql:query> SELECT * FROM PRODUCTS ORDER BY PRODUCTNAME </sql:query>  
</ROOT>"  
```  
  
 A continuación, escriba la cadena en la secuencia:  
  
```  
adoStreamQuery.WriteText sQuery, adWriteChar  
adoStreamQuery.Position = 0  
```  
  
 Asigne adoStreamQuery a la propiedad **CommandStream** de un objeto **Command** de ADO:  
  
```  
Dim adoCmd  
Set adoCmd  = Server.CreateObject("ADODB.Command"")  
adoCmd.CommandStream = adoStreamQuery  
```  
  
 Especifique el **dialecto** del lenguaje de comandos, que indica cómo el SQL Server proveedor de OLE DB debe interpretar la secuencia de comandos. El dialecto especificado por un GUID específico del proveedor:  
  
```  
adoCmd.Dialect = "{5D531CB2-E6Ed-11D2-B252-00C04F681B71}"  
```  
  
 Por último, ejecute la consulta y devuelva los resultados a un objeto de **conjunto de registros** :  
  
```  
Set objRS = adoCmd.Execute  
```
