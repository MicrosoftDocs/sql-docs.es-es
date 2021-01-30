---
description: Transiciones de entorno
title: Transiciones de entorno | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- environment transitions [ODBC]
- transitioning states [ODBC], environment
- state transitions [ODBC], environment
ms.assetid: 9d11b1ab-f4c8-48ca-9812-8c04303f939d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: daef0bc276831358cd3d82dd10a1dd44082214af
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194842"
---
# <a name="environment-transitions"></a>Transiciones de entorno
Los entornos ODBC tienen los tres Estados siguientes.  
  
|Estado|Descripción|  
|-----------|-----------------|  
|E0|Entorno sin asignar|  
|E1|Entorno asignado, conexión sin asignar|  
|E2|Entorno asignado, conexión asignada|  
  
 En las tablas siguientes se muestra cómo afecta cada función ODBC al estado del entorno.  
  
## <a name="sqlallochandle"></a>SQLAllocHandle  
  
|E0<br /><br /> Sin asignar|E1<br /><br /> Allocated|E2<br /><br /> Conexión|  
|------------------------|----------------------|-----------------------|  
|E1 [1]|--[4]|--[4]|  
|ADMITIR dos|E2 [5]<br />HY010 1,8|--[4]|  
|ADMITIR 3|ADMITIR|--[4]|  
  
 [1] esta fila muestra las transiciones cuando se SQL_HANDLE_ENV *HandleType* .  
  
 [2] esta fila muestra las transiciones cuando se SQL_HANDLE_DBC *HandleType* .  
  
 [3] esta fila muestra las transiciones cuando *HandleType* se SQL_HANDLE_STMT o SQL_HANDLE_DESC.  
  
 [4] al llamar a **SQLAllocHandle** con *OutputHandlePtr* que apuntan a un identificador válido se sobrescribe ese identificador. Puede tratarse de un error de programación de la aplicación.  
  
 [5] se estableció el atributo de entorno SQL_ATTR_ODBC_VERSION en el entorno.  
  
 [6] no se ha establecido el atributo de entorno SQL_ATTR_ODBC_VERSION en el entorno.  
  
## <a name="sqldatasources-and-sqldrivers"></a>SQLDataSources y SQLDrivers  
  
|E0<br /><br /> Sin asignar|E1<br /><br /> Allocated|E2<br /><br /> Conexión|  
|------------------------|----------------------|-----------------------|  
|ADMITIR|--[1]<br />HY010 dos|--[1]<br />HY010 dos|  
  
 [1] se estableció el atributo de entorno SQL_ATTR_ODBC_VERSION en el entorno.  
  
 [2] no se ha establecido el atributo de entorno SQL_ATTR_ODBC_VERSION en el entorno.  
  
## <a name="sqlendtran"></a>SQLEndTran  
  
|E0<br /><br /> Sin asignar|E1<br /><br /> Allocated|E2<br /><br /> Conexión|  
|------------------------|----------------------|-----------------------|  
|ADMITIR dimensional|--[3]<br />HY010 4|--[3]<br />HY010 4|  
|ADMITIR dos|ADMITIR|--|  
  
 [1] esta fila muestra las transiciones cuando se SQL_HANDLE_ENV *HandleType* .  
  
 [2] esta fila muestra las transiciones cuando se SQL_HANDLE_DBC *HandleType* .  
  
 [3] se estableció el atributo de entorno SQL_ATTR_ODBC_VERSION en el entorno.  
  
 [4] no se ha establecido el atributo de entorno SQL_ATTR_ODBC_VERSION en el entorno.  
  
## <a name="sqlfreehandle"></a>SQLFreeHandle  
  
|E0<br /><br /> Sin asignar|E1<br /><br /> Allocated|E2<br /><br /> Conexión|  
|------------------------|----------------------|-----------------------|  
|ADMITIR dimensional|E0|HY010|  
|ADMITIR dos|ADMITIR|--[4]<br />E1 [5]|  
|ADMITIR 3|ADMITIR|--|  
  
 [1] esta fila muestra las transiciones cuando se SQL_HANDLE_ENV *HandleType* .  
  
 [2] esta fila muestra las transiciones cuando se SQL_HANDLE_DBC *HandleType* .  
  
 [3] esta fila muestra las transiciones cuando *HandleType* se SQL_HANDLE_STMT o SQL_HANDLE_DESC.  
  
 [4] había otros identificadores de conexión asignados.  
  
 [5] el identificador de conexión especificado en el *identificador* era el único identificador de conexión asignado.  
  
## <a name="sqlgetdiagfield-and-sqlgetdiagrec"></a>SQLGetDiagField y SQLGetDiagRec  
  
|E0<br /><br /> Sin asignar|E1<br /><br /> Allocated|E2<br /><br /> Conexión|  
|------------------------|----------------------|-----------------------|  
|ADMITIR dimensional|--|--|  
|ADMITIR dos|ADMITIR|--|  
  
 [1] esta fila muestra las transiciones cuando se SQL_HANDLE_ENV *HandleType* .  
  
 [2] esta fila muestra las transiciones cuando *HandleType* se SQL_HANDLE_DBC, SQL_HANDLE_STMT o SQL_HANDLE_DESC.  
  
## <a name="sqlgetenvattr"></a>SQLGetEnvAttr  
  
|E0<br /><br /> Sin asignar|E1<br /><br /> Allocated|E2<br /><br /> Conexión|  
|------------------------|----------------------|-----------------------|  
|ADMITIR|--[1]<br />HY010 dos|--|  
  
 [1] se estableció el atributo de entorno SQL_ATTR_ODBC_VERSION en el entorno.  
  
 [2] no se ha establecido el atributo de entorno SQL_ATTR_ODBC_VERSION en el entorno.  
  
## <a name="sqlsetenvattr"></a>SQLSetEnvAttr  
  
|E0<br /><br /> Sin asignar|E1<br /><br /> Allocated|E2<br /><br /> Conexión|  
|------------------------|----------------------|-----------------------|  
|ADMITIR|--[1]<br />HY010 dos|HY011|  
  
 [1] se estableció el atributo de entorno SQL_ATTR_ODBC_VERSION en el entorno.  
  
 [2] el argumento de *atributo* no se SQL_ATTR_ODBC_VERSION y el atributo de entorno de SQL_ATTR_ODBC_VERSION no se ha establecido en el entorno.  
  
## <a name="all-other-odbc-functions"></a>Todas las demás funciones ODBC  
  
|E0<br /><br /> Sin asignar|E1<br /><br /> Allocated|E2<br /><br /> Conexión|  
|------------------------|----------------------|-----------------------|  
|ADMITIR|ADMITIR|--|
