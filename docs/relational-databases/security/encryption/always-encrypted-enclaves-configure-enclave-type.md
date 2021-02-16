---
title: Configuración del enclave seguro en SQL Server
description: Configure el enclave seguro para Always Encrypted con enclaves seguros en SQL Server.
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15 || = sqlallproducts-allversions'
ms.openlocfilehash: b2bd51dffb858f30e29b4a1b60e1c728afdbeb93
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067260"
---
# <a name="configure-the-secure-enclave-in-sql-server"></a>Configuración del enclave seguro en SQL Server

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

Antes de poder usar [Always Encrypted con enclaves seguros](always-encrypted-enclaves.md) en SQL Server, debe configurar la instancia para inicializar el enclave seguro durante el inicio. De forma predeterminada, SQL Server no inicializa el enclave seguro. Puede cambiarlo si establece la opción Configuración del servidor del **tipo de enclave de cifrado de columna** en el valor que represente un tipo de enclave válido para el entorno.

> [!NOTE]
> El rol responsable de la configuración del enclave seguro es el de DBA. Vea [Roles y responsabilidades al configurar la atestación con HGS](always-encrypted-enclaves-host-guardian-service-plan.md#roles-and-responsibilities-when-configuring-attestation-with-hgs).

El tipo de enclave admitido para [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] es la seguridad basada en la virtualización (VBS). Antes de configurar el tipo de enclave de VBS, se recomienda configurar la atestación con el Servicio de protección de host (HGS) para el equipo en el que se hospeda la instancia. Para empezar a trabajar con HGS, vea [Planificación de la atestación del Servicio de protección de host](always-encrypted-enclaves-host-guardian-service-plan.md). La configuración de la atestación también habilita la seguridad basada en la virtualización, que es necesaria para que un enclave de VBS se inicialice correctamente. Para obtener más información, vea [Verificación del funcionamiento de la seguridad basada en virtualización](always-encrypted-enclaves-host-guardian-service-register.md#step-2-verify-virtualization-based-security-is-running).

Para obtener instrucciones detalladas sobre cómo configurar el tipo de enclave, vea [Configuración del tipo de enclave como opción de configuración del servidor Always Encrypted](../../../database-engine/configure-windows/configure-column-encryption-enclave-type.md).

## <a name="next-steps"></a>Pasos siguientes

 [Administración de claves para Always Encrypted con enclaves seguros](always-encrypted-enclaves-manage-keys.md)

## <a name="see-also"></a>Consulte también  
 
 [Opciones de configuración de servidor (SQL Server)](../../../database-engine/configure-windows/server-configuration-options-sql-server.md)
