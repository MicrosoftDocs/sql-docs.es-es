---
description: Direcciones URL absolutas y relativas
title: Direcciones URL absolutas y relativas | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- relative URLs [ADO]
- absolute URLs [ADO]
- URLs [ADO]
ms.assetid: 6a34a7ef-50cc-4c3d-82f7-106b9a8f3caf
author: rothja
ms.author: jroth
ms.openlocfilehash: 50535a71c1062bc04aa009bea1aab08d3cf1faf0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028700"
---
# <a name="absolute-and-relative-urls"></a>Direcciones URL absolutas y relativas
Una dirección URL especifica la ubicación de un destino almacenado en un equipo local o en red. El destino puede ser un archivo, un directorio, una página HTML, una imagen, un programa, etc.  
  
 Una *dirección URL absoluta* contiene toda la información necesaria para buscar un recurso.  
  
 Una *dirección URL relativa* localiza un recurso mediante una dirección URL absoluta como punto de partida. En efecto, la "dirección URL completa" del destino se especifica mediante la concatenación de las direcciones URL absolutas y relativas.  
  
 Una *dirección URL absoluta* usa el siguiente formato: *Scheme://Server/path/Resource*  
  
 Normalmente, una dirección URL relativa solo consta de la *ruta de acceso* y, opcionalmente, del *recurso*, pero no de ningún *esquema* o *servidor*. En las tablas siguientes se definen las partes individuales del formato de dirección URL completo.  
  
 *scheme*  
 Especifica cómo se debe tener acceso al *recurso* .  
  
 *servidor*  
 Especifica el nombre del equipo donde se encuentra el *recurso* .  
  
 *path*  
 Especifica la secuencia de directorios que conduce al destino. Si se omite el *recurso* , el destino es el último directorio de la *ruta de acceso*.  
  
 *resource*  
 Si se incluye, el *recurso* es el destino y suele ser el nombre de un archivo. Puede tratarse de un *archivo simple* que contiene un solo flujo binario de bytes o un *documento estructurado,* que contiene uno o varios almacenamientos y secuencias binarias de bytes.  
  
## <a name="url-scheme-registration"></a>Registro de esquema de URL  
 Si un proveedor admite direcciones URL, el proveedor registrará uno o varios esquemas de dirección URL. El registro significa que todas las direcciones URL que usen el esquema invocarán automáticamente al proveedor registrado. Por ejemplo, el esquema *http* está registrado en el [proveedor de Microsoft OLE DB para la publicación en Internet](../appendixes/microsoft-ole-db-provider-for-internet-publishing.md). ADO supone que todas las direcciones URL que tienen el prefijo "http" representan las carpetas o los archivos Web que se van a usar con el proveedor de publicación en Internet. Para obtener información sobre los esquemas registrados por el proveedor, consulte la documentación del proveedor.  
  
## <a name="defining-context-with-a-url"></a>Definir el contexto con una dirección URL  
 Una función de una conexión abierta, representada por un objeto de [conexión](../../reference/ado-api/connection-object-ado.md) , consiste en restringir las operaciones subsiguientes al origen de datos representado por esa conexión. Es decir, la conexión define el contexto de las operaciones posteriores.  
  
 Con ADO 2,7 o posterior, una dirección URL absoluta también puede definir un contexto. Por ejemplo, cuando se abre un objeto de [registro](../../reference/ado-api/record-object-ado.md) con una dirección URL absoluta, se crea un objeto de **conexión** implícitamente para representar el recurso especificado por la dirección URL.  
  
 Se puede especificar una dirección URL absoluta que define un contexto en el parámetro *ActiveConnection* del método [Open](../../reference/ado-api/open-method-ado-record.md) del objeto **Record** . También se puede especificar una dirección URL absoluta como el valor de la palabra clave "URL =" en el parámetro *ConnectionString* del método [Open](../../reference/ado-api/open-method-ado-connection.md) del objeto de **conexión** y el parámetro *ActiveConnection* del método [Open](../../reference/ado-api/open-method-ado-recordset.md) del objeto de [conjunto de registros](../../reference/ado-api/recordset-object-ado.md) .  
  
 También se puede definir el contexto abriendo un **registro** o un objeto de **conjunto de registros** que represente un directorio, ya que estos objetos ya tienen un objeto de **conexión** implícita o explícitamente declarado que especifica el contexto.  
  
