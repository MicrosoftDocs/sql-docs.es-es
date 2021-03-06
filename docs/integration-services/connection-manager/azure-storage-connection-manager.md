---
title: Administrador de conexiones de Azure Storage | Microsoft Docs
description: El administrador de conexiones de Azure Storage permite que un paquete SSIS se conecte a una cuenta de Azure Storage.
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.afpstorageconn.f1
- sql14.dts.designer.afpstorageconn.f1
ms.assetid: 68bd1d04-d20f-4357-a34e-7c9c76457062
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 44193053e6a5f09b2864b95ded9c5ac933c4cf95
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91726026"
---
# <a name="azure-storage-connection-manager"></a>Administrador de conexiones de Azure Storage

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

El administrador de conexiones de Azure Storage permite que un paquete de SQL Server Integration Services (SSIS) se conecte a una cuenta de Azure Storage. El administrador de conexiones es un componente de [Feature Pack de SQL Server Integration Services (SSIS) para Azure](../../integration-services/azure-feature-pack-for-integration-services-ssis.md). 
  
En el cuadro de diálogo **Agregar administrador de conexiones SSIS**, seleccione **AzureStorage**y haga clic en  > **Agregar**.  
  
Están disponibles las propiedades siguientes:

- **Servicio:** especifica el servicio de almacenamiento al que conectarse.
- **Nombre de cuenta:** especifica el nombre de la cuenta de almacenamiento.
- **Autenticación:** especifica el método de autenticación que se va a utilizar. Se admite la autenticación AccessKey, ServicePrincipal y SharedAccessSignature.
    - **AccessKey:** con este método de autenticación, especifique el valor de **Clave de cuenta**.
    - **ServicePrincipal:** con este método de autenticación, especifique los valores de **Id. de la aplicación**, **Clave de aplicación** e **Id. de inquilino** de la entidad de servicio.
      Para que la **conexión de prueba** funcione, se debe asignar al menos el rol **Lector de datos de Storage Blob** a la entidad de servicio en la cuenta de almacenamiento.
      Para más información, consulte [Conceder acceso a datos de blob y cola de Azure con RBAC en Azure Portal](/azure/storage/common/storage-auth-aad-rbac-portal#assign-rbac-roles-using-the-azure-portal).
    - **SharedAccessSignature:** para este método de autenticación, especifique al menos el **Token** de la firma de acceso compartido.
      Para probar la conexión, especifique además el ámbito del recurso en el que se va a realizar la prueba. Puede ser **Servicio**, **Contenedor** o **BLOB**.
      Para **Contenedor** y **BLOB**, especifique el nombre del contenedor y la ruta de acceso del BLOB, respectivamente.
      Para obtener más información, consulte la [información general sobre las firmas de acceso compartido en Azure Storage](/azure/storage/common/storage-sas-overview).
- **Entorno:** especifica el entorno de nube que hospeda la cuenta de almacenamiento.

## <a name="managed-identities-for-azure-resources-authentication"></a>Identidades administradas para la autenticación de los recursos de Azure
Al ejecutar paquetes SSIS en el [entorno de ejecución de integración de Azure-SSIS en Azure Data Factory](/azure/data-factory/concepts-integration-runtime#azure-ssis-integration-runtime), puede usar la [identidad administrada](/azure/data-factory/connector-azure-sql-database#managed-identity) asociada a su factoría de datos para la autenticación de Azure Storage. Puede acceder a la factoría designada y copiar datos desde la cuenta de almacenamiento o en la cuenta de almacenamiento con esta identidad.

Consulte [Autenticación del acceso a Azure Storage mediante Azure Active Directory](/azure/storage/common/storage-auth-aad) para la autenticación de Azure Storage en general. Para usar la autenticación de identidad administrada para Azure Storage:

1. [Busque la identidad administrada de la factoría de datos en Azure Portal](/azure/data-factory/data-factory-service-identity). Vaya a **Propiedades** en la factoría de datos. Copie el valor de **Id. de aplicación de identidad administrada** (no el valor de **Id. del objeto de identidad administrada**).

1. Conceda a la identidad administrada el permiso adecuado en la cuenta de almacenamiento. Para más información sobre los roles, consulte [Administración de los derechos de acceso a los datos de Azure Storage con RBAC](/azure/storage/common/storage-auth-aad-rbac-portal).

    - **Como origen**, en Control de acceso (IAM), conceda al menos el rol **Lector de datos de Storage Blob**.
    - **Como destino**, en Control de acceso (IAM), conceda al menos el rol **Colaborador de datos de Storage Blob**.

Luego, configure la autenticación de identidad administrada para el administrador de conexiones de Azure Storage. Estas son las opciones para hacerlo:

- **Configuración durante su diseño.** En el Diseñador SSIS, haga doble clic en el administrador de conexiones de Azure Storage para abrir el **Editor del administrador de conexiones de Azure Storage**. Seleccione **Use managed identity to authenticate on Azure** (Usar la identidad administrada para autenticarse en Azure).
    > [!NOTE]
    >  Actualmente, esta opción no surte efecto (lo que indica que la autenticación de identidad administrada no funciona) cuando ejecuta un paquete SSIS en el Diseñador SSIS o en [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQL Server.
    
- **Configuración en tiempo de ejecución.** Al ejecutar el paquete a través de [SQL Server Management Studio (SSMS)](../ssis-quickstart-run-ssms.md) o de la [actividad para ejecutar paquetes SSIS de Azure Data Factory](/azure/data-factory/how-to-invoke-ssis-package-ssis-activity), busque el administrador de conexiones de Azure Storage. Actualice su propiedad `ConnectUsingManagedIdentity` a `True`.
    > [!NOTE]
    >  En el entorno de ejecución de integración de Azure-SSIS, todos los demás métodos de autenticación (por ejemplo, clave de acceso y entidad de servicio) configurados previamente en el administrador de conexiones de Azure Storage se invalidarán cuando la autenticación de identidad administrada se usa para las operaciones de almacenamiento.

> [!NOTE]
>  Para configurar la autenticación de identidad administrada en paquetes existentes, la manera preferida es volver a generar el proyecto SSIS con el [Diseñador SSIS más reciente](../../ssdt/download-sql-server-data-tools-ssdt.md) al menos una vez. Vuelva a implementar el proyecto SSIS en su entorno de ejecución de integración de Azure SSIS, de modo que la nueva propiedad del administrador de conexiones `ConnectUsingManagedIdentity` se agregue automáticamente a todos los administradores de conexiones de Azure Storage del proyecto SSIS. La manera alternativa es usar directamente la invalidación de propiedades con la ruta de acceso a la propiedad **\Package.Connections[{el nombre del administrador de conexiones}].Properties[ConnectUsingManagedIdentity]** en tiempo de ejecución.

## <a name="secure-network-traffic-to-your-storage-account"></a>Protección del tráfico de red en la cuenta de almacenamiento
Azure Data Factory ahora es un [servicio de Microsoft de confianza](/azure/storage/common/storage-network-security#trusted-microsoft-services) de Azure Storage. Al usar la autenticación de identidad administrada, se puede proteger la cuenta de almacenamiento [limitando el acceso a las redes seleccionadas](/azure/storage/common/storage-network-security#change-the-default-network-access-rule) mientras se sigue permitiendo a la factoría de datos acceder a la cuenta de almacenamiento. Consulte [Administración de excepciones](/azure/storage/common/storage-network-security#managing-exceptions) para obtener instrucciones.

## <a name="see-also"></a>Consulte también  
 [Conexiones de Integration Services &#40;SSIS&#41;](../../integration-services/connection-manager/integration-services-ssis-connections.md)