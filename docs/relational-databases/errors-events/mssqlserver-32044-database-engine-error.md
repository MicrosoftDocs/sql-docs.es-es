---
description: MSSQLSERVER_32044
title: MSSQLSERVER_32044 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 32044 (Database Engine error)
ms.assetid: f2d073be-d9a1-4837-8a38-028d3e3403bd
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 57e646e24729dfe242f553979cc4d986c545725d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200221"
---
# <a name="mssqlserver_32044"></a>MSSQLSERVER_32044
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|32044|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|SQLErrorNum32044|  
|Texto del mensaje|Se produjo la alerta de 'sobrecarga de confirmación del servidor reflejado'. El valor actual de '%d' sobrepasa el umbral '%d'.|  
  
## <a name="explanation"></a>Explicación  
Este evento de creación de reflejo de la base de datos se genera en la instancia del servidor principal para indicar que el tiempo de espera de confirmación agregado alcanzó o superó el valor umbral especificado por el usuario debido a la creación de reflejo de la base de datos. El tiempo de espera es el producto del número de transacciones y el tiempo de cada una. Por ejemplo, los siguientes casos producen 1000 milisegundos de tiempo de espera: 1000 transacciones * 1 milisegundo y 1 transacción \* 1000 milisegundos. El aumento en el tiempo de espera de confirmación puede deberse a una sobrecarga en el recuento de transacciones, retrasos al enviar el registro o retrasos al vaciar el registro en la instancia del servidor reflejado.  
  
La cantidad de sobrecarga de confirmación de reflejo es una medida del rendimiento que le puede ayudar a evaluar el impacto sobre el rendimiento actual del funcionamiento sincrónico. Esta medida solo es relevante en el modo de alta seguridad. Como el modo de alta seguridad es sincrónico, la instancia del servidor principal espera a confirmar la transacción después de enviar una entrada de registro a la instancia del servidor reflejado hasta que recibe la confirmación de que la instancia del servidor reflejado ha escrito la entrada de registro en el disco. La entrada de registro permanece en disco en la instancia del servidor reflejado mientras espera a ser restaurada en la base de datos reflejada.  
  
## <a name="user-action"></a>Acción del usuario  
Compruebe las cargas en las instancias del servidor principal y reflejado y su conexión de red para localizar la causa.  
  
## <a name="see-also"></a>Consulte también  
[Creación de reflejo de la base de datos &#40;SQL Server&#41;](~/database-engine/database-mirroring/database-mirroring-sql-server.md)  
[Usar alertas y umbrales de advertencia de las métricas de rendimiento de la creación de reflejo &#40;SQL Server&#41;](~/database-engine/database-mirroring/use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server.md)  
  
