---
title: Instalación de versiones de idioma distintas del inglés
description: Instalación de versiones de idioma de SQL Server Management Studio (SSMS) distintas del inglés. Este artículo se aplica a SSMS 17.x.
ms.prod: sql
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 04/25/2019
ms.openlocfilehash: 8a70ef94e5491a807414abbd69b1f8ef71b9ca7d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064900"
---
# <a name="install-non-english-language-versions-of-sql-server-management-studio-ssms"></a>Instalación de versiones de idioma de SQL Server Management Studio (SSMS) distintas del inglés

SSMS está disponible en varios idiomas, pero el programa de instalación de SSMS bloquea la instalación en equipos en los que la configuración regional del sistema no coincide con el idioma de SSMS.

> [!NOTE]
> Este artículo se aplica a SSMS 17.x. En SSMS 18.x, se ha retirado el bloqueo de la configuración en varios idiomas y ahora puede, por ejemplo, instalar SSMS en alemán en una versión de Windows en francés. Si el idioma del sistema operativo no coincide con el idioma de SSMS, establezca el idioma deseado en **Herramientas** > **Opciones** > **Configuración internacional**, en caso contrario, SSMS mostrará la interfaz de usuario en inglés.

Las instrucciones siguientes varían según la versión de Windows. Estas son para Windows 10.

## <a name="install-non-english-ssms-on-a-computer-running-an-english-operating-system-os"></a>Instalación de SSMS en un idioma distinto del inglés en un equipo que ejecuta un sistema operativo (SO) en inglés

1. Instale el paquete de idioma de Windows correspondiente al idioma que quiere que se use en SSMS:
   - **Configuración** > **Hora e idioma** > **Región y idioma** > **Agregar un idioma**
2. A continuación, establezca la configuración regional para usar el paquete de idioma instalado en el paso anterior; para ello, haga clic en el idioma que acaba de instalar y seleccione **Establecer como predeterminado**. (Después de instalar SSMS, puede devolver la configuración regional a inglés).
3. Cuando el sistema operativo se esté ejecutando en el idioma deseado, instale el idioma de SSMS que quiera. La primera vez que instale un nuevo idioma de SSMS, use el paquete completo. En las posteriores instalaciones, puede usar el paquete de actualización.
4. Ejecute SSMS. Debería mostrarse en el idioma instalado en el paso anterior.
5. Devuelva otra vez la configuración regional del equipo a inglés.

## <a name="install-ssms-in-a-language-other-than-the-language-of-the-installed-os"></a>Instalación de SSMS en un idioma distinto al del sistema operativo instalado

1. Instale el paquete de idioma de Windows correspondiente al idioma que quiere que se use en SSMS:
   - **Configuración** > **Hora e idioma** > **Región y idioma** > **Agregar un idioma**
2. A continuación, establezca la configuración regional para usar el paquete de idioma instalado en el paso anterior; para ello, haga clic en el idioma que acaba de instalar y seleccione **Establecer como predeterminado**.
3. Cuando el sistema operativo se esté ejecutando en el idioma deseado, instale el idioma de SSMS que quiera. La primera vez que instale un nuevo idioma de SSMS, use el paquete completo. En las posteriores instalaciones, puede usar el paquete de actualización.
4. Para cada idioma que desee instalar que no coincida con el idioma de la primera versión de SSMS que se ha instalado, instale el paquete de idioma correspondiente de Visual Studio 2015 Shell (aislado):
   - Vaya a [https://connect.microsoft.com/VisualStudio/ExtendVS](https://connect.microsoft.com/VisualStudio/ExtendVS) (es posible que tenga que iniciar sesión y completar el proceso *Registro en Connect*).
   - Descargue el paquete de idioma deseado de Visual Studio 2015 Shell (aislado) e instálelo.

   > [!IMPORTANT]
   > Use los pasos anteriores para instalar el paquete de idioma de Visual Studio 2015 aislado; no use el vínculo **Obtener idiomas adicionales** en **Herramientas** | **Opciones** | **Configuración internacional**.

5. Ejecute SSMS y seleccione el idioma con el que quiere usarlo:
   - **Herramientas** | **Opciones** | **Configuración**
6. Cierre y reinicie SSMS.

## <a name="next-steps"></a>Pasos siguientes

- [Tutorial: SQL Server Management Studio](./quickstarts/ssms-connect-query-sql-server.md)