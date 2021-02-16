---
description: Administrador de conexiones de Oracle
title: Administrador de conexiones de Oracle | Microsoft Docs
ms.custom: ''
ms.date: 08/14/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 258cce05d25b02b046c9fc4e79bd031d6b27fc22
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343252"
---
# <a name="oracle-connection-manager"></a>Administrador de conexiones de Oracle

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Un Administrador de conexiones de Oracle se usa para habilitar un paquete a fin de extraer y cargar datos desde y en bases de datos de Oracle.

La propiedad **ConnectionManagerType** del Administrador de conexiones de Oracle está establecida en **ORACLE**.

## <a name="configuring-the-oracle-connection-manager"></a>Configuración del Administrador de conexiones de Oracle

Integration Services en tiempo de ejecución resolverá los cambios de configuración del Administrador de conexiones de Oracle. Use el cuadro de diálogo **Editor del Administrador de conexiones de Oracle** para agregar una conexión a un origen de datos de Oracle.

![Connection Manager](media/oracle-connection-manager.png)

### <a name="options"></a>Opciones

#### <a name="connection-manager-information"></a>Información del administrador de conexiones

Escriba información sobre la conexión de Oracle.

**Nombre**

Escriba un nombre para la conexión de Oracle. El nombre predeterminado es Administrador de conexiones de Oracle. 

**Descripción** 

Escriba una descripción de la conexión. Esta entrada es opcional.

**Nombre de servicio TNS**

Escriba el nombre de la base de datos de Oracle con la que trabaja. El nombre de servicio TNS podría ser:

- El nombre del descriptor de la conexión que define el archivo tnsnames.ora que se encuentra en la carpeta Admin del cliente Oracle.

- Formato de EzConnect: [//]host[:puerto][/nombre_de_servicio]

Para obtener más información, vea la documentación de Oracle.

#### <a name="connection-manager-logging"></a>Registro del administrador de conexiones

Seleccione una de estas opciones:

- **Usar autenticación de Windows**: seleccione esta opción para usar la autenticación de Windows.

- **Usar autenticación de Oracle**: seleccione esta opción para usar la autenticación de Oracle Database. Si usa esta autenticación, escriba sus credenciales de Oracle de la siguiente manera:  
    **Nombre de usuario**: escriba el nombre de usuario que usa para conectarse a la base de datos de Oracle.  
    **Contraseña**: escriba la contraseña de la base de datos de Oracle para el usuario especificado en el campo Nombre de usuario.

> [!NOTE]
>
>La autenticación de Windows no es compatible con Oracle Server 18c.

**Probar conexión**

Haga clic en **Probar conexión** para comprobar si la información proporcionada es correcta. Recibirá el mensaje **La conexión de prueba se estableció correctamente** si la información especificada pudo establecer una conexión con la base de datos de Oracle.

### <a name="custom-properties"></a>Propiedades personalizadas

En el Administrador de conexiones de Oracle existen las siguientes propiedades personalizadas del administrador de conexiones:

- **EnableDetailedTracing**: no se usa.

- **OracleHome**: especifique la carpeta o el nombre del directorio de inicio de Oracle (Oracle Home) de 32 bits que el conector usará. (Opcional)

- **OracleHome64**: especifique la carpeta o el nombre del directorio de inicio de Oracle (Oracle Home) de 64 bits que el conector usará cuando se ejecute en modo de 64 bits. (Opcional)

Las propiedades personalizadas no se muestran en el Editor del Administrador de conexiones de Oracle. Para establecer las propiedades **OracleHome** y **OracleHome64**:

1. En el área del Administrador de conexiones, haga clic con el botón derecho en el administrador de conexiones de Oracle con el que está trabajando y seleccione **Propiedades**.

2. En el panel **Propiedades**, establezca la propiedad **OracleHome** o **OracleHome64** con la ruta de acceso completa al directorio de inicio de Oracle.

## <a name="next-steps"></a>Pasos siguientes

- Configure el [origen de Oracle](oracle-source.md).
- Configure el [destino de Oracle](oracle-destination.md).
- Si tiene alguna pregunta, visite [TechCommunity](https://aka.ms/AA5u35j).
