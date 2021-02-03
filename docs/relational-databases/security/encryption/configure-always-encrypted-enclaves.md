---
title: Configuración y uso de Always Encrypted con enclaves seguros | Microsoft Docs
description: Obtenga información sobre cómo configurar y usar Always Encrypted con enclaves seguros en SQL Server y Azure SQL Database, lo que permite una funcionalidad más completa con respecto a los datos confidenciales.
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 73a1c9f3a39ce51ce6ecc347af2e2eb0fb173fb6
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237298"
---
# <a name="configure-and-use-always-encrypted-with-secure-enclaves"></a>Configuración y uso de Always Encrypted con enclaves seguros 

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

[Always Encrypted con enclaves seguros](always-encrypted-enclaves.md) amplía la característica [Always Encrypted](always-encrypted-database-engine.md) existente para ofrecer una funcionalidad más completa en los datos confidenciales mientras mantiene la confidencialidad de estos. En este artículo se enumeran las tareas comunes para configurar y usar la característica.

Para obtener tutoriales en los que se muestra cómo empezar a trabajar con Always Encrypted con enclaves seguros, vea:

- [Tutorial: Introducción a Always Encrypted con enclaves seguros en SQL Server](../tutorial-getting-started-with-always-encrypted-enclaves.md)
- [Tutorial: Introducción a Always Encrypted con enclaves seguros en Azure SQL Database](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)

## <a name="set-up-the-secure-enclave-and-attestation"></a>Configuración del enclave seguro y la atestación

Antes de poder usar Always Encrypted con enclaves seguros, debe configurar el entorno para asegurarse de que el enclave seguro está disponible para la base de datos. También debe configurar la [atestación del enclave](always-encrypted-enclaves.md#secure-enclave-attestation). 

El proceso de configuración del entorno depende de si usa [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] o [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)].

### <a name="set-up-the-secure-enclave-and-attestation-in-ssnoversion-md"></a>Configuración del enclave seguro y la atestación en [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]

Para obtener detalles, vea los siguientes artículos:
- [Planificación de la atestación del Servicio de protección de host](./always-encrypted-enclaves-host-guardian-service-plan.md)
- [Implementación del Servicio de protección de host para [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]](./always-encrypted-enclaves-host-guardian-service-deploy.md)
- [Registro del equipo con el Servicio de protección de host](./always-encrypted-enclaves-host-guardian-service-register.md)
- [Configuración del enclave seguro en SQL Server](always-encrypted-enclaves-configure-enclave-type.md)

### <a name="set-up-the-secure-enclave-and-attestation-in-sssdsfull"></a>Configuración del enclave seguro y la atestación en [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)]

Para obtener detalles, vea los siguientes artículos:
- [Plan de atestación y enclaves de Intel SGX en [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)]](/azure/azure-sql/database/always-encrypted-enclaves-plan)
- [Habilitación de Intel SGX para la base de datos de Azure SQL](/azure/azure-sql/database/always-encrypted-enclaves-enable-sgx)
- [Configuración de Azure Attestation para el servidor lógico de Azure SQL Database](/azure/azure-sql/database/always-encrypted-enclaves-configure-attestation)

## <a name="manage-keys-for-always-encrypted-with-secure-enclaves"></a>Administración de claves para Always Encrypted con enclaves seguros
Consulte los artículos siguientes para más información:
- [Información general de la administración de claves para Always Encrypted con enclaves seguros](always-encrypted-enclaves-manage-keys.md)
- [Aprovisionamiento de claves habilitadas para el enclave](always-encrypted-enclaves-provision-keys.md)
- [Rotación de claves habilitadas para el enclave](always-encrypted-enclaves-rotate-keys.md)

## <a name="configure-columns-with-always-encrypted-with-secure-enclaves"></a>Configuración de columnas mediante Always Encrypted con enclaves seguros
Consulte los artículos siguientes para más información:
- [Información general de la configuración del cifrado de columna en contexto mediante Always Encrypted con enclaves seguros](always-encrypted-enclaves-configure-encryption.md)
- [Configuración del cifrado de columnas en contexto con Transact-SQL](always-encrypted-enclaves-configure-encryption-tsql.md)
- [Uso de Always Encrypted con enclaves seguros para las columnas cifradas existentes](always-encrypted-enclaves-enable-for-encrypted-columns.md)

## <a name="run-transact-sql-statements-using-secure-enclaves"></a>Configuración y uso de Always Encrypted con enclaves seguros
Consulte los artículos siguientes para más información:
- [Configuración y uso de Always Encrypted con enclaves seguros](always-encrypted-enclaves-query-columns.md)
- [Solución de problemas comunes de Always Encrypted con enclaves seguros](always-encrypted-enclaves-troubleshooting.md)

## <a name="create-and-use-indexes-on-enclave-enabled-columns"></a>Creación y uso de índices en columnas habilitadas para el enclave
Consulte los artículos siguientes para más información:
- [Creación y uso de índices en columnas mediante Always Encrypted con enclaves seguros](always-encrypted-enclaves-create-use-indexes.md)
  
## <a name="develop-applications-using-always-encrypted-with-secure-enclaves"></a>Desarrollo de aplicaciones mediante Always Encrypted con enclaves seguros
Consulte los artículos siguientes para más información:
- [Desarrollo de aplicaciones mediante Always Encrypted con enclaves seguros](always-encrypted-enclaves-client-development.md)
