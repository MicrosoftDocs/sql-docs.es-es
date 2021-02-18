---
title: servicio SQL Server Browser
description: Aprenda a usar SQL Server Browser, un servicio que escucha solicitudes de recursos de SQL Server y proporciona información sobre las instancias de SQL Server instaladas.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
f1_keywords:
- sql13.swb.browseservers.local.f1
- sql13.swb.browseservers.network.f1
helpviewer_keywords:
- services [SQL Server], security
- SQL Browser service (See SQL Server Browser Service)
- Browser Service
- SQL Server Browser service
ms.assetid: 3cc00d3a-487c-4cd9-a155-655f02485fa0
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: a4c58894434c503f92c8bcf4df4f74cf9fa2c688
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345193"
---
# <a name="sql-server-browser-service"></a>servicio SQL Server Browser
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  El programa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Browser se ejecuta como un servicio de Windows. El Explorador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] escucha las solicitudes entrantes de recursos de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y proporciona información sobre las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instaladas en el equipo. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser permite efectuar las siguientes acciones:  
  
-   Examinar una lista de los servidores disponibles  
  
-   Conectarse a la instancia correcta del servidor  
  
-   Conectarse a los extremos de la conexión de administrador dedicada (DAC)  
  
 Para cada instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] y [!INCLUDE[ssAS](../../includes/ssas-md.md)], el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser (sqlbrowser) proporciona el nombre de la instancia y el número de versión. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser se instala con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser se puede configurar durante la instalación o utilizando el Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . De manera predeterminada, el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser se inicia automáticamente:  
  
-   Cuando se actualiza una instalación.  
  
-   Cuando se instala en un clúster.  
  
-   Cuando se instala una instancia con nombre de [!INCLUDE[ssDE](../../includes/ssde-md.md)] que incluye todas las instancias de SQL Server Express.  
  
-   Cuando se instala una instancia con nombre de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
## <a name="background"></a>Información previa  
 En versiones anteriores a [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)], se podía instalar solo una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en un equipo. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] escuchaba las solicitudes de entrada en el puerto 1433, que es el puerto asignado oficialmente a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] por el organismo Internet Assigned Numbers Authority (IANA). Solo una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede utilizar un puerto, de modo que cuando [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] introdujo la compatibilidad con varias instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se desarrolló el Protocolo de resolución de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SSRP) para escuchar en el puerto UDP 1434. Este servicio de escucha respondía a las solicitudes del cliente con los nombres de las instancias instaladas y los puertos o canalizaciones con nombre utilizadas por la instancia. Para solucionar estas limitaciones del sistema SSRP, [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] incluyó el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser como sustituto de SSRP.  
  
## <a name="how-sql-server-browser-works"></a>Funcionamiento de SQL Server Browser  
 Cuando se inicia una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , se asigna un puerto TCP/IP al servidor si el protocolo TCP/IP está habilitado para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si el protocolo de canalizaciones con nombre está habilitado, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] escucha en una canalización con nombre específica. Esa instancia específica utiliza dicho puerto, o "canalización", para intercambiar datos con las aplicaciones cliente. Durante la instalación, el puerto TCP 1433 y la canalización `\sql\query` se asignan a la instancia predeterminada, pero el administrador del servidor puede cambiar estos valores más tarde mediante el Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Puesto que solo una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede utilizar un puerto o una canalización, se asignan números de puerto y nombres de canalizaciones diferentes para las instancias con nombre, incluido [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]. De forma predeterminada, cuando están habilitados, las instancias con nombre y [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] están configurados para utilizar puertos dinámicos, por lo que se asigna un puerto disponible cuando se inicia [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si lo desea, puede asignarse un puerto determinado a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Al conectarse, los clientes pueden especificar un puerto concreto, pero, si el puerto se asigna dinámicamente, el número de puerto puede cambiar siempre que se reinicie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , por lo que el cliente desconoce el número de puerto correcto.  
  
 En el inicio, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser se inicia y reclama el puerto UDP 1434. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser lee el Registro, identifica todas las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el equipo y registra los puertos y las canalizaciones con nombre que utilizan. Cuando un servidor tiene dos o más tarjetas de red, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser devuelve el primer puerto habilitado que encuentra para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser es compatible con Ipv6 e Ipv4.  
  
 Cuando los clientes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] solicitan los recursos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la biblioteca de red del cliente envía un mensaje UDP al servidor utilizando el puerto 1434. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser responde con el puerto TCP/IP o con la canalización con nombre de la instancia solicitada. Entonces, la biblioteca de red en la aplicación cliente completa la conexión enviando una solicitud al servidor mediante el puerto o la canalización con nombre de la instancia deseada.  
  
 Para obtener información sobre cómo iniciar y detener el servicio Explorador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consulte Libros en pantalla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="using-sql-server-browser"></a>Usar SQL Server Browser  
 Si el servicio Explorador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no se está ejecutando, todavía puede conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si proporciona el número de puerto o la canalización con nombre correctos. Por ejemplo, puede conectarse a la instancia predeterminada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante TCP/IP si se está ejecutando en el puerto 1433.  
  
 No obstante, si el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser no se está ejecutando, no funcionan las siguientes conexiones:  
  
