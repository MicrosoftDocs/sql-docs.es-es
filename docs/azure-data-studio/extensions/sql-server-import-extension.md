---
title: Extensión de importación de SQL Server
description: Obtenga información sobre cómo instalar y usar la extensión de importación de SQL Server para Azure Data Studio, un asistente que convierte archivos .txt y .csv en una tabla SQL.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan, sstein
ms.custom: ''
ms.date: 09/22/2020
ms.openlocfilehash: a1a4c9a83aca18ef41960f3c497b75f6de210109
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048385"
---
# <a name="sql-server-import-extension"></a>Extensión de importación de SQL Server

La extensión de importación de SQL Server convierte archivos .txt y .csv en una tabla SQL. Este asistente emplea un marco de Microsoft Research conocido como [Program Synthesis uing Examples (PROSE)](https://microsoft.github.io/prose/) para analizar de forma inteligente el archivo con una mínima intervención del usuario. Es un marco eficaz para la limpieza y transformación de datos y es la misma tecnología que se aplica al Relleno rápido de Microsoft Excel.

Para obtener más información sobre la versión de SSMS de esta característica, lea [este artículo](../../relational-databases/import-export/import-flat-file-wizard.md).

## <a name="install-the-sql-server-import-extension"></a>Instalación de la extensión de importación de SQL Server

1. Para abrir el administrador de extensiones y acceder a las extensiones disponibles, seleccione el icono de extensiones, o bien seleccione **Extensiones** en el menú **Ver**.
2. Seleccione una extensión disponible para ver sus detalles.

   ![Administrador de la extensión de importación](media/sql-server-import-extension/import-wizard-install.png)

3. Seleccione la extensión que quiera e **instálela**.
4. Seleccione **Recargar** para habilitar la extensión (solo es necesario la primera vez que se instala una extensión).

## <a name="start-import-wizard"></a>Inicie el asistente para importar

1. Para iniciar la importación de SQL Server, primero establezca una conexión con un servidor en la pestaña Servidores.
2. Después de establecer una conexión, explore en profundidad la base de datos de destino en la que quiere importar un archivo a una tabla SQL.
3. Haga clic con el botón derecho en la base de datos y seleccione **Asistente para importación**.

    ![Asistente para importación](media/sql-server-import-extension/open-import-wizard.png)

## <a name="importing-a-file"></a>Importar un archivo

1. Al hacer clic con el botón derecho para iniciar el asistente, el servidor y la base de datos ya se han rellenado automáticamente. Si hay otras conexiones activas, puede seleccionarlas en la lista desplegable. 

    Seleccione **Examinar** para seleccionar un archivo. Se debería rellenar automáticamente el nombre de la tabla en función del nombre de archivo, aunque también puede cambiarlo.

    De forma predeterminada, el esquema será DBO, pero puede cambiarlo. Seleccione **Siguiente** para continuar.

    ![Archivo de entrada](media/sql-server-import-extension/import-wizard-input-file.png)

2. El asistente generará una vista previa basada en las primeras 50 filas. No hay ninguna acción adicional en esta página que no sea comprobar que los datos parezcan precisos. Seleccione **Siguiente** para continuar.

    ![Vista previa de los datos](media/sql-server-import-extension/import-wizard-preview-data.png)

3. En esta página puede realizar cambios en el nombre de columna, el tipo de datos, si es una clave principal o para permitir valores NULL. Puede realizar todos los cambios que quiera. Seleccione **Importar datos** para continuar.

    ![Modificación de columnas](media/sql-server-import-extension/import-wizard-modify-columns.png)

4. Esta página proporciona un resumen de las acciones elegidas. También puede ver si la tabla se insertó correctamente o no.

    Puede seleccionar **Listo, Anterior** si necesita realizar cambios o **Importar nuevo archivo** para importar rápidamente otro archivo.

    ![Resumen](media/sql-server-import-extension/import-wizard-summary.png)

5. Compruebe si la tabla se importó correctamente actualizando la base de datos de destino o ejecutando una consulta SELECT en el nombre de la tabla.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información sobre el Asistente para importación, lea esta [entrada de blog](https://cloudblogs.microsoft.com/sqlserver/2018/08/30/the-august-release-of-sql-operations-studio-is-now-available/).
- Para obtener más información sobre PROSE, lea [la documentación.](https://microsoft.github.io/prose/)