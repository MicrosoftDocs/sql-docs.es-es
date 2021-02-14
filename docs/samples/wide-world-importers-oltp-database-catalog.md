---
title: Catálogo de base de datos OLTP de WideWorldImporters-SQL | Microsoft Docs
description: Comprenda los esquemas, las tablas, los procedimientos almacenados y las consideraciones de diseño para el catálogo de base de datos de ejemplo WideWorldImporters.
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.custom: ''
ms.date: 04/04/2018
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 991f001ea8fd43298cefe1f9ba542472de6935ef
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354094"
---
# <a name="wideworldimporters-database-catalog"></a>Catálogo de base de datos WideWorldImporters
[!INCLUDE [SQL Server Azure SQL Database](../includes/applies-to-version/sql-asdb.md)]
La base de datos WideWorldImporters contiene toda la información de la transacción y los datos diarios de ventas y compras, así como datos del sensor de vehículos y salones fríos.

## <a name="schemas"></a>Esquemas

WideWorldImporters usa esquemas para distintos propósitos, como el almacenamiento de datos, la definición de cómo los usuarios pueden tener acceso a los datos y el suministro de objetos para el desarrollo y la integración de almacenamiento de datos.

### <a name="data-schemas"></a>Esquemas de datos

Estos esquemas contienen los datos. Todos los demás esquemas necesitan varias tablas y se encuentran en el esquema de la aplicación.

|Schema|Descripción|
|-----------------------------|---------------------|
|Application|Usuarios, contactos y parámetros de toda la aplicación. También contiene tablas de referencia con datos que usan varios esquemas.|
|Compra|Compras de artículos bursátiles de proveedores y detalles acerca de los proveedores.|  
|Sales|Ventas de productos bursátiles a clientes de venta directa y detalles sobre clientes y vendedores. |  
|Warehouse|Inventario y transacciones de artículos de existencias.|  

### <a name="secure-access-schemas"></a>Esquemas de acceso seguro

Estos esquemas se usan para las aplicaciones externas que no tienen permiso de acceso directo a las tablas de datos. Contienen vistas y procedimientos almacenados que usan las aplicaciones externas.

|Schema|Descripción|
|-----------------------------|---------------------|
|Sitio web|Todo el acceso a la base de datos desde el sitio web de la empresa se realiza a través de este esquema.|
|Informes|Todo el acceso a la base de datos desde Reporting Services informes se realiza a través de este esquema.|
|PowerBI|Todo el acceso a la base de datos desde los paneles de Power BI a través de la puerta de enlace empresarial se realiza a través de este esquema.|

Tenga en cuenta que los informes y los esquemas de PowerBI no se utilizan en la versión inicial de la base de datos de ejemplo. Sin embargo, se recomienda que todos los ejemplos de Reporting Services y Power BI creados sobre esta base de datos usen estos esquemas.

### <a name="development-schemas"></a>Esquemas de desarrollo

Esquemas de propósito especial

|Schema|Descripción|
|-----------------------------|---------------------|
|Integración|Objetos y procedimientos necesarios para la integración del almacenamiento de datos (es decir, la migración de los datos a la base de datos WideWorldImportersDW).|
|Secuencias|Contiene secuencias utilizadas por todas las tablas de la aplicación.|

## <a name="tables"></a>Tablas

Todas las tablas de la base de datos están en los esquemas de datos.

### <a name="application-schema"></a>Esquema de aplicación

Detalles de los parámetros y las personas (usuarios y contactos), junto con las tablas de referencia comunes (comunes a varios esquemas adicionales).

|Tabla|Descripción|
|-----------------------------|---------------------|
|SystemParameters|Contiene parámetros configurables en todo el sistema.|
|Personas|Contiene los nombres de usuario, la información de contacto, para todos los usuarios que usan la aplicación y para las personas con las que se ocupan Wide World Importers en organizaciones de clientes. Esto incluye personal, clientes, proveedores y otros contactos. En el caso de las personas a las que se les ha concedido permiso para usar el sistema o el sitio web, la información incluye los detalles de inicio de sesión.|
|Cities (Ciudades)|Hay muchas direcciones almacenadas en el sistema para las personas, las direcciones de entrega de la organización del cliente, las direcciones de recogida en los proveedores, etc. Siempre que se almacena una dirección, hay una referencia a una ciudad en esta tabla. También hay una ubicación espacial para cada ciudad.|
|StateProvinces|Las ciudades forman parte de Estados o provincias. En esta tabla se detallan los detalles, incluidos los datos espaciales que describen los límites de cada Estado o provincia.|
|Países|Los Estados o provincias forman parte de los países. En esta tabla se detallan los detalles, incluidos los datos espaciales que describen los límites de cada país.|
|DeliveryMethods|Opciones para entregar elementos de existencias (por ejemplo, camión/furgoneta, publicación, recogida, Courier, etc.)|
|PaymentMethods|Opciones para realizar pagos (p. ej., efectivo, cheque, EFT, etc.)|
|TransactionTypes|Tipos de transacciones de clientes, proveedores o acciones (por ejemplo, factura, nota de crédito, etc.)|

