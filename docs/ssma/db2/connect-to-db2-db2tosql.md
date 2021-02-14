---
description: Conectarse a DB2 (DB2ToSQL)
title: Conectarse a DB2 (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 9d485fd0-ab5d-402a-a59a-e9982a61b7de
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 24ffef4775339baf182b01062f2a06136a04abf1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081576"
---
# <a name="connect-to-db2-db2tosql"></a>Conectarse a DB2 (DB2ToSQL)
Utilice el cuadro de diálogo **conectar con DB2** para conectarse a la base de datos DB2 que desea migrar.  
  
Para obtener acceso a este cuadro de diálogo, en el menú **archivo** , seleccione **conectarse a DB2**. Si ya se ha conectado, el comando se **vuelve a conectar a DB2**.  
  
## <a name="options"></a>Opciones  
**Proveedor**  
Seleccione el proveedor de acceso a datos para la conexión a la base de datos DB2. Los proveedores disponibles son el proveedor de cliente DB2 y el proveedor de OLE DB. El valor predeterminado es proveedor de cliente de DB2.  
  
**Modo**  
Seleccione estándar, TNSNAME o modo de cadena de conexión.  
  
-   En el modo estándar, escriba o seleccione los valores del proveedor, el nombre del servidor, el puerto del servidor, el SID de DB2, el nombre de usuario y la contraseña.  
  
-   En el modo TNSNAME, escriba el identificador de conexión (alias TNS) de la base de datos DB2, el nombre de usuario y la contraseña.  
  
-   En el modo de cadena de conexión, se proporciona una cadena de conexión.  
  
    > [!IMPORTANT]  
    > No se recomienda usar el modo de cadena de conexión porque el texto podría incluir contraseñas y se envía como texto sin cifrar.  
  
El valor predeterminado es el modo estándar.  
  
**Nombre del servidor**  
Escriba el nombre del servidor DB2. El nombre del servidor predeterminado es el mismo que el nombre del equipo. Se trata de una opción de modo estándar.  
  
**Puerto del servidor**  
Si usa un número de puerto distinto de 1521 (predeterminado) para las conexiones a DB2, escriba el número de puerto. Se trata de una opción de modo estándar.  
  
**Identificador de conexión**  
Escriba el identificador de la conexión de DB2. Este es el alias de la base de datos tal y como se define en el archivo archivo tnsnames. ora local.  
  
Se trata de una opción de modo TNSNAME.  
  
**SID DE DB2**  
Escriba el SID para la base de datos. El SID es un identificador que distingue la base de datos DB2 en un equipo. El SID predeterminado de una base de datos de son los ocho primeros caracteres del nombre de la base de datos.  
  
Se trata de una opción de modo estándar.  
  
**Nombre de usuario**  
Escriba el nombre de usuario que SSMA usará para conectarse a la base de datos DB2.  
  
**Contraseña**  
Escriba la contraseña del nombre de usuario.  
  
**Cadena de conexión**  
> [!IMPORTANT]  
> No se recomienda usar el modo de cadena de conexión porque el texto podría incluir contraseñas y se envía como texto sin cifrar.  
  
Si usa el modo de cadena de conexión, escriba la cadena de conexión completa para la conexión a DB2.  
  
Las cadenas de conexión constan de pares de nombre y valor de parámetro.  
  
-   Para obtener OLE DB información de la cadena de conexión, vea [proveedor OLE DB de Microsoft para DB2](../../ado/guide/appendixes/microsoft-ole-db-provider-for-oracle.md) artículo en MSDN Library.  
  
En el caso de las cadenas de conexión de SSMA, incluya siempre el parámetro Provider. Además, asegúrese de incluir el parámetro de puerto al conectarse a DB2.  
