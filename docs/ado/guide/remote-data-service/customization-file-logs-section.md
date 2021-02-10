---
description: Sección de registros del archivo de personalización
title: Sección de registros de archivo de personalización | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- logs section in RDS [ADO]
- customization file in RDS [ADO]
ms.assetid: a368e264-865c-41ee-be00-d9097255c2ea
author: rothja
ms.author: jroth
ms.openlocfilehash: 34485c91a20b8c5df668e5eddd11e442418844a9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036535"
---
# <a name="customization-file-logs-section"></a>Sección de registros del archivo de personalización
La sección de **registros** contiene una entrada de archivo de registro, que especifica el nombre de un archivo que registra los errores durante la operación de la **factoría** de archivos.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintaxis  
 Una entrada de archivo de registro tiene el formato:  
  
```console
  
err=  
FileName  
  
```  
  
## <a name="remarks"></a>Observaciones  
  
|Parte|Descripción|  
|----------|-----------------|  
|**ERR**|Cadena literal que indica que se trata de una entrada de archivo de registro.|  
|*FileName*|Ruta de acceso y nombre de archivo completos. El nombre de archivo típico es **c:\msdfmap.log**.|  
  
 El archivo de registro contendrá el nombre de usuario, HRESULT, fecha y hora de cada error.  
  
## <a name="see-also"></a>Consulte también  
 [Sección de conexión del archivo de personalización](./customization-file-connect-section.md)   
 [Sección SQL de archivo de personalización](./customization-file-sql-section.md)   
 [Sección UserList del archivo de personalización](./customization-file-userlist-section.md)   
 [Personalización de DataFactory](./datafactory-customization.md)   
 [Configuración de cliente requerida](./required-client-settings.md)   
 [Descripción del archivo de personalización](./understanding-the-customization-file.md)   
 [Escritura de un controlador personalizado](./writing-your-own-customized-handler.md)