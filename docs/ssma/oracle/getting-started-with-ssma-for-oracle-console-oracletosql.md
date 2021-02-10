---
description: Introducción a la consola de SSMA para Oracle (OracleToSQL)
title: Introducción con SSMA para la consola de Oracle (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Oracle Console, Console Output Conventions
- Oracle Console, Launching Console
ms.assetid: 667a5e4a-6848-4973-a72d-1287f64718ac
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: fd87502c1eda3dc8d4caf4c654804b164f24f36f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100068060"
---
# <a name="getting-started-with-ssma--for-oracle-console-oracletosql"></a>Introducción a la consola de SSMA para Oracle (OracleToSQL)
En esta sección se describe el procedimiento para iniciar y comenzar a usar la aplicación de consola de Oracle. Además, aquí se enumeran las convenciones que se usan en una ventana de salida de la consola de SSMA típica.  
  
## <a name="launching-ssma-console"></a>Inicio de la consola SSMA  
Siga estos pasos para iniciar la aplicación de consola SSMA:  
  
1.  Vaya a **Inicio** y seleccione **todos los programas**.  
  
2.  Haga clic en el acceso directo del **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] símbolo del sistema de Migration Assistant para Oracle** .  
  
    Muestra el menú de uso de la consola de SSMA y `(/? Help)` , para ayudarle a empezar a trabajar con la aplicación de consola.  
  
## <a name="procedure-for-using-the-ssma-console"></a>Procedimiento para usar la consola de SSMA  
Una vez que se haya iniciado correctamente la consola de en el sistema Windows, puede usar los pasos siguientes para trabajar en ella:  
  
1.  Configure la consola de SSMA a través de los archivos de script. Para obtener más información sobre esta sección, vea [crear archivos de Script &#40;OracleToSQL&#41;](../../ssma/oracle/creating-script-files-oracletosql.md) .  
  
2.  [Crear archivos de valores de variable &#40;OracleToSQL&#41;](../../ssma/oracle/creating-variable-value-files-oracletosql.md)  
  
3.  [Crear los archivos de conexión del servidor &#40;OracleToSQL&#41;](../../ssma/oracle/creating-the-server-connection-files-oracletosql.md)  
  
4.  [Ejecutar la consola de SSMA &#40;OracleToSQL&#41;](../../ssma/oracle/executing-the-ssma-console-oracletosql.md) según las necesidades del proyecto  
  
Características adicionales:  
  
1.  [Especificar una contraseña](managing-passwords-oracletosql.md) y exportarla o importarla en otras máquinas Windows  
  
2.  [Generar informes](generating-reports-oracletosql.md) para ver los informes de salida XML detallados para la evaluación de/conversion y la migración de datos. También se pueden generar informes de errores detallados para los comandos de actualización y sincronización.  
  
## <a name="ssma-console-output-conventions"></a>Convenciones de salida de la consola SSMA  
Al ejecutar los comandos y las opciones de script de SSMA, el programa de consola muestra los resultados y los mensajes (información, error, etc.) al usuario en la consola de o, si es necesario, redirige a un archivo de salida XML. Cada tipo de mensaje de la salida se indica mediante un color único. Por ejemplo, el mensaje de texto en color blanco denota los comandos del archivo de script; el color verde representa un mensaje para la entrada del usuario, etc.  
  
![Salida de la consola SSMA_Oracle](../../ssma/db2/media/ssmaconsoleoutput_oracle.jpg "Salida de la consola SSMA_Oracle")  
  
Interpretación de color de la salida de la consola en la tabla siguiente:  
  
|Color|Descripción|  
|---------|---------------|  
|Rojo|Error irrecuperable durante la ejecución|  
|Gris|Marca de fecha y hora, mensaje al usuario|  
|Blanco|Comandos de archivo de script, tipo de mensaje|  
|Amarillo|Advertencia|  
|Verde|Solicitar la entrada del usuario|  
|Cian|Inicio, finalización y resultado de una operación|  
  
## <a name="see-also"></a>Consulte también  
[Instalación de SSMA para Oracle](installing-ssma-for-oracle-oracletosql.md)  
  