### <a name="purchasing-schema"></a>Esquema de compra

Detalles de los proveedores y de compras de productos bursátiles.

|Tabla|Descripción|
|-----------------------------|---------------------|
|Suppliers|Tabla de entidades principales para proveedores (organizaciones)|
|SupplierCategories|Categorías para proveedores (por ejemplo, Novelties, juguetes, ropa, empaquetado, etc.)|
|SupplierTransactions|Todas las transacciones financieras que están relacionadas con el proveedor (facturas, pagos)|
|PurchaseOrders|Detalles de los pedidos de compra del proveedor|
|PurchaseOrderLines|Líneas de detalle de los pedidos de compra del proveedor|

 
### <a name="sales-schema"></a>Esquema de ventas

Detalles de clientes, vendedores y de ventas de productos de acciones.

|Tabla|Descripción|
|-----------------------------|---------------------|
|Clientes|Tablas de entidad principales para clientes (organizaciones o individuos)|
|CustomerCategories|Categorías para clientes (por ej., tiendas de novedad, supermercados, etc.)|
|BuyingGroups|Las organizaciones de clientes pueden formar parte de los grupos que ejercen mayor potencia de compra|
|CustomerTransactions|Todas las transacciones financieras relacionadas con el cliente (facturas, pagos)|
|SpecialDeals|Precios especiales. Esto puede incluir precios fijos, descuento en dólares o porcentaje de descuento.|
|Orders (Pedidos)|Detalles de los pedidos de clientes|
|OrderLines|Líneas de detalle de los pedidos de cliente|
|Facturas|Detalles de las facturas de los clientes|
|InvoiceLines|Líneas de detalle de las facturas de los clientes|

### <a name="warehouse-schema"></a>Esquema de almacén

Detalles de los artículos de existencias, sus contenidas y las transacciones.

|Tabla|Descripción|
|-----------------------------|---------------------|
|StockItems|Tabla de entidades principales para los elementos de stock|
|StockItemHoldings|Columnas no temporales para los elementos de stock. Estas columnas se actualizan con frecuencia.|
|StockGroups|Grupos para clasificar los elementos de existencias (por ejemplo, Novelties, juguetes, Novelties comestibles, etc.)|
|StockItemStockGroups|Qué elementos de existencias se encuentran en los grupos de existencias (varios a varios)|
|Colores|Los elementos de existencias pueden (opcionalmente) tener colores|
|PackageTypes|Formas en que se pueden empaquetar los elementos de existencias (por ejemplo, caja, cartón, pallet, kg, etc.).|
|StockItemTransactions|Transacciones que cubren todos los movimientos de todos los elementos de existencias (recepción, venta, escritura)|
|VehicleTemperatures|Temperaturas registradas periódicamente de refrigerantes de vehículos|
|ColdRoomTemperatures|Temperaturas registradas periódicamente de refrigerantes de la habitación fría|


## <a name="design-considerations"></a>Consideraciones de diseño

El diseño de la base de datos es subjetivo y no hay ninguna manera correcta o incorrecta de diseñar una base de datos. Los esquemas y las tablas de esta base de datos muestran ideas sobre cómo puede diseñar su propia base de datos.

### <a name="schema-design"></a>Diseño de esquema

WideWorldImporters usa un pequeño número de esquemas para que resulte fácil entender el sistema de base de datos y demostrar los principios de la base de datos.  

Siempre que sea posible, la base de datos coloca tablas que se consultan normalmente en el mismo esquema para minimizar la complejidad de la combinación.

El esquema de la base de datos se ha generado por código en función de una serie de tablas de metadatos en otra base de datos WWI_Preparation. Esto proporciona a WideWorldImporters un alto grado de coherencia de diseño, coherencia de nomenclatura y integridad. Para más información sobre cómo se ha generado el esquema, vea el código fuente: [Wide-World-Importers/WWI-Database-scripts](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/wide-world-importers/sample-scripts)

### <a name="table-design"></a>Diseño de tablas

