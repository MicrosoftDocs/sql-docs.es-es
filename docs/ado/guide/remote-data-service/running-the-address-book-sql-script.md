---
description: Ejecución del script de SQL de la libreta de direcciones
title: Ejecutando el script SQL de la libreta de direcciones | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- address book application scenario [ADO]
- RDS scenarios [ADO]
ms.assetid: 409b3f8b-0ced-4867-acbe-b245dcdf6702
author: rothja
ms.author: jroth
ms.openlocfilehash: 8e36c60e0c3e56ab0c159c362e98c2280e6c5426
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036402"
---
# <a name="running-the-address-book-sql-script"></a>Ejecución del script de SQL de la libreta de direcciones
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
 Debe usar la utilidad de línea de comandos ISQL/Query Analyzer o el administrador de SQL Server Enterprise para ejecutar el script SQL incluido (Sampleemp. SQL) que:  
  
-   Crea una base de datos nueva, AddrBookDB, en el dispositivo predeterminado.  
  
-   Se conecta a la base de datos, AddrBookDB.  
  
-   Crea una tabla de empleados.  
  
-   Rellena la tabla con datos de ejemplo.  
  
-   Ejecuta una instrucción SELECT simple para comprobar el rellenado de la tabla de base de datos.  
  
-   Configura una cuenta de usuario denominada "adcdemo" con la contraseña "adcdemo".  
  
#### <a name="to-run-the-sampleempsql-script-in-microsoft-sql-server-65"></a>Para ejecutar el script Sampleemp. SQL en Microsoft SQL Server 6,5  
  
1.  Haga clic en **Inicio**, elija **programas** y, a continuación, seleccione **Microsoft SQL Server 6,5**. Haga clic en **Administrador corporativo de SQL**.  
  
2.  En el menú **herramientas** , haga clic en **herramienta de consulta SQL**.  
  
3.  Haga clic en **cargar script SQL** y vaya a c:\Platform SDK\Samples\DataAccess\RDS\AddressBook.  
  
4.  Seleccione el archivo Sampleemp. SQL. Haga clic en **Abrir**.  
  
5.  Haga clic en el botón **Ejecutar consulta** (la flecha verde en la barra de herramientas).  
  
6.  Una vez ejecutada, cierre las ventanas **consulta** y **Administrador corporativo** .  
  
#### <a name="to-run-the-sampleempsql-script-in-microsoft-sql-server-70"></a>Para ejecutar el script Sampleemp. SQL en Microsoft SQL Server 7,0  
  
1.  Haga clic en **Inicio**, elija **programas** y, a continuación, seleccione **Microsoft SQL Server 7,0**. Haga clic en **Administrador corporativo**.  
  
2.  Asegúrese de que la SQL Server que desea usar está seleccionada en la lista de servidores registrados en el Administrador corporativo.  
  
3.  En el menú **herramientas** , haga clic en **SQL Server analizador de consultas**.  
  
4.  Haga clic en el botón **cargar script SQL** (abrir carpeta en la barra de herramientas) y vaya a c:\Platform SDK\Samples\DataAccess\RDS\AddressBook.  
  
5.  Seleccione el archivo Sampleemp. SQL. Haga clic en **Abrir**.  
  
6.  Haga clic en el botón **Ejecutar consulta** (la flecha verde en la barra de herramientas) o **F5**.  
  
7.  Una vez ejecutada, cierre las ventanas **consulta**, **analizador de consultas** y **Administrador corporativo** .  
  
## <a name="see-also"></a>Consulte también  
 [Ejecución de la aplicación de ejemplo de la libreta de direcciones](./running-the-address-book-sample-application.md)