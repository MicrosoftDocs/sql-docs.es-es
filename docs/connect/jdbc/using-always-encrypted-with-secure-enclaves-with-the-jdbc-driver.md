---
description: Uso de Always Encrypted con enclaves seguros y el controlador JDBC
title: Uso de Always Encrypted con enclaves seguros con el controlador JDBC | Microsoft Docs
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 271c0438-8af1-45e5-b96a-4b1cabe32707
author: reneye
ms.author: v-reye
ms.openlocfilehash: 3970374537cc3491a0d8d624c3eccd31db0134e7
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076973"
---
# <a name="using-always-encrypted-with-secure-enclaves-with-the-jdbc-driver"></a>Uso de Always Encrypted con enclaves seguros con el controlador JDBC
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

En esta página se proporciona información sobre cómo desarrollar aplicaciones Java mediante [Always Encrypted con enclaves seguros](../../relational-databases/security/encryption/always-encrypted-enclaves.md) y Microsoft JDBC Driver 8.2 (o posterior) para SQL Server.

Enclaves seguros es una adición a la característica [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md) existente. El propósito de los enclaves seguros es solucionar las limitaciones cuando se trabaja con datos de Always Encrypted. Anteriormente, los usuarios solo podían realizar comparaciones de igualdad en los datos de Always Encrypted y tenían que recuperar y descifrar los datos para realizar otras operaciones. Los enclaves seguros solucionan esta limitación al permitir realizar cálculos en datos de texto no cifrado dentro de un enclave seguro en el lado del servidor. Un enclave seguro es una región de memoria protegida dentro del proceso de SQL Server y sirve como un entorno de ejecución de confianza para procesar información confidencial dentro del motor de SQL Server. Un enclave seguro aparece como una caja negra para el resto de SQL Server y otros procesos en la máquina servidor. No hay ninguna manera de ver los datos ni el código que se encuentran dentro del enclave desde el exterior, incluso si se cuenta con un depurador.

## <a name="prerequisites"></a>Requisitos previos
- Asegúrese de que Microsoft JDBC Driver 8.2 (o posterior) para SQL Server está instalado en el equipo de desarrollo.
- Compruebe que las dependencias del entorno, como los archivos DLL, KeyStores, etc., se encuentran en las rutas de acceso correctas. Always Encrypted con enclaves seguros es un complemento de la característica de [Always Encrypted](../../connect/jdbc/using-always-encrypted-with-the-jdbc-driver.md) existente y comparte requisitos previos similares.

