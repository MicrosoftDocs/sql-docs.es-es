---
title: Propiedades de Protocolos de MSSQLSERVER (pestaña Opciones avanzadas)
description: Obtenga información sobre las ventajas y los requisitos de la protección ampliada para la autenticación para el Motor de base de datos de SQL Server. Vea cómo habilitarla y configurarla.
ms.custom: seo-lt-2019
ms.date: 01/22/2021
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: ff312fefc29021a28df51ed4a845067d54024d6a
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813220"
---
# <a name="protocols-for-mssqlserver-properties-advanced-tab"></a>Propiedades de Protocolos de MSSQLSERVER (pestaña Opciones avanzadas)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

Utilice la pestaña **Opciones avanzadas** del cuadro de diálogo **Protocolos de las propiedades de MSSQLSERVER** para configurar la **Protección ampliada para la autenticación** para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)]. **Protección ampliada** es una característica de los componentes de red implementada por el sistema operativo. **Protección ampliada** está disponible en Windows 7 y Windows Server 2008 R2, y se incluye en los Service Pack para los sistemas operativos anteriores. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es más seguro cuando las conexiones se realizan con **Extended Protection**. Algunas ventajas de **Protección ampliada** requieren que se seleccione **Forzar cifrado** en la pestaña **Marcadores** .

> [!IMPORTANT]  
> Windows no habilita la **protección ampliada** de forma predeterminada. Para obtener información sobre cómo habilitar la **protección ampliada**, consulte lo siguiente:
> - [Protección ampliada de Windows \<extendedProtection\>](/iis/configuration/system.webserver/security/authentication/windowsauthentication/extendedprotection/)
> - [Introducción a la protección ampliada para la autenticación](/dotnet/framework/wcf/feature-details/extended-protection-for-authentication-overview)

Para obtener más información sobre cómo configurar otros servicios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consulte [Administrar el servicio del motor de base de datos](../../database-engine/configure-windows/manage-the-database-engine-services.md). Para acceder a una descripción completa de la protección ampliada, consulte [Conectar al motor de base de datos con protección ampliada](../../database-engine/configure-windows/connect-to-the-database-engine-using-extended-protection.md).

La **Protección ampliada** es plenamente compatible con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client a partir de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]. Actualmente no se garantiza la compatibilidad de la **Protección ampliada** con otros proveedores de clientes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

## <a name="options"></a>Opciones

### <a name="extended-protection"></a>protección ampliada

Hay tres valores posibles:  

- **Off**: La **protección ampliada** está deshabilitada de forma predeterminada. La instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aceptará las conexiones de cualquier cliente independientemente de que esté o no protegido. La configuración **Desactivado** es compatible con los sistemas operativos sin revisiones y anteriores, pero es menos segura. Utilice este valor únicamente cuando sepa que los sistemas operativos clientes no admiten la protección ampliada.

- **Permitido**: significa que la **protección ampliada** se requiere para las conexiones de los sistemas operativos que admiten la **protección ampliada**. Se rechazan las conexiones de las aplicaciones cliente no protegidas que se estén ejecutando en sistemas operativos clientes protegidos. **Protección ampliada** se omite para las conexiones de los sistemas operativos no protegidos. Esta configuración es más segura que **Desactivado**, pero no es la más segura. Utilice esta configuración en entornos mixtos, donde no todos los sistemas operativos o aplicaciones admiten **Protección ampliada** .

- **Obligatorio**: significa que se acepte una conexión, deben proceder de una aplicación protegida en un sistema operativo protegido. Esta configuración es la más segura de las tres opciones. Sin embargo, las conexiones de los sistemas operativos que no admiten la **protección ampliada** no podrán conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

### <a name="accepted-ntlm-spns"></a>Se aceptan SPN NTLM

Una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede identificarse por más de un nombre de entidad de seguridad de servicio (SPN) NTLM. Los SPN se enumeran como una serie de cadenas separadas por punto y coma. Por ejemplo, el valor **MSSQLSvc/HostName1.Contoso.com;MSSQLSvc/HostName2.Contoso.com** indica que se permiten los clientes que intentan conectarse a los SPN denominados **MSSQLSvc/HOST1.Contoso.com** o **MSSQLSvc/HOST2.Contoso.com**. La variable tiene una longitud máxima de 2048 caracteres.

## <a name="see-also"></a>Consulte también

[Protección ampliada para la autenticación con Reporting Services](../../reporting-services/security/extended-protection-for-authentication-with-reporting-services.md)