---
description: Marcado de objetos de negocios como seguros para scripting
title: Marcar objetos comerciales como seguros para el scripting | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- business objects in RDS [ADO]
ms.assetid: 0be98d1a-ab3d-4dce-a166-dacda10d154a
author: rothja
ms.author: jroth
ms.openlocfilehash: 1866d72f438e263ec6cdb1f66cccb247d460a270
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031927"
---
# <a name="marking-business-objects-as-safe-for-scripting"></a>Marcado de objetos de negocios como seguros para scripting
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
 Para ayudar a garantizar un entorno de Internet seguro, debe marcar cualquier objeto empresarial del que se haya creado una instancia con [RDS. ](../../reference/rds-api/dataspace-object-rds.md) Método [CreateObject](../../reference/rds-api/createobject-method-rds.md) del objeto DataSpace como "Safe for scripting". Debe asegurarse de que estén marcados como tales en el área de licencia del registro del sistema antes de que se puedan usar en DCOM.  
  
> [!NOTE]
>  Se puede crear una instancia de los objetos de negocios marcados como "seguros para scripting" o seguros para la inicialización a través de la red. Marcar un objeto comercial como "seguro para scripting" no hace que sea seguro. Es importante asegurarse de que los objetos empresariales se codifican con la mayor seguridad para asegurarse de que dichos objetos no presenten un punto de acceso no protegido para datos confidenciales.  
  
 Para marcar manualmente el objeto comercial como seguro para el scripting, cree un archivo de texto con una extensión. reg que contenga el texto siguiente. En este ejemplo, \<*MyActiveXGUID*> es el número GUID hexadecimal del objeto comercial. Los dos números siguientes habilitan la característica Safe-for-scripting:  
  
```console
[HKEY_CLASSES_ROOT\CLSID\<MyActiveXGUID>\Implemented   
Categories\{7DD95801-9882-11CF-9FA9-00AA006C42C4}]  
[HKEY_CLASSES_ROOT\CLSID\<MyActiveXGUID>\Implemented   
Categories\{7DD95802-9882-11CF-9FA9-00AA006C42C4}]  
```  
  
 Guarde el archivo y fusione en el registro mediante el editor del registro o haga doble clic en el archivo. reg en el explorador de Windows.  
  
 Los objetos comerciales creados en Microsoft Visual Basic se pueden marcar automáticamente como "seguros para scripting" con el Asistente para la implementación y el paquete. Cuando el asistente le pida que especifique la configuración de seguridad, seleccione **seguro para inicialización** y **seguro para scripting**.  
  
 En el último paso, el Asistente para la instalación de la aplicación crea un archivo. htm y un archivo. cab. Después, puede copiar estos dos archivos en el equipo de destino y hacer doble clic en el archivo. htm para cargar la página y registrar correctamente el servidor.  
  
 Dado que el objeto comercial se instalará en el directorio Windows\System32\Occache de forma predeterminada, muévalo al directorio Windows\System32 y cambie la **\\** \<*MyActiveXGUID*> \\ clave del registroHKEY_CLASSES_ROOT\CLSID **InProcServer32** para que coincida con la ruta de acceso correcta.