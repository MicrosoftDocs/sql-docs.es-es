---
title: Migre el esquema de recursos humanos de Oracle a SQL Server en Linux | Microsoft Docs
description: Convertir el esquema de Oracle de ejemplo en SQL Server en Linux
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.custom: ''
ms.technology: ssma
ms.openlocfilehash: fb21fdc7da82d6dae2390ddcf0884ea1e8ac8a58
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044395"
---
# <a name="migrate-an-oracle-schema-to-sql-server-2017-on-linux-with-the-sql-server-migration-assistant"></a>Migre un esquema de Oracle a SQL Server 2017 en Linux con el SQL Server Migration Assistant

En este tutorial se usa SQL Server Migration Assistant (SSMA) para Oracle en Windows para convertir el esquema **HR** de ejemplo de oracle en [SQL Server 2017 en Linux](../../linux/sql-server-linux-overview.md).

> [!div class="checklist"]
> * Descargar e instalar SSMA en Windows
> * Crear un proyecto de SSMA para administrar la migración
> * Conectar con Oracle
> * Ejecutar un informe de migración
> * Conversión del esquema HR de ejemplo
> * Migración de los datos

## <a name="prerequisites"></a>Prerrequisitos

- Una instancia de Oracle 12C (12.2.0.1.0) con el esquema **HR** instalado
- Instancia de trabajo de SQL Server en Linux

> [!NOTE]
> Los mismos pasos se pueden usar para tener como destino SQL Server en Windows, pero debe seleccionar Windows en la configuración **migrar a** proyecto.

## <a name="download-and-install-ssma-for-oracle"></a>Descargar e instalar SSMA para Oracle

Hay varias ediciones de SQL Server Migration Assistant disponibles, dependiendo de la base de datos de origen.  Descargue la versión actual de [SQL Server Migration Assistant para Oracle](https://aka.ms/ssmafororacle) e instálela con las instrucciones que se encuentran en la página de descarga.

> [!NOTE]
> En este momento, el **paquete de extensiones de SSMA para Oracle** no es compatible con Linux, pero no es necesario para este tutorial.

## <a name="create-and-set-up-project"></a>Crear y configurar proyecto

Use los pasos siguientes para crear un nuevo proyecto de SSMA:

1. Abra SSMA para Oracle y elija **nuevo proyecto** en el menú **archivo** .

1. Asigne un nombre al proyecto.

1. Elija "SQL Server 2017 (Linux)-Preview" en el campo **migrar a** .

SSMA para Oracle no utiliza los esquemas de ejemplo de Oracle de forma predeterminada. Siga estos pasos para habilitar el esquema de recursos humanos:

1. En SSMA, seleccione el menú **herramientas** .

1. Seleccione **configuración predeterminada del proyecto** y, a continuación, elija **cargando objetos del sistema**.

1. Asegúrese de que **HR** está activado y elija **Aceptar**.

## <a name="connect-to-oracle"></a>Conectar con Oracle

A continuación, conecte SSMA a Oracle.

1. En la barra de herramientas, haga clic en **conectar con Oracle**.

1. Escriba el nombre del servidor, el puerto, el SID de Oracle, el nombre de usuario y la contraseña.

   ![Conectar con Oracle](./media/sql-server-linux-convert-from-oracle/ConnectToOracle.png)

1. Haga clic en **Conectar**. En unos momentos, SSMA para Oracle se conecta a la base de datos y lee sus metadatos.

## <a name="create-a-report"></a>Creación de un informe

Siga estos pasos para generar un informe de migración.

1. En el **Explorador de metadatos de Oracle**, expanda el nodo del servidor.

1. Expanda **esquemas**, haga clic con el botón secundario en **HR** y seleccione **crear informe**.

   ![Crear informe en el explorador de metadatos de Oracle](./media/sql-server-linux-convert-from-oracle/CreateReport.png)

1. Se abre una nueva ventana del explorador con un informe en el que se muestran todas las advertencias y los errores asociados a la conversión.

   > [!NOTE]
   > No es necesario hacer nada con esa lista para este tutorial. Si realiza estos pasos para su propia base de datos de Oracle, debe revisar el informe para solucionar cualquier problema importante en la conversión de la base de datos.

   ![Informe de migración de ejemplo](./media/sql-server-linux-convert-from-oracle/SSMAReport.png)

## <a name="connect-to-sql-server"></a>Conectar a SQL Server

A continuación, elija **conectar a SQL Server** y escriba la información de conexión adecuada.  Si utiliza un nombre de base de datos que aún no existe, SSMA para Oracle lo crea automáticamente.

![Conectar a SQL Server](./media/sql-server-linux-convert-from-oracle/ConnectToSQLServer.png)

## <a name="convert-schema"></a>Convertir esquema

Haga clic con el botón derecho en **HR** en el **Explorador de metadatos de Oracle** y elija convertir esquema.

![Convertir esquema](./media/sql-server-linux-convert-from-oracle/ConvertSchema.png)

## <a name="synchronize-database"></a>Sincronizar base de datos

A continuación, sincronice la base de datos.

1. Una vez finalizada la conversión, use el **SQL Server explorador de metadatos** para ir a la base de datos que creó en el paso anterior.

1. Haga clic con el botón derecho en la base de datos, seleccione **sincronizar con base de datos** y, a continuación, haga clic en Aceptar.

   ![Sincronizar con base de datos](./media/sql-server-linux-convert-from-oracle/SynchronizeWithDatabase.png)

## <a name="migrate-data"></a>Migración de los datos

El paso final es migrar los datos.

1. En el **Explorador de metadatos de Oracle**, haga clic con el botón derecho en **HR** y seleccione **migrar datos**.

1. El paso de migración de datos requiere que vuelva a escribir sus credenciales de Oracle y SQL Server.

1. Cuando termine, revise el informe de migración de datos, que debe ser similar a la siguiente captura de pantalla:

   ![Informe de migración de datos](./media/sql-server-linux-convert-from-oracle/DataMigrationReport.png)

## <a name="next-steps"></a>Pasos siguientes

Para un esquema de Orcale más complejo, el proceso de conversión implicaría más tiempo, pruebas y posibles cambios en las aplicaciones cliente. El propósito de este tutorial es mostrar cómo se puede usar SSMA para Oracle como parte del proceso general de migración.

En este tutorial, ha aprendido a:
> [!div class="checklist"]
> * Instalación de SSMA en Windows
> * Crear un nuevo proyecto de SSMA
> * Evaluar y ejecutar una migración desde Oracle

A continuación, explore otras formas de usar SSMA:

> [!div class="nextstepaction"]
>[SQL Server Migration Assistant documentación](../sql-server-migration-assistant.md)
