---
description: Administrador de conexiones OData
title: Administrador de conexiones OData | Microsoft Docs
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.custom: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 3caa4372-aff3-4c0f-9ecd-97870948b8d0
f1_keywords:
- sql13.dts.designer.odatasource.connectionmanager.f1
- sql13.dts.designer.odataconnectionmanager.f1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7d02fbd8a7029a794b933053bec5f676646eced1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338424"
---
# <a name="odata-connection-manager"></a>Administrador de conexiones OData

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


 Conéctese a un origen de datos OData con un administrador de conexiones OData. Un componente de origen OData usa un administrador de conexiones OData para conectarse a un origen de datos OData y consumir datos del servicio. Para obtener más información, vea [OData Source](../../integration-services/data-flow/odata-source.md).  
  
## <a name="adding-an-odata-connection-manager-to-an-ssis-package"></a>Agregar un administrador de conexiones ODATA a un paquete SSIS  
 Hay tres formas de agregar un nuevo administrador de conexiones OData a un paquete SSIS:  
  
-   Haga clic en el botón **Nuevo…** en el **Editor de origen de OData**.  
  
-   Haga clic con el botón derecho en la carpeta **Administradores de conexiones** en el **Explorador de soluciones** y, después, haga clic en **Nuevo administrador de conexiones**. Seleccione **ODATA** en **Tipo de administrador de conexión**.  
  
-   Haga clic con el botón derecho en el panel **Administradores de conexiones** en la parte inferior del diseñador de paquetes y seleccione **Nueva conexión**. Seleccione **ODATA** en **Tipo de administrador de conexión**.  
  
## <a name="connection-manager-authentication"></a>Autenticación del administrador de conexiones  
 El administrador de conexiones OData admite cinco modos de autenticación.  
  
-   Autenticación de Windows  
  
-   Autenticación básica (con el nombre de usuario y contraseña)  

-   Microsoft Dynamics AX Online (con nombre de usuario y contraseña)
  
-   Microsoft Dynamics CRM Online (con nombre de usuario y contraseña)
  
-   Microsoft Online Services (con nombre de usuario y contraseña)  
  
Para el acceso anónimo, seleccione la opción Autenticación de Windows.  

Para conectarse a Microsoft Dynamics AX Online o Microsoft Dynamics CRM Online, no puede usar la opción de autenticación **Microsoft Online Services**. Tampoco podrá usar ninguna opción que esté configurada para la autenticación multifactor. En este momento no se admite la autenticación moderna. 
  
### <a name="specifying-and-securing-credentials"></a>Especificar y proteger las credenciales  
 Si el servicio OData requiere autenticación básica, puede especificar un nombre de usuario y una contraseña en el [Editor del administrador de conexiones OData](). Los valores que especifica en el editor se conservan en el paquete. El valor de contraseña se cifra según el nivel de protección del paquete.  
  
 Hay varias maneras de parametrizar los valores de nombre de usuario y contraseña o almacenarlos fuera del paquete. Por ejemplo, puede usar parámetros o definir las propiedades del administrador de conexiones directamente al ejecutar el paquete desde SQL Server Management Studio.  
  
## <a name="odata-connection-manager-properties"></a>Propiedades del administrador de conexiones OData  
 En la lista siguiente se describen algunas de las propiedades del administrador de conexiones OData.  
  
|Propiedad|Descripción|  
|-|-|  
|Url|Dirección URL al documento de servicio.|  
|UserName|Nombre de usuario que se usará para la autenticación, si es necesario.|  
|Contraseña|Contraseña que se usará para la autenticación, si es necesario.|  
|ConnectionString|Incluye otras propiedades del administrador de conexiones.|  
  
## <a name="odata-connection-manager-editor"></a>Editor del administrador de conexiones OData
  Use el cuadro de diálogo **Editor del administrador de conexiones OData** para agregar una conexión o editar una conexión existente con un origen de datos OData.  
  
### <a name="options"></a>Opciones  
 **Nombre del administrador de conexiones**  
 Nombre del administrador de conexiones.  
  
 **Ubicación de documento de servicio**  
 Dirección URL del servicio OData. Por ejemplo: https://services.odata.org/V3/Northwind/Northwind.svc/.  
  
 **Autenticación**  
Seleccione una de las siguientes opciones:
-   **Autenticación de Windows**. Para obtener de acceso anónimo, seleccione esta opción.
-   **Autenticación básica** 
-   **Microsoft Dynamics AX Online** para Dynamics AX Online
-   **Microsoft Dynamics CRM Online** para Dynamics CRM Online
-   **Microsoft Online Services** para Microsoft Online Services

Si selecciona una opción distinta de la autenticación de Windows, escriba el **nombre de usuario** y la **contraseña**. 

Para conectarse a Microsoft Dynamics AX Online o Microsoft Dynamics CRM Online, no puede usar la opción de autenticación **Microsoft Online Services**. Tampoco podrá usar ninguna opción que esté configurada para la autenticación multifactor.

 **Probar conexión**  
 Haga clic en este botón para probar la conexión con el origen OData.