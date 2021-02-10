---
description: Uso de RDS con la agrupación de conexiones de ODBC
title: Usar RDS con la agrupación de conexiones ODBC | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- connection pooling in RDS [ADO]
ms.assetid: e8b912c1-da5b-4e85-a000-1e6648a94237
author: rothja
ms.author: jroth
ms.openlocfilehash: 5c353d7e0038f40eea574b895f2cd55ea3f47585
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032597"
---
# <a name="using-rds-with-odbc-connection-pooling"></a>Uso de RDS con la agrupación de conexiones de ODBC
Si utiliza un origen de datos ODBC, puede usar la opción de agrupación de conexiones en Internet Information Services (IIS) para lograr un alto rendimiento en el control de la carga de cliente. La agrupación de conexiones es un administrador de recursos para las conexiones, manteniendo el estado abierto en las conexiones usadas con frecuencia.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
 Para habilitar la agrupación de conexiones, consulte la documentación de Internet Information Services.  
  
 Tenga en cuenta que la habilitación de la agrupación de conexiones puede sujetar el servidor Web a otras restricciones, como se indica en la documentación de Microsoft Internet Information Services.  
  
 Para asegurarse de que la agrupación de conexiones es estable y proporciona mejoras de rendimiento adicionales, debe configurar Microsoft SQL Server para usar la biblioteca de red de Sockets TCP/IP.  
  
 Para ello, necesita lo siguiente:  
  
-   Configure el equipo SQL Server para usar Sockets TCP/IP.  
  
-   Configure el servidor web para que utilice Sockets TCP/IP.  
  
## <a name="configuring-the-sql-server-computer-to-use-tcpip-sockets"></a>Configuración del equipo SQL Server para usar Sockets TCP/IP  
 En el equipo SQL Server, ejecute el programa de instalación de SQL Server para que las interacciones con el origen de datos utilicen la biblioteca de red de Sockets TCP/IP.  
  
### <a name="to-specify-the-tcpip-socket-network-library-on-the-sql-server-computer"></a>Para especificar la biblioteca de red de Sockets TCP/IP en el SQL Server equipo  
  
### <a name="in-microsoft-sql-server-65"></a>En Microsoft SQL Server 6,5:  
  
1.  En el menú Inicio, seleccione programas, Microsoft SQL Server 6,5 y, a continuación, haga clic en programa de instalación de SQL.  
  
2.  Haga clic en continuar dos veces.  
  
3.  En el cuadro de diálogo Microsoft SQL Server-opciones, seleccione cambiar compatibilidad de red y, a continuación, haga clic en continuar.  
  
4.  Asegúrese de que la casilla Sockets TCP/IP está activada y haga clic en Aceptar.  
  
5.  Haga clic en continuar para finalizar y salga del programa de instalación.  
  
### <a name="in-microsoft-sql-server-70"></a>En Microsoft SQL Server 7,0:  
  
1.  En el menú Inicio, seleccione programas, Microsoft SQL Server 7,0 y, a continuación, haga clic en herramienta de red de servidor.  
  
2.  En la pestaña General del cuadro de diálogo, haga clic en Agregar.  
  
3.  En el cuadro de diálogo Agregar configuración de biblioteca de red, haga clic en TCP/IP.  
  
4.  En los cuadros número de puerto y dirección del proxy, escriba el número de puerto y la dirección del proxy proporcionados por el administrador de red.  
  
5.  Haga clic en Aceptar para finalizar y salga del programa de instalación.  
  
## <a name="configuring-the-web-server-to-use-tcpip-sockets"></a>Configuración del servidor web para usar Sockets TCP/IP  
 Hay dos opciones para configurar el servidor web para que utilice Sockets TCP/IP. Lo que haga depende de si se tiene acceso a todos los servidores de SQL Server desde el servidor web o SQL Server solo desde el servidor Web.  
  
 Si se tiene acceso a todos los servidores SQL Server desde el servidor Web, es necesario ejecutar la utilidad de configuración de cliente de SQL Server en el equipo servidor Web. Los siguientes pasos cambian la biblioteca de red predeterminada para todas las conexiones SQL Server realizadas desde este servidor Web de IIS para usar la biblioteca de red de Sockets TCP/IP.  
  
### <a name="to-configure-the-web-server-all-sql-servers"></a>Para configurar el servidor Web (todos los servidores SQL Server)  
  
### <a name="for-microsoft-sql-server-65"></a>Por Microsoft SQL Server 6,5:  
  
1.  En el menú Inicio, seleccione programas, Microsoft SQL Server 6,5 y, a continuación, haga clic en utilidad de configuración de cliente SQL.  
  
2.  Haga clic en la pestaña biblioteca de red.  
  
3.  En el cuadro red predeterminada, seleccione Sockets TCP/IP.  
  
4.  Haga clic en listo para guardar los cambios y salir de la utilidad.  
  
### <a name="for-microsoft-sql-server-70"></a>Por Microsoft SQL Server 7,0:  
  
1.  En el menú Inicio, seleccione programas, Microsoft SQL Server 7,0 y, a continuación, haga clic en herramienta de red de cliente.  
  
2.  Haga clic en la pestaña General.  
  
3.  En el cuadro biblioteca de red predeterminada, haga clic en TCP/IP.  
  
4.  Haga clic en Aceptar para guardar los cambios y salir de la utilidad.  
  
 Si se tiene acceso a un SQL Server específico desde un servidor Web, es necesario ejecutar la utilidad de configuración del cliente de SQL Server en el equipo del servidor Web. Para cambiar la biblioteca de red para una conexión de SQL Server específica, configure el software cliente de SQL Server en el equipo servidor web como se indica a continuación.  
  
### <a name="to-configure-the-web-server-a-specific-sql-server"></a>Para configurar el servidor Web (un SQL Server específico)  
  
### <a name="for-microsoft-sql-server-65"></a>Por Microsoft SQL Server 6,5:  
  
1.  En el menú Inicio, seleccione programas, Microsoft SQL Server 6,5 y, a continuación, haga clic en utilidad de configuración de cliente SQL.  
  
2.  Haga clic en la pestaña Opciones avanzadas.  
  
3.  En el cuadro servidor, escriba el nombre del servidor al que se va a conectar mediante Sockets TCP/IP.  
  
4.  En el cuadro Nombre de DLL, seleccione Sockets TCP/IP.  
  
5.  Haga clic en Agregar o modificar. Todos los orígenes de datos que apuntan a este servidor usarán ahora Sockets TCP/IP.  
  
6.  Haga clic en Done (Listo).  
  
### <a name="for-microsoft-sql-server-70"></a>Por Microsoft SQL Server 7,0:  
  
1.  En el menú Inicio, seleccione programas, Microsoft SQL Server 7,0 y, a continuación, haga clic en utilidad de configuración de cliente.  
  
2.  Haga clic en la pestaña General.  
  
3.  Haga clic en Agregar.  
  
4.  Escriba el alias del servidor en el cuadro alias del servidor. En el cuadro bibliotecas de red, haga clic en TCP/IP. En el cuadro Nombre de equipo, escriba el nombre de equipo del equipo que realiza escuchas para los clientes de Sockets TCP/IP. En el cuadro número de puerto, escriba el puerto en el que escucha el SQL Server.  
  
5.  Haga clic en aceptar y, a continuación, en Aceptar para salir de la utilidad.  
  
## <a name="see-also"></a>Consulte también  
 [Aspectos básicos de RDS](./rds-fundamentals.md)