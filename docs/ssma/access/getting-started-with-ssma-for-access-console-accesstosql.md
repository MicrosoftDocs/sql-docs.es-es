---
description: Introducción con SSMA para la consola de acceso (AccessToSQL)
title: Introducción con SSMA para la consola de acceso (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 8585ec16-7e0a-483a-b250-adab9b9232a3
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: d39f130c2b2ff8edd95fb52a78b7865b5c6dc868
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044515"
---
# <a name="getting-started-with-ssma-for-access-console-accesstosql"></a>Introducción con SSMA para la consola de acceso (AccessToSQL)
En esta sección se describe el procedimiento para iniciar y comenzar a usar la aplicación de consola de Access. Además, aquí se enumeran las convenciones que se usan en una ventana de salida de la consola de SSMA típica.  
  
## <a name="launching-ssma-console"></a>Inicio de la consola SSMA  
Siga estos pasos para iniciar la aplicación de consola SSMA:  
  
1.  Vaya a **Inicio** y seleccione **todos los programas**.  
  
2.  Haga clic en el acceso directo del **símbolo del sistema de SQL Server Migration Assistant para Access** .  
  
    Muestra el menú de uso de la consola de SSMA y `(/? Help)` , para ayudarle a empezar a trabajar con la aplicación de consola.  
  
## <a name="procedure-for-using-the-ssma-console"></a>Procedimiento para usar la consola de SSMA  
Una vez que se haya iniciado correctamente la consola de en el sistema Windows, puede usar los pasos siguientes para trabajar en ella:  
  
1.  Configure la consola de SSMA a través de los archivos de script. Para obtener más información sobre esta sección, vea [crear archivos de Script &#40;AccessToSQL&#41;](../../ssma/access/creating-script-files-accesstosql.md).  
  
2.  [Crear archivos de valores de variable &#40;AccessToSQL&#41;](../../ssma/access/creating-variable-value-files-accesstosql.md)  
  
3.  [Crear los archivos de conexión del servidor &#40;AccessToSQL&#41;](../../ssma/access/creating-the-server-connection-files-accesstosql.md)  
  
4.  [Ejecutar la consola de SSMA &#40;AccessToSQL&#41;](../../ssma/access/executing-the-ssma-console-accesstosql.md) según las necesidades del proyecto  
  
Características adicionales:  
  
1.  [Especificar una contraseña](managing-passwords-accesstosql.md) y exportarla o importarla en otras máquinas Windows  
  
2.  [Generar informes](generating-reports-accesstosql.md) para ver los informes de salida XML detallados para la evaluación de/conversion y la migración de datos. También se pueden generar informes de errores detallados para los comandos de actualización y sincronización.  
  
## <a name="ssma-console-output-conventions"></a>Convenciones de salida de la consola SSMA  
Al ejecutar los comandos y las opciones de script de SSMA, el programa de consola muestra los resultados y los mensajes (información, error, etc.) al usuario en la consola de o, si es necesario, redirige a un archivo de salida XML. Cada tipo de mensaje de la salida se indica mediante un color único. Por ejemplo, el mensaje de texto en color blanco denota los comandos del archivo de script; el color verde representa un mensaje para la entrada del usuario, etc.  
  
![Salida de la consola SSMA](../../ssma/access/media/ssmaconsoleoutput.jpg "Salida de la consola SSMA")  
  
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
[Instalación de SQL Server Migration Assistant para el acceso](installing-sql-server-migration-assistant-for-access-accesstosql.md)  
  
