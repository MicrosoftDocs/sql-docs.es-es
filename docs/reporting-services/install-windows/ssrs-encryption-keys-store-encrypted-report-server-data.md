---
description: Almacenamiento de datos cifrados del servidor de informes (Configuration Manager)
title: Almacenamiento de datos cifrados del servidor de informes (Configuration Manager) | Microsoft Docs
ms.date: 10/24/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
helpviewer_keywords:
- report servers [Reporting Services], encryption
- credentials [Reporting Services]
- cryptography [Reporting Services]
- confidential reports [Reporting Services]
- encryption [Reporting Services]
- databases [Reporting Services], encryption
ms.assetid: ac0f4d4d-fc4b-4c62-a693-b86e712e75f2
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 18ee544cebcb70d5564dce705e1ec8ed856274a9
ms.sourcegitcommit: fe59f8dc27fd633f5dfce54519d6f5dcea577f56
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91934717"
---
# <a name="ssrs-encryption-keys---store-encrypted-report-server-data"></a>Claves de cifrado de SSRS: almacenar datos cifrados del servidor de informes
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] almacena valores cifrados en la base de datos del servidor de informes y en archivos de configuración. La mayoría de los valores cifrados son credenciales que se utilizan para obtener acceso a orígenes de datos externos que proporcionan datos para informes. En este tema se describen los valores que se cifran, la funcionalidad de cifrado usada en [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]y otros tipos de datos confidenciales almacenados que debe conocer.  
  
## <a name="encrypted-values"></a>Valores cifrados  
 La siguiente lista contiene los valores que se almacenan en una instalación de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
-   Información de conexión y credenciales que utiliza un servidor de informes para conectarse a una base de datos del servidor de informes que almacena datos internos del servidor.  
  
     Estos valores se especifican y cifran durante la instalación o la configuración del servidor de informes. Puede actualizar la información de conexión en cualquier momento utilizando la herramienta de configuración de Reporting Services o la utilidad **rsconfig** . El cifrado de valores de configuración se realiza utilizando la clave de nivel de equipo del equipo local que está disponible para todos los usuarios. La información de conexión cifrada se almacena en el archivo rsreportserver.config (ningún otro archivo de configuración contiene configuración cifrada). Para más información, vea [Configurar una conexión a la base de datos del servidor de informes &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).  
  
-   Credenciales almacenadas que utiliza un servidor de informes para conectarse a orígenes de datos externos que proporcionan datos a un informe.  
  
     Estos valores se definen durante la configuración de la información de origen de datos para un informe y se almacenan después como valores cifrados en una base de datos del servidor de informes. El servidor de informes utiliza una clave simétrica para cifrar y descifrar estos datos. Para obtener más información sobre las credenciales almacenadas, consulte [Especificar información de credenciales y conexión para los orígenes de datos de informes](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md).  
  
-   Cuenta de usuario desatendida que el servidor de informes utiliza para conectarse a otros equipos y recuperar archivos de imágenes o datos externos empleados en un informe.  
  
     Esta cuenta se utiliza cuando se necesita una conexión a un equipo remoto y no hay más credenciales disponibles para realizar la conexión. Esta cuenta se utiliza principalmente para admitir el procesamiento de informes en modo desatendido de los informes que no utilizan credenciales para obtener acceso a un origen de datos. Si crea informes en función de los orígenes de datos que no requieren o utilizan credenciales cuando obtienen acceso a los datos, debe configurar esta cuenta para que el servidor de informes la utilice.  
  
     Esta cuenta es necesaria en ciertas circunstancias y solo puede crearse mediante la herramienta de configuración de Reporting Services o **rsconfig**. Este valor también se almacena en el archivo rsreportserver.config. Esta cuenta debe crearse manualmente. Para más información sobre esta cuenta y cómo se usa, vea [Configurar la cuenta de ejecución desatendida &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md).  
  
-   Clave simétrica utilizada para el cifrado.  
  
     Este valor se crea durante la instalación o la configuración del servidor y se almacena después como un valor cifrado en la base de datos del servidor de informes. El servicio Servidor de informes de Windows utiliza esta clave para cifrar y descifrar datos que se almacenan en la base de datos del servidor de informes.  
  
## <a name="encryption-functionality-in-reporting-services"></a>Funcionalidad de cifrado en Reporting Services  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] usa funciones de cifrado que forman parte del sistema operativo Windows. Se utilizan tanto el cifrado simétrico como el asimétrico.  
  
 La información de la base de datos del servidor de informes se cifra mediante una clave simétrica. Hay una clave simétrica única para cada base de datos del servidor de informes. Esta clave simétrica se cifra utilizando la clave pública de un par de claves asimétricas generadas por Windows. La clave privada se mantiene en la cuenta del servicio Servidor de informes de Windows.  
  
 En una implementación escalada del servidor de informes en la que varias instancias compartan la misma base de datos, todos los nodos del servidor de informes utilizarán una única clave simétrica. Cada nodo debe tener una copia de la clave simétrica compartida. Cuando se configura la implementación escalada, automáticamente se crea una copia de la clave simétrica para cada nodo. Cada nodo cifra su copia de la clave simétrica utilizando la clave pública de un par de claves específico para su cuenta de servicio de Windows. Para más información sobre cómo se crea la clave simétrica para implementaciones escaladas y de una sola instancia, vea [Inicializar un servidor de informes &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-initialize-a-report-server.md).  
 
 Además, a partir de 2019, la base de datos del servidor de informes se puede configurar con Cifrado de datos transparente en SQL Server para proporcionar protección adicional para los datos en reposo.
  
> [!NOTE]  
>  Cuando cambie la cuenta del servicio Servidor de informes de Windows, las claves asimétricas pueden dejar de ser válidas, lo que interrumpirá el funcionamiento del servidor. Para evitar que esto suceda, asegúrese de utilizar la herramienta de configuración de Reporting Services para modificar la configuración de la cuenta de servicio. Cuando utilice la herramienta de configuración, las claves se actualizarán automáticamente. Para más información, vea [Configurar la cuenta de servicio del servidor de informes &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md).  
  
## <a name="other-sources-of-confidential-data"></a>Otros orígenes de datos confidenciales  
 Un servidor de informes almacena otros datos no cifrados, pero que pueden contener información confidencial que desee proteger. En concreto, las instantáneas de historial del informe y de ejecución de informes contienen resultados de consultas que pueden incluir datos destinados a usuarios autorizados. Si utiliza la funcionalidad de instantáneas para informes que contienen datos confidenciales, tenga en cuenta que los usuarios que puedan abrir las tablas en una base de datos del servidor de informes podrán ver partes de un informe almacenado al examinar el contenido de la tabla.  
  
> [!NOTE]  
>  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] no admite el almacenamiento en caché o el historial de informes para informes que usan parámetros basados en la identidad de seguridad del usuario.  
  
## <a name="see-also"></a>Consulte también  
 [Configurar y administrar claves de cifrado &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md)  
  
  
