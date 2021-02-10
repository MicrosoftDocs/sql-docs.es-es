---
description: Crear los archivos de conexión del servidor (AccessToSQL)
title: Crear los archivos de conexión del servidor (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 08/17/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 829153be-aa8e-4162-87e8-69882feecf19
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 9d956167cd6a4cea41d9381996a0418b2cd5f4c3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044945"
---
# <a name="creating-the-server-connection-files-accesstosql"></a>Crear los archivos de conexión del servidor (AccessToSQL)
La información del servidor se puede especificar en la sección servidores del archivo de script. La información del servidor también se puede especificar en un archivo de conexión de servidor independiente. El parámetro de línea de comandos para el archivo de conexión de servidor es `-c <serverconnectionfile>` . Si el mismo identificador de servidor está presente en los archivos de conexión del servidor y del script, se considerará la definición del servidor en el archivo de script.  
  
```xml  
<!--Sample of server connection file commands -->  
<!--Connection to SQL Server-->  
  
<sql-server name="target_3">  
  
  <sql-server-authentication>  
  
    <server value="$TargetServerName$"/>  
  
    <database value="$TargetDB$"/>  
  
    <user-id value="$TargetUserName$"/>  
  
    <password value="$TargetPassword$"/>  
  
    <encrypt value="true"/>  
  
    <trust-server-certificate value="true"/>  
  
  </sql-server-authentication>  
  
</sql-server>  
```  
  
```xml  
<!--Connection to SQL Azure-->  
  
<sql-azure name="target_azure">  
  
  <server value="$TargetAzureServerName$"/>  
  
  <database value="$TargetAzureDB$"/>  
  
  <user-id value="$TargetAzureUserName$"/>  
  
  <password value="$TargetAzurePassword$"/>  
  
</sql-azure>  
```  
  
## <a name="server-connection-file-validation"></a>Validación del archivo de conexión de servidor  
El usuario puede validar fácilmente su archivo de conexión de servidor con el archivo de definición de esquema **' A2SSConsoleScriptServersSchema. xsd '** disponible en la carpeta ' schemas '.  
  
## <a name="next-step"></a>Paso siguiente  
El siguiente paso en el funcionamiento de la consola es la [ejecución de la consola de SSMA &#40;AccessToSQL&#41;](../../ssma/access/executing-the-ssma-console-accesstosql.md)  
  
## <a name="see-also"></a>Consulte también  
[Ejecutar la consola SSMA (acceso)](./executing-the-ssma-console-accesstosql.md)  
