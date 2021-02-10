---
title: Conectarse a Sybase (SybaseToSQL) | Microsoft Docs
description: Conéctese a la instancia de SAP ASE para comenzar la migración mediante SSMA para Sybase (SAP ASE). Utilice el cuadro de diálogo conectar a Sybase.
author: nahk-ivanov
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 524f95ef-10bd-497c-84ca-c06a0ae794fb
ms.author: alexiva
ms.openlocfilehash: 8d43e6a48746fbb1587327b430b67c6959731013
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064100"
---
# <a name="connect-to-sybase-sybasetosql"></a>Conectarse a Sybase (SybaseToSQL)

Utilice el cuadro de diálogo **conectar a Sybase** para conectarse a la instancia de Sybase Adaptive Server Enterprise (ASE) que desea migrar.

Para obtener acceso a este cuadro de diálogo, en el menú **archivo** , seleccione **conectarse a Sybase**. Si ya se ha conectado, el comando se **vuelve a conectar a Sybase**.

## <a name="options"></a>Opciones

**Proveedor**  
Seleccione cualquiera de los proveedores instalados en el equipo para conectarse al servidor de Sybase.

**Modo**  
Seleccione el modo de conexión estándar o avanzado. En el modo estándar, escriba o seleccione valores para el nombre del servidor, el puerto, el nombre de usuario y la contraseña. En el modo avanzado, se proporciona una cadena de conexión.

**Nombre del servidor**  
Escriba o seleccione el nombre o la dirección IP del servidor adaptable. El nombre del servidor predeterminado es el mismo que el nombre del equipo. Se trata de una opción de modo estándar.

**Puerto del servidor**  
Si usa un puerto no predeterminado para las conexiones con ASE, escriba el número de puerto. El número de puerto predeterminado es 5000. Se trata de una opción de modo estándar.
  
**Nombre de usuario**  
Escriba el nombre de usuario que se usa para conectarse a ASE. Se trata de una opción de modo estándar.

**Contraseña**  
Escriba la contraseña del nombre de usuario. Se trata de una opción de modo estándar.

**Cadena de conexión**  
Escriba la cadena de conexión completa para la conexión a ASE.

Las cadenas de conexión constan de pares de nombre y valor de parámetro. Los nombres de los parámetros varían con el proveedor que se está usando.

**Los parámetros de conexión para varios proveedores son los siguientes:**

1. Parámetros de conexión para el **proveedor de OLE DB**

   |Configuración|Parámetro Sybase 12,5|Parámetro de Sybase 15|
   |-----------|-------------------------|-----------------------|
   |Nombre de servidor|Nombre del servidor|Server|
   |Port|Dirección del puerto del servidor|Port|
   |Nombre de usuario|Identificador de usuario|Identificador de usuario|
   |Contraseña|Contraseña|Contraseña|
   |Proveedor|Proveedor|Proveedor|

   En el caso de Sybase ASE 12,5, una cadena de conexión de ejemplo es la siguiente:

   `Server Name=sybserver;User ID=MyUserID;Password=MyP@$$word;Provider=Sybase.ASEOLEDBProvider;`

   En el caso de Sybase ASE 15, una cadena de conexión de ejemplo es la siguiente:

   `Server=sybserver;User ID=MyUserID;Password=MyP@$$word;Provider=ASEOLEDB;Port=5000;`

2. Parámetros de conexión para el **proveedor ODBC**

   |Configuración|Parámetro de Sybase 12,5/15|
   |-----------|-----------------------------|
   |Nombre del controlador|controlador|
   |Nombre del servidor|Server|
   |Nombre de usuario|UID|
   |Contraseña|Pwd|
   |Número de puerto|Port|

   En el caso de Sybase ASE 12,5 o 15, una cadena de conexión de ejemplo es la siguiente:

   `driver=Adaptive Server Enterprise;Server=sybserver;uid=MyUserID;pwd=MyP@$$word;Port=5000;`

3. Parámetros de conexión para el **proveedor ADO.net**

   |Configuración|Parámetro de Sybase 12,5/15|
   |-----------|-----------------------------|
   |Nombre del servidor|Server|
   |Nombre de usuario|UID|
   |Contraseña|Pwd|
   |Número de puerto|Port|

   Un ejemplo de la cadena de conexión para el proveedor ADO.NET es el siguiente:

   `Server=sybserver;Port=5000;uid=MyUserID;pwd=MyP@$$word;`

   Para obtener más información, consulte la documentación de ASE.

   Se trata de una opción de modo avanzado.

## <a name="next-steps"></a>Pasos siguientes

El siguiente paso del proceso de migración consiste en [conectarse a SQL Server](connect-to-sql-server-sybasetosql.md).
