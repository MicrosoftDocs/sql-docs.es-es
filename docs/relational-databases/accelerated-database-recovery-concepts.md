---
description: Recuperación acelerada de bases de datos.
title: Recuperación de bases de datos acelerada | Microsoft Docs
ms.date: 05/20/2020
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- accelerated database recovery [SQL Server], recovery-only
- database recovery [SQL Server]
author: mashamsft
ms.author: mathoma
ms.reviewer: kfarlee
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: 70b68fa62c523a4c65bb4e70538e1882bc67290d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350077"
---
# <a name="accelerated-database-recovery"></a>Recuperación acelerada de bases de datos.

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

La recuperación de bases de datos acelerada (ADR) mejora considerablemente la disponibilidad de la base de datos, especialmente en presencia de transacciones de larga duración, al volver a diseñar el proceso de recuperación del motor de base de datos de SQL. ADR es nuevo en SQL Server 2019. 

ADR también está disponible para las bases de datos en Azure SQL Database, Azure SQL Managed Instance y Azure Synapse SQL. ADR está habilitado de forma predeterminada en SQL Database y SQL Managed Instance y no se puede deshabilitar. 

## <a name="overview"></a>Información general

Las principales ventajas de ADR son:

- **Recuperación de base de datos rápida y coherente**

  Con ADR, las transacciones de larga duración no afectan al tiempo de recuperación total, lo que permite una recuperación de base de datos rápida y coherente, con independencia del número de transacciones activas en el sistema o de sus tamaños.

- **Reversión de transacción instantánea**

  Con ADR, la reversión de la transacción es instantánea, independientemente del momento en que la transacción haya estado activa o del número de actualizaciones que haya realizado.

- **Truncamiento de registro agresivo**

  Con ADR, el registro de transacciones se trunca de manera agresiva, incluso en presencia de transacciones activas de larga duración, lo que evita que crezca fuera de control.

## <a name="the-current-database-recovery-process"></a>Proceso de recuperación de la base de datos actual

Sin ADR, la recuperación de la base de datos en SQL Server sigue el modelo de recuperación [ARIES](https://people.eecs.berkeley.edu/~brewer/cs262/Aries.pdf) y consta de tres fases, que se muestran en el diagrama siguiente y se explican con más detalle después del diagrama.

![Proceso de recuperación actual](./media/accelerated-database-recovery-concepts/current-recovery-process.png)

- **Fase de análisis**

  SQL Server realiza un examen hacia delante del registro de transacciones desde el principio del último punto de control correcto (o el LSN de página desfasada más antiguo) hasta el final, para determinar el estado de cada transacción en el momento en que la detuvo SQL Server.

- **Fase de rehacer**

  SQL Server realiza el examen hacia delante del registro de transacciones desde la transacción sin confirmar más antigua hasta el final, para poner la base de datos en el estado en que se encontraba en el momento del bloqueo rehaciendo todas las operaciones confirmadas.

- **Fase de deshacer**

  Para cada transacción que estaba activa en el momento del bloqueo, SQL Server recorre el registro hacia atrás, deshaciendo las operaciones realizadas por esta transacción.

En función de este diseño, el tiempo necesario para que el motor de base de datos se recupere de un reinicio inesperado es (aproximadamente) proporcional al tamaño de la transacción activa más larga del sistema en el momento del bloqueo. La recuperación requiere la reversión de todas las transacciones incompletas. El período de tiempo necesario es proporcional al trabajo que ha realizado la transacción y el tiempo que ha estado activa. Por lo tanto, el proceso de recuperación de SQL Server puede tardar mucho tiempo en la presencia de transacciones de larga duración (como operaciones de inserción masiva grandes u operaciones de generación de índice en una tabla grande).

Además, si se cancela o se revierte, una transacción grande basada en este diseño también puede tardar mucho tiempo, ya que se usa la misma fase de recuperación de deshacer, tal y como se ha descrito anteriormente.

Además, el motor de base de datos no puede truncar el registro de transacciones cuando hay transacciones de larga duración, porque sus entradas de registro correspondientes son necesarias para los procesos de recuperación y reversión. Como resultado, algunos registros de transacciones crecen mucho y consumen grandes cantidades de espacio en la unidad.

## <a name="the-accelerated-database-recovery-process"></a>Proceso de recuperación acelerada de bases de datos

ADR soluciona los problemas anteriores rediseñando completamente el proceso de recuperación del motor de base de datos para que:

- Sea constante/instantáneo, ya que evita tener que examinar el registro desde/hasta el principio de la transacción activa más antigua. Con ADR, el registro de transacciones solo se procesa desde el último punto de control correcto (o el número de secuencia de registro de la página desfasada más antigua). Como resultado, el tiempo de recuperación no se ve afectado por las transacciones de larga duración.
- Minimice el espacio del registro de transacciones necesario, ya que ya no es necesario procesar el registro para toda la transacción. Como resultado, el registro de transacciones se puede truncar de manera agresiva a medida que se producen puntos de comprobación y copias de seguridad.

En líneas generales, ADR consigue una recuperación de base de datos rápida mediante la creación de versiones de todas las modificaciones físicas de la base de datos y solo se deshacen las operaciones lógicas, que son limitadas y se pueden deshacer casi al instante. Las transacciones que estaban activas en el momento de un bloqueo se marcan como anuladas y, por lo tanto, las consultas de usuario simultáneas pueden omitir cualquier versión generada por estas transacciones.

El proceso de recuperación de ADR tiene las mismas tres fases que el proceso de recuperación actual. En este diagrama siguiente se muestra cómo funcionan estas fases con ADR.

![Proceso de recuperación de ADR](./media/accelerated-database-recovery-concepts/adr-recovery-process.png)

- **Fase de análisis**

  El proceso sigue siendo el mismo que hoy con la adición de reconstruir sLog y copiar entradas de registro para las operaciones sin control de versiones.
  
- **Fase de rehacer**

  Se divide en dos subfases
  - Subfase 1

      Rehacer desde sLog (transacción sin confirmar más antigua hasta el último punto de control). La operación de rehacer es rápida, ya que solo necesita procesar algunos registros de sLog.

  - Subfase 2

     La operación de rehacer desde el registro de transacciones empieza a partir del último punto de control (en lugar de la transacción sin confirmar más antigua).
     
- **Fase de deshacer**

   La fase de deshacer con ADR se completa de forma casi instantánea mediante el uso de sLog para deshacer las operaciones sin control de versiones y el almacén de versiones persistente (PVS) con reversión lógica para realizar la operación de deshacer basada en versiones de nivel de fila.

También puede ver este vídeo de 8 minutos en el que se explica la recuperación acelerada de bases de datos

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Advanced-Database-Recovery--Data-Exposed/player?WT.mc_id=dataexposed-c9-niner]

