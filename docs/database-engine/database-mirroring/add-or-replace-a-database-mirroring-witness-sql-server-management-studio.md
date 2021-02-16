---
title: Adición o reemplazo de un testigo del reflejo (SSMS)
description: Obtenga información sobre cómo agregar o reemplazar un testigo de creación de reflejos de bases de datos mediante SQL Server Management Studio cuando los puntos de conexión de creación de reflejos de bases de datos usan la autenticación de Windows.
ms.custom: seo-lt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- witness [SQL Server], establishing
- database mirroring [SQL Server], witness
ms.assetid: 4b5ecffd-f025-4ab7-b69d-8958c6477127
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 810b3ac70c66801335ac3235439e5a0731c43eb8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350746"
---
# <a name="add-or-replace-a-database-mirroring-witness-sql-server-management-studio"></a>Agregar o reemplazar un testigo de creación de reflejo de la base de datos (SQL Server Management Studio)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Si los extremos de la creación de reflejo de la base de datos usan la Autenticación de Windows, puede usar [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] para agregar o reemplazar un testigo. Cuando se agrega un testigo en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] , se cambia también el modo de operación al modo de alta seguridad con conmutación automática por error.  
  
> [!NOTE]  
>  Es muy recomendable que el testigo se encuentre en otro equipo de alguno de los asociados. La cuenta de servicio que usa el testigo debe estar en el mismo dominio que las cuentas de servicio que usan las instancias del servidor principal y del servidor reflejado, o bien debe estar en un dominio de confianza.  
  
### <a name="to-add-or-replace-a-witness"></a>Para agregar o reemplazar un testigo  
  
1.  Después de conectarse a la instancia del servidor principal, en el Explorador de objetos, haga clic en el nombre del servidor para expandir el árbol del servidor.  
  
2.  Expanda **Bases de datos** y seleccione la base de datos principal de la sesión en la que desee agregar o reemplazar un testigo.  
  
3.  Haga clic con el botón derecho en la base de datos, seleccione **Tareas** y, luego, haga clic en **Reflejado**. Así se abre la página **Creación de reflejo** del cuadro de diálogo **Propiedades de la base de datos** .  
  
4.  Haga clic en **Configurar seguridad**.  
  
5.  Cuando aparezca la pantalla de bienvenida del **Asistente para la configuración de seguridad de la creación de reflejo de bases de datos** , seleccione **Siguiente**.  
  
6.  En el cuadro de diálogo **Incluir servidor testigo** , haga clic en **Sí** y, después, en **Siguiente**.  
  
7.  En el cuadro de diálogo **Elegir los servidores para configurar** , se marcará automáticamente la casilla **Instancia del servidor testigo** . Haga clic en **Next**.  
  
8.  En el cuadro de diálogo **Instancia del servidor principal** , conserve las opciones existentes de puerto y extremos. Haga clic en **Next**.  
  
9. En el cuadro de diálogo **Instancia del servidor testigo** , haga clic en **Conectar**.  
  
10. En el cuadro de diálogo **Conectar al servidor** , especifique la instancia del servidor testigo en el campo **Nombre del servidor** y use la Autenticación de Windows (opción predeterminada). Haga clic en **Conectar**.  
  
11. Una vez que se haya establecido una conexión, en el cuadro de diálogo **Instancia del servidor testigo** se mostrará el puerto de escucha y el extremo de la creación de reflejo de la base de datos de la instancia del servidor testigo. Haga clic en **Next**.  
  
12. El cuadro de diálogo **Cuentas de servicio** contiene campos para las cuentas de servicio de dominio de las instancias del servidor reflejado, principal y testigo.  
  
    -   Si todas las instancias del servidor utilizan la misma cuenta de servicio, deje los campos en blanco.  
  
    -   Si la instancia del servidor testigo usa una cuenta de servicio diferente de las de los asociados, llene los campos **Principal**, **Reflejado** y **Testigo** con el nombre de la cuenta:  
  
         *DOMAINNAME* **\\** *nombre de usuario*  
  
         El nombre de dominio debe estar en mayúsculas.  
  
     Haga clic en **Next**.  
  
13. Si lo desea, en la pantalla de resumen **Finalización del asistente** , compruebe la configuración del testigo y haga clic en **Finalizar**.  
  
14. Cuando finalice, el asistente volverá al cuadro de diálogo **Propiedades de la base de datos** , donde en el campo **Testigo** se mostrará la dirección de red del servidor del testigo. Además, se seleccionará automáticamente **Modo de alta seguridad con conmutación por error automática (sincrónica)** , que es necesario con un testigo.  
  
     Para habilitar el testigo y cambiar la sesión al modo de alta seguridad con conmutación por error automática, haga clic en **Aceptar**.  
  
## <a name="see-also"></a>Consulte también  
 [Testigo de creación de reflejo de la base de datos](../../database-engine/database-mirroring/database-mirroring-witness.md)   
 [Creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Propiedades de la base de datos &#40;página Creación de reflejo&#41;](../../relational-databases/databases/database-properties-mirroring-page.md)   
 [Establecer una sesión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)   
 [Testigo de creación de reflejo de la base de datos](../../database-engine/database-mirroring/database-mirroring-witness.md)  
  
  
