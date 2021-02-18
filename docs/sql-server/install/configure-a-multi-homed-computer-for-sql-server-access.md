---
title: Configuración de un equipo de host múltiples para el acceso
description: Aprenda a configurar SQL Server y Firewall de Windows para proporcionar conexiones de red a una instancia de SQL Server en un entorno de hosts múltiples.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- ports [SQL Server], multi-homed computer
- multi-homed computer [SQL Server] configuring ports
- firewall systems [Database Engine], multi-homed computer
ms.assetid: ba369e5b-7d1f-4544-b7f1-9b098a1e75bc
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8a105d59c32308da01f9d875e69cb417dad31b10
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336791"
---
# <a name="configure-a-multi-homed-computer-for-sql-server-access"></a>Configurar un equipo de host múltiple para el acceso a SQL Server
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Cuando un servidor debe proporcionar una conexión a dos o más redes o subredes de la red, un escenario típico utiliza un equipo de host múltiple. Con frecuencia, este equipo se encuentra en una red perimetral (también conocida como DMZ, zona desmilitarizada o subred filtrada). En este artículo se describe cómo configurar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y Firewall de Windows con seguridad avanzada para proporcionar las conexiones de red a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en un entorno de host múltiple.  
  
> [!NOTE]  
>  Un equipo de host múltiple tiene varios adaptadores de red o se ha configurado con el fin de utilizar varias direcciones IP para un único adaptador de red. Un equipo de host doble tiene dos adaptadores de red o se ha configurado para utilizar dos direcciones IP para un único adaptador de red.  
  
 Antes de continuar con este artículo, debe conocer la información que se proporciona en el artículo [Configurar Firewall de Windows para permitir el acceso a SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md). Este artículo contiene información básica sobre cómo funcionan los componentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con el firewall.  
  
 **Las suposiciones para este ejemplo son:**  
  
-   Hay dos adaptadores de red instalados en el equipo. Algunos de los adaptadores de red pueden ser inalámbricos. Puede simular el uso de dos adaptadores de red utilizando la dirección IP de uno y la dirección IP de bucle invertido (127.0.0.1) como segundo adaptador de red.  
  
-   Por simplicidad, este ejemplo utiliza direcciones IPv4. Se pueden usar los mismos procedimientos utilizando direcciones IPv6.  
  
    > [!NOTE]  
    >  Las direcciones IPv4 son una serie de cuatro números conocidos como octetos. Cada número es menor que 255, separados por puntos, como 127.0.0.1. Las direcciones IPv6 son una serie de ocho números hexadecimales separada por dos puntos, como fe80:4898:23:3:49a6:f5c1:2452:b994.  
  