> [!Note]
> Si usa una versión anterior de JDK 8, es posible que tenga que descargar e instalar los archivos de Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy. Asegúrese de leer el archivo Léame incluido en el archivo zip para obtener instrucciones de instalación y detalles relevantes sobre los posibles problemas de importación o exportación.  
>
> Los archivos de directivas se pueden descargar desde [Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy Files 8 Download](https://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html)

## <a name="setting-up-secure-enclaves"></a>Configuración de enclaves seguros
Siga [Tutorial: Introducción a Always Encrypted con enclaves seguros en SQL Server](../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md) o [Tutorial: Introducción a Always Encrypted con enclaves seguros en Azure SQL Database](/azure/azure-sql/database/always-encrypted-enclaves-getting-started), para empezar a trabajar con enclaves seguro. Para obtener información más detallada, consulte [Always Encrypted con enclaves seguros](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

## <a name="connection-string-properties"></a>Propiedades de cadena de conexión

Para habilitar los cálculos de enclave para una conexión de base de datos, debe establecer las siguientes palabras clave de cadena de conexión, además de habilitar Always Encrypted.

- **enclaveAttestationProtocol**: especifica un protocolo de atestación. 
  - Si utiliza [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] y el Servicio de protección de host (HGS), el valor de esta palabra clave debe ser `HGS`.
  - Si utiliza [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y Microsoft Azure Attestation, el valor de esta palabra clave debe ser `AAS`.

- **enclaveAttestationUrl**: especifica una dirección URL de atestación (un punto de conexión de servicio de atestación). Debe obtener una dirección URL de atestación para su entorno por parte del administrador del servicio de atestación.
  - Si usa [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] y el Servicio de protección de host (HGS), vea [Determinación y uso compartido de la dirección URL de atestación de HGS](../../relational-databases/security/encryption/always-encrypted-enclaves-host-guardian-service-deploy.md#step-6-determine-and-share-the-hgs-attestation-url).
  - Si usa [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y Microsoft Azure Attestation, vea [Determinación de la dirección URL de atestación de la directiva de atestación](/sql/relational-databases/security/encryption/always-encrypted-enclaves?view=sql-server-ver15#secure-enclave-attestation).

Los usuarios deben habilitar **columnEncryptionSetting** y establecer correctamente las **dos** propiedades de cadena de conexión anteriores para habilitar Always Encrypted con enclaves seguros desde el [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)].

## <a name="working-with-secure-enclaves"></a>Uso de los enclaves seguros
Cuando las propiedades de conexión de enclave se establecen correctamente, la característica funcionará de forma transparente. El controlador determinará de forma automática si la consulta tiene que usar un enclave seguro. Los siguientes son ejemplos de consultas que desencadenan cálculos de enclave. Puede encontrar la configuración de la base de datos y la tabla en [Tutorial: Introducción a Always Encrypted con enclaves seguros en SQL Server](../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md) o [Tutorial: Introducción a Always Encrypted con enclaves seguros en Azure SQL Database](/azure/azure-sql/database/always-encrypted-enclaves-getting-started).


Las consultas enriquecidas desencadenarán los cálculos de enclave:
```java
private static final String URL = "jdbc:sqlserver://<server>:<port>;user=<username>;password=<password>;databaseName=ContosoHR;columnEncryptionSetting=enabled;enclaveAttestationUrl=<attestation-url>;enclaveAttestationProtocol=<attestation-protocol>;";
try (Connection c = DriverManager.getConnection(URL)) {
    try (PreparedStatement p = c.prepareStatement("SELECT * FROM Employees WHERE SSN LIKE ?")) {
        p.setString(1, "%6818");
        try (ResultSet rs = p.executeQuery()) {
            while (rs.next()) {
                // Do work with data
            }
        }
    }
    
    try (PreparedStatement p = c.prepareStatement("SELECT * FROM Employees WHERE SALARY > ?")) {
        ((SQLServerPreparedStatement) p).setMoney(1, new BigDecimal(0));
        try (ResultSet rs = p.executeQuery()) {
            while (rs.next()) {
                // Do work with data
            }
        }
    }
}
```

Al alternar el cifrado en una columna, también se desencadenan los cálculos de enclave:
```java
private static final String URL = "jdbc:sqlserver://<server>:<port>;user=<username>;password=<password>;databaseName=ContosoHR;columnEncryptionSetting=enabled;enclaveAttestationUrl=<attestation-url>;enclaveAttestationProtocol=<attestation-protocol>;";
try (Connection c = DriverManager.getConnection(URL);Statement s = c.createStatement()) {
    s.executeUpdate("ALTER TABLE Employees ALTER COLUMN SSN CHAR(11) NULL WITH (ONLINE = ON)");
}
```

## <a name="java-8-users"></a>Usuarios de Java 8
Esta característica requiere el algoritmo de firma RSASSA-PSA. Este algoritmo se agregó en JDK 11, pero no se ha incorporado a JDK 8. Los usuarios que quieran usar esta característica con la versión JDK 8 del [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] deben cargar su propio proveedor, que admite el algoritmo de firma RSASSA-PSA, o bien incluir la dependencia opcional BouncyCastleProvider. La dependencia se quitará en una fecha posterior si JDK 8 incorpora el algoritmo de firma o si finaliza el ciclo de vida de soporte técnico de JDK 8.

## <a name="see-also"></a>Consulte también
[Usar Always Encrypted con el controlador JDBC](../../connect/jdbc/using-always-encrypted-with-the-jdbc-driver.md)  
