---
description: Propiedad de dirección URL (RDS)
title: Propiedad URL (RDS) | Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
apitype: COM
helpviewer_keywords:
- URL property [ADO]
ms.assetid: 8c56b233-1be8-442c-8d0e-a4c96465bc99
author: rothja
ms.author: jroth
ms.openlocfilehash: 1a780d7790568950833cb177f6ba87bd18ebd409
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166071"
---
# <a name="url-property-rds"></a>Propiedad de dirección URL (RDS)
Indica una cadena que contiene una dirección URL relativa o absoluta.  
  
 Puede establecer la propiedad **URL** en tiempo de diseño en la etiqueta de objeto del objeto [DataControl](./datacontrol-object-rds.md) , o en tiempo de ejecución en el código de scripting.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Design time: <PARAM NAME="URL" VALUE="Server">  
Run time: DataControl.URL="Server"  
```  
  
#### <a name="parameters"></a>Parámetros  
 *Server*  
 Valor de **cadena** que contiene una dirección URL válida.  
  
 *DataControl*  
 Variable de objeto que representa un objeto **DataControl** .  
  
## <a name="remarks"></a>Observaciones  
 Normalmente, la dirección URL identifica un archivo de página de Active Server (. asp) que puede generar y devolver un [conjunto de registros](../ado-api/recordset-object-ado.md). Por lo tanto, el usuario puede obtener un **conjunto de registros** sin tener que invocar el objeto [DataFactory](./datafactory-object-rdsserver.md) del servidor o programar un objeto comercial personalizado.  
  
 Si se ha establecido la propiedad **URL** , [SubmitChanges](./submitchanges-method-rds.md) enviará los cambios a la ubicación especificada por la dirección URL.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto DataControl (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de la propiedad de dirección URL (VBScript)](./url-property-example-vbscript.md)