---
description: Uso del administrador de conexiones de Teradata
title: Uso del administrador de conexiones de Teradata | Microsoft Docs
ms.custom: ''
ms.date: 11/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 85465e017126a116289c396787d7d8ccecba042f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339878"
---
# <a name="use-the-teradata-connection-manager"></a>Uso del administrador de conexiones de Teradata

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Con el administrador de conexiones de Teradata, puede permitir que un paquete extraiga datos de bases de datos de Teradata y los cargue en estas.

Establezca la propiedad `ConnectionManagerType` del administrador de conexiones de Teradata en *TERADATA*.

## <a name="configure-the-teradata-connection-manager"></a>Configuración del administrador de conexiones de Teradata

Los cambios de configuración del administrador de conexiones de Teradata se resuelven en Integration Services en tiempo de ejecución. Para agregar una conexión a un origen de datos de Teradata, complete la información del panel **Editor del administrador de conexiones de Teradata**.

![El panel del Editor del administrador de conexiones de Teradata](media/teradata-connection-manager.png)

1. En el cuadro **Nombre**, escriba un nombre para la conexión. El nombre predeterminado es **Administrador de conexiones de Teradata**.

1. (Opcional) En el cuadro de texto **Descripción**, escriba una descripción de la conexión.

1. En el cuadro **Nombre del servidor**, escriba el nombre del servidor de Teradata al que desea conectarse.

1. En **Autenticación**, realice una de las siguientes acciones:

   - Para usar la autenticación de Windows, seleccione **Usar autenticación de Windows**.
   - Para usar la autenticación de base de datos de Teradata, seleccione **Usar autenticación Teradata** y escriba las siguientes credenciales para este tipo de autenticación:
     - En el cuadro **Mecanismo**, escriba el mecanismo de comprobación de seguridad que desea utilizar. Entre los valores de mecanismo válidos se incluyen TD1, TD2, LDAP, KRB5, KRB5C, NTLM y NTLMC.
     - En el cuadro **Parámetro**, escriba los tipos de parámetros necesarios para el mecanismo de comprobación de seguridad que ha especificado.
     - En el cuadro **Nombre de usuario**, escriba el nombre de usuario que usa para conectarse a la base de datos de Teradata.  
     - En el cuadro **Contraseña**, escriba la contraseña de la base de datos de Teradata del usuario.

1. (Opcional) En la lista desplegable **Base de datos predeterminada**, seleccione la base de datos de Teradata a la que desea conectarse. Si este permiso de acceso a la base de datos es incorrecto, se muestra un error y, luego, podrá escribir manualmente el nombre de la base de datos.

1. (Opcional) En el cuadro **Cuenta**, escriba el nombre de la cuenta que corresponde al nombre de usuario. Si este valor está vacío, se usa la cuenta del propietario inmediato de la base de datos.
1. Seleccione **Aceptar**.

## <a name="custom-property"></a>Propiedad personalizada

La propiedad personalizada `UseUTF8CharSet` especifica si se usa el juego de caracteres UTF-8. El valor predeterminado es *True*.

Para establecer la propiedad, siga estos pasos:

1. Abra SQL Server Data Tools (SSDT).
1. En el área **Administrador de conexiones**, haga clic con el botón derecho en **Administrador de conexiones de Teradata** y, luego, seleccione **Propiedades**.
1. En el panel **Propiedades**, seleccione *True* o *False* para la propiedad `UseUTF8CharSet`.

## <a name="troubleshoot-the-teradata-connection-manager"></a>Solución de problemas del administrador de conexiones de Teradata

Para registrar llamadas del administrador de conexiones de Teradata en el controlador de conectividad abierta de bases de datos (ODBC) de Teradata, habilite el seguimiento de ODBC de Windows en el administrador de orígenes de datos ODBC de Windows.

## <a name="next-steps"></a>Pasos siguientes

- Configurar el [origen de Teradata](teradata-source.md)
- Configurar el [destino de Teradata](teradata-destination.md).
- Si tiene alguna pregunta, visite [Tech Community](https://aka.ms/AA5u35j).
