---
title: Configuración del almacenamiento para las tablas con optimización para memoria | Microsoft Docs
description: Obtenga información sobre cómo configurar la capacidad de almacenamiento y las operaciones de entrada/salida por segundo (IOPS) para las tablas optimizadas para memoria en SQL Server.
ms.custom: ''
ms.date: 1/15/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 6e005de0-3a77-4b91-b497-14cc0f9f6605
author: markingmyname
ms.author: maghan
ms.openlocfilehash: fdb18826c75e55f41e3c02a97e2f8374a22778b3
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237556"
---
# <a name="configuring-storage-for-memory-optimized-tables"></a>Configurar el almacenamiento para las tablas con optimización para memoria
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Debe configurar la capacidad de memoria y las operaciones de entrada/salida por segundo (IOPS).  
  
## <a name="storage-capacity"></a>Capacidad de almacenamiento  

Use la información de [Estimar los requisitos de memoria para las tablas optimizadas para memoria](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md) para calcular el tamaño en memoria de las tablas durables optimizadas para memoria de la base de datos. Puesto que los índices no se conservan en las tablas optimizadas para memoria, no incluya el tamaño de los índices. 
 
Después de determinar el tamaño, debe proporcionar espacio en disco suficiente para contener los archivos de punto de comprobación, que se usan para almacenar los datos que se han modificado recientemente. Los datos que se almacenan no solo incluyen el contenido de las filas nuevas que se agregan a las tablas en memoria, sino también versiones nuevas de las filas existentes. Este almacenamiento aumenta al insertar o actualizar filas. Las versiones de fila se combinan y el almacenamiento se recupera cuando se produce el truncamiento del registro. Si el truncamiento del registro se retrasa por alguna razón, el almacén de OLTP en memoria aumentará.

Un buen punto de partida para ajustar el tamaño del almacenamiento para esta área consiste en reservar cuatro veces el tamaño de las tablas perdurables en memoria. Supervise el uso del espacio y prepárese para expandir el almacenamiento que está disponible, en caso necesario.
  
## <a name="storage-iops"></a>E/S de almacenamiento por segundo  
 [!INCLUDE[hek_2](../../includes/hek-2-md.md)] puede aumentar considerablemente el rendimiento de las cargas de trabajo. Por tanto, es importante asegurarse de que la entrada/salida no es un cuello de botella.  
  
-   Al migrar tablas basadas en disco a tablas optimizadas para memoria, asegúrese de que el registro de transacciones esté en un medio de almacenamiento que pueda admitir mayor actividad del registro de transacciones. Por ejemplo, si el medio de almacenamiento admite operaciones del registro de transacciones a 100 MB/s, y las tablas optimizadas para memoria producen un rendimiento cinco veces mayor, el medio de almacenamiento del registro de transacciones debe poder admitir una mejora del rendimiento cinco veces mayor para evitar que la actividad del registro de transacciones se convierta en un cuello de botella.  
  
-   Las tablas optimizadas para memoria se conservan en archivos de punto de comprobación, que están distribuidos en uno o varios contenedores. Normalmente cada contenedor debe estar asignado a su propio dispositivo de almacenamiento y se usa tanto para aumentar la capacidad de almacenamiento como para mejorar la E/S por segundo. Debe asegurarse de que la E/S por segundo secuencial del medio de almacenamiento puede admitir hasta tres veces el rendimiento sostenido del registro de transacciones. Las operaciones de escritura en archivos de punto de comprobación son de 256 KB para los archivos de datos y de 4 KB para los archivos delta.
  
     - Por ejemplo, si las tablas optimizadas para memoria generan 500 MB/s de actividad en el registro de transacciones, el almacenamiento de las tablas optimizadas para memoria debe admitir 1,5 GB/s de E/S por segundo. La necesidad de admitir el triple de rendimiento sostenido del registro de transacciones procede de la observación de que los pares de archivos de datos y delta se escriben primero con los datos iniciales y después se deben leer o volver a escribir como parte de una operación Merge.  
  
- Otro factor que hay que tener en cuenta para las E/S por segundo del almacenamiento es el tiempo de recuperación de las tablas optimizadas para memoria. Los datos de las tablas durables se deben leer en memoria antes de que una base de datos esté a disposición de las aplicaciones. Normalmente, la carga de datos en las tablas optimizadas para memoria se puede realizar a la velocidad de la IOPS. Por tanto, si el almacenamiento total de las tablas durables optimizadas para memoria es de 60 GB y desea poder cargar estos datos en 1 minuto, la IOPS para el almacenamiento se debe establecer en 1 GB/s.  
  
-   Los archivos de punto de comprobación normalmente se distribuyen uniformemente entre todos los contenedores con espacio disponible. Con SQL Server 2014, debe aprovisionar un número impar de contenedores para lograr una distribución uniforme; a partir de 2016, la distribución uniforme se consigue con cantidades pares e impares de contenedores.
  
## <a name="encryption"></a>Cifrado  
 En [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores, el almacenamiento de las tablas optimizadas para memoria se cifra en reposo como parte de la habilitación de Cifrado de datos transparente (TDE) en la base de datos. Para obtener más información, vea [Cifrado de datos transparente](../../relational-databases/security/encryption/transparent-data-encryption.md). En [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], los archivos de punto de comprobación no se cifran, aunque el TDE esté habilitado en la base de datos.

 Los datos de tablas optimizadas para memoria (SCHEMA_ONLY) [no duraderas](../../relational-databases/in-memory-oltp/defining-durability-for-memory-optimized-objects.md) no se escriben en el disco en ningún momento. Por lo tanto, TDE no se aplica a esas tablas.
  
## <a name="see-also"></a>Consulte también  
 [Crear y administrar el almacenamiento de objetos optimizados para memoria](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md)  
  
  
