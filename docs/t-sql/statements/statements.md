---
title: Instrucciones Transact-SQL
description: Instrucciones Transact-SQL
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Alter_TSQL
dev_langs:
- TSQL
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.custom: ''
ms.date: 04/17/2020
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 85f4838575e340a5b40dddc109802ed99afedd7c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189426"
---
# <a name="transact-sql-statements"></a>Instrucciones Transact-SQL

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Una instrucción SQL es una unidad atómica de trabajo que funciona correctamente o se produce un error general. Una instrucción SQL es un conjunto de instrucciones que consta de identificadores, parámetros, variables, nombres, tipos de datos y palabras reservadas de SQL que se compila correctamente. [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] crea una transacción *implícita* para una instrucción SQL si un comando `BeginTransaction` no especifica el inicio de una transacción. [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] siempre confirma una transacción implícita si la instrucción se ejecuta correctamente y revierte una transacción implícita si el comando produce un error.  

Hay muchos tipos de instrucciones. Quizá lo más importante es la instrucción [SELECT](../queries/select-transact-sql.md) que recupera filas de la base de datos y habilita la selección de una o varias filas o columnas de una o varias tablas en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En este artículo se resumen las categorías de instrucciones para utilizarlas con Transact-SQL (T-SQL), además de la instrucción `SELECT`. Puede encontrar todas las instrucciones que aparecen en el panel de navegación izquierdo.

## <a name="backup-and-restore"></a>Copia de seguridad y restauración

Las instrucciones de copia de seguridad y restauración proporcionan formas de crear copias de seguridad y restauración a partir de copias de seguridad.  Para obtener más información, consulte [Realizar copias de seguridad y restaurar bases de datos](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md).

## <a name="data-definition-language"></a>Lenguaje de definición de datos

Las instrucciones del lenguaje de definición de datos (DDL) definen estructuras de datos. Use estas instrucciones para crear, modificar o quitar estructuras de datos en una base de datos. Estas instrucciones incluyen las siguientes:

- ALTER
- Intercalaciones
- CREATE
- DROP
- DISABLE TRIGGER
- ENABLE TRIGGER
- RENAME
- UPDATE STATISTICS
- TRUNCATE TABLE

## <a name="data-manipulation-language"></a>Lenguaje de manipulación de datos

El lenguaje de manipulación de datos (DML) afecta a la información almacenada en la base de datos. Use estas instrucciones para insertar, actualizar y cambiar las filas de la base de datos.

- BULK INSERT
- Delete
- INSERT
- SELECT
- UPDATE
- MERGE

## <a name="permissions-statements"></a>Instrucciones de permisos

Las instrucciones de permisos determinan qué usuarios e inicios de sesión pueden tener acceso a datos y realizar operaciones. Para obtener más información sobre la autenticación y el acceso, consulte el [Centro de seguridad](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md).

## <a name="service-broker-statements"></a>Instrucciones de Service Broker

Service Broker es una característica que proporciona compatibilidad nativa para las aplicaciones de mensajería y de cola. Para obtener más información, vea [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md).

## <a name="session-settings"></a>Configuración de sesión

Las instrucciones SET determinan de qué forma la sesión actual gestiona las opciones de configuración del tiempo de ejecución. Para obtener información general, vea [Instrucciones SET](set-statements-transact-sql.md).
