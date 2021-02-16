---
title: Administrador de conexiones de Azure Data Lake Analytics | Microsoft Docs
description: Un paquete de SQL Server Integration Services (SSIS) puede usar el administrador de conexiones de Azure Data Lake Analytics para conectarse a una cuenta de Data Lake Analytics.
ms.custom: ''
ms.date: 05/18/2018
ms.prod: sql
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- SQL13.DTS.DESIGNER.AFPADLSCM.F1
- sql14.dts.designer.afpadlscm.f1
ms.assetid: f4c44553-0f08-4731-ac47-7534990b8c8d
author: yanancai
ms.author: yanacai
ms.reviewer: maghan
ms.openlocfilehash: ff7ad8e742469f7afea8770a7323352e99ee41ff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338452"
---
# <a name="azure-data-lake-analytics-connection-manager"></a>Administrador de conexiones de Azure Data Lake Analytics

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Un paquete de SQL Server Integration Services (SSIS) puede usar el administrador de conexiones de Azure Data Lake Analytics para conectarse a una cuenta de Data Lake Analytics con uno de los dos tipos de autenticación siguientes:
-   Identidad de usuario de Azure Active Directory (Azure AD)
-   Identidad de servicio de Azure AD 

El administrador de conexiones de Data Lake Analytics es un componente de [Feature Pack de SQL Server Integration Services (SSIS) para Azure](../../integration-services/azure-feature-pack-for-integration-services-ssis.md).
 
## <a name="configure-the-connection-manager"></a>Configuración del administrador de conexiones

1. En el cuadro de diálogo **Agregar administrador de conexiones SSIS**, seleccione **AzureDataLakeAnalytics** > **Agregar**. Se abrirá el cuadro de diálogo **Azure Data Lake Analytics Connection Manager Editor** (Editor del administrador de conexiones de Azure Data Lake Analytics).
  
2. En el cuadro de diálogo **Azure Data Lake Analytics Connection Manager Editor** (Editor del administrador de conexiones de Azure Data Lake Analytics), en el campo **ADLA Account Name** (Nombre de la cuenta de ADLA), escriba el nombre de la cuenta de Data Lake Analytics. Por ejemplo: myadlaaccountname.
  
3. En el campo **Autenticación**, elija el tipo de autenticación adecuado para tener acceso a los datos de Data Lake Analytics.

   a. Si seleccionó la opción de autenticación **Identidad de usuario de Azure AD**:
   
      i. Proporcione los valores para los campos **Nombre de usuario** y **Contraseña**.    
      ii. Seleccione **Probar conexión** para probar la conexión. Si usted o el administrador de inquilinos no permitió previamente que SSIS obtuviera acceso a la cuenta de Data Lake Analytics, seleccione **Aceptar** cuando se le solicite. Para obtener más información sobre esta experiencia del consentimiento, consulte [Integración de aplicaciones con Azure Active Directory](/azure/active-directory/manage-apps/plan-an-application-integration#integrating-applications-with-azure-ad).
    
   > [!NOTE] 
   > Al seleccionar la opción de autenticación **Identidad de usuario de Azure AD**, no se admitirá la autenticación multifactor ni la autenticación de la cuenta de Microsoft.
    
   b. Si seleccionó la opción de autenticación **Identidad de servicio de Azure AD**:
   
      i. Cree una entidad de servicio y una aplicación de Azure AD para tener acceso a la cuenta de Data Lake Analytics. Para obtener más información sobre esta opción de autenticación, consulte [Use portal to create Active Directory application and service principal that can access resources](/azure/azure-resource-manager/resource-group-create-service-principal-portal)(Utilizar el portal para crear una aplicación de Active Directory y una entidad de servicio que puedan tener acceso a los recursos).    
      ii. Asigne los permisos adecuados para permitir que esta aplicación de Azure AD tenga acceso a la cuenta de Data Lake Analytics. Obtenga información sobre cómo conceder permisos a la cuenta de Data Lake Analytics mediante el [Asistente para agregar usuario](/azure/data-lake-analytics/data-lake-analytics-manage-use-portal#add-a-new-user).    
      iii. Proporcione valores para los campos **Id. de la aplicación**, **Clave de autenticación** e **Identificador de inquilino**.    
      iv. Seleccione **Probar conexión** para probar la conexión.  

4. Seleccione **Aceptar** para cerrar el cuadro de diálogo **Azure Data Lake Analytics Connection Manager Editor** (Editor del administrador de conexiones de Azure Data Lake Analytics).  

## <a name="view-the-properties-of-the-connection-manager"></a>Ver las propiedades del administrador de conexiones
Puede ver las propiedades del Administrador de conexiones que creó en la ventana **Propiedades** .  
  
