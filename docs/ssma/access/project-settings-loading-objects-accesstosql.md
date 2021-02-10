---
description: Configuración del proyecto (cargar objetos) (AccessToSQL)
title: Configuración del proyecto (cargar objetos) (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 9ec1c1e8-a3e1-4e81-bf49-631f87daa209
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: a2d119ac2c58d0bea25a6f21ba4bf97cb47a8421
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066370"
---
# <a name="project-settings-loading-objects-accesstosql"></a>Configuración del proyecto (cargar objetos) (AccessToSQL)
La configuración del proyecto cargando objetos permite configurar el modo en que los objetos de base de datos de Access se sincronizan con los objetos de base de datos de SQL Server.  
  
Las acciones predeterminadas especifican la configuración predeterminada para actualizar objetos de la base de datos de Access y para sincronizar objetos con la base de datos de SQL Server. Para obtener más información, vea [actualizar desde la base de datos &#40;AccessToSQL&#41;](../../ssma/access/refresh-from-database-accesstosql.md)  
  
Puede tener acceso a dos páginas de sincronización diferentes que contengan la misma configuración:  
  
-   Para especificar la configuración de todos los proyectos de SSMA futuros, en el menú **herramientas** , haga clic en **configuración de DefaultProject** y, a continuación, haga clic en **cargar objetos** en la parte inferior del panel izquierdo.  
  
-   Para especificar la configuración del proyecto actual, en el menú **herramientas** , haga clic en **configuración del proyecto** y, a continuación, haga clic en **cargar objetos** en la parte inferior del panel izquierdo.  
  
## <a name="options"></a>Opciones  
  
##### <a name="misc"></a>Varios  
  
##### <a name="attempts"></a>Intentos  
Proporciona la información sobre el número de pasos que tardan los objetos en cargarse en SQL Server. La carga de objetos en SQL Server se realiza normalmente en varias fases. Los objetos que no se cargan en el primer paso, como las claves externas, pueden cargarse correctamente en la siguiente fase.  
  
De forma predeterminada, el valor es 2.  
  
## <a name="synchronization-for-sql-server"></a>Sincronización para SQL Server  
  
##### <a name="default-action-on-local-and-remote-object-change"></a>Acción predeterminada en el cambio de objetos locales y remotos  
Especifica el valor predeterminado en el cuadro de diálogo de sincronización cuando la definición del objeto cambia en SSMA y en el servidor de base de datos.  
  
-   Si selecciona **actualizar desde base de** datos, SSMA cargará las definiciones de base de datos en los metadatos cuando se cumpla la condición.  
  
-   Si selecciona **escribir en la base** de datos, SSMA actualizará los objetos de la base de datos según el contenido de los metadatos de SSMA cuando se cumpla la condición.  
  
-   Si selecciona **omitir**, SSMA no realizará ninguna acción de actualización.  
  
##### <a name="default-action-on-local-object-change"></a>Acción predeterminada en el cambio de objeto local  
Especifica el valor predeterminado en el cuadro de diálogo de sincronización cuando el objeto cambia en SSMA.  
  
-   Si selecciona **actualizar desde base de** datos, SSMA cargará las definiciones de base de datos en los metadatos cuando se cumpla la condición.  
  
-   Si selecciona **escribir en la base** de datos, SSMA actualizará el objeto en la base de datos según el contenido de los metadatos de SSMA cuando se cumpla la condición.  
  
-   Si selecciona **omitir**, SSMA no realizará ninguna acción de actualización.  
  
##### <a name="default-action-on-remote-object-change"></a>Acción predeterminada en el cambio de objeto remoto  
Especifica el valor predeterminado en el cuadro de diálogo de sincronización cuando los objetos cambian en el servidor de base de datos.  
  
-   Si selecciona **actualizar desde base de** datos, SSMA cargará las definiciones de base de datos en los metadatos cuando se cumpla la condición.  
  
-   Si selecciona **escribir en la base** de datos, SSMA actualizará el objeto en la base de datos según el contenido de los metadatos de SSMA cuando se cumpla la condición.  
  
-   Si selecciona **omitir**, SSMA no realizará ninguna acción de actualización.  
  
##### <a name="default-action-when-local-object-metadata-is-missing"></a>Acción predeterminada cuando faltan metadatos de objetos locales  
Especifica el valor predeterminado en el cuadro de diálogo de sincronización cuando faltan metadatos locales.  
  
-   Si selecciona **actualizar desde base de datos**, SSMA selecciona la opción actualizar desde la base de datos cuando se cumple la condición.  
  
-   Si selecciona **escribir en la base de datos**, SSMA eliminará el objeto de la base de datos cuando se cumpla la condición.  
  
-   Si selecciona **omitir**, SSMA no realizará ninguna acción de actualización.  
  
