---
title: Configuración de TLS 1,2
description: Recomendación para configurar TLS 1,2 en APS
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 561a0a4f1d45d78e0f2fd23d61aae67f6adb6ff7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052746"
---
# <a name="configure-tls-12-in-aps"></a>Configuración de TLS 1,2 en APS

Para proteger los AP para usar solo TLS 1,2, tendrá que deshabilitar explícitamente otro protocolo en todos los hosts físicos y virtuales. La deshabilitación de los protocolos requiere cambios en la configuración del registro. Los cambios en el registro requieren un reinicio de los hosts físicos y virtuales.

> [!WARNING]
> Esta sección, método o tarea contiene pasos que indican cómo modificar el Registro. Sin embargo, pueden producirse problemas graves si modifica incorrectamente el registro, lo que puede provocar la pérdida de datos y requerir la reinstalación del sistema operativo. Es muy recomendable realizar una copia de seguridad del registro antes de modificarlo. Y así, si se produce algún problema, puede restaurarlo. Para obtener más información acerca de cómo realizar copias de seguridad y restaurar el registro, haga clic en el número de artículo siguiente para ver el artículo en Microsoft Knowledge Base:<br>
[322756](https://support.microsoft.com/help/322756) cómo realizar una copia de seguridad y restaurar el registro en Windows

**Activa**
```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0]
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client]
"Enabled"=dword:00000000
"DisabledByDefault"=dword:00000001
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server]
"Enabled"=dword:00000000
"DisabledByDefault"=dword:00000001
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1]
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client]
"Enabled"=dword:00000000
"DisabledByDefault"=dword:00000001
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server]
"Enabled"=dword:00000000
"DisabledByDefault"=dword:00000001
```

Establezca también las siguientes claves en los equipos cliente donde están instaladas las herramientas como los adaptadores de destino SSIS de APS.
```
[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319]
"SystemDefaultTlsVersions"=dword:00000001
"SchUseStrongCrypto"=dword:00000001

[HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\.NETFramework\v4.0.30319]
"SystemDefaultTlsVersions"=dword:00000001
"SchUseStrongCrypto"=dword:00000001 
```



