---
title: Virtualizar datos externos
description: En esta página se detallan los pasos para usar el asistente de creación de tablas externas para orígenes de datos ODBC
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
ms.date: 12/13/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: polybase
monikerRange: '>= sql-server-ver15'
ms.metadata: seo-lt-2019
ms.openlocfilehash: 85365639565eaf42ca1bf23825016a08ef8f50bf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081346"
---
# <a name="use-the-external-table-wizard-with-odbc-data-sources"></a>Uso del asistente para tablas externas con orígenes de datos ODBC

Uno de los escenarios clave de SQL Server 2019 es la posibilidad de virtualizar datos. Este proceso permite que los datos permanezcan en su ubicación original. Puede *virtualizar* los datos de una instancia de SQL Server para que se puedan consultar como cualquier otra tabla de SQL Server. Este proceso minimiza la necesidad de procesos ETL. Este proceso es posible gracias a los conectores de PolyBase. Para obtener más información sobre la virtualización de datos, vea [Introducción a PolyBase](polybase-guide.md).

En este vídeo se proporciona una introducción a la virtualización de datos:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Introducing-Data-Virtualization/player?WT.mc_id=dataexposed-c9-niner]


## <a name="start-the-external-table-wizard"></a>Iniciar el Asistente para tablas externas

Conéctese a la instancia maestra mediante el número de puerto o la dirección IP del punto de conexión **sql-server-master** obtenido mediante el comando [**azdata cluster endpoints list**](../../big-data-cluster/deployment-guidance.md#endpoints). Expanda el nodo **Bases de datos** en el Explorador de objetos. Luego seleccione una de las bases de datos donde quiere virtualizar los datos de una instancia existente de SQL Server. Haga clic con el botón derecho en la base de datos y seleccione **Create External Table** (Crear tabla externa) para iniciar el asistente para virtualizar datos. También puede iniciar al asistente para virtualizar datos desde la paleta de comandos. Use Ctrl + Mayús + P en Windows o Cmd + Mayús + P en un equipo Mac.

![Asistente para virtualizar datos](media/data-virtualization/virtualize-data-wizard.png)
## <a name="select-a-data-source"></a>Selección de un origen de datos

Si ha iniciado el asistente desde una de las bases de datos, el cuadro de lista desplegable de destino se rellena automáticamente. También tiene la opción de especificar o cambiar la base de datos de destino en esta página. Los tipos de orígenes de datos externos compatibles con el asistente son SQL Server, Oracle, MongoDB y Teradata.

> [!NOTE]
>SQL Server aparece resaltado de forma predeterminada.


![Selección de un origen de datos](media/data-virtualization/select-data-source.png)

Seleccione **Siguiente** para continuar.

## <a name="create-a-database-master-key"></a>Creación de la clave maestra de una base de datos

En este paso, se crea una clave maestra de base de datos. Es necesario crear una clave maestra. Una clave maestra protege las credenciales que usa un origen de datos externo. Elija una contraseña segura para la clave maestra. Además, realice una copia de seguridad de la clave maestra con **BACKUP MASTER KEY**. Almacene la copia de seguridad en una ubicación segura fuera de las instalaciones.

![Creación de la clave maestra de una base de datos](media/data-virtualization/virtualize-data-master-key.png)

> [!IMPORTANT]
> Si ya tiene una clave maestra de base de datos, este paso se omitirá automáticamente.

## <a name="enter-external-data-source-credentials"></a>Especificar credenciales de origen de datos externo

En este paso, especifique el origen de datos externo y los detalles de las credenciales para crear un objeto de origen de datos externo. Las credenciales se usan para que el objeto de base de datos se conecte al origen de datos. Escriba un nombre para el origen de datos externo. Un ejemplo es Test. Proporcione los detalles de conexión de SQL Server del origen de datos externo. Escriba el **Nombre del servidor** y el **Nombre de la base de datos** donde quiere crear el origen de datos externo.

El siguiente paso es configurar una credencial. Escriba un nombre para la credencial. Este nombre es la credencial de ámbito de base de datos que se usa para almacenar de forma segura la información de inicio de sesión del origen de datos externo que crea. Un ejemplo es `TestCred`. Escriba un nombre de usuario y una contraseña para conectarse al origen de datos.

![Captura de pantalla que muestra el paso 3: Crear una conexión al origen de datos.](media/data-virtualization/data-source-credentials.png)

## <a name="external-data-table-mapping"></a>Asignación de tablas de datos externos

En la siguiente página, seleccione las tablas para las que quiere crear vistas externas. Al seleccionar bases de datos principales, también se incluyen las tablas secundarias. Después de seleccionar las tablas, aparece una tabla de asignación a la derecha. En ella puede realizar cambios en los tipos. También puede cambiar el nombre de la propia tabla externa seleccionada.

![Captura de pantalla que muestra el paso 4: Asignar los objetos de origen de datos a la tabla externa.](media/data-virtualization/data-table-map.png)

> [!NOTE]
>Para cambiar la vista de asignación, haga doble clic en otra tabla seleccionada.

> [!IMPORTANT]
>El tipo de fotografía no es compatible con la herramienta Tabla externa. Si crea una vista externa con un tipo de fotografía, aparece un error después de crear la tabla. Pero, aun así, la tabla se crea.

## <a name="summary"></a>Resumen

Este paso muestra un resumen de las selecciones. Proporciona el nombre de las credenciales de ámbito de base de datos y los objetos de origen de datos externo creados en la base de datos de destino. Seleccione **Generar script** para traducir a T-SQL la sintaxis empleada para crear el origen de datos externo. Seleccione **Crear** para crear el objeto de origen de datos externo.

![Pantalla de resumen](media/data-virtualization/virtualize-data-summary.png)

Si selecciona **Crear**, ve el objeto de origen de datos externo creado en la base de datos de destino.

![Orígenes de datos externos](media/data-virtualization/external-data-sources.png)

Si selecciona **Generar script**, ve la consulta T-SQL que se genera para crear el objeto de origen de datos externo.

![Generar script](media/data-virtualization/generated-script.png)

> [!NOTE]
> **Generar script** solo debería verse en la última página del asistente. De momento, aparece en todas las páginas.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre los clústeres de macrodatos de SQL Server y los escenarios relacionados, vea [¿Qué son los clústeres de macrodatos de SQL Server?](../../big-data-cluster/big-data-cluster-overview.md)
