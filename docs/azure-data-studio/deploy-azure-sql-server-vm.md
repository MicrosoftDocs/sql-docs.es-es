---
title: Implementación de SQL Server en Azure Virtual Machines
description: En este tutorial se muestra cómo crear SQL Server en Azure Virtual Machines
author: ninarn
ms.author: ninarn
ms.reviewer: alayu, maghan
ms.topic: tutorial
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: c5cffc4964e7a91b3b63da56e43e01db41bbe8a9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048635"
---
# <a name="create-sql-server-on-azure-virtual-machines-using-azure-data-studio"></a>Creación de SQL Server en Azure Virtual Machines mediante Azure Data Studio

Puede crear una máquina virtual de SQL mediante Azure Data Studio y el Asistente para la implementación y los notebooks.

## <a name="pre-requisites"></a>Requisitos previos

- [Azure Data Studio](download-azure-data-studio.md) instalado.
- Una cuenta y una suscripción activas de Azure. En caso de no tener ninguna, [cree una cuenta gratuita](https://azure.microsoft.com/free/).

## <a name="use-the-deployment-wizard"></a>Usar el Asistente para la implementación

Siga estos pasos para usar el Asistente para la implementación, que le guiará a través de la configuración necesaria en una experiencia de interfaz de usuario simple.

En primer lugar, busque y seleccione Azure SQL VM (Azure SQL VM) en el Asistente para la implementación.

1. En Azure Data Studio, seleccione el viewlet Conexiones en el panel de navegación izquierdo.

2. Seleccione el botón **...** en la parte superior del panel Conexiones y elija **Nueva implementación**.

3. En el Asistente para la implementación, seleccione el icono de **Azure SQL VM** (VM de Azure SQL) y active la casilla de aceptación de términos.

4. Si se le solicita, instale las herramientas necesarias y luego haga clic en **Seleccionar** en la parte inferior.

Después, escriba todos los parámetros necesarios en el Asistente para VM de Azure SQL.

1. Inicie sesión en su cuenta de Azure si aún no lo ha hecho. Puede actualizar la conexión si tiene problemas en esta página del asistente.

2. Seleccione la suscripción, el grupo de recursos y la región que quiera. Luego, seleccione **Siguiente**.

3. Escriba un nombre de máquina virtual único y las credenciales de nombre de usuario y contraseña.

4. Seleccione la imagen, el SKU y la versión que quiera, y luego elija el tamaño de máquina virtual que prefiera. Puede obtener más información sobre [tamaños de máquina virtual disponibles](/azure/virtual-machines/sizes) para ayudarle elegir. Luego, seleccione **Siguiente**.

5. Seleccione una red virtual existente en la lista desplegable o active la casilla **Nueva red virtual** para escribir un nombre para una nueva red virtual.

6. Haga lo mismo para seleccionar o crear una subred y una dirección IP pública.

7. Si quiere conectarse a la máquina virtual a través del Escritorio remoto (RDP), active la casilla **Enable RDP inbound port** (Habilitar el puerto de entrada RDP). Luego, seleccione **Siguiente**.

8. Escriba la configuración de SQL Server preferida. La recomendación para probar esta experiencia es establecer la conectividad de SQL en **Pública (Internet)** , escribir el puerto 1433 y habilitar la autenticación de SQL con el nombre de usuario y la contraseña que prefiera. Luego, seleccione **Siguiente**.

9. Revise los parámetros que ha escrito y, después, seleccione **Script a Notebook**.

Cuando se abra Notebook, revise el contenido y el código, y haga los cambios que considere necesarios. Tenga en cuenta que no se recomienda realizar cambios, ya que podría producir errores de validación.

El último paso es seleccionar **Ejecutar todo** para ejecutar todas las celdas del Notebook. Una vez completado, se habrá creado y se estará ejecutando:

- Una máquina virtual de Azure
- Una máquina virtual SQL
- Una red virtual, una subred y una dirección IP pública
- Un grupo de seguridad de red y una interfaz de red
- Acceso a RDP en la máquina virtual
- Acceso a diversas características de administración de SQL Server para controlar fácilmente la programación de copia de seguridad, la programación de instalación de revisiones, la edición y la administración de licencias de SQL Server, las configuraciones de rendimiento del almacenamiento y mucho más.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la migración de los datos a la nueva máquina virtual de SQL Server, vea el artículo siguiente.

> [!div class="nextstepaction"]
> [Migración de una base de datos a una máquina virtual SQL](/azure/azure-sql/virtual-machines/windows/migrate-to-vm-from-sql-server)

Para más información sobre el uso de SQL Server en Azure, consulte Ia página sobre [SQL Server en Azure Virtual Machines](/azure/azure-sql/virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview) y las [Preguntas más frecuentes](/azure/azure-sql/virtual-machines/windows/frequently-asked-questions-faq).