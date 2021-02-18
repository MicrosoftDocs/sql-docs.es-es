---
title: Opciones de línea de comandos de la herramienta de administración
description: La herramienta de administración de Distributed Replay de SQL Server, DReplay.exe, es una herramienta de línea de comandos para comunicarse con el controlador de reproducción distribuida.
titleSuffix: SQL Server Distributed Replay
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: c01b0ed3-67e4-4561-92d2-a8fbb086aca8
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 08/12/2016
ms.openlocfilehash: 031668199ce4d54a29a2e781e4f6d200f58ed509
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345954"
---
# <a name="administration-tool-command-line-options-distributed-replay-utility"></a>Opciones de línea de comandos de la herramienta de administración (utilidad Distributed Replay)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

La herramienta de administración de Distributed Replay de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], **DReplay.exe**, es una herramienta de línea de comandos para comunicarse con el controlador de reproducción distribuida. Utilice la herramienta de administración para iniciar, supervisar y cancelar operaciones en el controlador.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") Para más información sobre las convenciones de sintaxis que se usan con la sintaxis de la herramienta de administración, consulte [Convenciones de sintaxis de Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
dreplay {preprocess|replay|status|cancel} [options] [-?]}  
  
Usage:  
  
  dreplay preprocess [-m controller] -i input_trace_file  
    -d controller_working_dir [-c config_file] [-f status_interval]  
  
  dreplay replay [-m controller] -d controller_working_dir [-o]  
    [-s target_server] -w clients [-c config_file]  
    [-f status_interval]  
  
  dreplay status [-m controller] [-f status_interval]  
  
  dreplay cancel [-m controller] [-q]   
```  
  
## <a name="remarks"></a>Observaciones  
 Puede emitir las siguientes opciones de línea de comandos con **DReplay.exe**:  
  
 **preprocess**  
 Inicia la fase de preprocesamiento. El controlador prepara la información de seguimiento de entrada, que se capturó en el entorno de producción, para la reproducción en el servidor de destino.  
  
 **reproducir**  
 Inicia la fase de reproducción de eventos. El controlador envía los datos de la reproducción a los clientes especificados, inicia la reproducción distribuida y sincroniza los clientes. Opcionalmente, cada cliente que se ha seleccionado registra la actividad de reproducción y guarda los archivos de seguimiento del resultado localmente.  
  
 **status**  
 Consulta el controlador y muestra el estado actual.  
  
 **cancel**  
 Cancela la operación que se está ejecutando actualmente en el controlador.  
  
 Para obtener información de la sintaxis detallada que incluye los argumentos de comando y ejemplos, vea los siguientes temas:  
  
-   [Opción de preprocesamiento &#40;herramienta de administración Distributed Replay&#41;](../../tools/distributed-replay/preprocess-option-distributed-replay-administration-tool.md)  
  
-   [Opción Replay &#40;herramienta de administración Distributed Replay&#41;](../../tools/distributed-replay/replay-option-distributed-replay-administration-tool.md)  
  
-   [Opción Status &#40;herramienta de administración de Distributed Replay&#41;](../../tools/distributed-replay/status-option-distributed-replay-administration-tool.md)  
  
-   [Opción cancel &#40;herramienta de administración de Distributed Replay&#41;](../../tools/distributed-replay/cancel-option-distributed-replay-administration-tool.md)  
  
 Las RPC se reproducen como RPC y no como eventos de lenguaje.  
  
## <a name="permissions"></a>Permisos  
 Debe ejecutar la herramienta de administración como un usuario interactivo, como un usuario local o una cuenta de usuario de dominio. Para utilizar una cuenta de usuario local, la herramienta de administración y el controlador se deben estar ejecutando en el mismo equipo.  
  
 Para más información, consulte [Distributed Replay Security](../../tools/distributed-replay/distributed-replay-security.md).  
  
## <a name="see-also"></a>Consulte también  
 [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md)  
  
  
