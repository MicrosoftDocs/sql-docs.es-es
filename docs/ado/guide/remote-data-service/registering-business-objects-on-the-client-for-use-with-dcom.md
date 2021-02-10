---
description: Registro de objetos de negocios en el cliente para usarlos con DCOM
title: Registro de objetos comerciales en el cliente para su uso con DCOM | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- business objects in RDS [ADO]
ms.assetid: 75a21910-607f-463a-ae18-a17130dafb7e
author: rothja
ms.author: jroth
ms.openlocfilehash: 708a59b8c931b723c8f4142f036849da4965fc13
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031827"
---
# <a name="registering-business-objects-on-the-client-for-use-with-dcom"></a>Registro de objetos de negocios en el cliente para usarlos con DCOM
Los objetos comerciales personalizados deben asegurarse de que el lado cliente puede asignar el nombre del programa (ProgId) a un identificador (CLSID) que se puede usar a través de DCOM. Por esta razón, el ProgID del objeto DCOM debe estar en el registro del lado cliente y asignarse al identificador de clase del objeto comercial del lado servidor. Para los demás protocolos admitidos (HTTP, HTTPS y en proceso), esto no es necesario.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
 Por ejemplo, si expone un objeto empresarial de servidor denominado MyBObj con un identificador de clase específico, por ejemplo, "{00112233-4455-6677-8899-00AABBCCDDEE}", asegúrese de que se agregan las siguientes entradas al registro del lado cliente:  
  
```console
[HKEY_CLASSES_ROOT]  
\MyBObj\Clsid\(Default) "{00112233-4455-6677-8899-00AABBCCDDEE}"  
```