---
description: Desarrollo de aplicaciones mediante Always Encrypted con enclaves seguros
title: Desarrollo de aplicaciones mediante Always Encrypted con enclaves seguros | Microsoft Docs
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
dev_langs:
- CSharp
ms.assetid: 9595eb66-284c-4474-828f-8961a05ce989
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 64c628172ba95ff9de546c018ea00a9ba63943c9
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99075607"
---
# <a name="develop-applications-using-always-encrypted-with-secure-enclaves"></a>Desarrollo de aplicaciones mediante Always Encrypted con enclaves seguros
[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

[Always Encrypted con enclaves seguros](always-encrypted-enclaves.md) amplía [Always Encrypted](always-encrypted-database-engine.md) para habilitar funcionalidades más completas de las consultas de la aplicación en columnas de bases de datos confidenciales cifradas. Aprovecha las tecnologías de enclaves seguros para permitir que el ejecutor de las consultas en [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] delegue los cálculos de las columnas cifradas en un enclave seguro dentro del proceso de [!INCLUDE[ssde-md](../../../includes/ssde-md.md)].

## <a name="prerequisites"></a>Requisitos previos

- La instancia de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] o la base de datos y el servidor en [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] deben estar configurados correctamente para admitir los enclaves y la atestación. Para obtener más información, vea [Configuración del enclave seguro y la atestación](configure-always-encrypted-enclaves.md#set-up-the-secure-enclave-and-attestation).
- Debe obtener una dirección URL de atestación para su entorno por parte del administrador del servicio de atestación.

  - Si usa [!INCLUDE[ssnoversion-md](../../../includes/ssnoversion-md.md)] y el Servicio de protección de host (HGS), vea [Determinación y uso compartido de la dirección URL de atestación de HGS](../../../relational-databases/security/encryption/always-encrypted-enclaves-host-guardian-service-deploy.md#step-6-determine-and-share-the-hgs-attestation-url).
  - Si usa [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] y Microsoft Azure Attestation, vea [Determinación de la dirección URL de atestación de la directiva de atestación](/sql/relational-databases/security/encryption/always-encrypted-enclaves?view=sql-server-ver15#secure-enclave-attestation).

- La aplicación debe usar una versión del controlador cliente de SQL que admita enclaves seguros. Vea las secciones siguientes para obtener más información.

- Debe configurar un protocolo de atestación y una dirección URL de atestación para una conexión de base de datos. Los detalles sobre cómo configurar estos dos elementos dependen del controlador cliente que use.

## <a name="client-drivers-for-always-encrypted-with-secure-enclaves"></a>Controladores cliente para Always Encrypted con enclaves seguros

Para desarrollar aplicaciones mediante Always Encrypted con enclaves seguros, necesita una versión del controlador de cliente de SQL que admita los enclaves seguros. El controlador de cliente desempeña un papel fundamental:

- Antes de enviar una consulta que usa un enclave seguro a [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] para su ejecución, el controlador inicia la atestación del enclave para comprobar que el enclave seguro es de confianza y que se puede usar de forma segura para procesar datos confidenciales. Para obtener más información sobre la atestación, consulte [Atestación de un enclave seguro](always-encrypted-enclaves.md#secure-enclave-attestation).
- Una vez que la atestación se ha realizado correctamente, el controlador de cliente negocia un secreto compartido para establecer una sesión segura con el enclave.
- El controlador usa el secreto compartido para cifrar las claves de cifrado de columna que el enclave necesitará para procesar la consulta y envía las claves a [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], que las reenvía al enclave seguro que descifra las claves. 
- Por último, el controlador envía la consulta para su ejecución, lo que desencadena los cálculos dentro del enclave seguro.

## <a name="next-steps"></a>Pasos siguientes

Los siguientes controladores de cliente admiten Always Encrypted con enclaves seguros:

- Proveedor de datos de Microsoft .NET para SQL Server en .NET Framework 4.6 o posterior, y .NET Core 2.1 o posterior. 
    - Para más información, vea [Uso de Always Encrypted con el proveedor de datos Microsoft .NET Framework para SQL Server](../../../connect/ado-net/sql/sqlclient-support-always-encrypted.md).
    - Para seguir un tutorial paso a paso, consulte [Tutorial: Desarrolle una aplicación de .NET mediante Always Encrypted con enclaves seguros](../../../connect/ado-net/sql/tutorial-always-encrypted-enclaves-develop-net-apps.md).
    - Además, vea [Ejemplo que muestra el uso del proveedor de Azure Key Vault con Always Encrypted habilitado con enclaves seguros](../../../connect/ado-net/sql/azure-key-vault-enclave-example.md).
- Microsoft ODBC Driver for SQL Server, versión 17.4 o superior. 
    - Para obtener más información, vea [Uso de Always Encrypted con ODBC Driver](../../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md). 
    - Para obtener información sobre cómo habilitar los cálculos de enclave para una conexión de base de datos mediante ODBC, vea la sección [Habilitación de Always Encrypted con enclaves seguros](../../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#enabling-always-encrypted-with-secure-enclaves).
- Microsoft JDBC Driver para SQL Server, versión 8.2 o superior.
    - Para obtener más información, vea [Uso de Always Encrypted con enclaves seguros con el controlador JDBC](../../../connect/jdbc/using-always-encrypted-with-secure-enclaves-with-the-jdbc-driver.md).
- Proveedor de datos .NET Framework para SQL Server en .NET Framework 4.7.2 o superior. 
    - Para obtener más información, consulte [Uso de Always Encrypted con el proveedor de datos .NET Framework para SQL Server](../../../relational-databases/security/encryption/develop-using-always-encrypted-with-net-framework-data-provider.md).
    - Para seguir un tutorial paso a paso, consulte [Tutorial: Desarrollo de una aplicación de .NET Framework mediante Always Encrypted con enclaves seguros](../tutorial-always-encrypted-enclaves-develop-net-framework-apps.md)

## <a name="see-also"></a>Consulte también

- [Solución de problemas comunes de Always Encrypted con enclaves seguros](always-encrypted-enclaves-troubleshooting.md)
