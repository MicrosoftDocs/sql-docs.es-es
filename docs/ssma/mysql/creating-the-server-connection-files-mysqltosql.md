---
description: Creación de los archivos de conexión del servidor (MySQLToSQL)
title: Crear los archivos de conexión del servidor (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Server connection file validation
- Server connection files
ms.assetid: df0e970c-da0b-4118-b359-c9dcbbad16d6
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 439d70f428fff6e5445fca54e8bd669096a1c6c6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016635"
---
# <a name="creating-the-server-connection-files-mysqltosql"></a>Creación de los archivos de conexión del servidor (MySQLToSQL)
La información del servidor se puede especificar en la sección servidores del archivo de script o en un archivo de conexión de servidor independiente. El parámetro de línea de comandos para el archivo de conexión de servidor es, `-c <serverconnectionfile>` . Si el mismo identificador de servidor está presente en el archivo de script y en el archivo de conexión de servidor, se considera la definición del servidor en el archivo de script.  
  
**Ejemplo**:  
  
```xml  
<!--Sample of server connection file commands -->  
  
<mysql name ="<source-server-unique-name>">  
  
   <standard-mode>  
  
      <server value ="<server-name>"/>  
  
      <user-id value ="<user-name>"/>  
  
      <password value ="<password>"/>  
  
      <port value ="<port>"/>  
  
   </standard-mode>  
  
</mysql>  
```  
  
```xml  
<!--Connection to SQL Server-->  
  
<sql-server name="<target-server-unique-name>">  
  
   <sql-server-authentication>  
  
      <server value="<server-name>"/>  
  
      <database value="<database-name>"/>  
  
      <user-id value="<user-name>"/>  
  
      <password value="<password>"/>  
  
      <encrypt value="<true/false>"/>  
  
      <trust-server-certificate value="<true/false>"/>  
  
   </sql-server-authentication>  
  
</sql-server>  
```  
  
```xml  
<!--Connection to SQL Azure-->  
  
<sql-azure name="<target-server-unique-name>">  
  
   <server value="<server-name>"/>  
  
   <database value="<database-name>"/>  
  
   <user-id value="<user-name>"/>  
  
   <password value="<password>"/>  
  
</sql-azure>  
```  
  
## <a name="server-connection-file-validation"></a>Validación del archivo de conexión de servidor  
El usuario puede validar fácilmente su archivo de conexión de servidor con el archivo de definición de esquema **2ssconsolescriptserversschema. xsd '** disponible en la carpeta ' schemas '.  
  
## <a name="next-step"></a>siguiente paso  
El siguiente paso en el funcionamiento de la consola es la [ejecución de la consola de SSMA &#40;MySQLToSQL&#41;](../../ssma/mysql/executing-the-ssma-console-mysqltosql.md)  
  
## <a name="see-also"></a>Consulte también  
[Ejecutar la consola SSMA (MySQL)](./executing-the-ssma-console-mysqltosql.md)  
