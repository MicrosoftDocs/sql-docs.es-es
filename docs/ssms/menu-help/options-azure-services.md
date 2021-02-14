---
title: 'Página de opciones de SQL Server: servicios de Azure'
description: Opciones (servicios de Azure)
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- VS.ToolsOptionsPages.Azure_Services.Azure_Cloud
dev_langs:
- TSQL
author: markingmyname
ms.author: maghan
ms.date: 01/15/2021
ms.openlocfilehash: 7e4bb93f4a3a992c5033061ca8a2e0209125a5fa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076626"
---
# <a name="options-azure-services"></a>Opciones (servicios de Azure)

Use esta página para especificar las opciones relacionadas con los servicios en la nube de Azure. Para acceder a este cuadro de diálogo, vaya a **Herramientas > Opciones > Azure Services** en la barra de menús superior.

## <a name="miscellaneous"></a>Varios

| Opción | Información | Descripción |
|--------|-------------|-------------|
| Nivel de seguimiento de la ventana de salida de ADAL | **Información** <br> **Ninguno** <br> **Detallado** <br> **Warning (ADVERTENCIA)** | El nivel de seguimiento de inicio de sesión de Azure Active Directory (AAD) que se va a enviar a la ventana de salida. |
| URL del portal de Azure Data Factory | `https://adf.azure.com` | Especifica la dirección URL del portal de Azure Data Factory. |
| Habilitación del acceso condicional a Azure SQL Database (EXPERIMENTAL) | **True** <br> **False** | EXPERIMENTAL: Si es true, solicita tokens de acceso para Azure SQL Database que incluye una notificación para el acceso al servidor seleccionado. El establecimiento puede requerir el reinicio de SSMS para que surta efecto. |
| Punto de conexión de la galería | `https://gallery.azure.com` | Especifica el punto de conexión de la galería de Resource Manager de plantillas de implementación. |
| Audiencia de Graph | `https://graph.windows.net` | Identificador de recursos de punto de conexión de Graph. |
| Punto de conexión de Graph | `https://graph.windows.net` | Especifica la dirección URL de las solicitudes de Graph para Azure Active Directory. |
| URL del Portal de administración | `https://portal.azure.com` | Especifica la dirección URL del Portal de administración. |
| URL del archivo de configuración de publicación | `https://go.microsoft.com/fwlink/?LinkID=335839` | Especifica la dirección URL desde la que se puede descargar el archivo `.publishsettings`. |
| Nombre principal del servicio SQL Database | `https://database.windows.net/` | El SPN de Azure SQL Database para obtener un token cuando se usa la autenticación de AAD. También es la audiencia de JSON Web Token (JWT) para el análisis y la validación de JSON Web Token (JWT) del lado servidor. |

## <a name="resource-management"></a>Administración de recursos

| Opción | Información | Descripción |
|--------|-------------|-------------|
| Entidad de Active Directory | `https://login.microsoftonline.com` | Especifica la entidad base para la autenticación de Azure Active Directory (AAD). |
| Id. de recurso del punto de conexión del servicio Active Directory | `https://management.core.windows.net` | Especifica la audiencia de los tokens que autentican las solicitudes a los puntos de conexión de Azure Resource Manager (ARM) o de administración de servicios (RDFE). |
| Punto de conexión de Resource Manager | `https://management.azure.com` | Especifica la dirección URL de las solicitudes de administración de recursos. |
| Punto de conexión de servicio | `https://management.core.windows.net` | Especifica el punto de conexión de las solicitudes de administración de servicios. |

## <a name="select-an-azure-cloud"></a>Selección de una nube de Azure

| Opción | Información | Descripción |
|--------|-------------|-------------|
| Nombre | **Nube de Azure China** <br><br> **Nube de Azure** <br><br> **Nube de Azure German** <br><br> **Azure US Government** <br><br> **Personalizada** | Seleccione la nube de Azure a la que se conecta Management Studio para detectar y administrar recursos. Seleccione **Personalizado** para proporcionar sus propias direcciones URL y sufijos DNS. |

## <a name="service-addresses"></a>Direcciones de servicio

| Opción | Información | Descripción |
|--------|-------------|-------------|
| Punto de conexión de los servicios de Azure Data Lake | `azuredatalakeanalytics.net` | Especifica el catálogo de Azure Data Lake Analytics y el sufijo del extremo del trabajo. |
| Punto de conexión del sistema de archivos de Azure Data Lake Store | `azuredatalakestore.net` | Especifica el sufijo del punto de conexión del sistema de archivos de Azure Data Lake Store. | 
| Audiencia de Azure Key Vault | `https://vault.azure.net` | Especifica la audiencia de los tokens de acceso que autorizan solicitudes para servicios de Key Vault. |
| Sufijo DNS de Azure Key Vault | `vault.azure.net` | Especifica el sufijo del nombre de dominio para servidores de Azure Key Vault. |
| Sufijo DNS de SQL Database | `database.windows.net` | Especifica el sufijo del nombre de dominio para servidores de Azure SQL Database. |
| Punto de conexión de Storage | `core.windows.net` | Especifica el punto de conexión para el acceso al almacenamiento. La opción incluye almacenamiento en blobs, tablas, colas y archivos. |
| Sufijo DNS de Traffic Manager | `trafficmanager.net` | Especifica el sufijo del nombre de dominio para servicios de Azure Traffic Manager. |