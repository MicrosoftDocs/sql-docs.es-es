---
description: Sección UserList del archivo de personalización
title: Sección UserList del archivo de personalización | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- UserList section in rds [ADO]
- customization file in RDS [ADO]
ms.assetid: 42e8ec20-eaac-4a95-8cb8-4bba93a75bcb
author: rothja
ms.author: jroth
ms.openlocfilehash: 4ca0d2e0a2bff2e5f9a3ea057220ac8cda14c9af
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036515"
---
# <a name="customization-file-userlist-section"></a>Sección UserList del archivo de personalización
La sección **userList** pertenece a la sección **Connect** con el mismo parámetro de *identificador* de sección.  
  
 Esta sección puede contener una *entrada de acceso de usuario*, que especifica derechos de acceso para el usuario especificado e invalida la *entrada de acceso* *predeterminada* en la sección **conexión** coincidente.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintaxis  
 Una entrada de acceso de usuario tiene el siguiente formato:  
  
 _nombre de usuario_**=**   
 **_accessRights_**  
  
|Parte|Descripción|  
|----------|-----------------|  
|*userName*|El *nombre de usuario* de la persona que emplea esta conexión. Los nombres de usuario válidos se establecen con el cuadro de diálogo de **Service Manager** de IIS.|  
|**_accessRights_**|Uno de los siguientes derechos de acceso:<br /><br /> -   **NoAccess** : el usuario no puede tener acceso al origen de datos.<br />-   **ReadOnly** : el usuario puede leer el origen de datos.<br />-   **ReadWrite** : el usuario puede leer o escribir en el origen de datos.|  
  
## <a name="see-also"></a>Consulte también  
 [Sección de conexión del archivo de personalización](./customization-file-connect-section.md)   
 [Sección de registros de archivo de personalización](./customization-file-logs-section.md)   
 [Sección SQL de archivo de personalización](./customization-file-sql-section.md)   
 [Personalización de DataFactory](./datafactory-customization.md)   
 [Configuración de cliente requerida](./required-client-settings.md)   
 [Descripción del archivo de personalización](./understanding-the-customization-file.md)   
 [Escritura de un controlador personalizado](./writing-your-own-customized-handler.md)