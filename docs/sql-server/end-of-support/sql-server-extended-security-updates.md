---
title: ¿Qué son las Actualizaciones de seguridad extendidas?
description: Aprenda a usar el registro de SQL Server para obtener actualizaciones de seguridad extendidas para los productos de SQL Server de fin del soporte técnico y fin de la vida útil, como SQL Server 2008 y SQL Server 2008 R2.
ms.custom: ''
ms.date: 11/24/2020
ms.prod: sql
ms.technology: install
ms.topic: conceptual
author: cawrites
ms.author: chadam
ms.reviewer: pmasl
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 0f97fcf001db6fa561d54b3a9df6642f2aa148ea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346172"
---
# <a name="what-are-extended-security-updates-for-sql-server"></a>¿Qué son las Actualizaciones de seguridad extendidas para SQL Server?
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

En este artículo se proporciona información para usar el servicio del registro de SQL Server para recibir Actualizaciones de seguridad extendidas para [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] y [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)]. Para más información sobre otras opciones, consulte las [opciones de fin del soporte](sql-server-end-of-life-overview.md). 

## <a name="overview"></a>Información general
Una vez que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ha alcanzado el final de su ciclo de vida de soporte técnico, tiene la opción de registrarse para obtener una suscripción de Actualización de seguridad extendidas (ESU) para los servidores y permanecer protegido hasta por tres años, hasta que esté listo para actualizar a una versión más reciente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o migrar a [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Esta suscripción está disponible de dos maneras:
-  Se puede comprar para los servidores del entorno locales u hospedados.
-  Gratuita y habilitada de forma predeterminada al migrar servidores locales a Azure Virtual Machines. Luego, puede usar el servicio del **registro de SQL Server** en Azure Portal para registrar la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de fin del soporte técnico y descargar las actualizaciones cuando estén disponibles. 

Microsoft recomienda aplicar las revisiones de ESU tan pronto como estén disponibles para mantener protegida la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para información detallada sobre las ESU, consulte la página de [Preguntas más frecuentes sobre las Actualizaciones de seguridad extendidas](https://www.microsoft.com/cloud-platform/extended-security-updates).

> [!IMPORTANT]
> [El soporte extendido de SQL Server 2008 y SQL Server 2008 R2 finalizó el 10 de julio de 2019](https://www.microsoft.com/cloud-platform/windows-sql-server-2008). Para estas versiones, considere usar las Actualizaciones de seguridad extendidas que se describen en este artículo u otras opciones de migración. Para más información, consulte las [opciones del fin del soporte técnico](sql-server-end-of-life-overview.md).

## <a name="what-are-extended-security-updates"></a>¿Qué son las Actualizaciones de seguridad extendidas?
Las Actualizaciones de seguridad extendidas (ESU) para [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] y [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] incluyen el aprovisionamiento de actualizaciones de seguridad para los clientes que han comprado una suscripción a Actualización de soporte extendida.

Las ESU están disponibles **si es necesario** una vez que se detecta una vulnerabilidad de seguridad y el [Centro de respuestas de seguridad de Microsoft (MSRC)](https://portal.msrc.microsoft.com) la clasifica como **Crítica**. Por lo tanto, no hay ninguna cadencia de versión periódica para las ESU de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

Las ESU no incluyen:
- Nuevas características
- Mejoras funcionales
- Correcciones solicitadas por el cliente

### <a name="support"></a>Soporte técnico
Las ESU no incluyen soporte técnico, pero es posible usar un contrato de soporte técnico activo como [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default?activetab=software-assurance-default-pivot%3aprimaryr3) o Soporte técnico Premier/unificado en [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] / [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] para obtener soporte técnico en cargas de trabajo cubiertas por las ESU si opta por seguir en el entorno local. Como alternativa, si va a hospedar en Azure, puede usar un plan de soporte técnico de Azure para obtener soporte técnico. 

  > [!NOTE]
  > Microsoft no puede proporcionar soporte técnico para las instancias de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] y [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] (tanto locales como en entornos de hospedaje) que no se incluyen en una suscripción a ESU. 

## <a name="esu-availability-and-deployment"></a>Disponibilidad e implementación de ESU
Las ESU están disponibles para los clientes que ejecutan su carga de trabajo en Azure, en entornos locales o en entornos hospedados.

### <a name="azure-virtual-machines"></a>Azure Virtual Machines
Si migra sus cargas de trabajo a Azure Virtual Machines (IaaS), tendrá acceso a las Actualizaciones de seguridad extendidas de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] y [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] durante hasta tres años a contar de la finalización del soporte técnico, **sin cargos adicionales** por encima del costo que implica ejecutar la máquina virtual. Los clientes no necesitan Software Assurance para recibir las Actualizaciones de seguridad extendidas en Azure. 

Las instancias de Azure Virtual Machines que ejecutan [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en **Windows Server 2008 R2 y versiones superiores** recibirán las ESU de manera automática a través de los canales de actualizaciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] existentes cuando la máquina virtual se configura para usar la [aplicación automatizada de revisiones](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-patching).

Las máquinas virtuales (VM) de Azure que ejecutan [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en **Windows Server 2008** o las que **_no_ se han configurado para las [revisiones automatizadas](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-patching)** tendrán que descargar e implementar manualmente las revisiones de ESU como se describe en la sección [entornos locales o entornos hospedados](#on-premises-or-hosted-environments).

### <a name="on-premises-or-hosted-environments"></a>Entornos locales o entornos hospedados
Si tiene Software Assurance, puede comprar una suscripción Actualización de seguridad extendida (ESU) hasta tres años después de la fecha del fin del soporte técnico, en virtud de un Contrato Enterprise (EA), un Contrato Enterprise Subscription (SCE) o una Inscripción para soluciones de educación (EES). Solo puede comprar ESU solo para los servidores que tenga que proteger. Las ESU se pueden comprar directamente desde Microsoft o desde un asociado de licencia de Microsoft. 

Los clientes bajo la cobertura de contratos ESU deben seguir estos pasos para descargar e implementar una revisión de ESU:
-  [Registrar las instancias válidas](#register-instances-for-esus) con el **[registro de SQL Server](#create-sql-server-registry)** . 
-  Una vez que se haya realizado el registro, siempre que se publiquen revisiones de ESU, habrá un vínculo de descarga disponible en Azure Portal para descargar el paquete. 
-  El paquete descargado se puede implementar de forma manual en los entornos locales o los hospedados, o bien a través de la solución de orquestación de actualización que se use en la organización, como Microsoft Endpoint Configuration Manager (antes System Center Configuration Manager). 

> [!NOTE]
> Este es también el proceso que los clientes deben seguir para Azure Stack y Azure Virtual Machines que no estén configurados para recibir actualizaciones automáticas.

Para más información, consulte las [Preguntas frecuentes sobre las Actualizaciones de seguridad extendidas](https://www.microsoft.com/cloud-platform/extended-security-updates). 

## <a name="create-sql-server-registry"></a>Creación del registro de SQL Server
Para registrar las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] habilitadas para las ESU, primero debe crear el registro de SQL Server en Azure Portal. 

> [!IMPORTANT]
> No es necesario registrar instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para ESU cuando se ejecuta una máquina virtual de Azure que está configurada para [actualizaciones automáticas](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-patching). 

Para crear el registro de SQL Server, siga estos pasos:

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com). 
1. Seleccione la opción para **crear un recurso**. 
1. Escriba `SQL Server registry` en el cuadro de búsqueda.  
1. Elija la opción **Registro de SQL Server** publicada por [!INCLUDE[msCoName](../../includes/msconame-md.md)] y, luego, seleccione **Crear**. 

   ![Captura de pantalla de Azure Portal que muestra cómo crear un registro de SQL Server.](media/sql-server-extended-security-updates/sql-server-registry-service.png)

1. En **Detalles del proyecto**, elija su suscripción en el menú desplegable. Luego, elija un **grupo de recursos** existente o seleccione **Crear nuevo** para crear un grupo de recursos para el servicio del registro de SQL Server nuevo. 
1. En **Detalles del servicio**, especifique un nombre y una región para el recurso del **registro de SQL Server** nuevo: 

   ![Captura de pantalla del registro de SQL Server que muestra la pestaña Conceptos básicos.](media/sql-server-extended-security-updates/create-new-sql-server-registry.png)

1. Seleccione **Revisar y crear** para revisar los detalles del **registro de SQL Server**. Seleccione **Crear** una vez que se pase la validación. 

## <a name="register-instances-for-esus"></a>Registro de instancias para las Actualizaciones de seguridad extendidas

Una vez implementado el recurso del **registro de SQL Server**, puede elegir registrar una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [única](#single-sql-server-instance), o bien puede registrar varias instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de manera [masiva](#multiple-sql-server-instances-in-bulk). Es necesario que haya al menos una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] registrada en el ámbito del registro de SQL Server para poder descargar cualquier paquete de ESU. 

### <a name="single-sql-server-instance"></a>Instancia de SQL Server única

Para registrar una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] única, siga estos pasos:

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com). 
1. Vaya al recurso del **registro de SQL Server**. 
1. Seleccione **+ Registrar** en el panel de **información general**: 

   ![Elegir Registrar para registrar una instancia única de SQL Server](media/sql-server-extended-security-updates/register-single-sql-server-instance.png)

1. Proporcione la información necesaria tal como se detalla en esta tabla y, luego, seleccione **Registrar**: 

   |**Valor**| **Descripción**|
   | :-------| :------------- |
   | **Instancia** | Escriba la salida del comando `SELECT @@SERVERNAME`, como `MyServer\Instance01`. | 
   | **Versión de SQL** | Seleccione 2008 o 2008 R2 en la lista desplegable. | 
   | **Edición** | Seleccione la edición correspondiente en la lista desplegable: Datacenter, Developer (gratis para implementar si se compraron actualizaciones de seguridad extendidas), Enterprise, Standard, Web, Workgroup. | 
   | **Núcleos** | Escriba el número de núcleos para esta instancia | 
   | **Tipo de host** | Seleccione el tipo de host correspondiente en la lista desplegable: Máquina virtual (en el entorno local), servidor físico (en el entorno local), máquina virtual de Azure, Amazon EC2, Google Compute Engine, etc. |
   | **SubscriptionID**<sup>1</sup> | Escriba el valor de SubscriptionID donde se creó la máquina virtual.  |
   | **Grupo de recursos**<sup>1</sup> | Escriba el grupo de recursos donde se creó la máquina virtual.  | 
   | **Nombre de la VM de Azure**<sup>1</sup>  | Escriba el nombre del recurso de VM.  | 
   | **Sistema operativo de la máquina virtual de Azure**<sup>1</sup> | Seleccione la versión del sistema operativo Windows Server correspondiente en el menú desplegable. | 

   <sup>1</sup> Solo es necesario para las máquinas virtuales de Azure. 

La instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recién registrada ahora está visible en la sección **Register SQL Server instances** (Registrar instancias de SQL Server) del panel de **información general**: 

![Instancias de SQL Server registradas](media/sql-server-extended-security-updates/registered-sql-instance.png)

Una vez que se registra una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la sección **Actualizaciones de seguridad** aparece disponible. Las ESU disponibles aparecerán publicadas aquí. 

### <a name="multiple-sql-server-instances-in-bulk"></a>Instancias de SQL Server múltiples en masa

Las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] múltiples se pueden registrar de manera masiva mediante la carga de un archivo .csv. Una vez que el [archivo .csv tenga el formato correcto](#formatting-requirements-for-csv-file), puede seguir estos pasos para registrar de manera masiva las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con el recurso del registro de SQL Server: 

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com). 
1. Vaya al recurso del **registro de SQL Server**. 
1. Seleccione **Bulk Register** (Registro masivo) en el panel de **información general**:  

   ![Elija el registro masivo para registrar varias instancias de SQL Server](media/sql-server-extended-security-updates/bulk-register-sql-server-instances.png)

1. Seleccione el icono de archivo para ir a la ubicación del archivo .csv. Seleccione el archivo .csv. A continuación, seleccione **Registrar** para cargar el archivo y registrar varias instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 

   ![Cargue el archivo .csv para registrar varias instancias de SQL Server](media/sql-server-extended-security-updates/upload-csv-file-for-bulk-registration.png)

### <a name="formatting-requirements-for-csv-file"></a>Requisitos de formato para el archivo .csv
- Los valores se separan por comas
- Los valores no están entre comillas simples o dobles
- Los nombres de columnas no distinguen mayúsculas de minúsculas, pero se les debe **asignar nombres** como se indica a continuación: 
  - name
  - version
  - edition
  - cores
  - hostType
  - subscriptionID<sup>1</sup>
  - resourceGroup<sup>1</sup>
  - azureVmName<sup>1</sup>
  - AzureVmOS<sup>1</sup>

<sup>1</sup> Solo es necesario para las máquinas virtuales de Azure. 

#### <a name="csv-example-1---on-premises"></a>Ejemplo de CSV 1: en el entorno local

En el caso de las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el entorno local, el archivo .csv debe tener el aspecto siguiente: 

```csv
name,version,edition,cores,hostType
Server1\SQL2008,2008,Enterprise,12,Physical Server
Server1\SQL2008R2,2008 R2,Enterprise,12,Physical Server
Server2\SQL2008R2,2008 R2,Enterprise,24,Physical Server
Server3\SQL2008R2,2008 R2,Enterprise,12,Virtual Machine
Server4\SQL2008,2008,Developer,8,Physical Server  
```

Consulte [MyPhysicalServers.csv](https://github.com/microsoft/sql-server-samples/blob/master/samples/manage/sql-server-extended-security-updates/scripts/MyPhysicalServers.csv) para ver un ejemplo de archivo .csv.

#### <a name="csv-example-2---azure-vm"></a>Ejemplo de CSV 2: máquina virtual de Azure

En el caso de las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de una máquina virtual de Azure, el archivo .csv debe tener el aspecto siguiente: 

```csv
name,version,edition,cores,hostType,subscriptionId,resourceGroup,azureVmName,azureVmOS    
ProdServerUS1\SQL01,2008 R2,Enterprise,12,Azure Virtual Machine,61868ab8-16d4-44ec-a9ff-f35d05922847,RG,VM1,2012    
ProdServerUS1\SQL02,2008 R2,Enterprise,24,Azure Virtual Machine,61868ab8-16d4-44ec-a9ff-f35d05922847,RG,VM1,2012    
ServerUS2\SQL01,2008,Enterprise,12,Azure Virtual Machine,61868ab8-16d4-44ec-a9ff-f35d05922847,RG,VM2,2012 R2    
ServerUS2\SQL02,2008,Enterprise,8,Azure Virtual Machine,61868ab8-16d4-44ec-a9ff-f35d05922847,RG,VM2,2012 R2    
SalesServer\SQLProdSales,2008 R2,Developer,8,Azure Virtual Machine,61868ab8-16d4-44ec-a9ff-f35d05922847,RG,VM3,2008 R2  
```

Consulte [MyAzureVMs.csv](https://github.com/microsoft/sql-server-samples/blob/master/samples/manage/sql-server-extended-security-updates/scripts/MyAzureVMs.csv) para ver un ejemplo de archivo .csv con una máquina virtual de Azure como destino. 


> [!TIP]
> Para scripts de ejemplo de PowerShell y [!INCLUDE[tsql](../../includes/tsql-md.md)] que pueden generar la información de registro de instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se necesita en un archivo .csv, consulte los [ejemplos de scripts de registro de ESU](https://github.com/microsoft/sql-server-samples/blob/master/samples/manage/sql-server-extended-security-updates/scripts.md). 

## <a name="download-esus"></a>Descarga de Actualizaciones de seguridad extendidas

Una vez que las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se han registrado con el servicio de registro de SQL Server, puede descargar los paquetes de Actualización de seguridad extendida mediante el vínculo que se encuentra en Azure Portal, siempre que estén disponibles. 

Para descargar las Actualizaciones de seguridad extendidas, siga estos pasos: 

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com). 
1. Vaya al recurso del **registro de SQL Server**. 
1. Seleccione **Actualizaciones de seguridad** en el panel de navegación. 

   ![Compruebe si hay actualizaciones disponibles en el panel de actualizaciones de seguridad](media/sql-server-extended-security-updates/security-updates-sql-registry.png)

1. Descargue las actualizaciones de seguridad desde aquí, siempre que estén disponibles. 

## <a name="supported-regions-and-data-residency"></a>Regiones admitidas y residencia de datos

El servicio **Registro de SQL Server** (versión preliminar) está disponible en un subconjunto de las regiones de Azure. En la tabla siguiente se muestran las regiones admitidas y el tipo de residencia de datos en cada una de ellas.

| **Región** | **Residencia de datos** |
|:--|:--|
|Este de Australia|Geoárea|
|Sudeste de Australia|Geoárea|
|Centro de Canadá|Geoárea|
|Centro de Francia|Geoárea|
|Japón Oriental|Geoárea|
|Japón Occidental|Geoárea|
|Centro de Corea del Sur|Geoárea|
|Corea del Sur|Geoárea|
|Centro-Norte de EE. UU|Geoárea|
|Norte de Europa|Geoárea|
|Centro-sur de EE. UU.|Geoárea|
|Sudeste de Asia|Región única|
|Sur de la India|Geoárea|
|Norte de Sudáfrica|Geoárea|
|Sur de Reino Unido|Geoárea|
|Oeste de Reino Unido|Geoárea|
|Oeste de EE. UU.|Geoárea|
|Este de EE. UU.|Geoárea|
|Centro de EE. UU.|Geoárea|
|Este de Asia|Geoárea|
|Oeste de Europa|Geoárea|
|Centro-Oeste de EE. UU.|Geoárea|
|Oeste de EE. UU. 2|Geoárea|
|Este de EE. UU. 2|Geoárea|

En las regiones con residencia geográfica, el servicio Registro de SQL mantiene las copias de seguridad de los datos en una cuenta de almacenamiento con redundancia geográfica (GRS).  En las regiones con residencia de una sola región, el servicio Registro de SQL mantiene las copias de seguridad de los datos en una cuenta de almacenamiento con redundancia de zona (ZRS). Para más información, consulte el [Centro de confianza](https://azuredatacentermap.azurewebsites.net/).

## <a name="configure-regional-redundancy"></a>Configuración de la redundancia regional 

Los clientes que necesiten redundancia regional para su **registro de SQL Server** pueden crear datos de registro en dos regiones distintas. Después, los clientes pueden descargar actualizaciones de seguridad desde cualquier región en función de la disponibilidad del servicio de **registro de SQL Server**. 

En cuanto a la redundancia regional, el servicio de **registro de SQL Server** debe crearse en dos regiones diferentes, y el inventario de SQL Server debe dividirse entre estos dos servicios. De este modo, la mitad de los servidores SQL Server se registran en el servicio de registro de una región y, a continuación, la otra mitad de los servidores SQL Server se registran en el servicio de registro de la otra región. 

Para configurar la redundancia regional, siga estos pasos:

1. Divida el inventario de SQL Server 2008 o 2008 R2 en dos archivos, como upload1.csv y upload2.csv. 
  
   :::image type="content" source="media/sql-server-extended-security-updates/two-upload-files-for-regional-redundancy.png" alt-text="Archivos de carga de ejemplo":::

1. Cree el primer servicio de **registro de SQL Server** en una región y, a continuación, registre de forma masiva uno de los archivos CSV en él. Por ejemplo, cree el primer servicio de **registro de SQL Server** en la región **Oeste de EE. UU.** y registre de forma masiva los servidores SQL Server mediante el archivo upload1.csv. 
1. Cree el segundo servicio de **registro de SQL Server** en la segunda región y, a continuación, registre de forma masiva el otro archivo csv. Por ejemplo, cree el segundo servicio de **registro de SQL Server** en la región **Este de EE. UU.** y registre de forma masiva los servidores SQL Server con el archivo upload2.csv. 


Una vez que los datos se han registrado con dos diferentes recursos de **registro de SQL Server**, podrá descargar actualizaciones de seguridad desde cualquiera de las regiones, en función de la disponibilidad del servicio. 


## <a name="faq"></a>Preguntas más frecuentes

Las preguntas más frecuentes sobre las Actualizaciones de seguridad extendidas se pueden encontrar en las [preguntas más frecuentes sobre las Actualizaciones de seguridad extendidas](https://www.microsoft.com/cloud-platform/extended-security-updates). A continuación, aparecen las preguntas más frecuentes específicas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 

**¿Cuándo finalizó el soporte técnico para SQL Server 2008 y 2008 R2?**

La fecha de fin del soporte técnico de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] y [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] fue el 9 de julio de 2019. 

**¿Qué significa que haya finalizado el soporte técnico?**

La directiva del ciclo de vida ofrece 10 años de soporte técnico (5 años para soporte técnico estándar y 5 años para soporte técnico extendido) para productos de las ediciones Business y Developer (como [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y Windows Server). Según la directiva, una vez finalizado el período de soporte técnico extendido, no habrá revisiones ni actualizaciones de seguridad, lo que puede generar problemas de seguridad y cumplimiento, además de exponer las aplicaciones y el negocio de los clientes a riesgos de seguridad graves.

**¿Qué ediciones de SQL Server son elegibles para las Actualizaciones de seguridad extendidas?**

Las ediciones Enterprise, Datacenter, Standard, Web y Workgroup de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] y [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] son elegibles para las Actualizaciones de seguridad tanto para la versión x86 como x64. 

**¿Cuándo estará disponible la oferta de Actualizaciones de seguridad extendidas?**

Las Actualizaciones de seguridad extendidas ahora están disponibles para su compra y se pueden pedir desde [!INCLUDE[msCoName](../../includes/msconame-md.md)] o desde un asociado de licencia de [!INCLUDE[msCoName](../../includes/msconame-md.md)]. La entrega de las Actualizaciones de seguridad extendidas comenzará después de las fechas de finalización del soporte técnico, siempre que estén disponibles. Los clientes interesados en migrar a Azure pueden hacerlo de inmediato. 

**¿Qué incluyen las Actualizaciones de seguridad extendidas?** 

Las Actualizaciones de seguridad extendidas incluyen el aprovisionamiento de boletines y actualizaciones de seguridad clasificadas como **críticas** por el [Centro de respuestas de seguridad de Microsoft (MSRC)](https://portal.msrc.microsoft.com/), por un máximo de tres años a contr del 9 de julio de 2019. Las Actualizaciones de seguridad extendidas se distribuirán siempre que estén disponibles. Las Actualizaciones de seguridad extendidas no incluyen soporte técnico, pero puede usar otros planes de soporte técnico de [!INCLUDE[msCoName](../../includes/msconame-md.md)] para obtener ayuda con sus preguntas sobre [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] y [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] con respecto a las cargas de trabajo que se incluyen en las Actualizaciones de seguridad extendidas. Las Actualizaciones de seguridad extendidas no incluyen nuevas características, mejoras funcionales ni correcciones solicitadas por el cliente. Sin embargo, [!INCLUDE[msCoName](../../includes/msconame-md.md)] puede incluir correcciones que no sean de seguridad según sea necesario.

**¿Por qué las Actualizaciones de seguridad extendidas para SQL Server 2008 y 2008 R2 solo ofrecen actualizaciones "críticas"?**

En el caso de los eventos de Fin del soporte técnico del pasado, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] solo proporcionaba actualizaciones de seguridad críticas, lo que cumple con los criterios de cumplimiento de nuestros clientes empresariales. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no envía una actualización de seguridad mensual general. [!INCLUDE[msCoName](../../includes/msconame-md.md)] solo proporciona actualizaciones de seguridad (GDR) de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a petición para los boletines de MSRC donde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se identifica como un producto afectado.
En caso de que haya situaciones donde no se proporcionen actualizaciones importantes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nuevas y que el cliente considere críticas, pero MSRC no, trabajaremos en conjunto con el cliente para sugerir una mitigación correspondiente caso a caso.

**¿Qué programas de licencias son elegibles para las Actualizaciones de seguridad extendidas?**

Los clientes de Software Assurance pueden adquirir Actualizaciones de seguridad extendidas en el entorno local en virtud de un Contrato Enterprise (EA), un Contrato Enterprise Subscription (SCE) o una Inscripción para soluciones de educación (EES). No es necesario que Software Assurance esté en la misma inscripción.

**¿Los clientes de SQL Server deben ejecutar el Service Pack más actual para beneficiarse de las Actualizaciones de seguridad extendidas?**

Sí, los clientes deben ejecutar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Windows Server 2008 y 2008 R2 con el Service Pack más reciente para aplicar las Actualizaciones de seguridad extendidas. [!INCLUDE[msCoName](../../includes/msconame-md.md)] solo generará actualizaciones que se pueden aplicar en el Service Pack más reciente.

**¿Cuáles son las opciones para los clientes de SQL Server sin Software Assurance?** 

En el caso de los clientes que no tienen Software Assurance, la opción alternativa para acceder a las Actualizaciones de seguridad extendidas es migrar a Azure. En el caso de las cargas de trabajo variables, se recomienda que los clientes migren a Azure a través de Pago por uso, lo que les permite escalar o reducir verticalmente en cualquier momento. En el caso de las cargas de trabajo predecibles, se recomienda que los clientes migren a Azure a través de la suscripción de servidor y las instancias reservadas.
  
**¿Esta oferta también se aplica a SQL Server 2005?**

No. Para estas versiones anteriores, se recomienda actualizar a las versiones más recientes, pero los clientes podrían actualizar a las versiones [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] o [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] para aprovechar esta oferta.

**¿Puedo implementar una instancia de SQL Server 2008 o 2008 R2 nueva en Azure y seguir teniendo las Actualizaciones de seguridad extendidas?**

Sí, los clientes pueden iniciar una nueva instancia de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] o [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] en una máquina virtual de Azure y tener acceso a las Actualizaciones de seguridad extendidas.

**¿Puedo obtener soporte técnico local para SQL Server 2008 o 2008 R2 después de la fecha de Fin del soporte técnico, sin tener que comprar las Actualizaciones de seguridad extendidas?**

No. Si un cliente tiene [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] o [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] y elige permanecer en el entorno local durante una migración sin Actualizaciones de seguridad extendidas, no puede registrar una incidencia de soporte técnico, aunque tenga un plan de soporte técnico. Sin embargo, si migra a Azure, pueden obtener soporte técnico con su plan de soporte técnico de Azure.

**Si un cliente de SQL Server 2008 y 2008 R2 quiere traer su propia licencia (BYOL), ¿debe tener cobertura de Software Assurance?**

Sí, los clientes deben tener Software Assurance para aprovechar el programa BYOL para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en Azure Virtual Machines como parte del programa Movilidad de licencias. En el caso de los clientes que no tienen Software Assurance, se recomienda que migren a Azure SQL Managed Instance para sus entornos de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]. Los clientes también pueden migrar a Azure Virtual Machines de pago por uso. Los clientes de Software Assurance que obtienen licencias de SQL por núcleo también tienen la opción de migrar a Azure mediante la Ventaja híbrida de Azure (AHB).

Azure SQL Managed Instance es un servicio de Azure que proporciona una compatibilidad casi del 100 % con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el entorno local. Instancia administrada proporciona funcionalidades integradas de alta disponibilidad y recuperación ante desastres, además de características de rendimiento inteligentes y la capacidad de escalar sobre la marcha. Instancia administrada también proporciona una experiencia sin versión que elimina la necesidad de actualizaciones y revisiones de seguridad manuales. Consulte la página de guía de precios de Azure para más información sobre el programa BYOL.

**¿Qué opciones tienen los clientes para ejecutar SQL Server en Azure?**

Los clientes pueden trasladar entornos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] heredados a Azure SQL Managed Instance, un servicio de plataforma de datos (PaaS) totalmente administrado que ofrece una opción "sin versión" para eliminar los problemas con las fechas de Fin del soporte técnico, o bien a Azure Virtual Machines acceder a las actualizaciones de seguridad. Las bases de datos migradas conservarán su compatibilidad con el sistema heredado. Para obtener más información, vea [Certificación de compatibilidad](../../database-engine/install-windows/compatibility-certification.md).

Las Actualizaciones de seguridad extendidas estarán disponibles para [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] y [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] en Azure Virtual Machines después de la fecha de Fin del soporte técnico del 9 de julio de 2019 durante los próximos tres años. En el caso de los clientes que quieren actualizar desde [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] y [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)], se admitirán todas las versiones posteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] hasta [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], es necesario que los clientes estén en el último Service Pack admitido. A partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], se recomienda que los clientes tengan la actualización acumulativa más reciente. Tenga en cuenta que los Service Pack no estarán disponibles a partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], solo las actualizaciones acumulativas y las versiones de distribución general (GDR).

