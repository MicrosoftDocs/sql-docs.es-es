---
title: Tipo de conexión de Oracle (SSRS y Power BI Report Server)
description: Use la información de este artículo sobre el tipo de conexión de Oracle para obtener información sobre cómo crear un origen de datos.
ms.date: 11/03/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.assetid: 9db86dd2-beda-42d8-8af7-2629d58a8e3d
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: bc812447f38575375de10cea13d0376e879375c5
ms.sourcegitcommit: 0b400bb99033f4b836549cb11124a1f1630850a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2021
ms.locfileid: "99978427"
---
# <a name="oracle-connection-type-ssrs--power-bi-report-server"></a>Tipo de conexión de Oracle (SSRS y Power BI Report Server)

Para utilizar en el informe los datos de una base de datos de Oracle, debe tener un conjunto de datos basado en un origen de datos de informe de tipo Oracle. Este tipo de origen de datos integrado usa el proveedor de datos de Oracle directamente y requiere un componente de software del cliente Oracle. En este artículo se explica cómo descargar e instalar controladores para Reporting Services, Power BI Report Server, Generador de informes y Power BI Desktop.


Use la información de este artículo para crear un origen de datos. Para obtener instrucciones paso a paso, vea [Agregar y comprobar una conexión de datos o un origen de datos &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md). 

