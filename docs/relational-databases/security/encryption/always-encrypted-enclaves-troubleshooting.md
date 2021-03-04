---
title: Solución de problemas comunes de Always Encrypted con enclaves seguros
description: Solución de problemas comunes de Always Encrypted con enclaves seguros
ms.custom: ''
ms.date: 01/15/2021
ms.reviewer: vanto
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: security
ms.topic: how-to
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15 || = sqlallproducts-allversions'
ms.openlocfilehash: dc50006d17228bf3b5ec03c2a672ed113260bf3b
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839265"
---
# <a name="troubleshoot-common-issues-for-always-encrypted-with-secure-enclaves"></a>Solución de problemas comunes de Always Encrypted con enclaves seguros

En este artículo se describe cómo identificar y resolver problemas comunes que puede encontrar al ejecutar instrucciones de Transact-SQL (TSQL) mediante [Always Encrypted con enclaves seguros](always-encrypted-enclaves.md).

Para obtener información sobre cómo ejecutar consultas mediante enclaves seguros, vea [Ejecución de instrucciones de Transact-SQL con enclaves seguros](always-encrypted-enclaves-query-columns.md).

## <a name="database-connection-errors"></a>Errores de conexión de base de datos

Para ejecutar instrucciones mediante un enclave seguro, debe habilitar Always Encrypted y especificar una dirección URL de atestación para la conexión de base de datos, como se explica en [Requisitos previos para ejecutar instrucciones con enclaves seguros](always-encrypted-enclaves-query-columns.md#prerequisites-for-running-statements-using-secure-enclaves). Pero se producirá un error en la conexión si especifica una dirección URL de atestación pero la base de datos en [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] o la instancia de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] de destino no es compatible con los enclaves seguros, o bien no se ha configurado correctamente.

- Si usa [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)], compruebe que en la base de datos se utiliza la configuración de hardware de la [serie DC](/azure/azure-sql/database/service-tiers-vcore?tabs=azure-portal#dc-series). Para obtener más información, vea [Habilitación de Intel SGX para la base de datos de Azure SQL](/azure/azure-sql/database/always-encrypted-enclaves-enable-sgx).
- Si usa [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)], compruebe que el enclave seguro está configurado correctamente para la instancia. Para obtener más información, vea [Configuración del enclave seguro en SQL Server](always-encrypted-enclaves-configure-enclave-type.md).

## <a name="attestation-errors-when-using-microsoft-azure-attestation"></a>Errores de atestación al usar Microsoft Azure Attestation

> [!NOTE]
> Esta sección solo se aplica a [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)].

Antes de que un controlador cliente envíe una instrucción T-SQL al servidor lógico de Azure SQL para su ejecución, el controlador desencadena el siguiente flujo de trabajo de atestación de enclave mediante Microsoft Azure Attestation.

1. El controlador cliente pasa la dirección URL de atestación, especificada en la conexión de base de datos, al servidor lógico de Azure SQL.
2. El servidor lógico de Azure SQL recopila la evidencia sobre el enclave, su entorno de hospedaje y el código que se ejecuta dentro del enclave. Después, el servidor envía una solicitud de atestación al proveedor de atestación, al que se hace referencia en la dirección URL de atestación.
3. El proveedor de atestación valida la evidencia con respecto a la directiva configurada y emite un token de atestación para el servidor lógico de Azure SQL. El proveedor de atestación firma el token de atestación con su clave privada.
4. El servidor lógico de Azure SQL envía el token de atestación al controlador cliente.
5. El cliente se pone en contacto con el proveedor de atestación en la dirección URL de atestación especificada para recuperar su clave pública y comprueba la firma en el token de atestación.

Se pueden producir errores en varios pasos del flujo de trabajo anterior debido a un error de configuración. Estos son los errores comunes de atestación, sus causas principales y los pasos de solución de problemas recomendados:

