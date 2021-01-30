---
description: Objeto DataFactory (RDSServer)
title: Objeto DataFactory (RDSServer) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- DataFactory object [ADO]
ms.assetid: e75240c2-b749-471e-b6ea-98cae232efbe
author: rothja
ms.author: jroth
ms.openlocfilehash: 75c727e8c857dcb5c5922f52e56a37e99f223293
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163826"
---
# <a name="datafactory-object-rdsserver"></a>Objeto DataFactory (RDSServer)
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
 Este objeto empresarial de servidor predeterminado implementa métodos que proporcionan acceso a los datos de lectura y escritura a los orígenes de datos especificados para las aplicaciones del lado cliente.  
  
 El objeto **RDSServer. DataFactory** está diseñado como un objeto de automatización de servidor que recibe las solicitudes del cliente. En una implementación de Internet, reside en un servidor Web y se crea una instancia del componente ADISAPI. El objeto **RDSServer. DataFactory** proporciona acceso de lectura y escritura a los orígenes de datos especificados, pero no contiene ninguna lógica de reglas de negocios o de validación.  
  
 Si usa un método que está disponible tanto en **RDSServer. DataFactory** como en [RDS. Objetos DataControl](./datacontrol-object-rds.md) , el servicio de datos remotos utiliza **RDS.** Versión de control de DataControl de forma predeterminada. El valor predeterminado supone un escenario de programación básico, donde **RDSServer. DataFactory** actúa como un objeto de negocio del lado servidor genérico.  
  
 Si desea que la aplicación web controle el procesamiento del lado servidor específico de la tarea, puede reemplazar **RDSServer. DataFactory** por un objeto comercial personalizado.  
  
 Puede crear objetos de negocio del lado servidor que llamen a los métodos **RDSServer. DataFactory** , como [query](./query-method-rds.md) y [CreateRecordset](./createrecordset-method-rds.md). Esto resulta útil si desea agregar funcionalidad a los objetos de negocio, pero puede aprovechar las tecnologías existentes de servicio de datos remotos.  
  
 El objeto **DataFactory** no es seguro para los scripts que se ejecutan en el lado cliente.  
  
 El identificador de clase del objeto **RDSServer. DataFactory** es 9381D8F5-0288-11D0-9501-00AA00B911A5.  
  
 Esta sección contiene el siguiente tema.  
  
-   [Eventos, métodos y propiedades del objeto DataFactory (RDSServer)](./datafactory-object-rdsserver-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Consulte también  
 [Objeto DataFactory, método de consulta y ejemplo del método CreateObject (VBScript)](./datafactory-object-query-method-and-createobject-method-example-vbscript.md)