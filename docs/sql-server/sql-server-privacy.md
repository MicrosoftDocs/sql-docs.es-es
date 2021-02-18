---
description: Complemento de privacidad de SQL Server
title: Complemento de privacidad de SQL Server | Microsoft Docs
ms.date: 11/11/2020
ms.prod: sql
ms.technology: release-landing
ms.reviewer: wopeter
ms.custom: ''
ms.topic: conceptual
f1_keywords: ''
helpviewer_keywords: ''
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 1821ebdd7c52d306a34be98f6e506de24c31539e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341668"
---
# <a name="sql-server-privacy-supplement"></a>Complemento de privacidad de SQL Server

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

En este artículo se resumen las características habilitadas para Internet que pueden recopilar y enviar datos anónimos sobre el uso de características a Microsoft. SQL Server puede recopilar información estándar del equipo y datos sobre el uso y el rendimiento que se podría transmitir a Microsoft y analizarse a fin de mejorar la calidad, la seguridad y la confiabilidad del producto. Si instala SQL Server en una máquina virtual en el servicio de Microsoft Azure, se puede enviar información del entorno a Microsoft para que pueda instalar la extensión Agente de IaaS de SQL Server en la máquina virtual y registrar el recurso de máquina virtual con SQL con el proveedor de recursos de la VM con SQL, tal como se describe [aquí](/azure/azure-sql/virtual-machines/windows/sql-vm-resource-provider-register).