Azure SQL Managed Instance es una opción de implementación de ámbito de instancia en [!INCLUDE[ssSDS](../../includes/sssds-md.md)] que proporciona la compatibilidad más amplia de motor de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y de red virtual nativa (VNET), de modo que puede migrar las bases de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a Instancia administrada sin cambiar las aplicaciones. Combina la amplia área expuesta de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con las ventajas operativas y financieras de un servicio inteligente totalmente administrado. Aproveche el nuevo Azure Database Migration Service para trasladar [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] y [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] a Azure SQL Managed Instance con pocos o ningún cambio en el código de la aplicación.

**¿Los clientes pueden aprovechar las Ventaja híbrida de Azure para las versiones SQL Server 2008 y 2008 R2?**

Sí, los clientes que tienen Software Assurance activo o suscripciones de servidor equivalentes pueden aprovechar la Ventaja híbrida de Azure con las inversiones de licencias locales existentes para obtener precios con descuento en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se ejecuta en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y Azure Virtual Machines.

**¿Los clientes pueden obtener las Actualizaciones de seguridad extendidas gratuitas en regiones de Azure Government?**

Sí, las Actualizaciones de seguridad extendidas estarán disponibles en Azure Virtual Machines en las regiones de Azure Government.

**¿Los clientes pueden obtener las Actualizaciones de seguridad extendidas gratuitas en Azure Stack?**