- Todas las tablas tienen claves principales de columna única para la simplicidad de combinación.
- Todos los esquemas, tablas, columnas, índices y restricciones check tienen una propiedad extendida Description que se puede utilizar para identificar el propósito del objeto o la columna. Las tablas optimizadas para memoria son una excepción a esto, ya que actualmente no admiten propiedades extendidas.
- Todas las claves externas se indizan automáticamente a menos que haya otro índice no clúster que tenga el mismo componente izquierdo.
- La numeración automática en las tablas se basa en secuencias. Estas secuencias son más fáciles de usar en los servidores vinculados y en entornos similares a las columnas de identidad. Las tablas con optimización para memoria usan columnas de identidad, ya que no admiten en SQL Server 2016.
- Se usa una sola secuencia (TransactionID) para estas tablas: CustomerTransactions, SupplierTransactions y StockItemTransactions. Esto muestra cómo un conjunto de tablas puede tener una sola secuencia.
- Algunas columnas tienen los valores predeterminados adecuados.

### <a name="security-schemas"></a>Esquemas de seguridad

Por seguridad, WideWorldImporters no permite a las aplicaciones externas tener acceso a los esquemas de datos directamente. Para aislar el acceso, WideWorldImporters usa esquemas de acceso de seguridad que no contienen datos, pero contienen vistas y procedimientos almacenados. Las aplicaciones externas usan los esquemas de seguridad para recuperar los datos que pueden ver.  De este modo, los usuarios solo pueden ejecutar las vistas y los procedimientos almacenados en los esquemas de acceso seguro

Por ejemplo, este ejemplo incluye Power BI paneles. Una aplicación externa accede a estos paneles de Power BI desde la puerta de enlace de Power BI como un usuario que tiene permiso de solo lectura en el esquema de PowerBI.  En el caso del permiso de solo lectura, el usuario solo necesita el permiso SELECT y EXECUTe en el esquema de PowerBI. Un administrador de base de datos de WWI asigna estos permisos según sea necesario.

## <a name="stored-procedures"></a>Procedimientos almacenados

Los procedimientos almacenados se organizan en esquemas. La mayoría de los esquemas se usan para fines de configuración o de ejemplo.

El `Website` esquema contiene los procedimientos almacenados que puede usar un front-end web.

Los `Reports` `PowerBI` esquemas y están diseñados para los fines de Reporting Services y PowerBI. Se recomienda que todas las extensiones del ejemplo utilicen estos esquemas para la elaboración de informes.

### <a name="website-schema"></a>Esquema de sitios web

Estos son los procedimientos utilizados por una aplicación cliente, como un front-end web.

|Procedimiento|Propósito|
|-----------------------------|---------------------|
|ActivateWebsiteLogon|Permite que una persona (de `Application.People` ) tenga acceso al sitio Web.|
|ChangePassword|Cambia la contraseña de un usuario (para los usuarios que no usan mecanismos de autenticación externos).|
|InsertCustomerOrders|Permite insertar uno o varios pedidos de clientes (incluidas las líneas de pedido).|
|InvoiceCustomerOrders|Toma una lista de pedidos que se van a facturar y procesa las facturas.|
|RecordColdRoomTemperatures|Toma una lista de datos del sensor, como un parámetro con valores de tabla (TVP), y aplica los datos a la `Warehouse.ColdRoomTemperatures` tabla temporal.|
|RecordVehicleTemperature|Toma una matriz JSON y la usa para actualizar `Warehouse.VehicleTemperatures` .|
|SearchForCustomers|Busca los clientes por nombre o parte del nombre (el nombre de la empresa o el nombre de la persona).|
|SearchForPeople|Busca personas por nombre o parte del nombre.|
|SearchForStockItems|Busca los elementos de stock por nombre o parte del nombre o de los comentarios de marketing.|
|SearchForStockItemsByTags|Busca elementos de existencias por etiquetas.|
|SearchForSuppliers|Busca proveedores por nombre o parte del nombre (el nombre de la empresa o el nombre de la persona).|

### <a name="integration-schema"></a>Esquema de integración

El proceso ETL utiliza los procedimientos almacenados en este esquema. Obtienen los datos necesarios de varias tablas para el período de tiempo que requiere el [paquete ETL](wide-world-importers-perform-etl.md).

### <a name="dataloadsimulation-schema"></a>Esquema DataLoadSimulation

Simula una carga de trabajo que inserta ventas y compras. El procedimiento almacenado principal es `PopulateDataToCurrentDate` , que se usa para insertar datos de ejemplo hasta la fecha actual.

