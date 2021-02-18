---
title: Controlador OLE DB para la compatibilidad de SQL Server con la alta disponibilidad y la recuperación ante desastres
description: Obtenga información sobre la compatibilidad de OLE DB Driver for SQL Server con la alta disponibilidad y recuperación ante desastres al conectarse a bases de datos con esas características configuradas.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d5adce5bcda14dd275791f055359654bc120e79d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347899"
---
# <a name="ole-db-driver-for-sql-server-support-for-high-availability-disaster-recovery"></a>Controlador OLE DB para la compatibilidad de SQL Server con la alta disponibilidad y la recuperación ante desastres
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  En este artículo se describe la compatibilidad de *OLE DB Driver for SQL Server* para [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Para más información sobre [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], vea [Agentes de escucha de grupo de disponibilidad, conectividad de cliente y conmutación por error de una aplicación &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md), [Creación y configuración de grupos de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md), [Clúster de conmutación por error y grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md) y [Secundarias activas: réplicas secundarias legibles &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).  
  
 Puede especificar el agente de escucha del grupo de disponibilidad de un determinado grupo de disponibilidad en la cadena de conexión. Si una aplicación de controlador OLE DB para SQL Server se conecta a una base de datos de un grupo de disponibilidad que conmuta por error, la conexión original se interrumpe y la aplicación debe abrir una nueva conexión para continuar el trabajo después de la conmutación por error.  
  
 Si no va a conectarse a una escucha de grupo de disponibilidad y varias direcciones IP están asociadas a un nombre de host, el controlador OLE DB para SQL Server iterará secuencialmente a través de todas las direcciones IP asociadas a la entrada DNS. Esto puede llevar mucho tiempo si la primera dirección IP que devuelve el servidor DNS no está enlazada a una tarjeta (NIC) de interfaz de red. Al conectarse a una escucha de grupo de disponibilidad, el controlador OLE DB para SQL Server intentará establecer conexiones con todas las direcciones IP en paralelo y, si un intento de conexión se realiza correctamente, el controlador descartará los intentos pendientes.  
  
> [!NOTE]  
> El aumento del tiempo de espera de la conexión y la implementación de la lógica de reintento de conexión aumentarán la probabilidad de que una aplicación se conecte a un grupo de disponibilidad. Además, dado que una conexión puede producir un error debido a la conmutación por error de un grupo de disponibilidad, es aconsejable implementar la lógica de reintento de conexión y hacer que una conexión que no se ha podido establecer se reintente hasta que vuelva a conectarse.  
  
## <a name="connecting-with-multisubnetfailover"></a>Conectarse a MultiSubnetFailover  
 Especifique siempre **MultiSubnetFailover=Yes** al conectarse a una escucha de grupo de disponibilidad de AlwaysOn de SQL Server o a una instancia de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. **MultiSubnetFailover** habilita una conmutación por error más rápida para todos los grupos de disponibilidad AlwaysOn y la instancia del clúster de conmutación por error en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y reducirá significativamente el tiempo de la conmutación por error en las topologías AlwaysOn únicas y de varias subredes. En un clúster de conmutación por error de varias subredes, el cliente intentará conexiones en paralelo. Durante una conmutación por error de subred, OLE DB Driver for SQL Server intentará la conexión TCP.  
  
 La propiedad de conexión **MultiSubnetFailover** indica que la aplicación se está implementando en un grupo de disponibilidad o una instancia de clúster de conmutación por error y que [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] intentará conectarse a la base de datos en la instancia principal del controlador OLE DB para SQL Server mediante un intento de conexión a todas las direcciones IP. Cuando se especifica **MultiSubnetFailover=Yes** para una conexión, el cliente reintenta la conexión TCP más deprisa que los intervalos de retransmisión TCP predeterminados del sistema operativo. Esto permite una reconexión más rápida después de la conmutación por error de un grupo de disponibilidad AlwaysOn o una instancia de clúster de conmutación por error AlwaysOn, y es aplicable a instancias de clúster de conmutación por error y grupos de disponibilidad de una y varias subredes.  
  
 Para más información sobre las palabras clave de cadena de conexión, vea [Uso de palabras clave de cadena de conexión con el controlador OLE DB para SQL Server](../../oledb/applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md).  
  
 Si se especifica **MultiSubnetFailover=Yes** al conectarse a algo que no sea un agente de escucha de grupo de disponibilidad o una instancia de clúster de conmutación por error, el rendimiento puede verse afectado negativamente y, por ello, no se admite.  
  
 Utilice las siguientes instrucciones para conectarse a un servidor en un grupo de disponibilidad o una instancia de clúster de conmutación por error:  
  
-   Use la propiedad de conexión **MultiSubnetFailover** cuando se conecte a una única subred o a varias subredes, ya que mejorará el rendimiento en ambos casos.  
  
-   Para conectarse a un grupo de disponibilidad, especifique el agente de escucha del grupo de disponibilidad como el servidor en la cadena de conexión.  
  
-   La conexión a una instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] configurada con más de 64 direcciones IP producirá un error en la conexión.  
  
