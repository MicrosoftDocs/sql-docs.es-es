---
description: Obtención de los datos de diagnóstico después de un bloqueo de SQL Server Management Studio (SSMS)
title: Solución de problemas de un sistema no responde o un bloqueo con SSMS
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: c28ffa44-7b8b-4efa-b755-c7a3b1c11ce4
author: markingmyname
ms.author: maghan
ms.reviewer: drskwier, sstein
ms.custom: seo-lt-2019
ms.date: 09/18/2019
ms.openlocfilehash: 5e4b7e4926265a1d6358b95bba4fae7b63ae016f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066080"
---
# <a name="get-diagnostic-data-after-a-sql-server-management-studio-ssms-crash"></a>Obtención de los datos de diagnóstico después de un bloqueo de SQL Server Management Studio (SSMS)

[!INCLUDE[Applies to](../../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

## <a name="get-full-memory-dump-after-an-unresponsive-system-or-crash"></a>Obtención de un volcado de memoria completo después de un bloqueo o un sistema sin respuesta

Obtenga un volcado de memoria completo de SQL Server Management Studio (SSMS) cuando no responde o se produce un bloqueo.

Para capturar información de diagnóstico para solucionar un bloqueo o un SSMS que no responde, siga estos pasos.

1. Descargue [ProcDump](/sysinternals/downloads/procdump).

2. Descomprima la descarga en una carpeta.

3. Abra el símbolo del sistema y ejecute el siguiente comando.

    ```console
    <PathToProcDumpFolder>\procdump.exe -e -h -ma -w ssms.exe
    ```

    Si se le solicita que acepte un contrato de licencia, seleccione *Acepto*.

4. Inicie SSMS, si todavía no se ha iniciado.

5. Reproduzca el problema.

6. El texto debe aparecer en la ventana de símbolo del sistema cmd para escribir el archivo de volcado de memoria; espere a que termine.

7. Cree una carpeta y copie el archivo *.dmp que se escribe en esa carpeta.

8. Copie los archivos siguientes en la misma carpeta.

    "C:\Windows\Microsoft.NET\Framework\v4.0.30319\mscordacwks.dll"  "C:\Windows\Microsoft.NET\Framework\v4.0.30319\SOS.dll"  "C:\Windows\Microsoft.NET\Framework\v4.0.30319\clr.dll"

9. Comprima la carpeta.

## <a name="get-full-memory-dump-for-an-outofmemoryexception"></a>Obtención de un volcado de memoria completo para una excepción OutOfMemoryException

Obtenga un volcado de memoria completo de SSMS cuando se inicia una excepción OutOfMemoryException.

Puede obtener un volcado de memoria completo con cualquier excepción administrada.

Para capturar información de diagnóstico para solucionar problemas de una excepción OutOfMemoryException desde SSMS, siga estos pasos.

1. Descargue [ProcDump](/sysinternals/downloads/procdump).

2. Descomprima la descarga en una carpeta.

3. Abra el símbolo del sistema y ejecute el siguiente comando:

    ```console
    <PathToProcDumpFolder>\procdump.exe -e 1 -f System.OutOfMemoryException -ma -w ssms.exe
    ```

    Si se le solicita que acepte un contrato de licencia, seleccione *Acepto*.

4. Inicie SQL Server Management Studio si no se ha iniciado ya.

5. Reproduzca el problema.

6. El texto debe aparecer en la ventana de símbolo del sistema cmd para escribir el archivo de volcado de memoria; espere a que termine.

7. Cree una carpeta y copie el archivo *.dmp que se escribe en esa carpeta.

8. Copie los archivos siguientes en la misma carpeta.

    "C:\Windows\Microsoft.NET\Framework\v4.0.30319\mscordacwks.dll"  "C:\Windows\Microsoft.NET\Framework\v4.0.30319\SOS.dll"  "C:\Windows\Microsoft.NET\Framework\v4.0.30319\clr.dll"

9. Comprima la carpeta.