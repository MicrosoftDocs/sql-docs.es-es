---
title: Proteger SQL Server | Microsoft Docs
description: Use estos artículos para crear e implementar un plan de seguridad eficaz en SQL Server. Obtenga información sobre la plataforma, la autenticación, los objetos y las aplicaciones.
ms.custom: ''
ms.date: 06/21/2019
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- Security [SQL Server]
helpviewer_keywords:
- database objects [SQL Server], security
- SQL Server, security
- operating systems [SQL Server], security
- security [SQL Server], planning
- applications [SQL Server], security
ms.assetid: 4d93489e-e9bb-45b3-8354-21f58209965d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 2f3082f83296efe396fa2fca447782e13588df38
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340119"
---
# <a name="securing-sql-server"></a>Proteger SQL Server

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

La protección de la seguridad en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] conlleva una serie de pasos que afectan a cuatro áreas: la plataforma, la autenticación, los objetos (incluidos los datos) y las aplicaciones que tienen acceso al sistema. Los siguientes temas le guiarán durante el proceso de creación e implementación de un plan de seguridad eficaz.  
  
 Puede buscar más información sobre la seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el sitio web de [SQL Server](https://go.microsoft.com/fwlink/?LinkID=31629) . En él encontrará una guía de prácticas recomendadas y una lista de comprobación de seguridad. Este sitio también incluye las últimas descargas e información sobre los Service Packs.  
  
## <a name="platform-and-network-security"></a>Seguridad de la plataforma y de la red  
 La plataforma de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] incluye el hardware físico y los sistemas de redes que conectan los clientes con los servidores de bases de datos, así como los archivos binarios que se utilizan para procesar solicitudes de base de datos.  
  
### <a name="physical-security"></a>Seguridad física  
 Las recomendaciones de seguridad física limitan de forma estricta el acceso al servidor físico y a los componentes de hardware. Por ejemplo, use salas cerradas de acceso restringido para el hardware de servidor de base de datos y los dispositivos de red. Además, limite el acceso a los medios de copia de seguridad almacenándolos en una ubicación segura fuera de las instalaciones.  
  
 La implementación de la seguridad de la red física comienza por mantener a los usuarios no autorizados fuera de la red. En la tabla siguiente se incluye más información sobre la seguridad de la red.  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|[!INCLUDE[ssEW](../../includes/ssew-md.md)] y acceso de red a otras ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|"Configurar y proteger el entorno de servidor" en los Libros en pantalla de [!INCLUDE[ssEW](../../includes/ssew-md.md)]|  
| &nbsp; | &nbsp; |
  
### <a name="operating-system-security"></a>Seguridad del sistema operativo  
 Los Service Packs y las actualizaciones del sistema operativo incluyen mejoras de seguridad importantes. Aplique todas las revisiones y actualizaciones al sistema operativo después de probarlas con las aplicaciones de base de datos.  
  
 Los firewalls también proporcionan formas eficaces de implementar la seguridad. Lógicamente, un firewall es un separador o limitador del tráfico de red, que puede configurarse para aplicar la directiva de seguridad de datos de su organización. Si utiliza un firewall, aumentará la seguridad del sistema operativo, ya que proporciona un cuello de botella en el que pueden concentrarse las medidas de seguridad. En la tabla siguiente se incluye más información sobre la forma de usar un firewall con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Configurar un firewall para que funcione con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[Configuración de Firewall de Windows para el acceso al motor de base de datos](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)|  
