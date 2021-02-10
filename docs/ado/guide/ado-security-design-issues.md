---
description: Características de diseño de seguridad de ADO
title: Problemas de diseño de seguridad de ADO | Microsoft Docs
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.topic: conceptual
helpviewer_keywords:
- ADO, security
ms.assetid: 86b83a38-efdf-4831-a6d5-7e470d517d1c
author: rothja
ms.author: jroth
ms.openlocfilehash: c992b58b6b84f79efa62c63353e3b8f1efcc333d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030503"
---
# <a name="ado-security-design-features"></a>Características de diseño de seguridad de ADO
En las secciones siguientes se describen las características de diseño de seguridad de Objetos de datos ActiveX (ADO) 2,8 y versiones posteriores. Estos cambios se realizaron en ADO 2,8 para mejorar la seguridad. ADO 6,0, que se incluye en Windows DAC 6,0 en Windows Vista, es funcionalmente equivalente a ADO 2,8, que se incluyó en MDAC 2,8 en Windows XP y Windows Server 2003. En este tema se proporciona información sobre cómo proteger mejor las aplicaciones en ADO 2,8 o posterior.

> [!IMPORTANT]
>  Si va a actualizar la aplicación desde una versión anterior de ADO, se recomienda que pruebe la aplicación actualizada en un equipo que no sea de producción antes de implementarla en los clientes. De este modo, puede asegurarse de que conoce los problemas de compatibilidad antes de implementar la aplicación actualizada.

## <a name="internet-explorer-file-access-scenarios"></a>Escenarios de acceso a archivos de Internet Explorer
 Las siguientes características afectan al funcionamiento de ADO 2,8 y versiones posteriores cuando se utiliza en páginas web con script en Internet Explorer.

### <a name="revised-and-improved-security-warning-message-box-now-used-to-alert-users"></a>Cuadro de mensaje de advertencia de seguridad revisado y mejorado ahora usado para alertar a los usuarios
 Para ADO 2,7 y versiones anteriores, aparece el siguiente mensaje de advertencia cuando una página web con script intenta ejecutar código ADO desde un proveedor que no es de confianza:

```console
This page accesses data on another domain. Do you want to allow this? To
avoid this message in Internet Explorer, you can add a secure Web site to
your Trusted Sites zone on the Security tab of the Internet Options dialog
box.
```

 Para ADO 2,8 y versiones posteriores, ya no aparece el mensaje anterior. En su lugar, aparece el siguiente mensaje en este contexto:

```console
This Website uses a data provider that may be unsafe. If you trust the
Website, click OK, otherwise click Cancel.
```

 El mensaje anterior permite al usuario tomar decisiones informadas, a la vez que conoce las consecuencias de ambas opciones:

-   Si el usuario confía en el sitio, al hacer clic en aceptar se permitirá todo el código seguro para disco (todos los métodos y propiedades de ADO con las excepciones de las API accesibles en disco que se describen más adelante en este tema) para ejecutarse y ejecutarse en la ventana del explorador.

-   Si el usuario no confía en el sitio, al hacer clic en Cancelar se bloquea el código ADO para que el acceso a los datos se ejecute y se ejecute en su totalidad.

### <a name="disk-accessible-code-limited-now-to-trusted-sites"></a>Código accesible desde disco limitado ahora a sitios de confianza
 Se han realizado cambios de diseño adicionales en ADO 2,8 que restringen específicamente la capacidad de un conjunto limitado de API, lo que podría exponer la posibilidad de leer o escribir archivos en el equipo local. Estos son los métodos de API que se han restringido aún más para la seguridad al ejecutar Internet Explorer:

-   Para el objeto de **secuencia** de ADO, si se usan los métodos [LoadFromFile](../reference/ado-api/loadfromfile-method-ado.md) o [SaveToFile](../reference/ado-api/savetofile-method.md) .

-   Para el objeto **de conjunto de registros** ADO, si el método [Save](../reference/ado-api/save-method.md) o el método [Open](../reference/ado-api/open-method-ado-recordset.md) , como cuando se establece la opción **AdCmdFile** o se utiliza el proveedor de [persistencia de Microsoft OLE DB (MSPersist)](./appendixes/microsoft-ole-db-persistence-provider-ado-service-provider.md) .

 Para estos conjuntos limitados de funciones potencialmente accesibles en disco, se produce el comportamiento siguiente para ADO 2,8 y versiones posteriores, si se ejecuta cualquier código que use estos métodos en Internet Explorer:

-   Si el sitio que proporcionó el código se agregó previamente a la lista de zonas de sitios de confianza, el código se ejecuta en el explorador y se concede el acceso a los archivos locales.

-   Si el sitio no aparece en la lista de zonas de sitios de confianza, se bloquea el código y se deniega el acceso a los archivos locales.

    > [!NOTE]
    >  En ADO 2,8 y versiones posteriores, no se advierte al usuario ni le aconseja agregar sitios a la lista de zonas de sitios de confianza. Por lo tanto, la administración de la lista de sitios de confianza es responsabilidad de aquellos que implementan o admiten aplicaciones basadas en sitios web que requieren acceso al sistema de archivos local.

### <a name="access-blocked-to-the-activecommand-property-on-recordset-objects"></a>Acceso bloqueado a la propiedad ActiveCommand en objetos de conjunto de registros
 Al ejecutarse en Internet Explorer, ADO 2,8 ahora bloquea el acceso a la propiedad [ActiveCommand](../reference/ado-api/activecommand-property-ado.md) para un objeto **Recordset** activo y devuelve un error. El error se produce independientemente de si la página procede de un sitio Web registrado en la lista de sitios de confianza.