- El servidor lógico de Azure SQL no se puede conectar al proveedor de atestación en Azure Attestation (paso 2 del flujo de trabajo anterior), especificado en la dirección URL de atestación. Entre las causas probables se encuentran las siguientes:
  - La dirección URL de atestación es incorrecta o está incompleta. Para obtener más información, vea [Determinación de la dirección URL de atestación para la directiva de atestación](/azure/azure-sql/database/always-encrypted-enclaves-configure-attestation#determine-the-attestation-url-for-your-attestation-policy).
  - El proveedor de atestación se ha eliminado accidentalmente.
  - Se ha configurado el firewall para el proveedor de atestación, pero no permite acceder a los servicios de Microsoft.
  - Un error de red intermitente hace que el proveedor de atestación no esté disponible.
- El servidor lógico de Azure SQL no está autorizado para enviar solicitudes de atestación al proveedor de atestación. Asegúrese de que el administrador del proveedor de atestación ha agregado el servidor de base de datos al rol Lector de atestación. Para obtener más información, vea [Concesión de acceso al proveedor de atestación al servidor de base de datos de Azure SQL](/azure/azure-sql/database/always-encrypted-enclaves-configure-attestation#grant-your-azure-sql-database-server-access-to-your-attestation-provider).
- Se produce un error en la validación de la directiva de atestación (en el paso 3 del flujo de trabajo anterior).
  - Probablemente la causa principal sea una directiva de atestación incorrecta. Asegúrese de que usa la directiva recomendada por Microsoft. Para obtener más información, vea [Creación y configuración de un proveedor de atestación](/azure/azure-sql/database/always-encrypted-enclaves-configure-attestation#create-and-configure-an-attestation-provider).
  - También se puede producir un error en la validación de la directiva como resultado de una infracción de seguridad que pone en peligro el enclave del lado servidor.
- La aplicación cliente no se puede conectar al proveedor de atestación y recuperar la clave de firma pública (en el paso 5). Entre las causas probables se encuentran las siguientes:
  - Es posible que la configuración de los firewalls entre la aplicación y el proveedor de atestación bloquee las conexiones. Para solucionar problemas de la conexión bloqueada, compruebe que se puede conectar al punto de conexión OpenId del proveedor de atestación. Por ejemplo, use un explorador web desde el equipo en el que se hospeda la aplicación para ver si puede conectarse al punto de conexión OpenID. Para obtener más información, vea [Configuración de metadatos: Get](/rest/api/attestation/metadataconfiguration/get).

## <a name="attestation-errors-when-using-host-guardian-service"></a>Errores de atestación al usar el Servicio de protección de host

> [!NOTE]
> Esta sección solo se aplica a [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)].

Antes de que un controlador cliente envíe una instrucción T-SQL a [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] para su ejecución, el controlador desencadena el siguiente flujo de trabajo de atestación de enclave mediante el Servicio de protección de host (HGS).

1. El controlador cliente llama a [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] para iniciar la atestación.
2. [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] recopila la evidencia sobre su enclave, su entorno de hospedaje y el código que se ejecuta dentro del enclave. [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] solicita un certificado de mantenimiento de la instancia de HGS, el equipo que hospeda [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] con el que se ha registrado. Para obtener más información, vea [Registro del equipo con el servicio de protección de host](always-encrypted-enclaves-host-guardian-service-register.md).
3. HGS valida la evidencia y emite el certificado de mantenimiento para [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]. HGS firma el certificado de mantenimiento con su clave privada.
4. [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] envía el certificado de mantenimiento al controlador cliente.
5. El controlador cliente se pone en contacto con HGS en la dirección URL de atestación, especificada para la conexión de base de datos, a fin de recuperar la clave pública de HGS. El controlador cliente comprueba la firma en el certificado de mantenimiento.

Se pueden producir errores en varios pasos del flujo de trabajo anterior debido a errores de configuración. Estos son los errores de atestación comunes, sus causas principales y los pasos de solución de problemas recomendados:

- [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] no se puede conectar HGS (paso 2 del flujo de trabajo anterior) debido a un error de red intermitente. Para solucionar el problema de conexión, el administrador del equipo con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] debe comprobar que el equipo se puede conectar al equipo de HGS.
- Se produce un error en la validación en el paso 3. Para solucionar el error de validación:
  - El administrador del equipo con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] debe trabajar con el administrador de la aplicación cliente para comprobar que el equipo con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] está registrado con la misma instancia de HGS que la instancia a la que se hace referencia en la dirección URL de atestación en el lado cliente.
  - El administrador del equipo con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] debe confirmar que el equipo con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] puede realizar correctamente la atestación, siguiendo las instrucciones del [Paso 5: Confirmación de que el host puede realizar la atestación correctamente](always-encrypted-enclaves-host-guardian-service-register.md#step-5-confirm-the-host-can-attest-successfully).
- La aplicación cliente no se puede conectar a HGS y recuperar su clave de firma pública (en el paso 5). La causa más probable es la siguiente:
  - Es posible que la configuración de uno de los firewalls entre la aplicación y el proveedor de atestación bloquee las conexiones. Compruebe que el equipo en el que se hospeda la aplicación se puede conectar al equipo de HGS.

## <a name="in-place-encryption-errors"></a>Errores de cifrado en contexto

En esta sección se enumeran los errores comunes que se pueden producir al usar `ALTER TABLE`/`ALTER COLUMN` para el cifrado en contexto (además de los errores de atestación descritos en las secciones anteriores). Para obtener más información, vea [Configuración del cifrado de columnas en contexto mediante Always Encrypted con enclaves seguros](always-encrypted-enclaves-configure-encryption.md).

- La clave de cifrado de columna que intenta usar para cifrar, descifrar o volver a cifrar los datos no es una clave habilitada para el enclave. Para obtener más información sobre los requisitos previos para el cifrado en contexto, vea [Requisitos previos](always-encrypted-enclaves-configure-encryption.md#prerequisites). Para obtener información sobre cómo aprovisionar claves habilitadas para el enclave, vea [Aprovisionamiento de claves habilitadas para el enclave](always-encrypted-enclaves-provision-keys.md).
- No ha habilitado Always Encrypted y los cálculos de enclave para la conexión de base de datos. Vea [Requisitos previos para ejecutar instrucciones con enclaves seguros](always-encrypted-enclaves-query-columns.md#prerequisites-for-running-statements-using-secure-enclaves).
- La instrucción `ALTER TABLE`/`ALTER COLUMN` desencadena una operación criptográfica y cambia el tipo de datos de la columna, o bien establece una intercalación con otra página de códigos de la página de códigos de intercalación actual. No se permite la combinación de operaciones criptográficas con cambios del tipo de datos o la página de intercalación. Para solucionar el problema, use instrucciones independientes; una para cambiar el tipo de datos o la página de códigos de intercalación, y otra para el cifrado en contexto.

## <a name="errors-when-running-confidential-dml-queries-using-secure-enclaves"></a>Errores al ejecutar consultas DML confidenciales con enclaves seguros

En esta sección se enumeran los errores comunes que se pueden producir al ejecutar consultas DML confidenciales con enclaves seguros (además de los errores de atestación descritos en las secciones anteriores).

- La clave de cifrado de columna configurada para la columna que consulta no es una clave habilitada para el enclave.
- No ha habilitado Always Encrypted y los cálculos de enclave para la conexión de base de datos. Para obtener más información, vea [Requisitos previos para ejecutar instrucciones con enclaves seguros](always-encrypted-enclaves-query-columns.md#prerequisites-for-running-statements-using-secure-enclaves).
- La columna que consulta usa el cifrado determinista. Las consultas DML confidenciales que usan enclaves seguros no se admiten con el cifrado determinista. Para obtener más información sobre cómo cambiar el tipo de cifrado a aleatorio, vea [Habilitación de Always Encrypted con enclaves seguros para las columnas cifradas existentes](always-encrypted-enclaves-enable-for-encrypted-columns.md).
- La columna de cadena que consulta usa una intercalación que no es BIN2 o UTF-8. Cambie la intercalación a BIN2 o UTF-8. Para obtener más información, vea [Instrucciones DML con enclaves seguros](always-encrypted-enclaves-query-columns.md#dml-statements-using-secure-enclaves).
- La consulta desencadena una operación no admitida. Para obtener la lista de las operaciones admitidas dentro de enclaves, vea [Instrucciones DML con enclaves seguros](always-encrypted-enclaves-query-columns.md#dml-statements-using-secure-enclaves).
## <a name="next-steps"></a>Pasos siguientes

- [Desarrollo de aplicaciones mediante Always Encrypted con enclaves seguros](always-encrypted-enclaves-client-development.md)

## <a name="see-also"></a>Consulte también

- [Ejecución de instrucciones de Transact-SQL con enclaves seguros](always-encrypted-enclaves-query-columns.md).
- [Configuración del cifrado de columna en contexto mediante Always Encrypted con enclaves seguros](always-encrypted-enclaves-configure-encryption.md)
- [Creación y uso de índices en columnas mediante Always Encrypted con enclaves seguros](always-encrypted-enclaves-create-use-indexes.md)