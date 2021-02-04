---
title: Configuración de la cuenta de Launchpad
description: Cómo se modifica la cuenta del servicio de SQL Server Launchpad utilizada para la ejecución de scripts externos en SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/17/2018
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 0aff998d79d79c2ac2ef06c37ffba4b01eb73973
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076863"
---
# <a name="sql-server-launchpad-service-configuration"></a>Configuración del servicio de SQL Server Launchpad
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

[!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] es un servicio que administra y ejecuta scripts externos, de modo similar a la forma en la que el servicio de consulta e indexación de texto completo inicia un host independiente para procesar consultas de texto completo.

Para obtener más información, vea las secciones de Launchpad en [Arquitectura de extensibilidad en SQL Server Machine Learning Services](../../machine-learning/concepts/extensibility-framework.md#launchpad) y [Información general sobre seguridad para la plataforma de extensibilidad en SQL Server Machine Learning Services](../../machine-learning/concepts/security.md#launchpad).

## <a name="account-permissions"></a>Permisos de cuenta

De forma predeterminada, SQL Server Launchpad está configurado para ejecutarse con **NT Service\MSSQLLaunchpad**, que se aprovisiona con todos los permisos necesarios para ejecutar scripts externos. Si se quitan los permisos de esta cuenta, puede ser que Launchpad no se pueda iniciar o no pueda acceder a la instancia de SQL Server donde se deben ejecutar los scripts externos.

Si modifica la cuenta de servicio, asegúrese de usar la [consola de la directiva de seguridad local](/windows/security/threat-protection/security-policy-settings/how-to-configure-security-policy-settings).

En la tabla siguiente se muestran los permisos necesarios para esta cuenta.

| Configuración de directiva de grupo | Nombre invariable |
|----------------------|---------------|
| [Ajustar las cuotas de memoria de un proceso](/windows/security/threat-protection/security-policy-settings/adjust-memory-quotas-for-a-process) | SeIncreaseQuotaPrivilege | 
| [Omisión de la comprobación transversal](/windows/security/threat-protection/security-policy-settings/bypass-traverse-checking) | SeChangeNotifyPrivilege | 
| [Iniciar sesión como servicio](/windows/security/threat-protection/security-policy-settings/log-on-as-a-service) | SeServiceLogonRight | 
| [Reemplazar un token de nivel de proceso](/windows/security/threat-protection/security-policy-settings/replace-a-process-level-token) | SeAssignPrimaryTokenPrivilege | 

Para más información sobre los permisos para ejecutar servicios de SQL Server, vea [Configurar los permisos y las cuentas de servicio de Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).

<a name="bkmk_ChangingConfig"></a> 

## <a name="configuration-properties"></a>Propiedades de configuración

Normalmente no hay ningún motivo para modificar la configuración del servicio. Las propiedades que se pueden modificar incluyen la cuenta de servicio, el recuento de procesos externos (20 de forma predeterminada) o la política de restablecimiento de contraseña para las cuentas profesionales.

1. Abra el [Administrador de configuración de SQL Server](../../relational-databases/sql-server-configuration-manager.md).

2. En Servicios de SQL Server, haga clic con el botón derecho en Launchpad de SQL Server y elija **Propiedades**.
  + Haga clic en la pestaña **Iniciar sesión** para ver la cuenta de servicio.
  + Para aumentar el número de usuarios, haga clic en la pestaña **Avanzado** y cambie la **Cantidad de contextos de seguridad**.

> [!Note]
> En las primeras versiones de SQL Server 2016 R Services, se podían cambiar algunas propiedades del servicio editando el archivo de configuración de [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)]. Este archivo ya no se utiliza para cambiar las configuraciones. El Administrador de configuración de SQL Server es el más adecuado para los cambios en la configuración del servicio, como la cuenta de servicio y el número de usuarios.

## <a name="debug-settings"></a>Configuración de depuración

Solo se pueden cambiar algunas propiedades mediante el archivo de configuración de Launchpad, que puede ser útil en determinados casos, como la depuración. Este archivo de configuración se crea durante la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y, de manera predeterminada, se guarda como un archivo de texto sin formato en `<instance path>\binn\rlauncher.config`.

Para realizar cambios en este archivo, debe ser administrador en el equipo que ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si edita el archivo, se recomienda realizar una copia de seguridad antes de guardar los cambios.

En la siguiente tabla se muestran las configuraciones avanzadas para [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], con los valores permitidos.

|**Nombre de opción**|**Tipo**|**Descripción**|
|----|----|----|
|JOB\_CLEANUP\_ON\_EXIT|Entero |Se trata únicamente de una opción interna: no cambie este valor. </br></br>Especifica si se debe limpiar la carpeta de trabajo temporal creada para cada sesión de runtime externa una vez completada. Esta opción resulta útil para la depuración. </br></br>Los valores admitidos son **0** (deshabilitado) o **1** (habilitado). </br></br>El valor predeterminado es 1, lo que significa que los archivos de registro se quitan al salir.|
|TRACE\_LEVEL|Entero |Configura el nivel de detalle del seguimiento de MSSQLLAUNCHPAD con fines de depuración. Esto afecta a los archivos de seguimiento de la ruta de acceso especificada por el valor LOG_DIRECTORY. </br></br>Los valores admitidos son: **1** (error), **2** (rendimiento), **3** (advertencia), **4** (información). </br></br>El valor predeterminado es 1, lo que significa que solo se producen errores de salida.|

Todas las opciones se presentan como un par clave-valor y cada una de ellas se encuentra en una línea aparte. Por ejemplo, para cambiar el nivel de seguimiento, agregaría la línea `Default: TRACE_LEVEL=4`.

<a name="bkmk_EnforcePolicy"></a>

## <a name="enforcing-password-policy"></a>Aplicación de una directiva de contraseñas

Si su organización tiene una directiva que requiere el cambio de contraseñas de forma periódica, podría tener que forzar el servicio Launchpad para que regenere las contraseñas cifradas que Launchpad conserva para sus cuentas de trabajo.

Para habilitar esta opción de configuración y forzar la actualización de las contraseñas, abra el panel **Propiedades** del servicio Launchpad en el Administrador de configuración de SQL Server, haga clic en **Avanzado** y cambie **Restablecer contraseña de usuarios externos** a **Sí**. Al aplicar este cambio, las contraseñas se regenerarán inmediatamente para todas las cuentas de usuario. Para ejecutar un script externo después de este cambio, debe reiniciar el servicio Launchpad, momento en el cual leerá las contraseñas recién generadas.

Para restablecer las contraseñas a intervalos regulares, puede establecer esta marca manualmente o usar un script.

## <a name="next-steps"></a>Pasos siguientes

+ [Plataforma de extensibilidad](../concepts/extensibility-framework.md)
+ [Información general sobre seguridad](../concepts/security.md)