## <a name="scoped-operations"></a>Operaciones con ámbito  
 El contexto también define el ámbito, es decir, el directorio y sus subdirectorios que pueden participar en operaciones posteriores. El objeto de **registro** tiene varios métodos con ámbito que operan en un directorio y todos sus subdirectorios. Estos métodos incluyen [CopyRecord](../../reference/ado-api/copyrecord-method-ado.md), [MoveRecord](../../reference/ado-api/moverecord-method-ado.md)y [DeleteRecord](../../reference/ado-api/deleterecord-method-ado.md).  
  
## <a name="relative-urls-as-command-text"></a>Direcciones URL relativas como texto de comando  
 Puede especificar que se ejecute un comando en el origen de datos escribiendo una cadena en el parámetro *CommandText* del método [Execute](../../reference/ado-api/execute-method-ado-connection.md) del objeto **Connection** y en el parámetro *source* del método [Open](../../reference/ado-api/open-method-ado-recordset.md) del objeto de **conjunto de registros** .  
  
 Se puede especificar una dirección URL relativa en el parámetro *CommandText* o *source* . La dirección URL relativa no representa realmente un comando, como un comando SQL. simplemente especifica los parámetros. El contexto de la conexión activa debe ser una dirección URL absoluta y el parámetro *Option* debe establecerse en **adCmdTableDirect**.  
  
 Por ejemplo, en el ejemplo de código siguiente se muestra cómo abrir un **conjunto de registros** en el archivo Readme25.txt del directorio WinNT/system32:  
  
```  
recordset.Open "system32/Readme25.txt", "URL=https://YourServer/Winnt/",,,adCmdTableDirect  
```  
  
 La dirección URL absoluta en la cadena de conexión especifica el servidor ( `YourServer` ) y la ruta de acceso ( `Winnt` ). Esta dirección URL también define el contexto.  
  
 La dirección URL relativa en el texto del comando utiliza la dirección URL absoluta como punto de partida y especifica el resto de la ruta de acceso ( `system32` ) y el archivo que se va a abrir ( `Readme25.txt` ).  
  
 El campo de opciones ( `adCmdTableDirect` ) indica que el tipo de comando es una dirección URL relativa.  
  
 Como otro ejemplo, el código siguiente abrirá un **conjunto de registros** en el contenido del `Winnt` Directorio:  
  
```  
recordset.Open "", "URL=https://YourServer/Winnt/",,,adCmdTableDirect  
```  
  
## <a name="ole-db-provider-supplied-url-schemes"></a>OLE DB Provider-Supplied esquemas de dirección URL  
 La parte inicial de una dirección URL completa es el *esquema* que se usa para tener acceso al recurso identificado por el resto de la dirección URL. Algunos ejemplos son HTTP (Protocolo de transferencia de hipertexto) y FTP (protocolo de transferencia de archivos).  
  
 ADO admite proveedores de OLE DB que reconocen sus propios esquemas de dirección URL. Por ejemplo, el [proveedor de Microsoft OLE DB para la publicación en Internet](../appendixes/microsoft-ole-db-provider-for-internet-publishing.md)*,* que tiene acceso a los archivos de Windows 2000 "publicados", reconoce el esquema http existente.  
  
## <a name="see-also"></a>Consulte también  
 [Connection (objeto) (ADO)](../../reference/ado-api/connection-object-ado.md)   
 [Objeto Record (ADO)](../../reference/ado-api/record-object-ado.md)   
 [Objeto de conjunto de registros (ADO)](../../reference/ado-api/recordset-object-ado.md)