---
description: Configuración de la migración de datos (SybaseToSQL)
title: Configuración de migración de datos (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 91f7f558-025d-4f4d-ac2c-aa095e7d1ace
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 4bd91336e15f7d9404632aafd0b9cbc29e0dc2e0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058470"
---
# <a name="data-migration-settings-oracletosql"></a>Configuración de la migración de datos (SybaseToSQL)
  
## <a name="data-migration-settings"></a>Configuración de la migración de datos  
La **configuración de migración de datos** permite al usuario escribir consultas personalizadas para la migración de datos.  
  
-   Esta pestaña está disponible cuando **Opciones de migración de datos extendidos** está establecida en **Mostrar** y está oculta cuando el valor está establecido en **ocultar** en la configuración del proyecto. Para obtener más información sobre la configuración de la migración de proyectos, vea [configuración del proyecto (migración)](./project-settings-migration-oracletosql.md) .  
  
-   El análisis de instrucciones SQL personalizadas se implementará en la pestaña **configuración de migración de datos** del nodo de tabla.  
  
-   A continuación se muestran las dos casillas disponibles en la **configuración de migración de datos** , es decir:  
  
    1.  Truncar SQL Server tabla  
  
    2.  Usar selección personalizada  
  
1.  **Truncar SQL Server tabla:**  
     Esta opción permite que el usuario tenga una vista clara de los datos migrados en la base de datos de destino.  
  
    -   De forma predeterminada, este cuadro de texto está activado.  
  
    -   Si este cuadro de texto está desactivado, los datos migrados se agregarán a los datos existentes en la base de datos de destino.  
  
2.  **Usar selección personalizada:**  
     Esta opción permite al usuario modificar la instrucción **Select** (la instrucción **Select** permite a los usuarios seleccionar los datos que se van a mostrar en la base de datos de destino).  
  
    1.  De forma predeterminada, este cuadro de texto está desactivado.  
  
    2.  Si este cuadro de texto está activado, permite a los usuarios modificar la instrucción **Select** .  
  
Hay dos botones presentes:  
  
-   **Aplicar:** Haga clic en **aplicar** para aplicar la configuración que ha cambiado.  
  
-   **Cancelar:** Haga clic en **Cancelar** para restaurar la configuración presente antes de que se realizaran los cambios.  
  
## <a name="see-also"></a>Consulte también  
[Migración de datos de Oracle a SQL Server](migrating-oracle-data-into-sql-server-oracletosql.md)  
