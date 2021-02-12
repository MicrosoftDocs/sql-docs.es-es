---
title: Administración de Clústeres de macrodatos (BDC) con cuadernos de Jupyter y Azure Data Studio
titleSuffix: SQL Server Big Data Clusters
description: Administración de Clústeres de macrodatos (BDC) con cuadernos de Jupyter y Azure Data Studio en un clúster de macrodatos de SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 09/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 34592d9a6cb3db8bde5a2a25098314fd9ace1208
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039485"
---
# <a name="manage-big-data-clusters-bdc-the-cluster-with-notebooks"></a>Administración de Clústeres de macrodatos (BDC) con cuadernos

Esta página es un índice de los cuadernos para Clústeres de macrodatos de SQL Server. Estos cuadernos ejecutables (.ipynb) están diseñados para SQL Server 2019 como ayuda para la administración de Clústeres de macrodatos.

Cada cuaderno está diseñado para comprobar sus propias dependencias. Una operación de tipo **Ejecutar todas las celdas** se realizará correctamente o generará una excepción con una sugerencia con hipervínculo a otro cuaderno para resolver la dependencia que falta. Siga el hipervínculo de la sugerencia al cuaderno siguiente, presione **Ejecutar todas las celdas** y, cuando el proceso finalice correctamente, vuelva al cuaderno original y presione **Ejecutar todas las celdas**.

Una vez que se instalan todas las dependencias y que la operación **Ejecutar todas las celdas** produce un error, cada cuaderno analizará los resultados y, siempre que sea posible, generará una sugerencia con hipervínculo a otro cuaderno para ayudarlo a resolver el problema.


## <a name="installing-and-uninstalling-utilities-on-big-data-cluster-bdc"></a>Instalación y desinstalación de utilidades en el Clúster de macrodatos (BDC)

Esta sección contiene un conjunto de cuadernos útiles para instalar y desinstalar las herramientas de la línea de comandos y los paquetes necesarios para administrar Clústeres de macrodatos de SQL Server.

|Nombre |Descripción |
|---|---|---|---|
|SOP010: Actualización de un clúster de macrodatos|Use este cuaderno para actualizar un Clúster de macrodatos mediante azdata. |
|SOP012: Instalación de unixodbc para Mac|Use este cuaderno cuando obtenga errores al usar la instalación con brew de odbc para SQL Server.|
|SOP036: Instalación de la interfaz de la línea de comandos de kubectl|Use este cuaderno para instalar la interfaz de la línea de comandos kubectl independientemente del sistema operativo.|
|SOP037: Desinstalación de la interfaz de la línea de comandos de kubectl|Use este cuaderno para instalar la interfaz de la línea de comandos kubectl independientemente del sistema operativo.|
|SOP038: Instalación de la interfaz de la línea de comandos de Azure|Use este cuaderno para instalar la interfaz de la línea de comandos de la CLI de Azure independientemente del sistema operativo.|
|SOP039: Desinstalación de la interfaz de la línea de comandos de Azure|Use este cuaderno para desinstalar la interfaz de la línea de comandos de la CLI de Azure independientemente del sistema operativo.|
|SOP040: actualización de PIP en el espacio aislado de Python de ADS|Use este cuaderno para actualizar PIP en el espacio aislado de Python de ADS.|
|SOP054: Instalación de la interfaz de la línea de comandos de azdata|Use este cuaderno para instalar la interfaz de la línea de comandos de azdata independientemente del sistema operativo.|
|SOP055: Desinstalación de la interfaz de la línea de comandos de azdata|Use este cuaderno para instalar la interfaz de la línea de comandos de azdata independientemente del sistema operativo.|
|SOP059: instalación del módulo de Python de Kubernetes|Use este cuaderno para instalar los módulos de Kubernetes con Python.|
|SOP060: desinstalación del módulo de Kubernetes|Use este cuaderno para instalar los módulos de Kubernetes con Python.|
|SOP061: eliminación del clúster de macrodatos|Use este cuaderno para eliminar un clúster de BDC.|
|SOP062: instalación de los módulos ipython-sql y pyodbc|Use este cuaderno para instalar los módulos ipython-sql y pyodbc.|
|SOP063: Instalación de la CLI de azdata (con el administrador de paquetes)|Use este cuaderno para instalar la CLI de azdata (con el administrador de paquetes).|
|SOP064: Desinstalación de la CLI de azdata (con el administrador de paquetes)|Use este cuaderno para desinstalar la CLI de azdata (con el administrador de paquetes).|
|SOP069: Instalación de ODBC para SQL Server|Use este cuaderno para instalar el controlador ODBC, ya que algunos subcomandos de azdata requieren el controlador ODBC de SQL Server.|


