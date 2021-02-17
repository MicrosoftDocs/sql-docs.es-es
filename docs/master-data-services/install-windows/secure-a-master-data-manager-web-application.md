---
title: Proteger una aplicación web Master Data Services
description: En SQL Server, puede proteger la aplicación Web de Master Data Manager con HTTPS. Debe ser un administrador y MDS debe estar instalado en el servidor Web.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: e360ba3a-e96b-4f85-b588-ed1f767fa973
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 5e941ce4f06961f19e0d5f4a0dc96aff31039647
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344756"
---
# <a name="secure-a-master-data-manager-web-application"></a>Proteger una aplicación web Master Data Services

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Puede proteger la aplicación web de [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] con HTTPS.  
  
> [!NOTE]  
>  La aplicación web de [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] puede usa HTTP o HTTPS, pero no ambos.  
  
## <a name="prerequisites"></a>Prerrequisitos  
 Para realizar el procedimiento:  
  
-   Debe ser administrador en el servidor web donde está instalado [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] .  
  
-   MDS debe estar instalado en el servidor web y debe existir una aplicación web. Para obtener más información, vea [Instalar Master Data Services](../../master-data-services/install-windows/install-master-data-services.md) y [Crear una aplicación web de Master Data Manager &#40;Master Data Services&#41;](../../master-data-services/install-windows/create-a-master-data-manager-web-application-master-data-services.md).  

- La [protección ampliada de IIS para la autenticación de Windows](/iis/configuration/system.webserver/security/authentication/windowsauthentication/extendedprotection/) no debe estar habilitada. 

- Configure el servidor web para que escuche en todas las direcciones IP disponibles. No configure el servidor web para que escuche en una dirección IP específica. 

### <a name="to-secure-the-master-data-manager-web-application-with-https"></a>Para proteger la aplicación web Administrador de datos maestros con HTTP  
  
1.  Después de confirmar que la aplicación web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] está configurada correctamente con HTTP, cree un certificado en IIS. Para obtener más información, vea [Configurar certificados de servidor en IIS 7](https://technet.microsoft.com/library/cc732230\(WS.10\).aspx).  
  
2.  En el panel **Conexiones** , debajo de **Sitios**, haga clic en en el sitio que hospeda la aplicación web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] .  
  
3.  En el panel **Acciones**, haga clic en **Enlaces**.  
  
4.  Haga clic en **Agregar**.  
  
5.  En la lista, seleccione **https**.  
  
6.  Seleccione el certificado TLS/SSL.  
  
7.  Haga clic en **OK**.  
  
8.  Opcional. Para quitar HTTP de modo que los usuarios puedan tener acceso al sitio solo con HTTP, en la lista, haga clic en la fila con **http**. Haga clic en **Quitar** y, en el cuadro de diálogo de confirmación, haga clic en **Sí**.  
  
    > [!IMPORTANT]  
    >  Debe cambiar las configuraciones basicHttp y wsHttpBinding después de quitar HTTP.  
  
9. Para cerrar el cuadro de diálogo **Enlaces de sitios** , haga clic en **Cerrar**.  
  
10. Abra ahora el archivo web.config desde *unidad*:\Archivos de programa\Microsoft SQL Server\130\Master Data Services\WebApplication.  
  
11. Busque la cadena `<security mode="Message">` y cámbiela a `<security mode="Transport">`.  

12. Cambie `<serviceMetadata httpGetEnable="true" httpsGetEnabled="false">` por `<serviceMetadata httpGetEnable="false" httpsGetEnabled="true">` para evitar problemas que puedan aparecer en el cliente de Silverlight.

13. Guarde y cierre el archivo. Si obtiene un error, podría deberse a que ha habilitado UAC. Ahora los usuarios deben poder usar HTTPS para tener acceso al sitio.  

  
## <a name="see-also"></a>Consulte también  
 [Crear una aplicación web de Master Data Manager &#40;Master Data Services&#41;](../../master-data-services/install-windows/create-a-master-data-manager-web-application-master-data-services.md)  
  
  