|Configurar un firewall para que funcione con [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|[Servicio Integration Services &#40;servicio SSIS&#41;](../../integration-services/service/integration-services-service-ssis-service.md)|  
|Configurar un firewall para que funcione con [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|[Configurar Firewall de Windows para permitir el acceso a Analysis Services](/analysis-services/instances/configure-the-windows-firewall-to-allow-analysis-services-access)|  
|Abrir puertos específicos de un firewall para permitir el acceso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[Configure the Windows Firewall to Allow SQL Server Access (Configurar el Firewall de Windows para permitir el acceso a SQL Server)](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md)|  
|Configurar la compatibilidad con Protección ampliada para la autenticación utilizando enlace de canal y enlace de servicio|[Conectar al motor de base de datos con protección ampliada](../../database-engine/configure-windows/connect-to-the-database-engine-using-extended-protection.md)|  
| &nbsp; | &nbsp; |
  
 La reducción del área expuesta es una medida de seguridad que implica detener o deshabilitar los componentes que no se utilizan. La reducción del área expuesta ayuda a mejorar la seguridad al proporcionar menos accesos para ataques potenciales al sistema. La clave para limitar el área expuesta de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consiste en ejecutar los servicios requeridos que tienen "privilegios mínimos" concediendo exclusivamente los derechos necesarios a los servicios y los usuarios. En la tabla siguiente se incluye más información sobre el acceso al sistema y a los servicios.  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Servicios requeridos para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[Configurar los permisos y las cuentas de servicio de Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)|  
| &nbsp; | &nbsp; |
  
 Si el sistema [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa Internet Information Services (IIS), es necesario realizar pasos adicionales para proteger la superficie de la plataforma. En la tabla siguiente se incluye información sobre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e Internet Information Services.  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|La seguridad de IIS con [!INCLUDE[ssEW](../../includes/ssew-md.md)]|"Seguridad de IIS" en los Libros en pantalla de [!INCLUDE[ssEW](../../includes/ssew-md.md)]|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Autenticación|[Autenticación de Windows en Reporting Services](../../reporting-services/extensions/security-extension/authentication-in-reporting-services.md)|  
|[!INCLUDE[ssEW](../../includes/ssew-md.md)] y acceso a IIS|"Diagrama de flujo de seguridad de Internet Information Services" en los Libros en pantalla de [!INCLUDE[ssEW](../../includes/ssew-md.md)]|  
| &nbsp; | &nbsp; |
  
### <a name="sql-server-operating-system-files-security"></a>Seguridad de los archivos del sistema operativo de SQL Server  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa archivos del sistema operativo para el funcionamiento y el almacenamiento de datos. Las recomendaciones de seguridad de archivos indican que se restrinja el acceso a estos archivos. En la tabla siguiente se ofrece información sobre estos archivos.  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Archivos de programa|[Ubicaciones de archivos para las instancias predeterminadas y con nombre de SQL Server](../../sql-server/install/file-locations-for-default-and-named-instances-of-sql-server.md)|  
| &nbsp; | &nbsp; |
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proporcionan una seguridad mejorada. Para determinar el último Service Pack disponible para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea el sitio web de [SQL Server](https://go.microsoft.com/fwlink/?LinkID=31629) .  
  
 Puede usar el siguiente script para determinar el Service Pack instalado en el sistema:  
  
```sql
SELECT CONVERT(char(20), SERVERPROPERTY('productlevel'));  
```  
  
## <a name="principals-and-database-object-security"></a>Entidades de seguridad y seguridad de objetos de base de datos  
 Las entidades de seguridad son los individuos, grupos y procesos que tienen acceso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los "elementos protegibles" son el servidor, la base de datos y los objetos incluidos en la base de datos. Cada uno de estos elementos dispone de un conjunto de permisos que pueden configurarse para reducir el área expuesta de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . En la tabla siguiente se incluye información sobre las entidades de seguridad y los elementos protegibles.  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Usuarios, roles y procesos de servidor y base de datos|[Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)|  
|Seguridad de objetos de servidor y base de datos|[Elementos protegibles](../../relational-databases/security/securables.md)|  
|La jerarquía de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[Jerarquía de permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)|  
| &nbsp; | &nbsp; |
  
### <a name="encryption-and-certificates"></a>Cifrado y certificados  
 El cifrado no resuelve los problemas de control de acceso. Sin embargo, mejora la seguridad debido a que limita la pérdida de datos, incluso en el caso poco probable de que se superen los controles de acceso. Por ejemplo, si el equipo host de base de datos no está configurado correctamente y un usuario malintencionado obtiene datos confidenciales, como números de tarjetas de crédito, esa información robada podría resultar inservible si está cifrada. La tabla siguiente contiene más información acerca del cifrado en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|La jerarquía de cifrado de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[Jerarquía de cifrado](../../relational-databases/security/encryption/encryption-hierarchy.md)|  
|Implementar conexiones seguras|[Habilitar conexiones cifradas en el motor de base de datos &#40;Administrador de configuración de SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)|  
|Funciones de cifrado|[Funciones de cifrado &#40;Transact-SQL&#41;](../../t-sql/functions/cryptographic-functions-transact-sql.md)|  
  
 Los certificados son "claves" de software que se comparten entre dos servidores que habilitan las comunicaciones seguras a través de una autenticación segura. Puede crear y usar certificados en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para mejorar la seguridad de objetos y conexiones. La tabla siguiente contiene información sobre cómo utilizar los certificados con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Crear un certificado para utilizarlo con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)|  
|Usar un certificado para la creación de reflejo de la base de datos|[Usar certificados para un punto de conexión de creación de reflejo de la base de datos &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)|  
| &nbsp; | &nbsp; |

## <a name="application-security"></a>Seguridad de aplicaciones

### <a name="client-programs"></a>Programas de cliente

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] incluyen la escritura de aplicaciones cliente seguras. Para obtener más información sobre cómo proteger las aplicaciones cliente en el nivel de red, vea [Client Network Configuration](../../database-engine/configure-windows/client-network-configuration.md).

### <a name="windows-defender-application-control-wdac"></a>Control de aplicaciones de Windows Defender (WDAC)

<!--
This next live paragraph, about Windows Defender Application Control (WDAC), was requested by Bella Brahm, 2019/06/20. (GeneMi)

WDAC can also prevent the kind of highly sophisticated 'Nansh0u' attacks described in 'https://www.guardicore.com/2019/05/nansh0u-campaign-hackers-arsenal-grows-stronger/'. That webpage recommends this present article.
-->

Control de aplicaciones de Windows Defender (WDAC) evita la ejecución de código no autorizado. WDAC es una manera eficaz de mitigar la amenaza de malware basado en archivos ejecutables. Para obtener más información, vea la documentación [Control de aplicaciones de Windows Defender](/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control).

## <a name="sql-server-security-tools-utilities-views-and-functions"></a>Herramientas, utilidades, vistas y funciones de seguridad de SQL Server  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proporciona herramientas, utilidades, vistas y funciones que pueden utilizarse para configurar y administrar la seguridad.  
  
### <a name="sql-server-security-tools-and-utilities"></a>Herramientas y utilidades de seguridad de SQL Server  
 En la tabla siguiente se incluye información acerca de las herramientas y utilidades de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que puede utilizar para configurar y administrar la seguridad.  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Conectarse, configurar y controlar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[Usar SQL Server Management Studio](../../ssms/sql-server-management-studio-ssms.md)|  
|Conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y ejecutar consultas en el símbolo del sistema|[Utilidad sqlcmd](../../tools/sqlcmd-utility.md)|  
|Control y configuración de red para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[Administrador de configuración de SQL Server](../../relational-databases/sql-server-configuration-manager.md)|  
|Habilitar y deshabilitar características con Administración basada en directiva|[Administrar servidores mediante administración basada en directivas](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)|  
|Manipular claves simétricas para un servidor de informes|[rskeymgmt (utilidad) &#40;SSRS&#41;](../../reporting-services/tools/rskeymgmt-utility-ssrs.md)|  
| &nbsp; | &nbsp; |

### <a name="sql-server-security-catalog-views-and-functions"></a>Funciones y vistas de catálogo de seguridad de SQL Server  
 El [!INCLUDE[ssDE](../../includes/ssde-md.md)] expone información de seguridad en varias vistas y funciones que se optimizan en cuanto a rendimiento y utilidad. En la tabla siguiente se incluye más información acerca de las funciones y vistas de seguridad.  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Vistas de catálogo de seguridad , que devuelven información sobre permisos de base de datos y servidor, entidades de seguridad, roles, etc. También hay vistas de catálogo que proporcionan información acerca de las claves de cifrado, los certificados y las credenciales.|[Vistas de catálogo de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , que devuelven información sobre el usuario, los permisos y los esquemas actuales.|[Funciones de seguridad &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|[Funciones y vistas de administración dinámica relacionadas con la seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/security-related-dynamic-management-views-and-functions-transact-sql.md)|  
| &nbsp; | &nbsp; |

## <a name="related-content"></a>Contenido relacionado  
 [Consideraciones de seguridad para una instalación de SQL Server](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)  
 [Centro de seguridad para el Motor de base de datos de SQL Server y Azure SQL Database](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
[Procedimientos recomendados de seguridad de SQL Server 2012: tareas operativas y administrativas](https://download.microsoft.com/download/8/F/A/8FABACD7-803E-40FC-ADF8-355E7D218F4C/SQL_Server_2012_Security_Best_Practice_Whitepaper_Apr2012.docx)   
[Blog de seguridad de SQL Server](/archive/blogs/sqlsecurity/)  
[Notas del producto de procedimientos recomendados de seguridad y seguridad de etiquetas](/archive/blogs/sqlsecurity/security-best-practice-and-label-security-whitepapers)  
[Seguridad de nivel de fila](../../relational-databases/security/row-level-security.md)   
[Proteger su propiedad intelectual de SQL Server](../../relational-databases/security/protecting-your-sql-server-intellectual-property.md)   