## <a name="managing-certificates-on-big-data-clusters-bdc"></a>Administración de certificados en Clústeres de macrodatos (BDC)

Un conjunto de cuadernos para ejecutar un cuaderno para administrar certificados en Clústeres de macrodatos.

|Nombre |Descripción |
|---|---|---|---|
|CER001: Generación de un certificado de CA raíz|Genere un certificado de CA raíz. Considere la posibilidad de usar un certificado de CA raíz para todos los clústeres que no sean de producción en cada entorno, ya que esta técnica reduce el número de certificados de CA raíz que deben cargarse en los clientes que se conectan a estos clústeres. |
|CER002: Descarga del certificado de CA raíz existente|Use este cuaderno para descargar un certificado de CA raíz generado desde un clúster.|
|CER003: Carga del certificado de CA raíz existente|Cargue el certificado de CA raíz existente.|
|CER004: Descarga y carga del certificado de CA raíz existente|Descargue y cargue el certificado de CA raíz existente. |
|CER010: Instalación de la CA raíz generada localmente|Este cuaderno copiará localmente (desde un Clúster de macrodatos) el certificado de CA raíz generado que se instaló mediante **CER001: Generación de un certificado de CA raíz** o **CER003: Carga del certificado de CA raíz existente** y, a continuación, instalará el certificado de CA raíz en el almacén de certificados local de esta máquina.|
|CER020: Creación de certificado de proxy de administración|Este cuaderno crea un certificado para el punto de conexión proxy de administración.|
|CER021: Creación del certificado de Knox|Este cuaderno crea un certificado para el punto de conexión de la puerta de enlace de Knox.|
|CER022: Creación de un certificado de proxy de aplicación|Este cuaderno crea un certificado para el punto de conexión proxy de App-Deploy.|
|CER023: Creación de certificado maestro|Este cuaderno crea un certificado para el punto de conexión principal.|
|CER030: Firma de certificado de proxy de administración con la CA generada|Este cuaderno firma el certificado creado mediante **CER020: Creación de certificado de proxy de administración** con la generación de un certificado de CA raíz, creado con **CER001: Generación de un certificado de CA raíz** o **CER003: Carga del certificado de CA raíz existente**.|
|CER031: Firma de certificado de Knox con la CA generada|Este cuaderno firma el certificado creado mediante **CER021: Creación del certificado de Knox** con la generación de un certificado de CA raíz, creado con **CER001: Generación de un certificado de CA raíz** o **CER003: Carga del certificado de CA raíz existente**.|
|CER032: Firma del certificado de proxy de aplicación con la CA generada|Este cuaderno firma el certificado creado mediante **CER022: Creación de un certificado de proxy de aplicación** con la generación de un certificado de CA raíz, creado con **CER001: Generación de un certificado de CA raíz** o **CER003: Carga del certificado de CA raíz existente**.|
|CER033: Firmar de certificado principal con CA generada|Este cuaderno firma el certificado creado mediante **CER023: Creación de certificado maestro** con la generación de un certificado de CA raíz, creado con **CER001: Generación de un certificado de CA raíz** o **CER003: Carga del certificado de CA raíz existente**.|
|CER040: Instalación del certificado de proxy de administración firmado|Este cuaderno se instala en el Clúster de macrodatos con el certificado firmado mediante **CER030: Firma de certificado de proxy de administración con la CA generada**.|
|CER041: Instalación de un certificado de Knox firmado|Este cuaderno se instala en el Clúster de macrodatos con el certificado firmado mediante **CER031: Firma de certificado de Knox con la CA generada**.|
|CER042: Instalación del certificado de proxy de aplicación firmado|Este cuaderno se instala en el Clúster de macrodatos con el certificado firmado mediante **CER032: Firma del certificado de proxy de aplicación con la CA generada**.|
|CER043: Instalación del certificado de controlador firmado|Este cuaderno se instala en el Clúster de macrodatos con el certificado firmado mediante **CER034: Firma del certificado de controlador con CA raíz de clúster** y tenga en cuenta que al final de este cuaderno el pod Controlador y todos los pods que usan PolyBase (los pods Grupo maestro y Grupo de proceso) se reiniciarán para cargar los nuevos certificados.|
|CER050: Espera hasta que el BDC esté en buen estado|Este cuaderno esperará hasta que el Clúster de macrodatos haya vuelto a un estado correcto, después de que el pod Controlador y los pods que usan PolyBase se hayan reiniciado para cargar los nuevos certificados.|
|CER100: Configuración del clúster con certificados autofirmados|Este cuaderno generará una nueva CA raíz en el Clúster de macrodatos y creará nuevos certificados para cada punto de conexión (esos puntos de conexión son: administración, puerta de enlace, aplicación-proxy y controlador). Firme cada nuevo certificado con la nueva CA raíz generada, excepto el certificado Controlador (que está firmado con la CA raíz del clúster existente) y, después, instale cada certificado en el Clúster de macrodatos. Descargue la nueva CA raíz generada en el almacén de certificados de Entidades de certificación raíz de confianza de este equipo. Todos los certificados autofirmados generados se almacenarán en el pod Controlador en la ubicación test_cert_store_root.|
|CER101: Configuración del clúster con certificados autofirmados mediante la CA raíz existente|Este cuaderno usará una CA raíz generada en el Clúster de macrodatos (cargada con **CER003**) y creará nuevos certificados para cada punto de conexión (administración, puerta de enlace, aplicación-proxy y controlador) y, a continuación, firme cada nuevo certificado con la nueva CA raíz generada, excepto el certificado Controlador (que está firmado con la CA raíz del clúster existente), e instale cada certificado en el Clúster de macrodatos. Todos los certificados autofirmados generados se almacenarán en el pod Controlador (en la ubicación test_cert_store_root). Tras la finalización de este cuaderno, todo el acceso de https:// al Clúster de macrodatos desde este equipo (y cualquier equipo que instale la nueva CA raíz) se mostrará como seguro. El capítulo Notebook Runner asegurará que los CronJobs creados (OPR003) para ejecutar App-Deploy instalarán la CA raíz del clúster para permitir la obtención segura de tokens JWT y swagger.json.|

## <a name="backup-and-restore-from-big-data-cluster-bdc"></a>Copia de seguridad y restauración desde un Clúster de macrodatos (BDC)

Esta sección contiene un conjunto de cuadernos útiles para las operaciones de copia de seguridad y restauración de Clústeres de macrodatos de SQL Server.

| Nombre | Descripción |
|--|--|
| SOP008: Copia de seguridad de archivos HDFS en Azure Data Lake Storage Gen2 con distcp | Este procedimiento operativo estándar (SOP) realiza una copia de seguridad de los datos desde el sistema de archivos HDFS del clúster de macrodatos de SQL Server 2019 de origen a la cuenta de Azure Data Lake Store Gen2 que especifique. Asegúrese de que la cuenta de Azure Data Lake Store Gen2 está configurada con la opción "espacio de nombres jerárquico" habilitada. |

## <a name="next-steps"></a>Pasos siguientes

Vea [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md) para obtener más información sobre los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)].
