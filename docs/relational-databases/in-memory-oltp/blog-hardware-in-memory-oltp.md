---
title: Hardware para OLTP en memoria de SQL | Microsoft Docs
description: Obtenga información sobre las consideraciones de hardware para el rendimiento de OLTP en memoria en SQL Server. OLTP en memoria usa la memoria y el disco de maneras diferentes a las tablas basadas en disco.
ms.custom: ''
ms.date: 03/28/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||=azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: ba34a1b550d9726c3de479c56f92e40adf404214
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063711"
---
# <a name="hardware-considerations-for-in-memory-oltp-in-sql-server"></a>Consideraciones de hardware para OLTP en memoria en SQL Server

OLTP en memoria usa la memoria y el disco de maneras diferentes a las tablas basadas en disco tradicionales. La mejora de rendimiento observada con OLTP en memoria depende del hardware que se use. En esta entrada de blog se tratan varias consideraciones de hardware generales y se proporcionan directrices genéricas sobre el hardware que se va a usar con OLTP en memoria.

> [!NOTE]
> El origen de este artículo es un blog publicado el 1 de agosto de 2013 por el equipo de Microsoft SQL Server 2014. La página web del blog está en proceso de retirada.
>
> [OLTP en memoria de SQL Server](./in-memory-oltp-in-memory-optimization.md)

<!--
    Here was the link to the blog. This blog was captured into this new article on 2018/11/30, by GeneMi (MightyPen).
    https://cloudblogs.microsoft.com/sqlserver/2013/08/01/hardware-considerations-for-in-memory-oltp-in-sql-server-2014/
    At least one pre-existing article that contained the obsolete blog link was:
        relational-databases\in-memory-oltp\sample-database-for-in-memory-oltp.md
-->

## <a name="cpu"></a>CPU

OLTP en memoria no necesita un servidor avanzado para admitir una carga de trabajo de OLTP de alto rendimiento. Se recomienda usar un servidor de gama media con dos sockets de CPU. Debido a la mejora de rendimiento que ofrece OLTP en memoria, dos sockets probablemente basten para las necesidades empresariales.

Se recomienda activar la tecnología Hyper-Threading con OLTP en memoria. Con algunas cargas de trabajo de OLTP, se han observado mejoras de rendimiento de hasta un 40 % al usar Hyper-Threading.

## <a name="memory"></a>Memoria

Todas las tablas optimizadas para memoria residen totalmente en la memoria. Por lo tanto, debe tener suficiente memoria física para las propias tablas y para soportar la carga de trabajo que se ejecuta en la base de datos: la cantidad de memoria que necesita realmente depende de la carga de trabajo, pero como punto de partida, probablemente le baste con memoria disponible para aproximadamente dos veces el tamaño de los datos. También necesita suficiente memoria para el grupo de búferes si la carga de trabajo también opera en tablas basadas en disco tradicionales.

Para determinar cuánta memoria usa una tabla optimizada para memoria determinada, ejecute la consulta siguiente:

```sql
select object_name(object_id), * from sys.dm_db_xtp_table_memory_stats;
```

Los resultados muestran la memoria usada por las tablas optimizadas para memoria y sus índices. La tabla de datos incluye datos de usuario, así como todas las versiones de fila anteriores que aún necesitan las transacciones en ejecución o que el sistema todavía no ha limpiado. La memoria usada por los índices hash es constante y no depende del número de filas de la tabla.

Al usar OLTP en memoria, es importante tener en cuenta que no es necesario que toda la base de datos quepa en la memoria. Puede tener una base de datos de varios terabytes y seguir beneficiándose de OLTP en memoria, siempre y cuando el tamaño de los datos de acceso frecuente (es decir, las tablas optimizadas para memoria) no supere los 256 GB. El número máximo de archivos de datos de punto de comprobación que SQL Server puede administrar para una sola base de datos es 4000, y cada archivo tiene 128 MB. Aunque esto daría un máximo teórico de 512 GB, para garantizar que SQL Server pueda atender la combinación de archivos de punto de comprobación y no alcance el límite de 4000 archivos, se admiten hasta 256 GB. Tenga en cuenta que este límite se aplica únicamente a las tablas optimizadas para memoria; no hay limitación de tamaño semejante en el caso de las tablas basadas en disco tradicionales de la misma base de datos de SQL Server.

Las tablas optimizadas para memoria no duraderas (NDT), es decir, las tablas optimizadas para memoria con DURABILITY=SCHEMA_ONLY no se conservan en disco. Aunque las NDT no están limitadas por el número de archivos de punto de comprobación, solo se admiten 256 GB. Las consideraciones para las unidades de datos y de registro del resto de esta publicación no se aplican a las tablas no duraderas, ya que los datos solo existen en memoria.

## <a name="log-drive"></a>Unidad de registro

Las entradas de registro que pertenecen a tablas optimizadas para memoria se escriben en el registro de transacciones de base de datos, junto con las demás entradas de registro de SQL Server.

Siempre es importante colocar el archivo de registro en una unidad que tenga baja latencia, de forma que las transacciones no tengan que esperar demasiado tiempo y se evite la contención en la E/S de registro. El sistema se ejecuta tan rápido como el componente más lento (ley de Amdahl). Tiene que asegurarse de que, al ejecutar OLTP en memoria, el dispositivo de E/S de registro no se convierta en un cuello de botella. Se recomienda usar un dispositivo de almacenamiento con baja latencia, por lo menos SSD.

Tenga en cuenta que las tablas optimizadas para memoria usan menos ancho de banda de registro que las tablas basadas en disco, ya que no registran operaciones de índice ni registros UNDO. Esto puede ayudar a aliviar la contención de E/S de registro.

## <a name="data-drive"></a>Unidad de datos

La persistencia de las tablas optimizadas para memoria que usan archivos de punto de comprobación emplea E/S de streaming. Por lo tanto, estos archivos no necesitan una unidad con baja latencia ni E/S aleatoria rápida. En su lugar, el factor principal de estas unidades es la velocidad de E/S secuencial y el ancho de banda del adaptador de bus host (HBA). Así, no se necesita SSD para los archivos de punto de comprobación; puede colocarlos en ejes de alto rendimiento (p. ej., SAS), siempre que su velocidad de E/S secuencial cumpla los requisitos.

El factor más importante a la hora de determinar el requisito de velocidad es el RTO [objetivo de tiempo de recuperación] al reinicio del servidor. Durante la recuperación de la base de datos, todos los datos de las tablas optimizadas para memoria deben leerse desde el disco, en la memoria. La recuperación de la base de datos se produce en la velocidad de lectura secuencial del subsistema de E/S; el disco es el cuello de botella.

Para satisfacer requisitos estrictos de RTO, se recomienda distribuir los archivos de punto de comprobación entre varios discos mediante la adición de varios contenedores al grupo de archivos MEMORY_OPTIMIZED_DATA. SQL Server admite la carga en paralelo de archivos de punto de comprobación de varias unidades: la recuperación ocurre en la velocidad agregada de las unidades.

En cuanto a capacidad de disco, se recomienda tener disponible dos o tres veces el tamaño de las tablas optimizadas para memoria.

## <a name="see-also"></a>Consulte también

[Base de datos de ejemplo para OLTP en memoria](sample-database-for-in-memory-oltp.md)