Este artículo es un anexo a la [Declaración de privacidad de Microsoft](https://go.microsoft.com/fwlink/?LinkId=521839) general. La clasificación de datos de este artículo solo se aplica a versiones del producto local de SQL Server. No se aplica a los elementos:

- Azure SQL Database
- [SQL Server Management Studio (SSMS)](../ssms/sql-server-management-studio-telemetry-ssms.md)
- SQL Server Data Tools (SSDT)
- Azure Data Studio
- Database Migration Assistant
- SQL Server Migration Assistant
- Extensión MS-SQL

Definición de *escenarios de uso permitidos*. En el contexto de este artículo, Microsoft define los "escenarios de uso permitidos" como las acciones o las actividades que se inician a través de Microsoft.

## <a name="access-control"></a>Control de acceso

Información relacionada con las credenciales que se usa para proteger inicios de sesión, usuarios o cuentas en una instalación de SQL Server.

### <a name="examples-of-access-control"></a>Ejemplos de control de acceso

- Contraseñas
- Certificados

### <a name="permitted-usage-scenarios"></a>Escenarios de uso permitidos

|Escenario |Restricciones de acceso              |Requisitos de retención |
|---------|---------|---------|
|Estas credenciales nunca salen del equipo del usuario con los datos de diagnóstico y uso. |- |- |
|Los volcados de memoria pueden contener datos de control de acceso. |- |Volcado de memoria: máximo 30 días. |
|Estas credenciales nunca salen del equipo del usuario con los comentarios del usuario, a menos que el cliente las inserte manualmente. |Limitado al uso interno de Microsoft sin acceso de terceros. |Comentarios del usuario: máximo 1 año.|
|&nbsp;|&nbsp;|&nbsp;|

## <a name="customer-data"></a>Datos del cliente

Los datos del cliente se definen como los datos almacenados en tablas de usuario, directa o indirectamente. Estos datos incluyen estadísticas o literales de usuario en los textos de consulta que pueden almacenarse en tablas de usuario.

### <a name="examples-of-customer-data"></a>Ejemplos de datos del cliente

- Valores de datos almacenados en las filas de cualquier tabla de usuario.
- Objetos de estadísticas que contienen copias de valores en las filas de cualquier tabla de usuario.
- Textos de consulta que contienen valores literales.

### <a name="permitted-usage-scenarios"></a>Escenarios de uso permitidos

|Escenario  |Restricciones de acceso               |Requisitos de retención |
|---------|---------|---------|
|Estos datos no salen del equipo del usuario con los datos de uso y diagnóstico. |- |- |
|Los volcados de memoria pueden incluir datos del cliente y pueden emitirse para Microsoft. |- |Volcados de memoria: máximo 30 días. |
|Los clientes pueden enviar a Microsoft comentarios del usuario (con su consentimiento) que incluyan datos del cliente. |Limitado al uso interno de Microsoft sin acceso de terceros. Microsoft puede exponer los datos al cliente original. |Comentarios del usuario: máximo 1 año. |

## <a name="personal-data"></a>Datos personales

Datos recibidos de un usuario o generados a partir de su uso del producto.
- Se pueden vincular a un usuario individual.
- No incluyen datos del cliente.

### <a name="examples-of-personal-data"></a>Ejemplos de datos personales

- Identificación de interfaces. La dirección IP completa
- Nombre del equipo
- Nombres de inicio de sesión o usuario
- Parte local de la dirección de correo electrónico (joe@contoso.com)
- Información de la ubicación
- Identificación del cliente

### <a name="permitted-usage-scenarios"></a>Escenarios de uso permitidos

|Escenario  |Restricciones de acceso               |Requisitos de retención|
|---------|---------|---------|
|Estos datos no salen del equipo del usuario con los datos de uso y diagnóstico. |- |- |
|Los volcados de memoria pueden incluir datos personales y emitirse para Microsoft. |- |Volcados de memoria: máximo 30 días |
|Puede emitirse para Microsoft un identificador de cliente con el fin de ofrecer nuevas características híbridas y en la nube a las que se han suscrito los usuarios. |- |Actualmente no existe ninguna característica híbrida o en la nube de este tipo.|
|Los clientes pueden enviar a Microsoft comentarios del usuario (con su consentimiento) que incluyan datos del cliente.|Limitado al uso interno de Microsoft sin acceso de terceros. Microsoft puede exponer los datos al cliente original. |Comentarios del usuario: máximo 1 año |

## <a name="internet-based-services-data"></a>Datos de servicios basados en Internet

Datos necesarios para ofrecer servicios basados en Internet, según los términos de licencia de SQL Server.

### <a name="examples-of-internet-based-services-data"></a>Ejemplos de datos de servicios basados en Internet

- Información sobre las especificaciones del equipo
- Nombre/versión del explorador
- Versión de SQL Server
- Código de lenguaje
- Una dirección IP con determinados octetos quitados
- Datos de mapa

### <a name="permitted-usage-scenarios"></a>Escenarios de uso permitidos

|Escenario  |Restricciones de acceso               |Requisitos de retención|
|---------|---------|---------| 
|Microsoft podría usarlos para mejorar las características y corregir errores de las características actuales. |Limitado al uso interno de Microsoft sin acceso de terceros. Microsoft puede exponer los datos al cliente original.  Por ejemplo, a través de paneles. |Mínimo 90 días, máximo 3 años. |
|Los clientes pueden enviar a Microsoft comentarios del usuario (con su consentimiento) que incluyan datos del cliente. |Limitado al uso interno de Microsoft sin acceso de terceros. |Los clientes pueden enviar a Microsoft comentarios del usuario (con su consentimiento) que incluyan datos del cliente. |
|Los elementos de mapa de Power View y SQL Reporting Services podrían enviar datos para el uso de Mapas de Bing. |Limitado a datos de la sesión. |- |

## <a name="non-personal-data"></a>Datos no personales

1. Datos recibidos de una organización o generados a partir de su uso del producto. Se pueden vincular a una organización y no contienen datos de clientes.

   - Ejemplo
     - Nombre de la organización (ejemplo: Microsoft Corp.)

   - Escenarios de uso permitidos

     |Escenario  |Restricciones de acceso               |Requisitos de retención|
     |---------|---------|---------|
     | Microsoft puede recopilar datos de uso genérico de instancias de SQL Server que se ejecutan en Azure Virtual Machines con el fin de ofrecer a los clientes ventajas opcionales en Azure para usar SQL Server en Azure Virtual Machines. | Microsoft puede exponer los datos a los clientes, por ejemplo, a través de Azure Portal, para que aquellos clientes que ejecutan SQL Server en Azure Virtual Machines puedan acceder a ventajas específicas por ejecutar SQL Server en Azure. </br></br>Microsoft no usará estos datos a efectos de auditorías de licencias sin consentimiento previo del cliente. | Mínimo 90 días, máximo 3 años. |

2. Datos que describen o se usan para configurar servidores, bases de datos, tablas y otros recursos creados o proporcionados por los clientes. Incluyen la tabla de la base de datos y los nombres de columna, pero no el contenido de las filas de la base de datos u otros datos del cliente. Los clientes no deben incluir datos personales en esos campos ni crear aplicaciones diseñadas para almacenar datos personales en dichos campos. En el caso de los escenarios de uso que se indican a continuación, solo se usa el formato de hash para determinar los patrones de uso con el fin de mejorar el producto.

   - Ejemplo
      - Nombres de bases de datos de SQL Server
      - Nombres de tablas y columnas
      - Nombres de estadísticas

    - Escenarios de uso permitidos

      > [!NOTE]
      > Se aplica el algoritmo hash a todos los valores de metadatos antes de la colección.
      >

      |Escenario  |Restricciones de acceso               |Requisitos de retención|
      |---------|---------|---------|
      |Microsoft podría usarlos para mejorar las características y corregir errores de las características actuales. |Limitado al uso interno de Microsoft sin acceso de terceros. |Mínimo 90 días, máximo 3 años.|

3. Datos que se generan durante el proceso de ejecución del servidor. No contiene datos de clientes, datos no personales, como se indica en el punto 1. o 2. (arriba), datos de control de acceso de clientes ni datos personales.

   - Ejemplo
     - GUID de base de datos
     - Hash de nombre de equipo
     - Hash de nombre de instancia
     - Nombre de la aplicación
     - Datos de uso y comportamiento
     - Datos del Programa para la mejora de la experiencia del usuario de SQL (SQLCEIP)
     - Datos de configuración del servidor, por ejemplo, las opciones de sp_configure
     - Datos de configuración de características
     - Nombres de eventos y códigos de error
     - Configuración de hardware e identificación, por ejemplo, Fabricante OEM

   Microsoft examina los valores de nombre de aplicación establecidos por otros programas que usan SQL Server (por ejemplo, SharePoint o paquetes de aplicaciones de terceros), e incluye esta información en los campos de metadatos enviados a Microsoft cuando los datos de uso están habilitados. Los clientes no deben incluir datos personales en esos campos de metadatos ni crear aplicaciones diseñadas para almacenar datos personales en dichos campos.

   - Escenarios de uso permitidos

     |Escenario  |Restricciones de acceso               |Requisitos de retención|
     |---------|---------|---------|
     |Microsoft podría usarlos para mejorar las características y corregir errores de las características actuales.|Limitado al uso interno de Microsoft sin acceso de terceros. |Mínimo 90 días, máximo 3 años. |
     |Pueden usarse para realizar sugerencias para el cliente.  Por ejemplo, "En función de su uso del producto, considere la posibilidad de usar la característica *X*, ya que obtendría mejores resultados". |Microsoft puede exponer los datos al cliente original, por ejemplo, a través de paneles. |Registros de seguridad de datos de cliente: mínimo 3 años, máximo 6 años. |
     |Microsoft podría usarlos para la planeación de productos en el futuro. |Microsoft puede compartir esta información con otros proveedores de hardware y software para mejorar el funcionamiento de sus productos con software de Microsoft. |Mínimo 90 días, máximo 3 años.|
     |Microsoft podría usarlos para proporcionar servicios en la nube basados en los datos de uso y diagnóstico emitidos. Por ejemplo, a través de un panel de cliente en el que se muestra el uso de características en todas las instalaciones de SQL Server de una organización. |Microsoft puede exponer los datos al cliente original, por ejemplo, a través de paneles. |Mínimo 90 días, máximo 3 años. |
     |Los clientes pueden enviar a Microsoft comentarios del usuario (con su consentimiento) que incluyan datos del cliente. |Limitado al uso interno de Microsoft sin acceso de terceros. Microsoft puede exponer los datos al cliente original. |Comentarios del usuario: máximo 1 año. |
     |Puede usar el nombre de la base de datos y el de la aplicación para asignar categorías conocidas a las bases de datos y aplicaciones, por ejemplo, para las que pueden estar ejecutando software proporcionado por Microsoft u otras empresas.|Limitado al uso interno de Microsoft sin acceso de terceros.|Mínimo 90 días, máximo 3 años. |

## <a name="system-generated-logs-controls"></a>Controles de registros generados por el sistema

Se puede hacer referencia a las instrucciones sobre cómo se pueden activar o desactivar los registros generados por el sistema en el producto aquí: [Configuración de la recopilación de datos de uso y diagnóstico para SQL Server (CEIP)](usage-and-diagnostic-data-configuration-for-sql-server.md).

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]
