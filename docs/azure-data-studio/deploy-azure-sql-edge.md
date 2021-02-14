---
title: Implementación de Azure SQL Edge con Azure Data Studio
description: Procedimiento para implementar instancias de Azure SQL Edge en Azure Data Studio
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: how-to
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: ''
ms.date: 09/22/2020
ms.openlocfilehash: b0dd541ce4be57dd9b065b93413ea567c7f121b7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052226"
---
# <a name="deploy-azure-sql-edge-with-azure-data-studio-preview"></a>Implementación de Azure SQL Edge con Azure Data Studio (Preview)

[Azure SQL Edge](/azure/azure-sql-edge/overview) es un motor de base de datos relacional optimizado para implementaciones de IoT y Azure IoT Edge. Proporciona funcionalidades para crear una capa de procesamiento y almacenamiento de datos de alto rendimiento para las aplicaciones y soluciones de IoT. En este artículo se muestra cómo implementar una instancia de Azure SQL Edge con Azure Data Studio y los escenarios de implementación compatibles con el asistente para la implementación.  

Los siguientes escenarios son compatibles con el asistente para la implementación en Azure Data Studio:

- Instancia de contenedor local
- Instancia de contenedor remoto
- Nueva instancia de Azure IoT Hub y VM
- Dispositivo existente de una instancia de Azure IoT Hub
- Varios dispositivos de una instancia de Azure IoT Hub

| Herramientas necesarias | `Docker` | `Azure CLI` |
| ------------- | :---: | :---: |
| Instancia de contenedor local | x | |
| Instancia de contenedor remoto | | |
| Nueva instancia de Azure IoT Hub y VM | | x |
| Dispositivo existente de una instancia de Azure IoT Hub |  | x |
| Varios dispositivos de una instancia de Azure IoT Hub |   |  x |

> [!NOTE]
> La implementación de Azure SQL Edge (Preview) está disponible mediante la extensión de implementación de Azure SQL Edge mientras estas características se encuentran en versión preliminar. Para que las características estén a su disposición, instale la extensión en Azure Data Studio.

Para iniciar una implementación en Azure Data Studio, abra el menú en la vista **Conexiones** y seleccione la opción **Nueva implementación…**  En todos los escenarios de Azure SQL Edge, seleccione **Azure SQL Edge** en la pantalla siguiente y el escenario deseado en la lista desplegable **Destino de implementación**. El asistente para la implementación genera un cuaderno de TSQL que se puede ejecutar para completar la implementación. Tenga en cuenta que la información de conexión, incluida la contraseña de administrador de SQL, se incluye en el cuaderno de manera predeterminada.

![Vista general de la implementación](media/deploy-azure-sql-edge/deploy-overview.png)

## <a name="local-container-instance"></a>Instancia de contenedor local

Azure SQL Edge se puede implementar en un contenedor de Docker en el localhost especificando el nombre del contenedor, la contraseña de usuario `sa` y el puerto del host para la conectividad de Azure SQL Edge.  Después de completar la implementación, hay disponibles varios vínculos en la parte inferior del cuaderno para realizar más acciones:

- **Haga clic aquí para conectarse a la instancia de Azure SQL Edge**: crea una conexión en Azure Data Studio a la nueva instancia de Azure SQL Edge.
- **Abrir la ventana de terminal**: abre un terminal (existente o nuevo) en Azure Data Studio.
- **Detener el contenedor**: copia un comando en el terminal que, cuando lo ejecuta el usuario, detiene el contenedor.
- **Quitar el contenedor**: copia un comando en el terminal que, cuando lo ejecuta el usuario, quita el contenedor.

## <a name="remote-container-instance"></a>Instancia de contenedor remoto

Azure SQL Edge se puede implementar en un contenedor de Docker en un host remoto con Docker instalado. Para ello, debe especificarse la información del contenedor y de la máquina host o de destino.  Después de completar la implementación, hay un vínculo de conexión disponible en la parte inferior del cuaderno.  Debido al entorno del host de contenedor remoto, para detener o quitar el contenedor, debe copiar los comandos y ejecutarlos en un shell remoto.

## <a name="new-azure-iot-hub-and-vm"></a>Nueva instancia de Azure IoT Hub y VM

El asistente para la implementación de Azure SQL Edge puede crear varios recursos de Azure que son necesarios para implementar una máquina virtual (VM) habilitada para Edge conectada a una instancia de Azure IoT Hub. Se necesita una suscripción activa de Azure. Esta implementación crea:

- Grupo de recursos (si el grupo de recursos actual no está seleccionado)
- Grupo de seguridad de red
- Máquina virtual
- Dirección IP pública estática

Opcionalmente, se puede comprimir un archivo DACPAC en una carpeta e implementarlo en la nueva instancia de Azure SQL Edge como parte del proceso.  Si se proporciona un archivo DACPAC, se crea una cuenta de Azure Blob Storage en el mismo grupo de recursos.

## <a name="existing-device-of-an-azure-iot-hub"></a>Dispositivo existente de una instancia de Azure IoT Hub

Si tiene un centro de IoT y un dispositivo conectado, Azure SQL Edge se puede implementar en el dispositivo según el grupo de recursos, el nombre del centro de IoT y el identificador de dispositivo.
La dirección IP proporcionada en el asistente para la implementación se usa para generar un vínculo de conexión rápida en la parte inferior del cuaderno.

Opcionalmente, se puede comprimir un archivo DACPAC en una carpeta e implementarlo en la nueva instancia de Azure SQL Edge como parte del proceso.  Si se proporciona un archivo DACPAC, se crea una cuenta de Azure Blob Storage en el mismo grupo de recursos.

## <a name="multiple-devices-of-an-azure-iot-hub"></a>Varios dispositivos de una instancia de Azure IoT Hub

Si tiene un centro de IoT y un dispositivo conectado, Azure SQL Edge se puede implementar en el dispositivo según el grupo de recursos, el nombre del centro de IoT y una [condición de destino](/azure/iot-edge/module-deployment-monitoring#target-condition) para seleccionar dispositivos.
La dirección IP proporcionada en el asistente para la implementación se usa para generar un vínculo de conexión rápida en la parte inferior del cuaderno.

Opcionalmente, se puede comprimir un archivo DACPAC en una carpeta e implementarlo en la nueva instancia de Azure SQL Edge como parte del proceso.  Si se proporciona un archivo DACPAC, se crea una cuenta de Azure Blob Storage en el mismo grupo de recursos.

## <a name="next-steps"></a>Pasos siguientes

- [Obtenga más información acerca de Azure SQL Edge](/azure/azure-sql-edge/).