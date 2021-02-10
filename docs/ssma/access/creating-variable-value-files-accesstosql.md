---
description: Crear archivos de valor variable (AccessToSQL)
title: Crear archivos de valor variable (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 08/17/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 808595c3-8ef1-40bd-a93e-5cf237950e08
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: b7653d131ca9f345353de46b2579735a527bca5a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044935"
---
# <a name="creating-variable-value-files-accesstosql"></a>Crear archivos de valor variable (AccessToSQL)
Un archivo de valores de variable es un archivo XML que contiene los valores de los parámetros de los comandos (como el nombre del servidor de origen o de destino) que cambian con frecuencia en las migraciones del servidor. Cuando se produce un gran número de migraciones de base de datos, se crean y se hace referencia a varios archivos de variables para almacenar el valor de cada servidor de origen en un archivo de script maestro con el modificador **-v** en la línea de comandos. Este comportamiento ayuda a mantener valores estáticos en algunos archivos de script con los valores de variable en varios archivos de variables.  
  
> [!NOTE]  
> -  Los nombres de variable tienen el prefijo y el sufijo con un símbolo $ (dólar). Si no se asigna un valor a una variable en el archivo de valores de variable, se producirá un error durante el análisis del archivo de script, lo que provocará que se detenga el proceso de ejecución de la consola.  
> -  El carácter de escape para **$** es **$$** . Si el valor de una variable o un valor estático de un parámetro contiene un **$** símbolo (dólar), **$$** debe especificarse para tratarlo como un carácter en lugar de una variable.  
> -  En lo que respecta a la facilidad de mantenimiento, las variables se pueden declarar dentro de `'variable-group'` elementos para la separación lógica de las variables definidas por el usuario.  El uso de este elemento no es obligatorio.  
  
**Ejemplos:**  
  
**Ejemplo 1:**  
  
```xml  
<!--Sample of variable value file commands-->  
  
<variables>  
  
  <variable-group name="ProjectSpecs">  
  
    <variable name="$type$" value="MyProject"/>  
  
    <variable name="$project_folder$" value=".\$project_name$"/>  
  
    <variable name="$project_name$" value="$type$ConsoleProject"/>  
  
    <variable name="$project_overwrite$" value="true"/>  
  
    <variable name="$project_type$" value="sql-server-2008"/>  
  
  </variable-group>  
  
</variables>  
```  
**Ejemplo 2:**  
  
```xml  
<!--Sample of variable value file commands-->  
  
<variables>  
  
  <variable-group name="SQLServerParams">  
  
    <variable-group name="SqlServerConnectionParams">  
  
      <variable name="$TargetServerName$" value="xxx"/>  
  
      <variable name="$TargetDB$" value="xxx"/>  
  
      <variable name="$TargetUserName$" value="xxx"/>  
  
      <variable name="$TargetPassword$" value="xxx"/>  
  
      <variable name="$TargetIsTrusted$" value="xxx"/>  
  
      <variable name="$TrustedConnection$" value="xxx"/>  
  
    </variable-group>  
  
    <variable-group name="SqlServerObjectParams">  
  
      <variable name="$ObjectName1$" value="TestTable1"/>  
  
      <variable name="$ObjectName2$" value="TestProc1"/>  
  
    </variable-group>  
  
  </variable-group>  
  
</variables>  
```  
  
## <a name="variable-value-file-validation"></a>Validación de archivos de valores de variables  
El usuario puede validar fácilmente su archivo de valores de variable en el archivo de definición de esquema **ConsoleScriptVariablesSchema. xsd** disponible en la carpeta ' schemas '.  
  
## <a name="next-step"></a>Paso siguiente  
El siguiente paso en el funcionamiento de la consola es [la creación de los archivos de conexión del servidor &#40;AccessToSQL&#41;](../../ssma/access/creating-the-server-connection-files-accesstosql.md)  
  
## <a name="see-also"></a>Consulte también  
[Crear los archivos de conexión del servidor (acceso)](./creating-the-server-connection-files-accesstosql.md)  