-   Cualquier componente que intente conectarse a una instancia con nombre sin especificar completamente todos los parámetros (por ejemplo, un puerto TCP/IP o una canalización con nombre).  
  
-   Cualquier componente que genere o pase información de servidor o de instancia que más adelante otros componentes podrían utilizar para volver a conectarse.  
  
-   La conexión a una instancia con nombre sin proporcionar el número de puerto o la canalización.  
  
-   DAC en una instancia con nombre o la instancia predeterminada si no se utiliza el puerto TCP/IP 1433.  
  
-   El servicio redirector de OLAP.  
  
-   La enumeración de servidores en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], el Administrador corporativo o el Analizador de consultas.  
  
 Si utiliza [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en un escenario cliente-servidor (por ejemplo, cuando su aplicación obtiene acceso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a través de una red) o si detiene o deshabilita el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser, debe asignar un número de puerto específico a cada instancia y escribir el código de la aplicación cliente para que siempre utilice ese número de puerto. Este enfoque plantea los siguientes problemas:  
  
-   Debe actualizar y mantener el código de la aplicación cliente para asegurarse de que se conecta al puerto apropiado.  
  
-   Otro servicio o aplicación del servidor puede utilizar el puerto que elija para cada instancia, lo que hará que la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no esté disponible.  
  
## <a name="clustering"></a>Agrupación en clústeres  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser no es un recurso agrupado y no admite la conmutación por error de un nodo del clúster al otro. Por tanto, en el caso de un clúster, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser debe instalarse y activarse para cada nodo del clúster. En los clústeres, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser escucha en IP_ANY.  
  
> [!NOTE]  
>  Cuando se escucha en IP_ANY y se habilita la escucha en unas direcciones IP específicas, el usuario debe configurar el mismo puerto TCP en cada IP porque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve el primer par de IP/puerto que detecta.  
  
## <a name="installing-uninstalling-and-running-from-the-command-line"></a>Instalar, desinstalar y ejecutar desde la línea de comandos  
 De forma predeterminada, el programa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser se instala en C:\Archivos de programa (x86)\Microsoft SQL Server\90\Shared\sqlbrowser.exe.  
  
 El servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser se desinstala cuando se quita la última instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser puede iniciarse desde el símbolo del sistema para solucionar problemas mediante el modificador **-c** :  
  
```  
<drive>\<path>\sqlbrowser.exe -c  
```  
  
## <a name="security"></a>Seguridad  
  
### <a name="account-privileges"></a>Privilegios de cuenta  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser escucha en un puerto UDP y acepta solicitudes no autenticadas mediante el protocolo de resolución de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SSRP). [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser debe ejecutarse en el contexto de seguridad de un usuario con pocos privilegios para minimizar el riesgo de sufrir un ataque malintencionado. La cuenta de inicio de sesión puede cambiarse mediante el Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Los derechos mínimos de usuario para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser son:  
  
-   Denegar el acceso desde la red a este equipo  
  
-   Denegar inicio de sesión localmente  
  
-   Denegar inicio de sesión como un trabajo por lotes  
  
-   Denegar inicio de sesión a través de Terminal Services  
  
-   Iniciar sesión como servicio  
  
-   Leer y escribir las claves del Registro de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] relacionadas con la comunicación de red (puertos y canalizaciones)  
  
### <a name="default-account"></a>Cuenta predeterminada  
 El programa de instalación configura [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser de forma que utilice la cuenta seleccionada para los servicios durante la instalación. Entre otras posibles cuentas se incluyen las siguientes:  
  
-   Cualquier cuenta **de dominio o local**  
  
-   Cuenta de **servicio local**  
  
-   La cuenta de **sistema local** (no recomendada, ya que tiene privilegios innecesarios)  
  
### <a name="hiding-sql-server"></a>Ocultar SQL Server  
 Las instancias ocultas son instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que solo admiten las conexiones de memoria compartida. Para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], establezca la marca `HideInstance` para señalar que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser no debería responder con información acerca de esta instancia del servidor.  
  
### <a name="using-a-firewall"></a>Utilizar un firewall  
 Para comunicarse con el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser en un servidor protegido por un firewall, abra el puerto UDP 1434 y el puerto TCP utilizado por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (p. ej., 1433). Para obtener información sobre cómo trabajar con un firewall, vea "Cómo: Configurar un firewall para el acceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]" en Libros en pantalla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="see-also"></a>Consulte también  
 [Protocolos de red y bibliotecas de red](../../sql-server/install/network-protocols-and-network-libraries.md)  
  
  
