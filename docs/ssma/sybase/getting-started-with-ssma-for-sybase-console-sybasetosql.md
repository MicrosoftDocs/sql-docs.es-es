---
description: Introducción con la consola de SSMA para Sybase (SybaseToSQL)
title: Introducción con la consola de SSMA para Sybase (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/30/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Sybase Console,Launching SSMA Console
- Sybase Console,Output Conventions
- Sybase Console,Procedure for Using Console
ms.assetid: 43219dbe-bcfa-427d-9242-f07b1455f15f
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 19fca27ba0606a8e56c1594df852edbc439e7a92
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064970"
---
# <a name="getting-started-with-the-ssma-for-sybase-console-sybasetosql"></a>Introducción con la consola de SSMA para Sybase (SybaseToSQL)
En esta sección se describe el procedimiento para iniciar y comenzar a usar la aplicación de consola SSMA para Sybase. También se enumeran las convenciones que se usan en una ventana de salida de la consola de SSMA típica.  
  
## <a name="launching-the-ssma-console"></a>Inicio de la consola de SSMA  
Siga estos pasos para iniciar la aplicación de consola SSMA:  
  
1.  Vaya a Inicio y seleccione todos los programas.  
  
2.  Haga clic en el acceso directo del **símbolo del sistema de SQL Server Migration Assistant para Sybase** .  
  
    Muestra el menú de uso de la consola de SSMA y `(/? Help)` , para ayudarle a empezar a trabajar con la aplicación de consola.  
  
## <a name="using-the-ssma-console"></a>Usar la consola de SSMA  
Una vez que se haya iniciado correctamente la consola de en el sistema Windows, puede usar los pasos siguientes para trabajar en ella:  
  
1.  Configure la consola de SSMA a través de los archivos de script. Para obtener más información sobre esta sección, vea [crear archivos de Script &#40;SybaseToSQL&#41;](../../ssma/sybase/creating-script-files-sybasetosql.md).  
  
2.  [Crear archivos de valores de variable &#40;SybaseToSQL&#41;](../../ssma/sybase/creating-variable-value-files-sybasetosql.md)  
  
3.  [Crear los archivos de conexión del servidor &#40;SybaseToSQL&#41;](../../ssma/sybase/creating-the-server-connection-files-sybasetosql.md)  
  
4.  [La ejecución de la consola de SSMA &#40;SybaseToSQL&#41;](../../ssma/sybase/executing-the-ssma-console-sybasetosql.md) según las necesidades del proyecto. 
  
Características adicionales:  
  
1.  [Especifique una contraseña](managing-passwords-sybasetosql.md) y expórtela o impórtela en otros equipos de Windows.  
  
2.  [Generar informes](generating-reports-sybasetosql.md) para ver los informes de salida XML detallados para la evaluación, la conversión y la migración de datos. También puede generar informes de errores detallados para los comandos de actualización y sincronización.  
  
## <a name="ssma-console-output-conventions"></a>Convenciones de salida de la consola SSMA  
Al ejecutar los comandos y las opciones de script de SSMA, el programa de consola muestra los resultados y los mensajes (información, error, etc.) al usuario en la consola de o, si es necesario, redirige a un archivo de salida XML. Cada tipo de mensaje de la salida se indica mediante un color único. Por ejemplo, el mensaje de texto en color blanco denota los comandos del archivo de script; el color verde representa un mensaje para la entrada del usuario, etc.  
  
![Captura de pantalla que muestra un ejemplo de salida de Sybase de la consola de SSMA.](../../ssma/sybase/media/ssmaconsoleoutput_sybase.JPG "Salida de la consola SSMA_Sybase")  
  
La interpretación de color de la salida de la consola aparece en la tabla siguiente:  
  
|Color|Descripción|  
|---------|---------------|  
|Rojo|Error irrecuperable durante la ejecución|  
|Gris|Marca de fecha y hora, mensaje al usuario|  
|Blanco|Comandos de archivo de script, tipo de mensaje|  
|Amarillo|Advertencia|  
|Verde|Solicitar la entrada del usuario|  
|Cian|Inicio, finalización y resultado de una operación|  
  
## <a name="see-also"></a>Consulte también  
[Instalación de SSMA para SAP ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-for-sybase-sybasetosql.md)  