> [!IMPORTANT]
> Los siguientes comandos que usan la herramienta OraProvCfg.exe de Oracle para registrar los controladores de ODP.NET administrados y no administrados de Oracle se proporcionan como ejemplos para su uso con los productos de Microsoft mencionados anteriormente. Para configurar los controladores de ODP.NET específicos para su entorno, puede que tenga que ponerse en contacto con el servicio de soporte técnico de Oracle o consultar la documentación de Oracle para [configurar el proveedor de datos de Oracle para .NET](https://docs.oracle.com/en/database/oracle/oracle-database/19/odpnt/InstallConfig.html#GUID-1F689B90-2CC4-4907-B8FE-A5F4EE36F673).

## <a name="64-bit-drivers-for-the-report-servers"></a>Controladores de 64 bits para los servidores de informes

En el sitio de descarga de Oracle, instale [Oracle Universal Installer (OUI) de ODAC para Oracle de 64 bits](https://www.oracle.com/technetwork/topics/dotnet/downloads/odacdeploy-4242173.html). Solo tiene que seguir estos pasos si usa los controladores 12.2 de Oracle ODAC y versiones posteriores. De lo contrario, de forma predeterminada para una nueva instalación doméstica de Oracle, se utiliza una configuración con la que no se aplica a todo el equipo. En estos pasos se asume que ha instalado los archivos de ODAC 18.x en la carpeta c:\oracle64.


### <a name="paginated-rdl-reports-use-managed-odpnet"></a>Los informes paginados (RDL) usan ODP.NET administrado

En Power BI Report Server y SQL Server Reporting Services 2016 y versiones posteriores se usa **ODP.NET administrado** para los informes paginados (RDL). Siga estos pasos para registrar ODP.NET administrado:

1. Registre el cliente de ODP.NET administrado en GAC:

   ```
   C:\oracle64\product\18.0.0\client_1\odp.net\bin\4\OraProvCfg.exe /action:gac /providerpath:C:\oracle64\product\18.0.0\client_1\odp.net\managed\common\Oracle.ManagedDataAccess.dll
   ```

2. Agregue las entradas del cliente de ODP.NET administrado al archivo machine.config:

   ```
   C:\oracle64\product\18.0.0\client_1\odp.net\bin\4\OraProvCfg.exe /action:config /force /product:odpm /frameworkversion:v4.0.30319 /providerpath:C:\oracle64\product\18.0.0\client_1\odp.net\managed\common\Oracle.ManagedDataAccess.dll
   ```

### <a name="power-bi-reports-use-unmanaged-odpnet"></a>Los informes de Power BI usan ODP.NET no administrado

Power BI Report Server usa **ODP.NET no administrado** para los informes de Power BI. Siga estos pasos para registrar ODP.NET no administrado:

1. Registre el cliente de ODP.NET no administrado en GAC:

   ```
   C:\oracle64\product\18.0.0\client_1\odp.net\bin\4\OraProvCfg.exe /action:gac /providerpath:C:\oracle64\product\18.0.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll
   ```
2. Agregue las entradas del cliente de ODP.NET no administrado al archivo machine.config:

   ```
   C:\oracle64\product\18.0.0\client_1\odp.net\bin\4\OraProvCfg.exe /action:config /force /product:odp /frameworkversion:v4.0.30319 /providerpath:C:\oracle64\product\18.0.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll
   ```

## <a name="32-bit-drivers-for-report-builder"></a>Controladores de 32 bits para Generador de informes

Report Builder usa **ODP.NET administrado** para la creación de informes paginados (RDL). Solo tiene que seguir estos pasos si usa los controladores 12.2 de Oracle ODAC y versiones posteriores. De lo contrario, de forma predeterminada para una nueva instalación doméstica de Oracle, se utiliza una configuración con la que no se aplica a todo el equipo. En estos pasos se asume que ha instalado los archivos de ODAC 18.x en la carpeta c:\oracle32, donde está instalado Report Builder. Siga estos pasos para registrar ODP.NET administrado:

1. En el sitio de descarga de Oracle, instale [Oracle Universal Installer (OUI) de ODAC para Oracle de 32 bits](https://www.oracle.com/technetwork/topics/dotnet/downloads/odacdev-4242174.html).

2. Registre el cliente de ODP.NET administrado en GAC:

   ```
   C:\oracle32\product\18.0.0\client_1\odp.net\bin\4\OraProvCfg.exe /action:gac /providerpath:C:\oracle32\product\18.0.0\client_1\odp.net\managed\common\Oracle.ManagedDataAccess.dll
   ```

3. Agregue las entradas del cliente de ODP.NET administrado al archivo machine.config:

   ```
   C:\oracle32\product\18.0.0\client_1\odp.net\bin\4\OraProvCfg.exe /action:config /force /product:odpm /frameworkversion:v4.0.30319 /providerpath:C:\oracle32\product\18.0.0\client_1\odp.net\managed\common\Oracle.ManagedDataAccess.dll
   ```

## <a name="64-bit-and-32-bit-drivers-for-power-bi-desktop"></a>Controladores de 64 bits y de 32 bits para Power BI Desktop

Power BI Desktop usa **ODP.NET no administrado** para la creación de informes de Power BI. Solo tiene que seguir estos pasos si usa los controladores 12.2 de Oracle ODAC y versiones posteriores. De lo contrario, de forma predeterminada para una nueva instalación doméstica de Oracle, se utiliza una configuración con la que no se aplica a todo el equipo. En estos pasos se asume que ha instalado los archivos de ODAC 18.x en la carpeta c:\oracle64 para la versión de 64 bits de Power BI Desktop o en la carpeta c:\oracle32 para la versión de 32 bits de Power BI Desktop. Siga estos pasos para registrar ODP.NET no administrado:

### <a name="64-bit-power-bi-desktop"></a>Power BI Desktop de 64 bits

1. En el sitio de descarga de Oracle, instale [Oracle Universal Installer (OUI) de ODAC para Oracle de 64 bits](https://www.oracle.com/technetwork/topics/dotnet/downloads/odacdeploy-4242173.html).
2. Registre el cliente de ODP.NET no administrado en GAC:

   ```
   C:\oracle64\product\18.0.0\client_1\odp.net\bin\4\OraProvCfg.exe /action:gac /providerpath:C:\oracle64\product\18.0.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll
   ```

3. Agregue las entradas del cliente de ODP.NET no administrado al archivo machine.config:

   ```
   C:\oracle64\product\18.0.0\client_1\odp.net\bin\4\OraProvCfg.exe /action:config /force /product:odp /frameworkversion:v4.0.30319 /providerpath:C:\oracle64\product\18.0.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll
   ```

### <a name="32-bit-power-bi-desktop"></a>Power BI Desktop de 32 bits

1. En el sitio de descarga de Oracle, instale [Oracle Universal Installer (OUI) de ODAC para Oracle de 32 bits](https://www.oracle.com/technetwork/topics/dotnet/downloads/odacdev-4242174.html).

2. Registre el cliente de ODP.NET no administrado en GAC:

   ```
   C:\oracle32\product\18.0.0\client_1\odp.net\bin\4\OraProvCfg.exe /action:gac /providerpath:C:\oracle32\product\18.0.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll
   ```

3. Agregue las entradas del cliente de ODP.NET no administrado al archivo machine.config:

   ```
   C:\oracle32\product\18.0.0\client_1\odp.net\bin\4\OraProvCfg.exe /action:config /force /product:odp /frameworkversion:v4.0.30319 /providerpath:C:\oracle32\product\18.0.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll
   ```

##  <a name="connection-string"></a><a name="Connection"></a> Cadena de conexión  

Póngase en contacto con el administrador de bases de datos y solicite la información de conexión y las credenciales que debe usar para conectar con el origen de datos. En el siguiente ejemplo de cadena de conexión, se especifica una base de datos de Oracle en el servidor denominado "Oracle18" mediante Unicode. El nombre del servidor debe coincidir con el nombre de instancia del servidor de Oracle definido en el archivo de configuración Tnsnames.ora.  
  
```  
Data Source="Oracle"; Unicode="True"  
```  
  
Para más ejemplos de cadenas de conexión, consulte [Creación de cadenas de conexión de datos - Generador de informes y SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).  
  
##  <a name="credentials"></a><a name="Credentials"></a> Credenciales  
Se necesitan credenciales para ejecutar consultas y obtener una vista previa del informe localmente y desde el servidor de informes.  
  
Después de publicar el informe, es posible que necesite cambiar las credenciales para el origen de datos de tal forma que, cuando el informe se ejecute en el servidor de informes, los permisos para recuperar los datos sean válidos.  
  
Para más información, consulte [Especificar información de credenciales y conexión para los orígenes de datos de informes](specify-credential-and-connection-information-for-report-data-sources.md).  
  
##  <a name="queries"></a><a name="Query"></a> Consultas  
Para crear un conjunto de datos, se puede seleccionar un procedimiento almacenado en una lista desplegable o se puede crear una consulta SQL. Para generar una consulta, debe usar el diseñador de consultas basado en texto. Para más información, vea [Interfaz de usuario del Diseñador de consultas basado en texto &#40;Generador de informes&#41;](../../reporting-services/report-data/text-based-query-designer-user-interface-report-builder.md).  
  
Puede especificar los procedimientos almacenados que solo devuelven un conjunto de resultados. No se admiten las consultas basadas en cursor.  
  
##  <a name="parameters"></a><a name="Parameters"></a> Parámetros  

Si la consulta incluye las variables de consulta, se generan automáticamente los parámetros de informe correspondientes. Esta extensión admite los parámetros con nombre. En Oracle versión 9 o posterior, se admiten los parámetros de varios valores.  
  
 Los parámetros de informe se crean con valores de propiedad predeterminados que quizá necesite modificar. Por ejemplo, los parámetro de informe son un tipo de datos **Texto**. Una vez creados los parámetros de informe, podría suceder que tenga que cambiar los valores predeterminados. Para más información, vea [Parámetros de informe &#40;Generador de informes y Diseñador de informes&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md).  
  
##  <a name="remarks"></a><a name="Remarks"></a> Comentarios  

Antes de que pueda conectar con un origen de datos de Oracle, el administrador del sistema debe tener instalada la versión del proveedor de datos .NET para Oracle que permite la recuperación de datos desde la base de datos de Oracle. Este proveedor de datos debe estar instalado en el mismo equipo que el Generador de informes, además de en el servidor de informes.  
  
Para más información, consulte los siguientes artículos.  
  
- [Cómo usar Reporting Services para tener acceso a un origen de datos de Oracle y configurarlo](/archive/blogs/dataaccesstechnologies/configure-oracle-data-source-for-sql-server-reporting-services-ssdt-and-report-server)  
- [Cómo agregar permisos para la entidad de seguridad NETWORK SERVICE](https://mskb.pkisolutions.com/kb/870668)  
  
### <a name="alternate-data-extensions"></a>Extensiones de datos alternativas 

También puede recuperar los datos de una base de datos de Oracle utilizando un tipo de origen de datos OLE DB. Para obtener más información, vea [Tipo de conexión OLE DB &#40;SSRS&#41;](../../reporting-services/report-data/ole-db-connection-type-ssrs.md).  

::: moniker range=">=sql-server-2017"

### <a name="report-models"></a>Modelos de informe

 También se pueden crear modelos basados en una base de datos de Oracle.  
::: moniker-end

### <a name="platform-and-version-information"></a>Información de plataforma y de versión  

Para más información sobre la compatibilidad con plataformas y versiones, vea [Orígenes de datos admitidos por Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md).  

## <a name="see-also"></a>Consulte también

[Parámetros de informe &#40;Generador de informes y Diseñador de informes&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   

[Filtrar, agrupar y ordenar datos &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   

[Expresiones &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)
