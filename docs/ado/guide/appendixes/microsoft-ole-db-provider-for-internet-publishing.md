---
description: Información general de Microsoft OLE DB Provider for Internet Publishing
title: Proveedor de Microsoft OLE DB para la publicación en Internet | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- OLE DB provider for Internet publishing [ADO]
- providers [ADO], OLE DB provider for Internet publishing
- Internet Publishing provider [ADO]
ms.assetid: 66a208d9-b580-4655-a41e-1d36e5b5bfca
author: rothja
ms.author: jroth
ms.openlocfilehash: e769310609ebae9c7df8cfe0d518acce058d32e7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029289"
---
# <a name="microsoft-ole-db-provider-for-internet-publishing-overview"></a>Información general de Microsoft OLE DB Provider for Internet Publishing
El proveedor de Microsoft OLE DB para la publicación en Internet permite a ADO obtener acceso a los recursos servidos por Microsoft FrontPage o Microsoft Internet Information Server. Los recursos incluyen archivos de origen web como archivos HTML o carpetas Web de Windows 2000.

## <a name="connection-string-parameters"></a>Parámetros de la cadena de conexión
 Para conectarse a este proveedor, establezca el argumento de *proveedor* de la propiedad [ConnectionString](../../reference/ado-api/connectionstring-property-ado.md) en:

```vb
MSDAIPP.DSO
```

 Este valor también se puede establecer o leer mediante la propiedad [Provider](../../reference/ado-api/provider-property-ado.md) .

## <a name="typical-connection-string"></a>Cadena de conexión típica
 Una cadena de conexión típica para este proveedor es:

```vb
"Provider=MSDAIPP.DSO;Data Source=ResourceURL;User ID=MyUserID;Password=MyPassword;"
```

 o bien

```vb
"URL=ResourceURL;User ID=MyUserID;Password=MyPassword;"
```

 La cadena consta de estas palabras clave:

|Palabra clave|Descripción|
|-------------|-----------------|
|**Proveedor**|Especifica el proveedor de OLE DB para la publicación en Internet.|
|**Origen de datos** o **URL**|Especifica la dirección URL de un archivo o directorio publicado en una carpeta Web.|
|**Id. de usuario**|Especifica el nombre de usuario.|
|**Contraseña**|Especifica la contraseña del usuario.|

> [!NOTE]
>  Si se va a conectar a un proveedor de origen de datos que admite la autenticación de Windows, debe especificar **Trusted_Connection = Yes** o **Integrated Security = SSPI** en lugar de la información de identificador de usuario y contraseña en la cadena de conexión.

 Si establece el valor de *ResourceURL* de la "dirección URL =" en la cadena de conexión en un valor no válido, el proveedor de publicación de Internet, de forma predeterminada, genera un cuadro de diálogo para solicitar un valor válido. Se trata de un comportamiento no deseado para un componente en el nivel intermedio de una aplicación, porque suspende la ejecución del programa hasta que el cuadro de diálogo se desactiva y el cliente parece inmovilizarse porque no ha recibido una respuesta del componente.

> [!NOTE]
>  Si MSDAIPP. DSO se especifica explícitamente como el valor del proveedor, ya sea con la palabra clave de la cadena de conexión del *proveedor* o la propiedad del **proveedor** , no se puede usar "URL =" en la cadena de conexión. Si lo hace, se producirá un error. En su lugar, solo tiene que especificar la dirección URL tal como se muestra en el tema [uso de ADO con el proveedor de OLE DB para la publicación en Internet](../data/the-ole-db-provider-for-internet-publishing.md).

## <a name="see-also"></a>Consulte también
 [Escenario de publicación en Internet](../data/internet-publishing-scenario.md) [del proveedor de OLE DB para la publicación en Internet](../data/the-ole-db-provider-for-internet-publishing.md)