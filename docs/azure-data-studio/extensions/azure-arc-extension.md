---
title: Extensión de Azure Arc (versión preliminar)
description: Información sobre cómo instalar y usar la extensión de Azure Arc para probar los servicios de datos de Azure Arc.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan, sstein
ms.custom: ''
ms.date: 09/22/2020
ms.openlocfilehash: 195404df404c6fec7ef98f41a9921d937c16eab7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048595"
---
# <a name="azure-arc-extension-for-azure-data-studio-preview"></a>Extensión de Azure Arc para Azure Data Studio (versión preliminar)

La [extensión de Azure Arc (versión preliminar)](/azure/azure-arc/data/) es una extensión para crear y administrar recursos de servicios de datos de Azure Arc.

**Estas son algunas de las acciones principales:**
- Creación de un recurso
    - Controlador de datos
    - SQL Managed Instance para Azure Arc
    - PostgreSQL para Azure Arc
- Administración de un recurso
    - Visualización del panel del controlador de datos
    - Visualización del panel de SQL Managed Instance para Azure Arc
    - Visualización del panel de PostgreSQL para Azure Arc
- Ejecución de Jupyter Book de Azure Arc

## <a name="install-the-extension"></a>Instalación de la extensión
- Instale la extensión de la **CLI de Azure Data** desde la galería.
- Instale la extensión de **Azure Arc** desde la galería.
- Reinicio de Azure Data Studio

## <a name="sign-in-with-azure-account"></a>Inicio de sesión con la cuenta de Azure
1. Seleccione Cuentas en la parte inferior izquierda.
1. Seleccione Agregar cuenta.
1. Esta acción abrirá un explorador. Inicie sesión en la cuenta de Azure.

## <a name="create-a-resource"></a>Creación de un recurso
Esta extensión admite la implementación de controladores de datos de Azure Arc, Postgres para Azure Arc y SQL Managed Instance para Azure Arc. Las implementaciones se pueden realizar con el asistente para implementación integrado.

1. Seleccione el viewlet Conexiones en la barra de actividad de la izquierda.
1. Seleccione los tres puntos y, después, **Nueva implementación**.
1. Siga las indicaciones para crear un recurso de Azure Arc.

## <a name="manage-a-resource"></a>Administración de un recurso
Después de implementar un controlador de datos de Azure Arc desde azdata, Azure Portal o Azure Data Studio, puede conectarse a él desde Azure Data Studio.

1. Abra el viewlet Conexiones en la barra de actividad de la izquierda.
1. Expanda **Azure Arc controllers** (Controladores de Azure Arc).
1. Seleccione **Connect controller** (Conectar controlador).
1. Rellene los parámetros y conéctese.

Una vez conectado, podrá ver los recursos implementados en el controlador de datos. Después, puede hacer clic con el botón derecho y seleccionar **Administrar** para acceder al panel de recursos.  

Estos paneles proporcionarán información adicional sobre el recurso, incluida la opción de abrirlo en Azure Portal.

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre los servicios de datos de Azure Arc, [consulte nuestra documentación](/azure/azure-arc/data/).