-   El comportamiento de una aplicación que usa la propiedad de conexión **MultiSubnetFailover** no se ve afectado por el tipo de autenticación: autenticación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], autenticación Kerberos o autenticación de Windows.  
  
-   Puede aumentar el valor de **loginTimeout** para tener en cuenta el tiempo de conmutación por error y reducir los reintentos de conexión de la aplicación.  
  
-   No se admiten las transacciones distribuidas.  
  
Si no está activado el enrutamiento de solo lectura, no se podrá conectar a una ubicación de réplica secundaria en un grupo de disponibilidad en las siguientes situaciones:  
  
1.  Si la ubicación de réplica secundaria no está configurada para aceptar conexiones.  
  
2.  Si una aplicación usa **ApplicationIntent=ReadWrite** (se describe a continuación) y la ubicación de réplica secundaria está configurada para acceso de solo lectura.  
  
Una conexión producirá un error si una réplica principal está configurada para rechazar cargas de trabajo de solo lectura y la cadena de conexión contiene **ApplicationIntent=ReadOnly**.  
  
## <a name="upgrading-to-use-multi-subnet-clusters-from-database-mirroring"></a>Actualizar para utilizar clústeres de varias subredes a partir de la creación de reflejo de la base de datos  
Se producirá un error de conexión si las palabras clave de conexión **MultiSubnetFailover** y **Failover_Partner** se encuentran en la cadena de conexión. También se producirá un error si se usa **MultiSubnetFailover** y [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] devuelve una respuesta del asociado de conmutación por error que indica que forma parte de un par de creación de reflejo de la base de datos.  
  
Si actualiza una aplicación de controlador OLE DB para SQL Server que actualmente use la creación de reflejo de la base de datos en un escenario de varias subredes, debe quitar la propiedad de conexión **Failover_Partner** y reemplazarla por **MultiSubnetFailover** establecida en **Yes**, así como reemplazar el nombre del servidor en la cadena de conexión por una escucha de grupo de disponibilidad. Si usa una cadena de conexión **Failover_Partner** y **MultiSubnetFailover=Yes**, el controlador generará un error. En cambio, si usa una cadena de conexión **Failover_Partner** y **MultiSubnetFailover=No** (o **ApplicationIntent=ReadWrite**), la aplicación usará la creación de reflejo de la base de datos.  
  
El controlador devuelve un error si la creación de reflejo de la base de datos se usa en la base de datos principal del grupo de disponibilidad y si **MultiSubnetFailover=Yes** se usa en la cadena de conexión que se conecta a una base de datos principal en lugar de a un agente de escucha de grupo de disponibilidad.  


[!INCLUDE[specify-application-intent_read-only-routing](~/includes/paragraph-content/specify-application-intent-read-only-routing.md)]


## <a name="ole-db"></a>OLE DB  
OLE DB Driver for SQL Server admite las palabras clave **ApplicationIntent** y **MultiSubnetFailover**.   
  
Las dos palabras clave de cadena de conexión OLE DB se agregaron para admitir [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] en OLE DB Driver for SQL Server:  
  
-   **ApplicationIntent** 
-   **MultiSubnetFailover**  
  
 Para más información sobre las palabras clave de cadena de conexión en el controlador OLE DB, vea [Uso de palabras clave de cadena de conexión con el controlador OLE DB para SQL Server](../../oledb/applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md).  

### <a name="application-intent"></a>Intención de aplicaciones 

Las propiedades de conexión equivalentes son:  
  
-   **SSPROP_INIT_APPLICATIONINTENT**  
  
