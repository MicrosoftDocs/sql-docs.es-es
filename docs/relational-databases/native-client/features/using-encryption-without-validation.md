---
title: Uso del cifrado sin validación | Microsoft Docs
description: Obtenga información sobre cómo el proveedor de OLE DB de SQL Server Native Client y el controlador ODBC admiten el cifrado sin validación y recomendaciones sobre cuándo usarlos.
ms.custom: ''
ms.date: 12/21/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- data access [SQL Server Native Client], encryption
- cryptography [SQL Server Native Client]
- SQLNCLI, encryption
- encryption [SQL Server Native Client]
- SQL Server Native Client, encryption
ms.assetid: f4c63206-80bb-4d31-84ae-ccfcd563effa
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 09cccfb889bb2eae2b80c2c466eaed1a96520be8
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462006"
---
# <a name="using-encryption-without-validation-in-sql-server-native-client"></a>Usar el cifrado sin validación en SQL Server Native Client
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] cifra siempre los paquetes de red asociados al inicio de sesión. Si no se ha proporcionado ningún certificado en el servidor cuando este se inicia, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] genera un certificado autofirmado que se utiliza para cifrar los paquetes de inicio de sesión.  

Los certificados autofirmados no garantizan la seguridad. El protocolo de enlace cifrado se basa en NT LAN Manager (NTLM). Se recomienda encarecidamente que aprovisione un certificado comprobable en SQL Server para la conectividad segura. La seguridad de la capa de transporte (TLS) solo se puede proteger con la validación de certificados.

Las aplicaciones pueden solicitar también el cifrado de todo el tráfico de red mediante palabras clave de cadenas de conexión o propiedades de conexión. Las palabras clave son "Encrypt" para ODBC y OLE DB al usar una cadena de proveedor con **IDbInitialize:: Initialize** o "usar cifrado para datos" para ADO y OLE DB cuando se usa una cadena de inicialización con **IDataInitialize**. También puede configurarse mediante Configuration Manager de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] mediante la opción **Forzar cifrado de protocolo** y al configurar el cliente para que solicite conexiones cifradas. De forma predeterminada, el cifrado de todo el tráfico de red de una conexión requiere que se proporcione un certificado en el servidor. Al configurar el cliente para que confíe en el certificado del servidor, podría ser vulnerable a ataques de tipo "Man in the middle". Si implementa un certificado comprobable en el servidor, asegúrese de cambiar la configuración de cliente acerca de confiar en el certificado en FALSE.

Para obtener información sobre las palabras clave de cadena de conexión, vea [usar palabras clave de cadena de conexión con SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
 Para permitir usar el cifrado cuando no se ha proporcionado un certificado en el servidor, se puede usar el Administrador de configuración de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] para establecer las opciones **Forzar cifrado de protocolo** y **Confiar en certificado de servidor**. En ese caso, el cifrado utilizará un certificado de servidor autofirmado sin validación si no se ha proporcionado ningún certificado comprobable en el servidor.  
  
 Las aplicaciones también pueden utilizar la palabra clave "TrustServerCertificate" o su atributo de conexión asociado para garantizar que se realiza el cifrado. La configuración de las aplicaciones nunca reduce el nivel de seguridad establecido por el Administrador de configuración cliente de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], pero sí puede reforzarlo. Por ejemplo, si **Forzar cifrado de protocolo** no está establecido para el cliente, una aplicación puede solicitar su propio cifrado. Para garantizar el cifrado incluso cuando no se ha proporcionado un certificado de servidor, una aplicación puede solicitar el cifrado y "TrustServerCertificate". Sin embargo, si "TrustServerCertificate" no está habilitado en la configuración del cliente, se requiere igualmente un certificado de servidor. En la tabla siguiente se describen todos los casos:  
  
|Cliente configurado con Forzar cifrado de protocolo|Cliente configurado con Confiar en certificado de servidor|Cadena/atributo de conexión Encrypt/Use Encryption for Data|Cadena/atributo de conexión Trust Server Certificate|Resultado|  
|----------------------------------------------|---------------------------------------------|------------------------------------------------------------------------------|----------------------------------------------------------------------|------------|  
|No|N/D|No (valor predeterminado)|Omitido|No se produce el cifrado.|  
|No|N/D|Sí|No (valor predeterminado)|El cifrado solamente se produce si hay un certificado de servidor comprobable; de lo contrario, se produce un error en el intento de conexión.|  
|No|N/D|Sí|Sí|El cifrado se produce siempre, pero puede que se utilice un certificado de servidor autofirmado.|  
|Sí|No|Omitido|Omitido|El cifrado solamente se produce si hay un certificado de servidor comprobable; de lo contrario, se produce un error en el intento de conexión.|  
|Sí|Sí|No (valor predeterminado)|Omitido|El cifrado se produce siempre, pero puede que se utilice un certificado de servidor autofirmado.|  
|Sí|Sí|Sí|No (valor predeterminado)|El cifrado solamente se produce si hay un certificado de servidor comprobable; de lo contrario, se produce un error en el intento de conexión.|  
|Sí|Sí|Sí|Sí|El cifrado se produce siempre, pero es posible que se use un certificado de servidor autofirmado.|  
||||||

> [!CAUTION]
> En la tabla anterior solo se proporciona una guía sobre el comportamiento del sistema en distintas configuraciones. Para lograr una conectividad segura, asegúrese de que tanto el cliente como el servidor requieren cifrado. Asegúrese también de que el servidor tiene un certificado comprobable y de que el valor **TrustServerCertificate** en el cliente se establece en FALSE.

## <a name="sql-server-native-client-ole-db-provider"></a>Proveedor OLE DB de SQL Server Native Client  
 El [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] proveedor de OLE DB de Native Client admite el cifrado sin validación mediante la adición de la propiedad de inicialización de origen de datos SSPROP_INIT_TRUST_SERVER_CERTIFICATE, que se implementa en el conjunto de propiedades DBPROPSET_SQLSERVERDBINIT. Además, se ha agregado una nueva palabra clave de cadena de conexión, "TrustServerCertificate". Acepta valores sí o no; no es el valor predeterminado. Cuando se utilizan componentes de servicio, acepta valores true o false; false es el valor predeterminado.  
  
 Para más información sobre las mejoras realizadas en el conjunto de propiedades DBPROPSET_SQLSERVERDBINIT, consulte [Propiedades de inicialización y autorización](../../../relational-databases/native-client-ole-db-data-source-objects/initialization-and-authorization-properties.md).  
  
## <a name="sql-server-native-client-odbc-driver"></a>Controlador ODBC de SQL Server Native Client  
 El [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] controlador ODBC de Native Client admite el cifrado sin validación mediante adiciones a las funciones [SQLSetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) y [SQLGetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md) . SQL_COPT_SS_TRUST_SERVER_CERTIFICATE se ha agregado para aceptar SQL_TRUST_SERVER_CERTIFICATE_YES o SQL_TRUST_SERVER_CERTIFICATE_NO, siendo SQL_TRUST_SERVER_CERTIFICATE_NO el valor predeterminado. Además, se ha agregado una nueva palabra clave de cadena de conexión, "TrustServerCertificate". Acepta valores sí o no; "no" es el valor predeterminado.  
  
## <a name="see-also"></a>Consulte también  
 [Características de SQL Server Native Client](../../../relational-databases/native-client/features/sql-server-native-client-features.md)  
  
  