|Procedimiento|Propósito|
|-----------------------------|---------------------|
|Configuration_ApplyDataLoadSimulationProcedures|Vuelve a crear los procedimientos necesarios para la simulación de carga de datos. Esto es necesario para llevar los datos a la fecha actual.|
|Configuration_RemoveDataLoadSimulationProcedures|Esto quita los procedimientos de nuevo una vez completada la simulación de datos.|
|DeactivateTemporalTablesBeforeDataLoad|Quita la naturaleza temporal de todas las tablas temporales y, si procede, aplica un desencadenador para que los cambios se puedan realizar como si se aplicaran en una fecha anterior a la que permiten las tablas sys-temporales.|
|PopulateDataToCurrentDate|Se utiliza para llevar los datos hasta la fecha actual. Debe ejecutarse antes que cualquier otra opción de configuración después de restaurar la base de datos a partir de una copia de seguridad inicial.|
|ReactivateTemporalTablesAfterDataLoad|Vuelve a establecer las tablas temporales, incluida la comprobación de la coherencia de los datos. (Quita los desencadenadores asociados).|


### <a name="application-schema"></a>Esquema de aplicación

Estos procedimientos se utilizan para configurar el ejemplo. Se usan para aplicar las características de la edición Enterprise a la versión Standard Edition del ejemplo, así como para agregar auditoría y indización de texto completo.

|Procedimiento|Propósito|
|-----------------------------|---------------------|
|AddRoleMemberIfNonexistant|Agrega un miembro a un rol si el miembro todavía no está en el rol.|
|Configuration_ApplyAuditing|Agrega auditoría. La auditoría de servidor se aplica a las bases de datos de la edición Standard. se ha agregado una auditoría de base de datos adicional para Enterprise Edition.|
|Configuration_ApplyColumnstoreIndexing|Aplica la indexación de almacén de columnas a `Sales.OrderLines` y `Sales.InvoiceLines` y reindexa adecuadamente.|
|Configuration_ApplyFullTextIndexing|Aplica los índices de texto completo a `Application.People` , `Sales.Customers` , `Purchasing.Suppliers` y `Warehouse.StockItems` . Reemplaza `Website.SearchForPeople` , `Website.SearchForSuppliers` , `Website.SearchForCustomers` , `Website.SearchForStockItems` , `Website.SearchForStockItemsByTags` por los procedimientos de reemplazo que usan la indización de texto completo.|
|Configuration_ApplyPartitioning|Aplica la creación de particiones de tabla a `Sales.CustomerTransactions` y `Purchasing.SupplierTransactions` , y reorganiza los índices para que se adapten.|
|Configuration_ApplyRowLevelSecurity|Aplica la seguridad de nivel de fila para filtrar los clientes por roles relacionados con el territorio de ventas.|
|Configuration_ConfigureForEnterpriseEdition|Aplica la indexación de almacén de columnas, texto completo, en memoria, polybase y creación de particiones.|
|Configuration_EnableInMemory|Agrega un grupo de archivos optimizados para memoria (cuando no funciona en Azure), reemplaza `Warehouse.ColdRoomTemperatures` `Warehouse.VehicleTemperatures` con equivalentes en memoria y migra los datos, vuelve a crear los `Website.OrderIDList` tipos de `Website.OrderList` tabla,, `Website.OrderLineList` , `Website.SensorDataList` con equivalentes optimizados para memoria, quita y vuelve a crear los procedimientos `Website.InvoiceCustomerOrders` , `Website.InsertCustomerOrders` y `Website.RecordColdRoomTemperatures` que usa estos tipos de tabla.|
|Configuration_RemoveAuditing|Quita la configuración de auditoría.|
|Configuration_RemoveRowLevelSecurity|Quita la configuración de seguridad de nivel de fila (esto es necesario para los cambios en las tablas asociadas).|
|CreateRoleIfNonExistant|Crea un rol de base de datos si aún no existe.|


### <a name="sequences-schema"></a>Esquema de secuencias

Procedimientos para configurar las secuencias en la base de datos.

|Procedimiento|Propósito|
|-----------------------------|---------------------|
|ReseedAllSequences|Llama al procedimiento ReseedSequenceBeyondTableValue para todas las secuencias.|
|ReseedSequenceBeyondTableValue|Se usa para cambiar la posición del valor de secuencia siguiente más allá del valor de cualquier tabla que use la misma secuencia. (Como un DBCC CHECKIDENT para las columnas de identidad equivalentes para las secuencias, pero en potencialmente varias tablas).|
