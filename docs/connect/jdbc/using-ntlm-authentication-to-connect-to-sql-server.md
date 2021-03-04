---
title: Uso de autenticación NTLM para conectarse con SQL Server
description: Obtenga información sobre cómo establecer una conexión de base de datos SQL mediante la autenticación NTLM con el controlador JDBC.
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.suite: sql
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: ''
author: lilgreenbird
ms.author: v-susanh
manager: kenvh
ms.openlocfilehash: a0d58875603781f4959ee8d10cb683c98bd884c1
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837475"
---
# <a name="using-ntlm-authentication-to-connect-to-sql-server"></a>Empleo de autenticación NTLM para conectar con SQL Server

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Permite que una aplicación use la propiedad de conexión **authenticationScheme** para indicar que desea conectarse a una base de datos mediante la autenticación NTLM v2. [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 

También se usan las siguientes propiedades para la autenticación NTLM:

- **domain = domainName** (opcional)
- **user = userName**
- **password = contraseña**
- **integratedSecurity = true**

Salvo **domain**, las otras propiedades son obligatorias. El controlador generará un error si falta alguna al usarse la propiedad authenticationScheme **NTLM**. 

Para más información sobre las propiedades de conexión, consulte [Establecimiento de las propiedades de conexión](../../connect/jdbc/setting-the-connection-properties.md). Para más información sobre el protocolo de autenticación NTLM de Microsoft, consulte [Microsoft NTLM](/windows/desktop/SecAuthN/microsoft-ntlm).

## <a name="remarks"></a>Observaciones

Consulte [Seguridad de red: nivel de autenticación de LAN Manager](/windows/security/threat-protection/security-policy-settings/network-security-lan-manager-authentication-level) para ver una descripción de la configuración de SQL Server, que controla el comportamiento de la autenticación NTLM. 

## <a name="logging"></a>Registro

Se ha agregado un nuevo registrador para admitir la autenticación NTLM: com.microsoft.sqlserver.jdbc.internals.NTLMAuthentication. Para obtener más información, vea [Hacer un seguimiento del funcionamiento del controlador](../../connect/jdbc/tracing-driver-operation.md).

## <a name="datasource"></a>DataSource

Al usar un origen de datos para crear conexiones, las propiedades de NTLM se pueden establecer mediante programación con **setAuthenticationScheme**, **setDomain**, y (opcionalmente) **setServerSpn**.

```java
SQLServerDataSource ds = new SQLServerDataSource();
ds.setServerName("<server>");
ds.setPortNumber(1433); // change if necessary
ds.setIntegratedSecurity(true);
ds.setAuthenticationScheme("NTLM");
ds.setDomain("<domainName>");
ds.setUser("<userName>");
ds.setPassword("<password>");
ds.setDatabaseName("<database>");
ds.setServerSpn("<serverSpn");

try (Connection c = ds.getConnection(); Statement s = c.createStatement();
        ResultSet rs = s.executeQuery("select auth_scheme from sys.dm_exec_connections where session_id=@@spid")) {
    while (rs.next()) {
        System.out.println("Authentication Scheme: " + rs.getString(1));
    }
}
```

## <a name="service-principal-names"></a>Nombres de entidades de seguridad de servicio

Un nombre principal de servicio (SPN) es el nombre por el que un cliente identifica de forma unívoca una instancia de un servicio.

Puede especificar el SPN con la propiedad de conexión **serverSpn** o dejar que el controlador lo genere automáticamente (el valor predeterminado). Esta propiedad tiene el formato "MSSQLSvc/fqdn:port\@REALM", donde fqdn es el nombre de dominio completo, port es el número de puerto y REALM es el dominio de SQL Server en letras mayúsculas. La parte correspondiente al dominio Kerberos de esta propiedad es opcional, ya que el dominio Kerberos predeterminado es el mismo que el dominio Kerberos del servidor.

Por ejemplo, el SPN podría ser: "MSSQLSvc/some-server.zzz.corp.contoso.com:1433"

Para obtener más información sobre los nombres de entidad de seguridad de servicio (SPN), vea:

- [Compatibilidad con Nombre de la entidad de seguridad de servicio (SPN) en conexiones cliente](../../relational-databases/native-client/features/service-principal-name-spn-support-in-client-connections.md)

> [!NOTE]  
> El atributo de conexión serverSpn solo es compatible con Microsoft JDBC Driver 4.2 y superior.

> Antes de la versión 6.2 del controlador JDBC, debe establecer **serverSpn** explícitamente. A partir de la versión 6.2, el controlador podrá compilar **serverSpn** de forma predeterminada, aunque también se puede usar **serverSpn** explícitamente.

## <a name="security-risks"></a>Riesgos de seguridad

El protocolo NTLM es un protocolo de autenticación antiguo con diversas vulnerabilidades, que suponen un riesgo de seguridad. Se basa en un esquema criptográfico relativamente débil y es vulnerable a distintos ataques. Se reemplaza por Kerberos, que es mucho más seguro y recomendado. La autenticación NTLM solo debe usarse en un entorno de confianza seguro o en caso de que no se pueda utilizar Kerberos.

[!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] solo admite NTLM v2, que cuenta con algunas mejoras de seguridad con respecto al protocolo v1 original. También se recomienda habilitar la protección ampliada o usar el cifrado SSL para aumentar la seguridad. 

Para obtener más información sobre cómo habilitar la protección ampliada, consulte:

- [Conectar al motor de base de datos con protección ampliada](../../database-engine/configure-windows/connect-to-the-database-engine-using-extended-protection.md)

Para obtener más información sobre cómo conectarse con el cifrado SSL, consulte:

- [Conexión con cifrado SSL](../../connect/jdbc/connecting-with-ssl-encryption.md)

> [!NOTE]
> En la versión 7,4, no **se** admite la habilitación de la protección ampliada y el cifrado.

## <a name="see-also"></a>Consulte también

[Conexión a SQL Server con el controlador JDBC](../../connect/jdbc/connecting-to-sql-server-with-the-jdbc-driver.md)
