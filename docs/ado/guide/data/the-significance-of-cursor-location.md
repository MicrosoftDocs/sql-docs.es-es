---
description: La importancia de la ubicación del Cursor
title: Importancia de la ubicación del cursor | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- server-side cursors [ADO]
- cursors [ADO], client-side
- client-side cursors [ADO]
- cursors [ADO], server-side
ms.assetid: 70ef5b1c-0459-41a1-b796-031f61a29a8a
author: rothja
ms.author: jroth
ms.openlocfilehash: a3caa56a5c3560367cc2f816c6aa2b1c12cbba2b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036944"
---
# <a name="the-significance-of-cursor-location"></a>La importancia de la ubicación del Cursor
Cada cursor utiliza recursos temporales para almacenar sus datos. Estos recursos pueden ser memoria, un archivo de paginación de disco, archivos temporales de disco o incluso almacenamiento temporal en la base de datos. El cursor se denomina cursor *del lado cliente* cuando estos recursos se encuentran en el equipo cliente. El cursor se denomina cursor de *servidor* cuando estos recursos se encuentran en el servidor.  
  
## <a name="client-side-cursors"></a>Client-Side cursores  
 En ADO, llame a para un cursor del lado cliente mediante el **CursorLocationEnum adUseClient.** Con un cursor que no sea de conjunto de claves del lado cliente, el servidor envía el conjunto de resultados completo a través de la red al equipo cliente. El equipo cliente proporciona y administra los recursos temporales necesarios para el cursor y el conjunto de resultados. La aplicación del lado cliente puede examinar todo el conjunto de resultados para determinar qué filas requiere.  
  
 Los cursores estáticos y controlados por conjunto de claves del lado cliente pueden suponer una carga significativa en la estación de trabajo si incluyen demasiadas filas. Aunque todas las bibliotecas de cursores son capaces de generar cursores con miles de filas, las aplicaciones diseñadas para capturar conjuntos de filas de gran tamaño pueden tener un rendimiento deficiente. Por supuesto, hay excepciones. En algunas aplicaciones, un cursor de gran tamaño del lado cliente puede ser perfectamente adecuado y el rendimiento podría no ser un problema.  
  
 Una ventaja obvia del cursor del lado cliente es una respuesta rápida. Una vez descargado el conjunto de resultados en el equipo cliente, el examen de las filas es muy rápido. Normalmente, la aplicación es más escalable con los cursores del lado cliente, ya que los requisitos de recursos del cursor se colocan en cada cliente independiente y no en el servidor.  
  
## <a name="server-side-cursors"></a>Server-Side cursores  
 En ADO, llame a para un cursor del servidor mediante el **CursorLocationEnum de adUseServer.** Con un cursor de servidor, el servidor administra el conjunto de resultados mediante los recursos proporcionados por el equipo servidor. El cursor de servidor devuelve solo los datos solicitados a través de la red. Este tipo de cursor puede proporcionar a veces mejor rendimiento que el cursor del lado cliente, especialmente en situaciones en las que el tráfico de red excesivo es un problema.  
  
 Sin embargo, es importante señalar que un cursor del lado servidor es, al menos, un gran consumo de recursos de servidor para todos los clientes activos. Debe planear en consecuencia para asegurarse de que el hardware del servidor es capaz de administrar todos los cursores del lado servidor solicitados por los clientes activos. Además, un cursor en el servidor puede ser lento porque solo proporciona acceso a una sola fila: no hay ningún cursor de lote disponible.  
  
 Los cursores del lado servidor son útiles cuando se insertan, actualizan o eliminan registros. Con los cursores del lado servidor, puede tener varias instrucciones activas en la misma conexión.
