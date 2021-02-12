---
description: Obtención de un volcado de memoria completo
title: Obtención de un volcado de memoria completo para solucionar problemas de SSMS
Description: Capture información de diagnóstico de SQL Server Management Studio (SSMS) para que pueda solucionar problemas con bloqueos o sistemas que no responden.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: c28ffa44-7b8b-4efa-b755-c7a3b1c11ce4
author: markingmyname
ms.author: maghan
ms.reviewer: drskwier, sstein
ms.custom: seo-lt-2019
ms.date: 05/17/2019
ms.openlocfilehash: 38e711e9857aea6601878f712c9c5de8cf908e03
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017405"
---
# <a name="get-full-memory-dump"></a>Obtención de un volcado de memoria completo

[!INCLUDE[Applies to](../../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

En este artículo, obtendrá información sobre cómo capturar información de diagnóstico para solucionar problemas de un sistema que no responde o se bloquea, que haya experimentado en SQL Server Management Studio (SSMS).

Para capturar información de diagnóstico para solucionar problemas, siga estos pasos.

1. Descargue [ProcDump](/sysinternals/downloads/procdump).

2. Descomprima la descarga en una carpeta.

3. Abra una ventana de símbolo del sistema (como `cmd.exe`) y ejecute el siguiente comando:

    ```console
    <PathToProcDumpFolder>\procdump.exe -e -h -ma -w ssms.exe
    ```

    Debe solicitarle que acepte un contrato de licencia y seleccione **Acepto**.

4. Inicie SQL Server Management Studio (SSMS) si aún no se ha iniciado ya.

5. Reproduzca el problema.

6. El texto debe aparecer en la ventana de símbolo del sistema cmd para escribir el archivo de volcado de memoria; espere a que termine.

7. Cree una carpeta y copie el archivo *.dmp que se escribe en esa carpeta.

8. Copie los archivos siguientes en la misma carpeta.

    * "C:\Windows\Microsoft.NET\Framework\v4.0.30319\mscordacwks.dll"
    * "C:\Windows\Microsoft.NET\Framework\v4.0.30319\SOS.dll"
    * "C:\Windows\Microsoft.NET\Framework\v4.0.30319\clr.dll"

9. Comprima la carpeta.

## <a name="outofmemoryexception"></a>OutOfMemoryException

También puede obtener el volcado de memoria completo de SSMS cuando produce una excepción OutOfMemoryException (puede ser cualquier excepción administrada).

Para capturar información de diagnóstico para solucionar problemas de una excepción OutOfMemoryException desde SSMS, siga estos pasos.

1. Descargue [ProcDump](/sysinternals/downloads/procdump).

2. Descomprima la descarga en una carpeta.

3. Abra el símbolo del sistema y ejecute el siguiente comando:

    ```cmd
    <PathToProcDumpFolder>\procdump.exe -e 1 -f System.OutOfMemoryException -ma -w ssms.exe
    ```

    Debe solicitarle que acepte un contrato de licencia y seleccione **Acepto**.

4. Inicie SQL Server Management Studio si no se ha iniciado ya.

5. Reproduzca el problema.

6. El texto debe aparecer en la ventana de símbolo del sistema cmd para escribir el archivo de volcado de memoria; espere a que termine.

7. Cree una carpeta y copie el archivo *.dmp que se escribe en esa carpeta.

8. Copie los archivos siguientes en la misma carpeta.

    * "C:\Windows\Microsoft.NET\Framework\v4.0.30319\mscordacwks.dll"
    * "C:\Windows\Microsoft.NET\Framework\v4.0.30319\SOS.dll"
    * "C:\Windows\Microsoft.NET\Framework\v4.0.30319\clr.dll"

9. Comprima la carpeta.

## <a name="share-the-information"></a>Compartir la información

1. Para compartir la información con el equipo de SSMS, registre el problema en https://aka.ms/sqlfeedback.

2. Después, comparta el archivo de volcado de memoria recopilado a OneDrive (o equivalente), donde el archivo se puede recopilar.

    > [!Important]
    > Los archivos de volcado de memoria pueden contener información confidencial.

## <a name="next-steps"></a>Pasos siguientes

[SQL Server Management Studio](../sql-server-management-studio-ssms.md)