Sí, los clientes pueden migrar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y Windows Server 2008 y [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] a Azure Stack y recibir las Actualizaciones de seguridad extendidas sin costo adicional después de las fechas de Fin del soporte técnico.

**En el caso de los clientes con un clúster de SQL 2008 y 2008 R2 con almacenamiento compartido, ¿cuál es la guía para la migración a Azure?**

Actualmente, Azure no admite la agrupación en clústeres de almacenamiento compartido. Si quiere consejos sobre cómo configurar una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de alta disponibilidad en Azure, consulte la [guía de alta disponibilidad de SQL Server](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr).

**¿Los clientes pueden aprovechar las Actualizaciones de seguridad extendidas para SQL Server con un proveedor de hospedaje de terceros?**

Los clientes no pueden aprovechar las Actualizaciones de seguridad extendidas si mueven su entorno de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] a una implementación de PaaS en otros proveedores de nube. Si los clientes quieren migrar a máquinas virtuales (IaaS), pueden aprovechar Movilidad de licencias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a través de Software Assurance para realizar la migración y comprar las Actualizaciones de seguridad extendidas de [!INCLUDE[msCoName](../../includes/msconame-md.md)] para aplicar manualmente las revisiones a las instancias de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] que se ejecutan en una máquina virtual (IaaS) en un servidor de un proveedor de servicios de hosting de SPLA autorizado. Sin embargo, la oferta más atractiva son las actualizaciones gratuitas en Azure.