### <a name="changes-in-handling-for-ole-db-providers-and-integrated-security"></a>Cambios en el control de los proveedores de OLE DB y la seguridad integrada
 Al revisar ADO 2,7 y versiones anteriores para detectar posibles problemas de seguridad y preocupaciones, se detectó el siguiente escenario:

 En algunos casos, OLE DB proveedores que admiten la propiedad Integrated Security [DBPROP_AUTH_INTEGRATED](/previous-versions/windows/desktop/ms712973(v=vs.85)) podría permitir que las páginas web con script reutilicen el objeto de conexión ADO para conectarse de forma no intencionada a otros servidores con las credenciales de inicio de sesión actuales de los usuarios. Para evitar esto, ADO 2,8 y versiones posteriores controlan OLE DB proveedores en función de cómo hayan elegido proporcionar, o no, la seguridad integrada.

 En el caso de las páginas web que se cargan desde sitios que aparecen en la lista de zonas de sitios de confianza, en la tabla siguiente se proporciona un desglose del modo en que ADO 2,8 y las versiones posteriores administran las conexiones ADO en cada caso.

|Configuración de IE para la autenticación de usuario, Inicio de sesión|El proveedor admite "Integrated Security" y se especifican UID y PWD (SQLOLEDB)|El proveedor no admite "Integrated Security" (JOLT, MSDASQL, MSPersist)|El proveedor es compatible con "Integrated Security" y se establece en SSPI (no se especifica ningún UID/PWD)|
|------------------------------------------------|----------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------|
|Inicio de sesión automático con nombre de usuario y contraseña actuales|Permitir conexión|Permitir conexión|Permitir conexión|
|Solicitar el nombre de usuario y la contraseña|Permitir conexión|Error de conexión|Error de conexión|
|Inicio de sesión automático solo en la zona de intranet|Permitir conexión|Preguntar al usuario con la advertencia de seguridad|Preguntar al usuario con la advertencia de seguridad|
|Inicio de sesión anónimo|Permitir conexión|Error de conexión|Error de conexión|

 En el caso de que aparezca una advertencia de seguridad, el cuadro de mensaje informa a los usuarios:

```console
This Website is using your identity to access a data source. If you trust this Website, click OK, otherwise click Cancel.
```

 El mensaje anterior permite al usuario tomar una decisión más informada y continuar en consecuencia.

> [!NOTE]
>  En el caso de los sitios que no son de confianza (es decir, los sitios que no aparecen en la lista de zonas de sitios de confianza), si el proveedor tampoco es de confianza (como se explicó anteriormente en esta sección), el usuario podría ver dos advertencias de seguridad en una fila, una advertencia sobre el proveedor no seguro y una segunda advertencia sobre el intento de usar su identidad. Si el usuario hace clic en aceptar en la primera ADVERTENCIA, se ejecuta la configuración de Internet Explorer y el código de comportamiento de respuesta descrito en la tabla anterior.

## <a name="controlling-whether-password-text-is-returned-in-ado-connection-strings"></a>Controlar si el texto de la contraseña se devuelve en cadenas de conexión ADO
 Cuando se intenta obtener el valor de la propiedad [ConnectionString](../reference/ado-api/connectionstring-property-ado.md) en un objeto de **conexión** ADO, se producen los siguientes eventos:

1.  Si la conexión está abierta, se realiza una llamada de inicialización al proveedor de OLE DB subyacente para obtener la cadena de conexión.

2.  Dependiendo de la configuración del proveedor de OLE DB de la propiedad [DBPROP_AUTH_PERSIST_SENSITIVE_AUTHINFO](/previous-versions/windows/desktop/ms714905(v=vs.85)) , las contraseñas se incluyen junto con otra información de la cadena de conexión que se devuelve.

 Por ejemplo, si la propiedad dinámica de conexión ADO **Persist Security info** está establecida en **true**, la información de la contraseña se incluye en la cadena de conexión devuelta. De lo contrario, si el proveedor subyacente ha establecido la propiedad en **false** (por ejemplo, con el proveedor SQLOLEDB), se omite la información de contraseña en la cadena de conexión devuelta.

 Si usa proveedores de OLE DB de terceros (es decir, que no son de Microsoft) con el código de aplicación ADO, puede comprobar cómo se implementa la propiedad **DBPROP_AUTH_PERSIST_SENSITIVE_AUTHINFO** para determinar si se permite la inclusión de información de contraseña con cadenas de conexión ADO.

## <a name="checking-for-non-file-devices-when-loading-and-saving-recordsets-or-streams"></a>Comprobando si hay dispositivos que no son archivos al cargar y guardar conjuntos de registros o secuencias
 En el caso de ADO 2,7 y versiones anteriores, las operaciones de entrada/salida de archivos como [abrir](../reference/ado-api/open-method-ado-recordset.md) y [Guardar](../reference/ado-api/save-method.md) que se usaron para leer y escribir datos basados en archivos podrían, en algunos casos, permitir el uso de una dirección URL o un nombre de archivo que especifica un tipo de archivo no basado en disco. Por ejemplo, LPT1, COM2, PRN.TXT, AUX podría usarse como alias para la entrada/salida entre impresoras y dispositivos auxiliares en el sistema mediante determinados

 Para ADO 2,8 y versiones posteriores, esta funcionalidad se ha actualizado. Para abrir y guardar objetos **Recordset** y **Stream** , ADO ahora realiza una comprobación de tipo de archivo para asegurarse de que el dispositivo de entrada o salida especificado en una dirección URL o un nombre de archivo es un archivo real.

> [!NOTE]
>  La comprobación de tipos de archivo tal como se describe en esta sección solo se aplica a Windows 2000 y versiones posteriores. No se aplica a situaciones en las que se ejecuta ADO 2,8 o posterior en versiones anteriores de Windows, como Windows 98.