-   **DBPROP_INIT_PROVIDERSTRING**  
  
Una aplicación de OLE DB Driver for SQL Server puede utilizar uno de los métodos siguientes para especificar la intención de aplicaciones:  
  
 -   **IDBInitialize::Initialize**  
 **IDBInitialize::Initialize** usa el conjunto de propiedades previamente configurado para inicializar el origen de datos y crear el objeto de origen de datos. Especifique la intención de aplicaciones como una propiedad del proveedor o como parte de la cadena de propiedades extendidas.  
  
 -   **IDataInitialize::GetDataSource**  
 **IDataInitialize::GetDataSource** toma una cadena de conexión de entrada que puede contener la palabra clave **Application Intent**.  
  
 -   **IDBProperties::SetProperties**  
 Para establecer el valor de la propiedad **ApplicationIntent**, llame a **IDBProperties::SetProperties** pasando la propiedad **SSPROP_INIT_APPLICATIONINTENT** con valor "**ReadWrite**" o "**ReadOnly**" o la propiedad **DBPROP_INIT_PROVIDERSTRING** con el valor que contiene "**ApplicationIntent=ReadOnly**" o "**ApplicationIntent=ReadWrite**".  
  
Puede especificar la intención de aplicaciones en el campo Propiedades de intención de aplicaciones de la pestaña Todo del cuadro de diálogo **Propiedades de vínculo de datos**.  
  
Cuando se establezcan conexiones implícitas, estas usarán la configuración de la intención de aplicaciones de la conexión primaria. De forma similar, cuando se creen varias sesiones con el mismo origen de datos estas heredarán la configuración de la intención de aplicaciones del origen de datos.  
  
### <a name="multisubnetfailover"></a>MultiSubnetFailover

Las propiedades de conexión equivalentes son:  
  
-   **SSPROP_INIT_MULTISUBNETFAILOVER**  
  
-   **DBPROP_INIT_PROVIDERSTRING**  

Una aplicación de OLE DB Driver for SQL Server puede usar uno de los métodos siguientes para establecer la opción MultiSubnetFailover:  

 -   **IDBInitialize::Initialize**  
 **IDBInitialize::Initialize** usa el conjunto de propiedades previamente configurado para inicializar el origen de datos y crear el objeto de origen de datos. Especifique la intención de aplicaciones como una propiedad del proveedor o como parte de la cadena de propiedades extendidas.  
  
 -   **IDataInitialize::GetDataSource**  
 **IDataInitialize::GetDataSource** toma una cadena de conexión de entrada que puede contener la palabra clave **MultiSubnetFailover**.  

-   **IDBProperties::SetProperties**  
Para establecer el valor de propiedad **MultiSubnetFailover**, llame a **IDBProperties::SetProperties** pasando la propiedad **SSPROP_INIT_MULTISUBNETFAILOVER** con el valor **VARIANT_TRUE** o **VARIANT_FALSE** o la propiedad **DBPROP_INIT_PROVIDERSTRING** con el valor que contiene "**MultiSubnetFailover=Yes**" o "**MultiSubnetFailover=No**".

#### <a name="example"></a>Ejemplo

```
DBPROP rgPropMultisubnet;

rgPropMultisubnet.dwPropertyID = SSPROP_INIT_MULTISUBNETFAILOVER;
rgPropMultisubnet.dwOptions = DBPROPOPTIONS_REQUIRED;
rgPropMultisubnet.dwStatus = DBPROPSTATUS_OK;
rgPropMultisubnet.colid = DB_NULLID;
V_VT(&(rgPropMultisubnet.vValue)) = VT_BOOL;
V_BOOL(&(rgPropMultisubnet.vValue)) = VARIANT_TRUE;

DBPROPSET PropSet;

PropSet.rgProperties = &rgPropMultisubnet;
PropSet.cProperties = 1;
PropSet.guidPropertySet = DBPROPSET_SQLSERVERDBINIT;
IDBProperties* pIDBProperties = NULL;
hr = pIDBInitialize->QueryInterface(IID_IDBProperties, (void **)&pIDBProperties);
pIDBProperties->SetProperties(1, &PropSet);
```

## <a name="see-also"></a>Consulte también  
 [Características del controlador OLE DB para SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md)    
 [Uso de palabras clave de cadena de conexión con el controlador OLE DB para SQL Server](../../oledb/applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md)  
  
  
