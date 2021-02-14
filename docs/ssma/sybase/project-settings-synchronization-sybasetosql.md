---
description: Configuración del proyecto (sincronización) (SybaseToSQL)
title: Configuración del proyecto (sincronización) (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 2cd6bc01-b8e5-4312-83a4-eac66dc1d460
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: a5db262979e7c7e267491e34b54302c0a5b36999
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070520"
---
# <a name="project-settings-synchronization-sybasetosql"></a>Configuración del proyecto (sincronización) (SybaseToSQL)
La página sincronización del cuadro de diálogo **configuración del proyecto** contiene opciones que personalizan el modo en que SSMA carga los objetos de base de datos, como tablas y procedimientos almacenados, en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure.  
  
Puede tener acceso a dos páginas de sincronización diferentes que contengan la misma configuración:  
  
-   Si desea especificar la configuración de todos los proyectos de SSMA futuros, en el menú **herramientas** , seleccione **configuración predeterminada del proyecto**, seleccione el tipo de proyecto de migración para el que se deben ver o cambiar las opciones de configuración en la lista desplegable de la versión de destino de la **migración** y, a continuación, seleccione **sincronización** en la parte inferior del panel izquierdo.  
  
-   Para especificar la configuración del proyecto actual, en el menú **herramientas** , seleccione **configuración del proyecto** y, a continuación, seleccione **sincronización** en la parte inferior del panel izquierdo.  
  
## <a name="options"></a>Opciones  
**Intentos**  
Especifica el número de intentos que SSMA debe realizar cuando carga objetos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Los objetos que no se cargan en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el intento actual se intentarán de nuevo hasta que SSMA alcance el número máximo de intentos en el proceso de sincronización actual.  
  
## <a name="synchronization-for-sql-server"></a>Sincronización para SQL Server  
**Actualizar el objeto local en el cambio de objeto local y remoto**  
Especifica si SSMA debe reemplazar los metadatos del objeto local con metadatos de objetos remotos si cambian los objetos locales y remotos.  
Si selecciona **actualizar desde base de** datos, SSMA cargará las definiciones de base de datos en los metadatos cuando se cumpla la condición.  
Si selecciona **escribir en la base** de datos, SSMA actualizará los objetos de la base de datos según el contenido de los metadatos de SSMA cuando se cumpla la condición.  
Si selecciona **omitir**, SSMA no realizará ninguna acción de actualización.   
El conjunto de opciones predeterminado es **escribir en la base de datos.**  
  
**Actualizar objeto local en el cambio de objeto local**  
Especifica si SSMA debe reemplazar los metadatos del objeto local con metadatos de objetos remotos si cambia el objeto remoto.  
Si selecciona **actualizar desde base de** datos, SSMA cargará las definiciones de base de datos en los metadatos cuando se cumpla la condición.  
Si selecciona **escribir en la base** de datos, SSMA actualizará el objeto en la base de datos según el contenido de los metadatos de SSMA cuando se cumpla la condición.  
Si selecciona **omitir**, SSMA no realizará ninguna acción de actualización.   
El conjunto de opciones predeterminado es **escribir en la base de datos**.  
  
**Actualizar objeto local en cambio de objeto remoto**  
Especifica si SSMA debe reemplazar los metadatos del objeto local con metadatos de objetos remotos si cambia el objeto remoto.  
Si selecciona **actualizar desde base de** datos, SSMA cargará las definiciones de base de datos en los metadatos cuando se cumpla la condición.  
Si selecciona **escribir en la base** de datos, SSMA actualizará el objeto en la base de datos según el contenido de los metadatos de SSMA cuando se cumpla la condición.  
Si selecciona **omitir**, SSMA no realizará ninguna acción de actualización.   
El conjunto de opciones predeterminado es **actualizar desde base de datos**.  
  
**Actualizar cuando faltan metadatos de objetos locales**  
Especifica si SSMA debe crear metadatos locales si existe un objeto en la base de datos de destino, pero no localmente.  
Si selecciona **actualizar desde base de datos**, SSMA selecciona la opción actualizar desde la base de datos cuando se cumple la condición.  
Si selecciona **escribir en la base de datos**, SSMA eliminará el objeto de la base de datos cuando se cumpla la condición.  
Si selecciona **omitir**, SSMA no realizará ninguna acción de actualización.   
El conjunto de opciones predeterminado es **actualizar desde base de datos**.  
  
