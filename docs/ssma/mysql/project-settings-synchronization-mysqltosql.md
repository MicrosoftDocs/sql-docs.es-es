---
description: Configuración del proyecto (sincronización) (MySQLToSQL)
title: Configuración del proyecto (sincronización) (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 42061ff7-e6e7-497b-a0d9-440b9cf5986c
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 3f778b2bd361fab5ac1210005076692c813bb4b9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017315"
---
# <a name="project-settings-synchronization-mysqltosql"></a>Configuración del proyecto (sincronización) (MySQLToSQL)
La **configuración del proyecto** de sincronización le permite configurar el modo en que los objetos de base de datos de MySQL se sincronizan con SQL Server objetos de base de datos.  
  
Las acciones predeterminadas especifican la configuración predeterminada para actualizar objetos de la base de datos MySQL y para sincronizar objetos con la base de datos SQL Server. Para obtener más información, vea [actualizar desde la base de datos &#40;MySQLToSQL&#41;](../../ssma/mysql/refresh-from-database-mysqltosql.md)  
  
Puede tener acceso a dos páginas de sincronización diferentes que contengan la misma configuración:  
  
-   Para especificar la configuración de todos los proyectos de SSMA futuros, en el menú **herramientas** , haga clic en **configuración de DefaultProject** y, a continuación, haga clic en **sincronización** en la parte inferior del panel izquierdo.  
  
-   Para especificar la configuración del proyecto actual, en el menú **herramientas** , haga clic en **configuración del proyecto** y, a continuación, haga clic en **sincronización** en la parte inferior del panel izquierdo.  
  
## <a name="options"></a>Opciones  
  
##### <a name="misc"></a>Varios  
  
##### <a name="attempts"></a>Intentos  
Proporciona la información sobre el número de pasos que tardan los objetos en cargarse en SQL Server. La carga de objetos en SQL Server se realiza normalmente en varias fases. Los objetos que no se cargan en el primer paso, como las claves externas, pueden cargarse correctamente en la siguiente fase.  
  
De forma predeterminada, el valor es 2.  
  
## <a name="synchronization-for-mysql"></a>Sincronización para MySQL  
  
##### <a name="action-on-local-and-remote-object-change"></a>Acción en el cambio de objetos locales y remotos  
Especifica el valor predeterminado en el cuadro de diálogo de sincronización cuando la definición del objeto cambia en SSMA y en el servidor de base de datos.  
  
-   Si selecciona actualizar desde base de datos, SSMA cargará las definiciones de base de datos en los metadatos cuando se cumpla la condición.  
  
-   Si selecciona omitir, SSMA no realizará ninguna acción de actualización.  
  
##### <a name="action-on-local-object-change"></a>Acción en el cambio de objeto local  
Especifica el valor predeterminado en el cuadro de diálogo de sincronización cuando el objeto cambia en SSMA.  
  
-   Si selecciona **actualizar desde base de** datos, SSMA cargará las definiciones de base de datos en los metadatos cuando se cumpla la condición.  
  
-   Si selecciona **omitir**, SSMA no realizará ninguna acción de actualización.  
  
##### <a name="action-on-remote-object-change"></a>Acción en el cambio de objeto remoto  
Especifica el valor predeterminado en el cuadro de diálogo de sincronización cuando los objetos cambian en el servidor de base de datos.  
  
-   Si selecciona **actualizar desde base de** datos, SSMA cargará las definiciones de base de datos en los metadatos cuando se cumpla la condición.  
  
-   Si selecciona **omitir**, SSMA no realizará ninguna acción de actualización.  
  
##### <a name="action-when-local-object-metadata-is-missing"></a>Acción cuando faltan metadatos de objeto local  
Especifica el valor predeterminado en el cuadro de diálogo de sincronización cuando faltan los metadatos locales.  
  
-   Si selecciona **actualizar desde base de** datos, SSMA cargará las definiciones de base de datos en los metadatos cuando se cumpla la condición.  
  
-   Si selecciona **omitir**, SSMA no realizará ninguna acción de actualización  
  
## <a name="synchronization-for-sql-server"></a>Sincronización para SQL Server  
  
##### <a name="action-on-local-and-remote-object-change"></a>Acción en el cambio de objetos locales y remotos  
Especifica el valor predeterminado en el cuadro de diálogo de sincronización cuando la definición del objeto cambia en SSMA y en el servidor de base de datos.  
  
-   Si selecciona **actualizar desde base de** datos, SSMA cargará las definiciones de base de datos en los metadatos cuando se cumpla la condición.  
  
-   Si selecciona **escribir en la base** de datos, SSMA actualizará los objetos de la base de datos según el contenido de los metadatos de SSMA cuando se cumpla la condición.  
  
-   Si selecciona **omitir**, SSMA no realizará ninguna acción de actualización.  
  
##### <a name="action-on-local-object-change"></a>Acción en el cambio de objeto local  
Especifica el valor predeterminado en el cuadro de diálogo de sincronización cuando el objeto cambia en SSMA.  
  
-   Si selecciona **actualizar desde base de** datos, SSMA cargará las definiciones de base de datos en los metadatos cuando se cumpla la condición.  
  
-   Si selecciona **escribir en la base** de datos, SSMA actualizará el objeto en la base de datos según el contenido de los metadatos de SSMA cuando se cumpla la condición.  
  
-   Si selecciona **omitir**, SSMA no realizará ninguna acción de actualización.  
  
##### <a name="action-on-remote-object-change"></a>Acción en el cambio de objeto remoto  
Especifica el valor predeterminado en el cuadro de diálogo de sincronización cuando los objetos cambian en el servidor de base de datos.  
  
-   Si selecciona **actualizar desde base de** datos, SSMA cargará las definiciones de base de datos en los metadatos cuando se cumpla la condición.  
  
-   Si selecciona **escribir en la base** de datos, SSMA actualizará el objeto en la base de datos según el contenido de los metadatos de SSMA cuando se cumpla la condición.  
  
-   Si selecciona **omitir**, SSMA no realizará ninguna acción de actualización.  
  
##### <a name="action-when-local-object-metadata-is-missing"></a>Acción cuando faltan metadatos de objeto local  
Especifica el valor predeterminado en el cuadro de diálogo de sincronización cuando faltan metadatos locales.  
  
-   Si selecciona **actualizar desde base de datos**, SSMA selecciona la opción actualizar desde la base de datos cuando se cumple la condición.  
  
-   Si selecciona **escribir en la base de datos**, SSMA eliminará el objeto de la base de datos cuando se cumpla la condición.  
  
-   Si selecciona **omitir**, SSMA no realizará ninguna acción de actualización.  
  
