---
description: Crear clases de proxy del servicio web Master Data Manager
title: Crear clases de proxy del servicio web Master Data Manager
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: reference
ms.assetid: 8bdab026-a0c0-41f3-9d36-f3919c23247f
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 2df08cca57a68d38c72f19921243ae95ff394c23
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350088"
---
# <a name="create-master-data-manager-web-service-proxy-classes"></a>Crear clases de proxy del servicio web Master Data Manager

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  El servicio web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] le permite usar las características de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] mediante programación desde cualquier equipo que pueda acceder a su sitio web de [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] . Antes de empezar a escribir código para acceder al servicio web, debe generar clases de proxy. La clase de proxy principal que utiliza para realizar operaciones del servicio web es la clase <xref:Microsoft.MasterDataServices.ServiceClient>, que implementa la interfaz <xref:Microsoft.MasterDataServices.IService>.  
  
## <a name="enable-web-service-metadata-publishing"></a>Habilitar la publicación de metadatos del servicio web  
 Para poder generar clases de proxy, debe habilitar la publicación de metadatos del servicio web. Para ello, siga estos pasos:  
  
1.  Abra el archivo Web.config de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] en un editor de texto. Este archivo se encuentra en la carpeta WebApplication de la ruta de instalación de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)].  
  
2.  Busque la sección **mdsWsHttpBehavior** en **\<serviceBehaviors>** . Para el **\<serviceMetadata>** elemento, establezca **httpGetEnabled** en **true**.  
  
    > [!NOTE]  
    >  Si desea habilitar los servicios web a través de la seguridad de la capa de transporte (TLS), conocido anteriormente como Capa de sockets seguros (SSL), establezca **httpsGetEnabled** en **true** en la sección **mdsWsHttpBehavior** del archivo de web.config. También debe cambiar **mdsWsHTTPBinding** para que esté configurado para TLS, y convertir en comentario la sección que no sea de TLS.  
  
3.  Guarde los cambios realizados en el archivo.  
  
4.  Pruebe la publicación de metadatos yendo a la dirección URL del servicio (por ejemplo, `https://yourserver/MDS/service/service.svc`). Si la publicación de metadatos está habilitada, se muestra una página que empieza con   
    "Ha creado un servicio".  
  
## <a name="creating-proxy-classes-by-using-visual-studio"></a>Crear clases de proxy usando Visual Studio  
 Si tiene instalado Visual Studio 2010, la manera más sencilla de generar clases de proxy consiste en agregar una **referencia de servicio** al proyecto. La dirección de la referencia de servicio es la URL de la aplicación de [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)], seguida de /service/service.svc. Por ejemplo: `https://yourserver/MDS/service/service.svc`. Para más información, vea [Cómo: Agregar, actualizar o quitar una referencia de servicio](/previous-versions/bb628652(v=vs.140)).  
  
## <a name="creating-proxy-classes-by-using-svcutilexe"></a>Crear clases de proxy usando Svcutil.exe  
 Debe tener [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] instalado o el [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows SDK para poder tener Svcutil.exe en el equipo. Si utiliza [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], debe utilizar el símbolo del sistema de [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] para ejecutar el comando. Para más información, vea [Herramienta de utilidad de metadatos de ServiceModel (Svcutil.exe)](/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe) y [Generación de un cliente WCF a partir de los metadatos de servicio](/dotnet/framework/wcf/feature-details/generating-a-wcf-client-from-service-metadata).  
  
 Para crear un conjunto de clases de proxy en C# usando Svcutil.exe, utilice un comando como el siguiente:  
  
```  
svcutil.exe https://<server_name:port>/<virtual_path>/Service/Service.svc   
/out:<proxy_name>.cs /messageContract /tcv:Version35   
/noconfig /ct:System.Collections.ObjectModel.Collection`1   
/namespace:*,Microsoft.MasterDataServices  
```  
  
 Donde:  
  
-   *servername*:*port* son el nombre de equipo y el número de puerto del equipo que hospeda [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)].  
  
-   *virtual_path* es la ruta de acceso virtual de [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] en Internet Information Services (IIS).  
  
-   *proxy_name* es el nombre del archivo de proxy generado.  
  
## <a name="see-also"></a>Consulte también  
 [Operaciones de servicio web clasificadas &#40;Master Data Services&#41;](../../master-data-services/develop/categorized-web-service-operations-master-data-services.md)  
  
