---
title: Migre SQL Server a Azure SQL Database mediante el Data Migration Assistant
description: Aprenda a usar Data Migration Assistant para migrar un SQL Server local a Azure SQL Database
ms.date: 06/29/2020
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, on-premises SQL Server
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.custom: seo-lt-2019
ms.openlocfilehash: 7bd846f4c4abd2a6b9fbcdc434964b789f32d047
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030643"
---
# <a name="migrate-on-premises-sql-server-or-sql-server-on-azure-vms-to-azure-sql-database-using-the-data-migration-assistant"></a>Migre SQL Server o SQL Server locales en máquinas virtuales de Azure para Azure SQL Database con el Data Migration Assistant

El Data Migration Assistant proporciona evaluaciones sin problemas de SQL Server locales y actualizaciones a versiones posteriores de SQL Server o migraciones a SQL Server en máquinas virtuales de Azure o Azure SQL Database.

En este artículo se proporcionan instrucciones paso a paso para migrar SQL Server de forma local a Azure SQL Database mediante el Data Migration Assistant.

## <a name="create-a-new-migration-project"></a>Creación de un proyecto de migración

1. En el panel izquierdo, seleccione **nuevo** (+) y, a continuación, seleccione el tipo de proyecto de **migración** .

2. Establezca el tipo de origen en **SQL Server** y el tipo de servidor de destino en **Azure SQL Database**.

3. Seleccione **Crear**.

   ![Crear proyecto de migración](../dma/media/NewCreate1.png)

## <a name="specify-the-source-server-and-database"></a>Especifique el servidor de origen y la base de datos

1. En el origen, en **conectar con el servidor de origen**, en el cuadro de texto **nombre de servidor** , escriba el nombre de la instancia de SQL Server de origen.

2. Seleccione el **tipo de autenticación** admitido por la instancia de SQL Server de origen.

   > [!NOTE]
   > Se recomienda cifrar la conexión activando la casilla **cifrar conexión** en **conexión poperties**.

    ![Seleccionar servidor de origen](../dma/media/select-source-server.png)

3. Seleccione **Conectar**.

4. Seleccione una única base de datos de origen para migrar a Azure SQL Database.

   > [!NOTE]
   > Si desea evaluar la base de datos y ver y aplicar las correcciones recomendadas antes de la migración, active la casilla **evaluar la base de datos antes** de la migración.

    ![Seleccionar base de datos de origen](../dma/media/select-source-database.png)

5. Seleccione **Siguiente**.

## <a name="specify-the-target-server-and-database"></a>Especifique el servidor de destino y la base de datos

1. En el destino, en el cuadro de texto **nombre del servidor** , en **conectar con el servidor de destino**, escriba el nombre de la instancia de Azure SQL Database. 

2. Seleccione el **tipo de autenticación** admitido por la instancia de Azure SQL Database de destino.

   > [!NOTE]
   > Se recomienda cifrar la conexión activando la casilla **cifrar conexión** en **conexión poperties**.

     ![Seleccionar servidor de destino](../dma/media/select-target-server.png)

3. Seleccione **Conectar**.

4. Seleccione una única base de datos de destino a la que migrar.

   > [!NOTE]
   > Si tiene previsto migrar usuarios de Windows, en el cuadro de texto **nombre de dominio de usuario externo de destino** , asegúrese de que el nombre de dominio de usuario externo de destino esté especificado correctamente.

    ![Seleccionar base de datos de destino](../dma/media/select-target-database.png)

5. Seleccione **Siguiente**.

## <a name="select-schema-objects"></a>Selección de los objetos de esquema

1. Seleccione los objetos de esquema de la base de datos de origen que desea migrar a Azure SQL Database.

    ![Selección de los objetos de esquema](../dma/media/select-schema-objects.png)

    > [!NOTE]
    > Algunos de los objetos que no se pueden convertir tal cual se presentan con oportunidades de corrección automática. Al hacer clic en estos objetos en el panel izquierdo se muestran las correcciones sugeridas en el panel derecho. Revise las correcciones y elija aplicar o ignorar todos los cambios, objeto por objeto. Tenga en cuenta que aplicar o ignorar todos los cambios de un objeto no afecta a los cambios de otros objetos de la base de datos. Las instrucciones que no se pueden convertir o fijar automáticamente se copian en la base de datos de destino y se comentan.

    ![Corrección sugerida](../dma/media/suggested-fix.png)

2. Seleccione **script SQL general**.

3. Revise el script generado.

    ![Script generado](../dma/media/generated-script.png)

## <a name="deploy-schema"></a>Implementación del esquema

1. Seleccione **implementar esquema**.

2. Revise los resultados de la implementación del esquema.

    ![Resultados de la implementación del esquema](../dma/media/schema-deployment-results.png)

3. Seleccione **migrar datos** para iniciar el proceso de migración de datos.

4. Seleccione las tablas con los datos que desea migrar.

    ![Seleccionar las tablas que se van a migrar](../dma/media/select-tables-to-migrate.png) 

5. Seleccione **Iniciar migración de datos**.

La pantalla final muestra el estado general.

   ![Estado de migración](../dma/media/migration-status.png) 

## <a name="see-also"></a>Consulte también

* [Data Migration Assistant (DMA)](../dma/dma-overview.md)
* [Data Migration Assistant: opciones de configuración](../dma/dma-configurationsettings.md)
* [Data Migration Assistant: procedimientos recomendados](../dma/dma-bestpractices.md)
