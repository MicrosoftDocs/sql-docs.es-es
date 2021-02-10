---
description: Ejecución de objetos de negocios en servicios de componentes
title: Ejecutar objetos comerciales en servicios de componentes | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- component services in RDS [ADO]
ms.assetid: 3077d0b6-42d6-4f10-8e5d-42e6204f1109
author: rothja
ms.author: jroth
ms.openlocfilehash: 7129254d609e1bd80853ccf5008527bfbf938c30
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036385"
---
# <a name="running-business-objects-in-component-services"></a>Ejecución de objetos de negocios en servicios de componentes
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
 Los objetos comerciales pueden ser archivos ejecutables (. exe) o bibliotecas de vínculos dinámicos (. dll). La configuración que se usa para ejecutar el objeto comercial depende de si el objeto es un archivo. dll o. exe:  
  
-   Los objetos comerciales creados como archivos. exe se pueden llamar a través de DCOM. Si estos objetos comerciales se usan a través de Internet Information Services (IIS), están sujetos a la serialización de datos adicional, lo que ralentizará el rendimiento del cliente.  
  
-   Los objetos comerciales creados como archivos. dll se pueden usar a través de IIS y, por tanto, también mediante HTTP. También se pueden usar a través de DCOM a través de servicios de componentes, o a través de Microsoft Transaction Server, si usa Windows NT. Los archivos dll de objetos comerciales deberán registrarse en el equipo del servidor IIS para tener acceso a ellos a través de IIS. Para obtener información acerca de cómo configurar un archivo DLL para que se ejecute en DCOM, consulte la sección [habilitar una DLL para que se ejecute en DCOM](./enabling-a-dll-to-run-on-dcom.md).  
  
> [!NOTE]
>  Cuando los objetos comerciales del nivel intermedio se implementan como componentes de servicios de componentes mediante **GetObjectContext**, **SetComplete** y **SetAbort**, los objetos empresariales pueden usar servicios de componentes (o MTS, si usa Windows NT) objetos de contexto para mantener su estado en varias llamadas de cliente. Este escenario es posible con DCOM, que normalmente se implementa entre clientes y servidores de confianza en una intranet. En este caso, el [objeto RDS.](../../reference/rds-api/dataspace-object-rds.md) El objeto DataSpace y el método [CreateObject](../../reference/rds-api/createobject-method-rds.md) en el lado cliente se sustituyen por el objeto de contexto de transacción y el método **CreateInstance** , que proporcionan la interfaz **ITransactionContext** y los implementan los servicios de componentes.  
  
## <a name="see-also"></a>Consulte también  
 [Aspectos básicos de RDS](./rds-fundamentals.md)