-   Las reglas de firewall podrían permitir el acceso a través de un puerto concreto, por ejemplo el 1433. O bien, las reglas de firewall podrían permitir el acceso al programa de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] (sqlservr.exe). Ninguno de los métodos es mejor que el otro. Dado que un servidor de una red perimetral es más vulnerable ante los ataques que los servidores de una intranet, en este artículo se supone que quiere tener un control más preciso y seleccionar individualmente los puertos que abre. Por esa razón, en este artículo se supone que configurará [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para escuchar en un puerto fijo. Para obtener más información sobre los puertos que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa, vea [Configurar Firewall de Windows para permitir el acceso a SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
  
-   En este ejemplo se configura el acceso a [!INCLUDE[ssDE](../../includes/ssde-md.md)] utilizando el puerto TCP 1433. Los otros puertos que usan los diferentes componentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se pueden configurar utilizando los mismos pasos generales.  
  
 **Los pasos generales de este ejemplo son los siguientes:**  
  
-   Determine las direcciones IP en el equipo.  
  
-   Configure [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para escuchar en un puerto TCP específico.  
  
-   Configure Firewall de Windows con seguridad avanzada.  
  
## <a name="optional-procedures"></a>Procedimientos opcionales  
 Si ya sabe qué direcciones IP están disponibles en el equipo y que se usan para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], puede omitir estos procedimientos.  
  
#### <a name="to-determine-the-ip-addresses-available-on-the-computer"></a>Para determinar las direcciones IP disponibles en el equipo  
  
1.  En el equipo en el que está instalado [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , haga clic en **Inicio** y en **Ejecutar**, escriba **cmd** y, después, [!INCLUDE[clickOK](../../includes/clickok-md.md)].  
  
2.  En la ventana del símbolo del sistema, escriba **ipconfig,** y presione ENTRAR para mostrar las direcciones IP disponibles en este equipo.  
  
    > [!NOTE]  
    >  El comando **ipconfig** muestra a veces muchas conexiones posibles, incluidas las que están desconectadas. El comando **ipconfig** puede mostrar tanto direcciones IPv4 como IPv6.  
  
3.  Observe las direcciones IPv4 y las direcciones IPv6 que se están usando. La otra información de la lista, como las direcciones temporales, máscaras de subred y puerta de enlace predeterminada, es importante para configurar una red TCP/IP. Pero esta información no se usa en este ejemplo.  
  
#### <a name="to-determine-the-ip-addresses-and-ports-used-by-ssnoversion"></a>Para determinar las direcciones IP y los puertos que usa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
1.  Haga clic en **Inicio**, elija **Todos los programas**, seleccione [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], seleccione **Herramientas de configuración** y, después, haga clic en **Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** .  
  
2.  En el panel de la consola del **Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** , expanda **Configuración de red de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** y **Protocolos de \<instance name>** y, después, haga doble clic en **TCP/IP**.  
  
3.  En el cuadro de diálogo **Propiedades de TCP/IP** , en la pestaña **Direcciones IP** , aparecen varias direcciones IP con el formato **IP1**, **IP2**, hasta **IPAll**. Una de estas direcciones IP, 127.0.0.1, se utiliza para el adaptador de bucle invertido. Aparecen direcciones IP adicionales para cada dirección IP configurada en el equipo.  
  
4.  Para cualquier dirección IP, si el cuadro de diálogo **Puertos dinámicos TCP** contiene **0**, indica que [!INCLUDE[ssDE](../../includes/ssde-md.md)] escucha en los puertos dinámicos. Este ejemplo usa puertos fijos en lugar de puertos dinámicos que podrían cambiarse después de reiniciar. Por lo tanto, si el cuadro de diálogo **Puertos dinámicos TCP** contiene **0**, elimine el 0.  
  
5.  Observe el puerto TCP que aparece para cada dirección IP que desea configurar. En este ejemplo, suponga que ambas direcciones IP están escuchando en el puerto predeterminado, 1433.  
  
6.  Si no desea que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] use algunos de los puertos disponibles, en la pestaña **Protocolo** , cambie el valor de **Escuchar todo** por **No**; y en la pestaña **Direcciones IP** , cambie el valor de **Activo** por **No** para las direcciones IP que no desee usar.  
  
## <a name="configuring-windows-firewall-with-advanced-security"></a>Configurar Firewall de Windows con seguridad avanzada  
 Cuando conozca las direcciones IP que usa el equipo y los portales que utiliza [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , podrá crear reglas de firewall y configurarlas después para direcciones IP concretas.  
  
#### <a name="to-create-a-firewall-rule"></a>Para crear una regla de firewall  
  
1.  En el equipo en el que está instalado [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , inicie sesión como administrador.  
  
2.  Haga clic en **Inicio**, en **Ejecutar**, escriba **wf.msc** y luego haga clic en **Aceptar**.  
  
3.  En el cuadro de diálogo **Control de cuentas de usuario** , haga clic en **Continuar** para usar las credenciales de administrador para abrir Firewall de windows con el complemento Seguridad avanzada.  
  
4.  En la página **Información general** , confirme que Firewall de Windows esté habilitado.  
  
5.  En el panel izquierdo, haga clic en **Reglas de entrada**.  
  
6.  Haga clic con el botón derecho en **Reglas de entrada** y, después, haga clic en **Nueva regla** para abrir el **Asistente para nueva regla de entrada**.  
  
7.  Podría crear una regla para el programa de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Sin embargo, dado que este ejemplo usa un puerto fijo, seleccione **Puerto** y haga clic en **Siguiente**.  
  
8.  En la página **Protocolos y puertos** , seleccione **TCP**.  
  
9. Seleccione **Puertos locales especificados**. Escriba los números de puertos separados por comas y, a continuación, haga clic en **Siguiente**. En este ejemplo, configurará el puerto predeterminado; por lo tanto, escriba **1433**.  
  
10. En la página **Acción** , revise las opciones. En este ejemplo no se usa el firewall para aplicar conexiones seguras. Por lo tanto, haga clic en **Permitir la conexión** y, a continuación, haga clic en **Siguiente**.  
  
    > [!NOTE]  
    >  El entorno podría requerir conexiones seguras. Si selecciona una de las opciones de conexiones seguras, podría tener que configurar un certificado y la opción **Forzar cifrado** . Para obtener más información sobre las conexiones seguras, vea [Habilitar conexiones cifradas en el motor de base de datos &#40;Administrador de configuración de SQL Server&#41;.](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)[Habilitar conexiones cifradas en el motor de base de datos & #40; Administrador de configuración de SQL Server & #41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)  
  
11. En la página **Perfil** , seleccione uno o varios perfiles para la regla. Si no conoce los perfiles de firewall, haga clic en el vínculo **Más información acerca de los perfiles** del programa de firewall.  
  
    -   Si el equipo es un servidor y está disponible solo cuando se conecta a un dominio, seleccione **Dominio** y haga clic en **Siguiente**.  
  
    -   Si el equipo es móvil, por ejemplo un portátil, es probable que use varios perfiles al conectarse a redes diferentes. En un equipo móvil puede configurar capacidades de acceso diferentes para perfiles distintos. Por ejemplo, podría permitir el acceso cuando el equipo use el perfil Dominio y no permitirlo si usa el perfil Público.  
  
12. En la página **Nombre** , proporcione un nombre y una descripción para la regla, y  haga clic en **Finalizar**.  
  
13. Repita este procedimiento con el fin de crear otra regla para cada dirección IP que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vaya a usar.  
  
 Después de crear una o varias reglas, realice los pasos siguientes para configurar cada dirección IP del equipo de modo que use una regla.  
  
#### <a name="to-configure-the-firewall-rule-for-a-specific-ip-addresses"></a>Para configurar la regla de firewall de una dirección IP concreta  
  
1.  En la página **Reglas de entrada** del **Firewall de Windows con seguridad avanzada**, haga clic con el botón derecho en la regla recién creada, y después, haga clic en **Propiedades**.  
  
2.  En el cuadro de diálogo **Propiedades de la regla**, seleccione la pestaña **Ámbito** .  
  
3.  En el área **Dirección IP local**, seleccione **Estas direcciones IP**, y a continuación, haga clic en **Agregar**.  
  
4.  En el cuadro de diálogo **Dirección IP** , seleccione **Esta dirección IP o subred** y escriba una de las direcciones IP que desea configurar.  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
6.  En el área **Dirección IP remota**, seleccione **Estas direcciones IP**, y a continuación, haga clic en **Agregar**.  
  
7.  Use el cuadro de diálogo **Dirección IP** para configurar la conectividad de la dirección IP seleccionada en el equipo. Puede habilitar las conexiones desde direcciones IP especificadas, intervalos de direcciones IP, subredes enteras o ciertos equipos. Para configurar esta opción correctamente, debe tener un gran conocimiento de la red. Para obtener información sobre la red, consulte al administrador de red.  
  
8.  Para cerrar el cuadro de diálogo **Dirección IP**, haga clic en **Aceptar** y haga clic en **Aceptar** para cerrar el cuadro de diálogo **Propiedades de la regla**.  
  
9. Para configurar las otras direcciones IP en un equipo de host múltiple, repita este procedimiento con otra dirección IP y otra regla.  
  
## <a name="see-also"></a>Consulte también  
 [Servicio SQL Server Browser &#40;motor de base de datos y SSAS&#41;](../../database-engine/configure-windows/sql-server-browser-service-database-engine-and-ssas.md)   
 [Conectarse a SQL Server a través de un servidor proxy &#40;Administrador de configuración de SQL Server&#41;](../../database-engine/configure-windows/connect-to-sql-server-through-a-proxy-server-sql-server-configuration-manager.md)  
  
  
