---
author: MikeRayMSFT
ms.prod: sql
ms.technology: big-data-cluster
ms.topic: include
ms.date: 06/22/2020
ms.author: mikeray
ms.openlocfilehash: 599b4072c5d03c8a4753c7c00bc7edc14e0f3b05
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100038955"
---
A partir de SQL Server 2019 CU5, al implementar un nuevo clúster con autenticación básica, todos los puntos de conexión, incluido el uso de puerta de enlace `AZDATA_USERNAME` y `AZDATA_PASSWORD`. Los puntos de conexión de los clústeres que se actualizan a CU5 continúan usando `root` como nombre de usuario para conectarse al punto de conexión de puerta de enlace. Este cambio no se aplica a las implementaciones que utilizan la autenticación de Active Directory. Consulte [Credenciales para acceder a los servicios a través del punto de conexión de puerta de enlace](../big-data-cluster/release-notes-big-data-cluster.md#credentials-for-accessing-services-through-gateway-endpoint) en las notas de la versión.