**¿Cuáles son los procedimientos recomendados para mejorar el rendimiento de SQL Server en las máquinas virtuales de Azure?** 

Si quiere consejos sobre cómo optimizar el rendimiento de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en máquinas virtuales de Azure, consulte la [guía de optimización de SQL Server](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-performance). 

## <a name="see-also"></a>Consulte también

- [Página del ciclo de vida de SQL Server 2008 / 2008 R2](https://support.microsoft.com/lifecycle/search?alpha=sql%20server%202008)
- [Página de Fin del soporte técnico de SQL Server 2008 / 2008 R2](./sql-server-end-of-life-overview.md?WT.mc_id=akamseos)
- [Preguntas más frecuentes sobre las Actualizaciones de seguridad extendidas](https://aka.ms/sqleosfaq)
- [Centro de respuestas de seguridad de Microsoft (MSRC)](https://portal.msrc.microsoft.com/security-guidance/summary)
- [Administración de las actualizaciones de Windows con Azure Automation](/azure/automation/update-management/overview)
- [Aplicación de revisiones automatizada de SQL Server](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-patching)
- [Guía de migración de datos de Microsoft](https://datamigration.microsoft.com/)
- [Azure Migrate: opciones de migración mediante lift-and-shift para migrar SQL Server 2008 / 2008 R2 actual a una máquina virtual de Azure](https://azure.microsoft.com/services/azure-migrate/)
- [Plataforma de adopción de la nube para la migración de SQL](/azure/cloud-adoption-framework/migrate/expanded-scope/sql-migration)
- [Scripts relacionados con las Actualizaciones de seguridad extendidas en GitHub](https://github.com/microsoft/sql-server-samples/tree/master/samples/manage/sql-server-extended-security-updates/scripts)