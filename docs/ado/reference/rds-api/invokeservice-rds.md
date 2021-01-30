---
description: InvokeService (RDS)
title: InvokeService (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- InvokeService [RDS]
ms.assetid: ad45c676-ec7e-4a3a-9a6b-a54f75eb3012
author: rothja
ms.author: jroth
ms.openlocfilehash: 4d162b0291a19a80ec0133307c785ad3af588aee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168892"
---
# <a name="invokeservice-rds"></a>InvokeService (RDS)
Devuelve un puntero a la interfaz solicitada en una versión más capaz del objeto.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al  [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
object.InvokeService(REFID riid, IUknown* punkNotSoFunctionalInterface, IUknown** ppunkMoreFunctionalInterface) As HRESULT  
```  
  
#### <a name="parameters"></a>Parámetros  
 *riid*  
  
 de Identificador de la interfaz que se solicita.  
  
 *punkNotSoFunctionalInterface*  
  
 de Objeto de origen menos compatible.  
  
 *ppunkMoreFunctionalInterface*  
  
 enuncia Dirección de la variable de puntero que recibe el puntero de interfaz solicitado en *riid*. Cuando la devolución es correcta, el parámetro *ppunkMoreFunctionalInterface* contiene el puntero de interfaz solicitado al objeto. Si el objeto no admite la interfaz especificada en *riid*, *ppunkMoreFunctionalInterface* se establece en NULL.  
  
## <a name="return-value"></a>Valor devuelto  
 Un valor HRESULT que indica si la llamada al método **InvokeService** se realizó correctamente.  
  
## <a name="remarks"></a>Observaciones  
 La implementación del motor de cursor de RDS de **InvokeService** toma el conjunto de filas de entrada (o varios objetos de resultados), rellena el motor de cursor desde el conjunto de filas de entrada y, a continuación, devuelve un puntero a sí mismo.  
  
## <a name="applies-to"></a>Se aplica a  
 [Interfaz IRDSService (RDS)](./irdsservice-interface-rds.md)  
  
## <a name="see-also"></a>Consulte también  
 [Métodos RDS](./rds-methods.md)