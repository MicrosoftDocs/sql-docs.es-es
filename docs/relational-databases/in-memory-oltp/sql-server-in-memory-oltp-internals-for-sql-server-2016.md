---
title: Parámetros internos de OLTP en memoria de SQL Server
description: Obtenga información sobre la implementación de la tecnología OLTP en memoria de SQL Server, en la que se declaran tablas como optimizadas para memoria a fin de habilitar las funciones de OLTP en memoria.
ms.custom: seo-dt-2019
ms.date: 09/14/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: b14da361-a6b8-4d85-b196-7f2f13650f44
author: kevin-farlee
ms.author: kfarlee
ms.openlocfilehash: 1df603174fb4f17fef0f04ea41fa4b95154ab247
ms.sourcegitcommit: 2b6760408de3b99193edeccce4b92a2f9ed5bcc6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92175957"
---
# <a name="sql-server-in-memory-oltp-internals-for-sql-server-2016"></a>Parámetros internos de OLTP en memoria de SQL Server para SQL Server 2016
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

**Resumen:** OLTP en memoria, denominado normalmente por su nombre en código "Hekaton", se introdujo en SQL Server 2014.
Esta eficaz tecnología permite sacar partido de grandes cantidades de memoria y varias decenas de núcleos para aumentar el rendimiento de las operaciones OLTP de 30 a 40 veces. SQL Server 2016 continúa la inversión en OLTP en memoria mediante la eliminación de muchas de las limitaciones que se encuentran en SQL Server 2014 y la mejora de los algoritmos de procesamiento interno para que OLTP en memoria pueda proporcionar mejoras todavía mayores. En este documento se describe la implementación de la tecnología OLTP en memoria de SQL Server 2016 a partir de SQL Server 2016 RTM. Al usar OLTP en memoria, las tablas se pueden declarar como "optimizadas para memoria" con el fin de habilitar las funciones de OLTP en memoria. Las tablas optimizadas en memoria son completamente transaccionales y se puede acceder a ellas mediante Transact-SQL. Los procedimientos almacenados de Transact-SQL, los desencadenadores y los UDF escalares se pueden compilar en código de máquina para obtener nuevas mejoras de rendimiento en tablas optimizadas en memoria. El motor está diseñado para obtener una alta simultaneidad sin ningún bloqueo.    
  
**Escritor:** Kalen Delaney  
  
**Revisores técnicos:** Sunil Agarwal y Jos de Bruijn  
  
**Fecha de publicación:** Junio de 2016  
  
**Se aplica a:** SQL Server 2016  
  
Para revisar el documento, descargue el documento [Parámetros internos de OLTP en memoria de SQL Server para SQL Server 2016](https://download.microsoft.com/download/8/3/6/8360731A-A27C-4684-BC88-FC7B5849A133/SQL_Server_2016_In_Memory_OLTP_White_Paper.pdf).   
