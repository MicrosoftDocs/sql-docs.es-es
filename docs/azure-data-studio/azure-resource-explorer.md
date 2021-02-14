---
title: Exploración de recursos de Azure SQL con Azure Resource Explorer
description: Aprenda a explorar y administrar Azure SQL Server, Azure SQL Database e Instancia administrada de Azure SQL con Azure Resource Explorer.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: how-to
author: yanancai
ms.author: yanacai
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: 5d28054e9f459bc3334a863e7ee5edb3bd98afea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048735"
---
# <a name="explore-and-manage-azure-sql-resources-with-azure-resource-explorer"></a>Exploración y administración de recursos de Azure SQL con Azure Resource Explorer

En este documento aprenderá a explorar y administrar recursos de Azure SQL Server, Azure SQL Database e Instancia administrada de Azure SQL con Azure Resource Explorer en [!INCLUDE [Azure Data Studio](../includes/name-sos-short.md)].

>[!NOTE]
>El Azure Resource Explorer se admite en SQL Server 2019. Llegado ese momento, podrá instalar la extensión mediante el [administrador de extensiones](./extensions/add-extensions.md) o mediante **Archivo** > **Instalar paquete desde paquete VSIX**.

## <a name="connect-to-azure"></a>Conexión con Azure

Después de instalar el complemento de SQL, aparecerá un icono de Azure en la barra de menús de la izquierda. Haga clic en él para abrir Azure Resource Explorer. Si no lo ve, haga clic con el botón derecho en la barra de menús de la izquierda y seleccione **Azure Resource Explorer**.

### <a name="add-an-azure-account"></a>Adición de una cuenta de Azure

Para ver los recursos de SQL asociados a una cuenta de Azure, primero debe agregar la cuenta a [!INCLUDE [Azure Data Studio](../includes/name-sos-short.md)].

1. Abra el cuadro de diálogo **Cuentas vinculadas** a través del icono de administración de cuentas en la parte inferior izquierda, o bien usando el vínculo **Iniciar sesión en Azure...** de Azure Resource Explorer.

    ![Inicio de sesión en Azure](media/azure-resource-explorer/sign-in-to-azure.png)

2. En el cuadro de diálogo **Cuentas vinculadas**, haga clic en **Agregar una cuenta**.

    ![Adición de una cuenta de Azure](media/azure-resource-explorer/add-an-azure-account.png)

3. Haga clic en **Copiar y abrir** para abrir el explorador para autenticar la cuenta.

    ![Abrir la página de autenticación en el explorador](media/azure-resource-explorer/open-authentication-in-browser.png)

4. Pegue el **Código de usuario** en la página web y haga clic en **Continuar** para autenticarse.

    ![Autenticarse en el explorador](media/azure-resource-explorer/authenticate-in-browser.png)

5. En [!INCLUDE [Azure Data Studio](../includes/name-sos-short.md)], ahora la cuenta de Azure con la que ha iniciado sesión debería aparecer en el cuadro de diálogo **Cuentas vinculadas**.

    ![Cuenta de Azure con sesión iniciada](media/azure-resource-explorer/signed-in-azure-account.png)

### <a name="add-more-azure-accounts"></a>Adición de más cuentas de Azure

En [!INCLUDE [Azure Data Studio](../includes/name-sos-short.md)] se admiten varias cuentas de Azure. Para agregar más cuentas de Azure, haga clic en el botón situado en la parte superior derecha del cuadro de diálogo **Cuentas vinculadas** y realice el mismo procedimiento de la sección Adición de una cuenta de Azure para agregar más cuentas de Azure.

![Agregar más cuentas de Azure](media/azure-resource-explorer/add-more-azure-account.png)

### <a name="remove-an-azure-account"></a>Eliminación de una cuenta de Azure

Para quitar una cuenta de Azure existente con una sesión iniciada:

1. Abra el cuadro de diálogo **Cuentas vinculadas** mediante el icono de administración de cuentas situado en la parte inferior izquierda.
2. Haga clic en el botón **X** situado a la derecha de la cuenta de Azure para quitarla.

    ![Quitar una cuenta de Azure](media/azure-resource-explorer/remove-azure-account.png)

## <a name="filter-subscription"></a>Filtrado de suscripciones

Una vez que haya iniciado sesión en una cuenta de Azure, todas las suscripciones asociadas a esa cuenta de Azure se mostrarán en Azure Resource Explorer. Puede filtrar las suscripciones de cada cuenta de Azure.

1. Haga clic en el botón **Seleccionar suscripción** situado a la derecha de la cuenta de Azure.

   ![Filtrado de suscripciones](media/azure-resource-explorer/filter-subscription.png)

2. Active las casillas de las suscripciones de cuenta que quiera examinar y, después, haga clic en **Aceptar**.

   ![Seleccionar suscripción](media/azure-resource-explorer/select-subscription.png)

## <a name="explore-azure-sql-resources"></a>Exploración de recursos de Azure SQL

Para desplazarse por un recurso de Azure SQL en Azure Resource Explorer, expanda el grupo de cuentas y tipos de recurso de Azure.

Actualmente, Azure Resource Explorer es compatible con Azure SQL Server, con Azure SQL Database y con Instancia administrada de Azure SQL.

## <a name="connect-to-azure-sql-resources"></a>Conexión a recursos de Azure SQL

Azure Resource Explorer proporciona un acceso rápido que ayuda a conectarse a los servidores y bases de datos de SQL Server para su consulta y administración.

1. Explore el recurso de SQL al que quiera conectarse desde la vista de árbol.
2. Haga clic con el botón derecho en el recurso y seleccione **Conectar**; encontrará este botón también a la derecha del recurso.

   ![Conectarse a un recurso de Azure SQL](media/azure-resource-explorer/connect-to-azure-sql-resource.png)

3. En el cuadro de diálogo **Conexión** abierto, escriba la contraseña y haga clic en **Conectar**.

   ![Cuadro de diálogo de conexión SQL](media/azure-resource-explorer/sql-connection-dialog.png)
4. La ventana **Servidores** se abrirá automáticamente con el nuevo servidor o base de datos SQL conectado cuando la conexión se haya establecido correctamente.

## <a name="next-steps"></a>Pasos siguientes

- [Uso de [!INCLUDE[Azure Data Studio](../includes/name-sos-short.md)] para conectarse a una base de datos de Azure SQL y consultarla](quickstart-sql-database.md)
- [Uso de [!INCLUDE[Azure Data Studio](../includes/name-sos-short.md)] para conectarse a Azure Synapse Analytics y realizar consultas de datos](quickstart-sql-dw.md)