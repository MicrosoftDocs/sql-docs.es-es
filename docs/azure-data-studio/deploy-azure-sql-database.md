---
title: Implementación de una base de datos de Azure SQL a través de Notebook
description: En este tutorial se muestra cómo crear una base de datos de Azure SQL.
author: ninarn
ms.author: ninarn
ms.reviewer: alayu, markingmyname
ms.topic: tutorial
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.custom: ''
ms.date: 10/16/2020
ms.openlocfilehash: 8bac1e0d10208283e40843288ab7dbe7d042b8ad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052216"
---
# <a name="create-an-azure-sql-database-using-azure-data-studio"></a>Creación de una base de datos de Azure SQL mediante Azure Data Studio

Puede crear una base de datos de Azure SQL mediante Azure Data Studio y el Asistente para la implementación y los notebooks.

## <a name="pre-requisites"></a>Requisitos previos

 - [Azure Data Studio](download-azure-data-studio.md) instalado.
 - Una cuenta y una suscripción activas de Azure. Si no dispone de ninguna, [cree una cuenta gratuita](https://azure.microsoft.com/free/).

## <a name="use-the-deployment-wizard"></a>Usar el Asistente para la implementación

Siga estos pasos para usar el Asistente para la implementación, que le guiará a través de la configuración necesaria en una experiencia de interfaz de usuario simple.

Primero, busque y seleccione Azure SQL Database en el Asistente para la implementación.

 1. En Azure Data Studio, seleccione el viewlet Conexiones en el panel de navegación izquierdo.
 2. Seleccione el botón **...** en la parte superior del panel Conexiones y elija **Nueva implementación**.
 3. En el Asistente para la implementación, seleccione el icono de **Azure SQL Database** y active la casilla de aceptación de términos.
 4. En la lista desplegable Tipo de recurso, seleccione lo que quiere crear: una base de datos, un servidor de base de datos o un grupo elástico. Si no tiene ninguna base de datos SQL Database en Azure, se recomienda empezar por crear una.
 5. Seleccione **Crear en Azure Portal** si ha elegido crear un servidor o un grupo de servidores, o bien elija **Seleccionar** si quiere crear una base de datos.

Si ha decidido crear una base de datos, siga los pasos que hay a continuación.

 1. Inicie sesión en su cuenta de Azure si aún no lo ha hecho. Puede actualizar la conexión si tiene problemas en esta página del asistente.
 2. Seleccione la suscripción y el servidor que quiera. Luego, seleccione **Siguiente**.
 3. Especifique un nombre para la base de datos.
 4. Especifique un nombre para la regla de firewall y el intervalo IP de la máquina de cliente local que ejecuta Azure Data Studio. De esta forma, se podrá conectar al servidor y a la base de datos de ADS una vez que se hayan creado.
 5. Seleccione **Next** (Siguiente).
 6. Revise los parámetros que ha escrito y, después, seleccione **Script a Notebook**.

Cuando se abra Notebook, revise el contenido y el código, y haga los cambios que considere necesarios. En concreto, puede cambiar la configuración predeterminada para el proceso y el almacenamiento en caso de que tenga un rendimiento concreto o preferencias relativas a los costos. Sin embargo, tenga en cuenta que los cambios que aplique a Notebook podrían generar errores de validación.

El último paso es seleccionar **Ejecutar todo** para ejecutar todas las celdas del Notebook. Una vez que se haya completado este proceso, dispondrá de una instancia de SQL Database creada y en ejecución que podrá conectar a ADS y realizar consultas.

## <a name="next-steps"></a>Pasos siguientes

Después de crear la base de datos, el servidor o el grupo, puede [conectarse y consultar](quickstart-sql-database.md) la base de datos en Azure Data Studio.