## <a name="adr-recovery-components"></a>Componentes de recuperación de ADR

Estos son los cuatro componentes clave de ADR:

- **Almacén de versiones persistentes (PVS)**

  El almacén de versiones persistentes es un mecanismo del motor de base de datos para conservar las versiones de fila generadas en la propia base de datos en lugar del almacén de versiones `tempdb` tradicional. El PVS permite el aislamiento de recursos y mejora la disponibilidad de las secundarias legibles.

- **Reversión lógica**

  La reversión lógica es el proceso asincrónico responsable de realizar la operación de deshacer en el nivel de fila y según la versión, proporcionando una reversión de transacción instantánea y deshacer todas las operaciones con versiones.

  - Realiza el seguimiento de todas las transacciones anuladas
  - Realiza una reversión mediante el PVS para todas las transacciones de usuario
  - Libera todos los bloqueos inmediatamente después de la anulación de la transacción

- **sLog**

  sLog es una secuencia de registro en memoria secundaria que almacena entradas de registro para operaciones sin control de versiones (como la invalidación de la caché de metadatos, las adquisiciones de bloqueos, etc.). sLog tiene estas características:

  - Es de bajo volumen y en memoria
  - Se conserva en el disco mediante su serialización durante el proceso de punto de comprobación
  - Se trunca periódicamente a medida que se confirman las transacciones
  - Acelera las operaciones de rehacer y deshacer procesando solo las operaciones sin control de versiones  
  - Habilita el truncamiento del registro de transacciones agresivo conservando solo las entradas de registro necesarias

- **Limpiador**

  El limpiador es el proceso asincrónico que se reactiva periódicamente y limpia las versiones de página que no son necesarias.

## <a name="who-should-consider-accelerated-database-recovery"></a>Quién debe considerar la recuperación de base de datos acelerada

Los siguientes tipos de clientes deben considerar la posibilidad de habilitar la ADR:

- Clientes que tienen cargas de trabajo con transacciones de larga duración.
- Clientes que han detectado casos en los que las transacciones activas hacen que el registro de transacciones crezca de forma significativa.  
- Clientes que han experimentado largos períodos de falta de disponibilidad de la base de datos debido a la recuperación de larga duración de SQL Server (por ejemplo, un reinicio inesperado de SQL Server o la reversión manual de transacciones).

>[!IMPORTANT]
>Recuperación acelerada de la base de datos no se admite para las bases de datos inscritas en la creación de reflejo de la base de datos.

## <a name="see-also"></a>Consulte también  

[Administración de la recuperación de bases de datos acelerada](accelerated-database-recovery-management.md)
