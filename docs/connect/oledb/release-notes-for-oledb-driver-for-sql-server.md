---
title: Notas de la versión de OLE DB Driver
description: En este artículo sobre notas de la versión se describen los cambios de cada versión de Microsoft OLE DB Driver for SQL Server.
ms.date: 12/01/2020
ms.prod: sql
ms.technology: connectivity
ms.topic: conceptual
ms.reviewer: genemi
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: dfc66e7c1f1aa787ccc366c7dc50613741724a1f
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837495"
---
# <a name="release-notes-for-the-microsoft-ole-db-driver-for-sql-server"></a>Notas de la versión del controlador Microsoft OLE DB para SQL Server

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

En esta página se describe lo que se ha agregado en cada versión del controlador Microsoft OLE DB para SQL Server.

<!--
USE THE TABLE FORMAT!
Hello, from now on, please use the table-based format standard for all new Release Notes content.
See section "## 18.2.1" for a live example in this article.
Thank you. For questions, contact GeneMi. (2019/03/16)
-->

## <a name="1850"></a>18.5.0
![descargar](../../ssms/media/download-icon.png) [Descargar instalador x64](https://go.microsoft.com/fwlink/?linkid=2135577)  
![descargar](../../ssms/media/download-icon.png) [Descargar instalador x86](https://go.microsoft.com/fwlink/?linkid=2135722)  

Fecha de publicación: 1 de diciembre de 2020

Si necesita descargar el instalador en un idioma distinto al que se ha detectado, puede usar estos vínculos directos.  
    Para el controlador x64: [Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x40a)  
    Para el controlador x86: [Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x40a)  

### <a name="features-added"></a>Características agregadas

| Característica agregada | Detalles |
| :------------ | :------ |
| Compatibilidad con la [clasificación y detección de datos de SQL](../../relational-databases/security/sql-data-discovery-and-classification.md) | [Uso de la clasificación de datos](features/using-data-classification.md) |
| Compatibilidad con la autenticación de entidad de servicio de Azure Active Directory (`ActiveDirectoryServicePrincipal`) | [Uso de Azure Active Directory](features/using-azure-active-directory.md) |

### <a name="bugs-fixed"></a>Errores corregidos

| Error corregido | Detalles |
| :-------- | :------ |
| Se ha corregido un problema con los caracteres NUL insertados. | Se ha corregido un error que provocaba que el controlador devolviera una longitud incorrecta de cadenas con caracteres NUL insertados. |
| Se ha corregido una fuga de memoria en la interfaz [IBCPSession](ole-db-interfaces/ibcpsession-ole-db.md). | Se ha corregido una fuga de memoria en la interfaz [IBCPSession](ole-db-interfaces/ibcpsession-ole-db.md) que implicaba operaciones de copia masiva del tipo de datos `sql_variant`. |
| Se han corregido los errores que provocaban que se devolvieran valores incorrectos para las propiedades `SSPROP_INTEGRATEDAUTHENTICATIONMETHOD` y `SSPROP_MUTUALLYAUTHENTICATED`. | Las versiones anteriores del controlador devolvían valores truncados de la propiedad `SSPROP_INTEGRATEDAUTHENTICATIONMETHOD`. Además, en el caso de la autenticación `ActiveDirectoryIntegrated`, el valor devuelto de la propiedad `SSPROP_MUTUALLYAUTHENTICATED` era `VARIANT_FALSE` incluso cuando ambos lados se autenticaban mutuamente.|
| Se corrigió un error de inserción de tabla remota del servidor vinculado. | Se corrigió un error que hacía que se produjera un error en la inserción de tabla remota del servidor vinculado si se habilitaba la [opción de configuración del servidor NOCOUNT](../../database-engine/configure-windows/configure-the-user-options-server-configuration-option.md). |

## <a name="previous-releases"></a>Versiones anteriores

## <a name="1840"></a>18.4.0
![descargar](../../ssms/media/download-icon.png) [Descargar instalador x64](https://go.microsoft.com/fwlink/?linkid=2129954)  
![descargar](../../ssms/media/download-icon.png) [Descargar instalador x86](https://go.microsoft.com/fwlink/?linkid=2131003)  

Fecha de publicación: Mayo de 2020

Si necesita descargar el instalador en un idioma distinto al que se ha detectado, puede usar estos vínculos directos.  
Para el controlador x64: [Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x40a)  
Para el controlador x86: [Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x40a)  

### <a name="features-added"></a>Características agregadas

| Característica agregada | Detalles |
| :------------ | :------ |
| Compatibilidad con la resolución de IP de red transparente (TNIR) |[Resolución de IP de red transparente (TNIR)](features/using-transparent-network-ip-resolution.md)|
| Compatibilidad con la codificación de cliente UTF-8 | [Compatibilidad de UTF-8 con el controlador OLE DB para SQL Server](features/utf-8-support-in-oledb-driver-for-sql-server.md) |

### <a name="bugs-fixed"></a>Errores corregidos

| Error corregido | Detalles |
| :-------- | :------ |
| Se han corregido varios errores en la interfaz [ISequentialStream](/previous-versions/windows/desktop/ms718035(v=vs.85)) | Algunos errores que afectan a las páginas de códigos multibyte hacen que la interfaz notifique prematuramente el final de la secuencia durante la operación de lectura.|
| Se ha corregido una fuga de memoria en la interfaz [IOpenRowset::OpenRowset](/previous-versions/windows/desktop/ms716724(v=vs.85)). | Se ha corregido una fuga de memoria en la interfaz [IOpenRowset::OpenRowset](/previous-versions/windows/desktop/ms716724(v=vs.85)) cuando se habilita la propiedad `SSPROP_IRowsetFastLoad`. |
| Se ha corregido un error en escenarios con un tipo de datos `sql_variant` y cadenas no ASCII. | La ejecución de ciertos escenarios en los que participan un tipo de datos `sql_variant` y cadenas no ASCII puede provocar daños en los datos. Para obtener detalles, consulte: [Problemas conocidos](ole-db-data-types/ssvariant-structure.md#known-issues). |
| Se han corregido problemas con el botón *Probar conexión* del cuadro de diálogo [Configuración de UDL](help-topics/data-link-pages.md). | El botón *Probar conexión* del cuadro de diálogo [Configuración de UDL](help-topics/data-link-pages.md) ahora respeta las propiedades de inicialización establecidas en la pestaña *Todos*. |
| Se ha corregido el control de los valores predeterminados de la propiedad `SSPROP_INIT_PACKETSIZE`. | Se ha corregido un error inesperado cuando la propiedad `SSPROP_INIT_PACKETSIZE` se establece en su valor predeterminado de `0`. Para obtener más información sobre esta propiedad, vea [Propiedades de inicialización y autorización](ole-db-data-source-objects/initialization-and-authorization-properties.md). |
| Se han corregido los problemas de desbordamiento de búfer en [IBCPSession](ole-db-interfaces/ibcpsession-ole-db.md). | Se han corregido problemas de desbordamiento de búfer cuando se usan archivos de datos mal formados. |
| Se han corregido los problemas de accesibilidad. | Se han corregido problemas de accesibilidad en la interfaz de usuario del instalador y en el [cuadro de diálogo Inicio de sesión SQL Server](help-topics/sql-server-login-dialog.md) (lectura de contenido, tabulaciones). |

## <a name="1830"></a>18.3.0

![descargar](../../ssms/media/download-icon.png) [Descargar instalador x64](https://go.microsoft.com/fwlink/?linkid=2117515)  
![descargar](../../ssms/media/download-icon.png) [Descargar instalador x86](https://go.microsoft.com/fwlink/?linkid=2117517)  

Fecha de publicación: Octubre de 2019

Si necesita descargar el instalador en un idioma distinto al que se ha detectado, puede usar estos vínculos directos.  
Para el controlador x64: [Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x40a)  
Para el controlador x86: [Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x40a)  

### <a name="features-added"></a>Características agregadas

| Característica agregada | Detalles |
| :------------ | :------ |
| Compatibilidad con la autenticación de Azure Active Directory (`ActiveDirectoryInteractive`, `ActiveDirectoryMSI`) | [Uso de Azure Active Directory](features/using-azure-active-directory.md) |
| Incluye la biblioteca de autenticación de Azure Active Directory (ADAL) y el instalador. | Ahora incluido en la instalación del controlador base, el instalador OLE DB actualizará las instalaciones existentes de la Biblioteca de autenticación de Microsoft Active Directory para SQL Server, y las quita de la lista de aplicaciones instaladas en Windows. |
| &nbsp; | &nbsp; |

### <a name="bugs-fixed"></a>Errores corregidos

| Error corregido | Detalles |
| :-------- | :------ |
| Se corrigió la lógica de DROP INDEX en [IIndexDefinition::DropIndex](/previous-versions/windows/desktop/ms722733(v=vs.85)). | Las versiones anteriores del controlador de OLE DB no pueden quitar un índice de clave principal cuando el identificador de esquema y el identificador de usuario del propietario del índice no son iguales. |
| &nbsp; | &nbsp; |

Para descargar las versiones anteriores del controlador OLE DB, haga clic en los vínculos de descarga de las secciones siguientes:

## <a name="1823"></a>18.2.3

![descargar](../../ssms/media/download-icon.png) [Descargar instalador x64](https://go.microsoft.com/fwlink/?linkid=2119554)  
![descargar](../../ssms/media/download-icon.png) [Descargar instalador x86](https://go.microsoft.com/fwlink/?linkid=2119738)  

Fecha de publicación: Junio de 2019

Si necesita descargar el instalador en un idioma distinto al que se ha detectado, puede usar estos vínculos directos.  
Para el controlador x64: [Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x40a)  
Para el controlador x86: [Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x40a)  

### <a name="features-added-in-1823"></a>Características agregadas en la versión 18.2.3

| Característica agregada | Detalles |
| :------------ | :------ |
| Compatibilidad con las actualizaciones de controladores desde medios extraíbles de SQL Server | Esta mejora permite actualizar controladores directamente desde medios extraíbles de SQL Server. |
| &nbsp; | &nbsp; |

## <a name="1822"></a>18.2.2

![descargar](../../ssms/media/download-icon.png) [Descargar instalador x64](https://go.microsoft.com/fwlink/?linkid=2118512)  
![descargar](../../ssms/media/download-icon.png) [Descargar instalador x86](https://go.microsoft.com/fwlink/?linkid=2118415)  

Fecha de publicación: Mayo de 2019

Si necesita descargar el instalador en un idioma distinto al que se ha detectado, puede usar estos vínculos directos.  
Para el controlador x64: [Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x40a)  
Para el controlador x86: [Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x40a)  

### <a name="bugs-fixed-in-1822"></a>Errores corregidos en la versión 18.2.2

| Error corregido | Detalles |
| :-------- | :------ |
| Se ha corregido un problema de interactividad en la autenticación de Azure Active Directory en contenedores multiproceso (MTA). | El controlador OLE DB 18.2.1 intenta cambiar por error el modelo de simultaneidad de COM en un contenedor previamente inicializado como multiproceso (MTA). Como resultado de esto, en una aplicación que realiza más de una llamada subsiguiente a [CoInitialize](/windows/win32/api/objbase/nf-objbase-coinitialize) o [CoInitializeEx](/windows/win32/api/combaseapi/nf-combaseapi-coinitializeex) antes de llamar a la interfaz [IDBInitialize::Initialize](/previous-versions/windows/desktop/ms718026(v=vs.85)), el controlador no puede establecer una conexión al usar cualquier modo de autenticación de Azure Active Directory. |
| &nbsp; | &nbsp; |

## <a name="1821"></a>18.2.1

![descargar](../../ssms/media/download-icon.png) [Descargar instalador x64](https://go.microsoft.com/fwlink/?linkid=2118511)  
![descargar](../../ssms/media/download-icon.png) [Descargar instalador x86](https://go.microsoft.com/fwlink/?linkid=2118278)  

Fecha de publicación: Febrero de 2019

Si necesita descargar el instalador en un idioma distinto al que se ha detectado, puede usar estos vínculos directos.  
Para el controlador x64: [Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x40a)  
Para el controlador x86: [Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x40a)  

### <a name="features-added-in-1821"></a>Características agregadas en la versión 18.2.1

| Característica agregada | Detalles |
| :------------ | :------ |
| Compatibilidad con la codificación de servidor UTF-8 | [Compatibilidad de UTF-8 con el controlador OLE DB para SQL Server](features/utf-8-support-in-oledb-driver-for-sql-server.md) |
| Compatibilidad con la autenticación de Azure Active Directory | [Uso de Azure Active Directory](features/using-azure-active-directory.md) |
| &nbsp; | &nbsp; |

## <a name="1810"></a>18.1.0

![descargar](../../ssms/media/download-icon.png) [Descargar instalador x64](https://go.microsoft.com/fwlink/?linkid=2118506)  
![descargar](../../ssms/media/download-icon.png) [Descargar instalador x86](https://go.microsoft.com/fwlink/?linkid=2118509)  

Fecha de publicación: Julio de 2018

Si necesita descargar el instalador en un idioma distinto al que se ha detectado, puede usar estos vínculos directos.  
Para el controlador x64: [Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x40a)  
Para el controlador x86: [Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2118509&2118509=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x40a)  

### <a name="features-added-in-1810"></a>Características agregadas en la versión 18.1.0

| Característica agregada | Detalles |
| :------------ | :------ |
| Compatibilidad con la palabra clave de cadena de conexión `UseFMTONLY` y la propiedad de inicialización `SSPROP_INIT_USEFMTONLY` | `UseFMTONLY` controla cómo se recuperan los metadatos al conectarse a [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones más recientes.<br/><br/>Para más información, consulte: [Uso de palabras clave de cadena de conexión con el controlador OLE DB para SQL Server](applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md). |
| &nbsp; | &nbsp; |

### <a name="bugs-fixed-in-1810"></a>Errores corregidos en la versión 18.1.0

| Error corregido | Detalles |
| :-------- | :------ |
| Se ha corregido la versión incorrecta del archivo de formato de BCP. | El controlador OLE DB 18.0 establece incorrectamente la versión del archivo de formato de BCP en 18.0, ya que el valor correcto es 11.0.<br/>El controlador OLE DB 18.1 no puede leer los archivos de formato generador por el controlador OLE DB 18.0.<br/>Si tiene que usar los archivos de formato generados por la versión anterior del controlador con el nuevo, puede editar manualmente los archivos para cambiar la versión a la 11.0. |
| &nbsp; | &nbsp; |

## <a name="1802"></a>18.0.2

![descargar](../../ssms/media/download-icon.png) [Descargar instalador x64](https://go.microsoft.com/fwlink/?linkid=2118504)  
![descargar](../../ssms/media/download-icon.png) [Descargar instalador x86](https://go.microsoft.com/fwlink/?linkid=2118277)  

Fecha de publicación: Marzo de 2018

Si necesita descargar el instalador en un idioma distinto al que se ha detectado, puede usar estos vínculos directos.  
Para el controlador x64: [Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x40a)  
Para el controlador x86: [Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x40a)  

### <a name="features-added-in-1802"></a>Características agregadas en la versión 18.0.2

| Característica agregada | Detalles |
| :------------ | :------ |
| Compatibilidad con la palabra clave de cadena de conexión `MultiSubnetFailover` y la propiedad de inicialización `SSPROP_INIT_MULTISUBNETFAILOVER`. | Para más información, consulte:<br/>&bull; &nbsp; [Compatibilidad de OLE DB Driver for SQL Server con la alta disponibilidad y la recuperación ante desastres](features/oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md)<br/>&bull; &nbsp; [Uso de palabras clave de cadena de conexión con OLE DB Driver for SQL Server](applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md). |
| &nbsp; | &nbsp; |

## <a name="see-also"></a>Consulte también

[Controlador Microsoft OLE DB para SQL Server](oledb-driver-for-sql-server.md)
