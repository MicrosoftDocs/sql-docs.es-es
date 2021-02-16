---
title: Base de datos de ejemplo para OLTP en memoria | Microsoft Docs
description: Obtenga información sobre la característica OLTP en memoria y sus ventajas de rendimiento. En este ejemplo se muestran tablas optimizadas para memoria y procedimientos almacenados compilados de forma nativa.
ms.custom: ''
ms.date: 12/14/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: df347f9b-b950-4e3a-85f4-b9f21735eae3
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 67f29ef6d47de5cc14cb248d20a243053857c9e9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353639"
---
# <a name="sample-database-for-in-memory-oltp"></a>Base de datos de ejemplo para OLTP en memoria
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
    
## <a name="overview"></a>Información general  
 Este ejemplo muestra la característica OLTP en memoria. Muestra tablas optimizadas para memoria y procedimientos almacenados compilados de forma nativa y se puede usar para mostrar las ventajas de rendimiento de OLTP en memoria.  
  
> [!NOTE]  
>  Para ver este tema para [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], consulte [Extensiones de AdventureWorks para mostrar OLTP en memoria](./overview-and-usage-scenarios.md#in-memory-oltp-overview).  
  
 El ejemplo migra cinco tablas de la base de datos AdventureWorks a tablas optimizadas para memoria e incluye una carga de trabajo de demostración para el procesamiento de pedidos de venta. Se puede usar esta carga de trabajo de demostración para ver la ventaja de rendimiento que supone emplear OLTP en memoria en el servidor.  
  
 En la descripción del ejemplo se explican los compromisos realizados al migrar las tablas a OLTP en memoria para compensar las características que no se admiten (todavía) en las tablas optimizadas para memoria.  
  
 La documentación de este ejemplo está estructurada de la manera siguiente:  
  
-   [Requisitos previos](#Prerequisites) para instalar el ejemplo y ejecutar la carga de trabajo de demostración  
  
-   Instrucciones para [Instalar el ejemplo de In-Memory OLTP basado en AdventureWorks](#InstallingtheIn-MemoryOLTPsamplebasedonAdventureWorks)  
  
-   [Descripción de las tablas y los procedimientos de ejemplo](#Descriptionofthesampletablesandprocedures): contiene descripciones de las tablas y los procedimientos que el ejemplo de OLTP en memoria ha agregado a AdventureWorks, y aspectos que se deben tener en cuenta para migrar algunas de las tablas originales de AdventureWorks a tablas optimizadas para memoria.  
  
-   Instrucciones para realizar [Medidas de rendimiento con la carga de trabajo de demostración](#PerformanceMeasurementsusingtheDemoWorkload): contiene instrucciones para instalar y ejecutar ostress, una herramienta que se usa para controlar la carga de trabajo y para ejecutar la carga de trabajo de demostración propiamente dicha.  
  
-   [Uso de memoria y de espacio en disco del ejemplo](#MemoryandDiskSpaceUtilizationintheSample)  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Requisitos previos  
  
-   [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]  
  
-   Para las pruebas de rendimiento, un servidor con unas especificaciones similares al entorno de producción. Para esta muestra concreta, debe haber al menos 16 GB de memoria disponible para SQL Server. Para obtener instrucciones generales de hardware para OLTP en memoria, vea la entrada de blog siguiente: [Consideraciones de hardware para OLTP en memoria en SQL Server 2014](blog-hardware-in-memory-oltp.md)

##  <a name="installing-the-in-memory-oltp-sample-based-on-adventureworks"></a><a name="InstallingtheIn-MemoryOLTPsamplebasedonAdventureWorks"></a> Instalar el ejemplo de In-Memory OLTP basado en AdventureWorks  
 Siga estos pasos para instalar el ejemplo:  
  
1.  Descargue AdventureWorks2016CTP3.bak y SQLServer2016CTP3Samples.zip desde [https://github.com/microsoft/sql-server-samples/releases/tag/adventureworks](https://github.com/microsoft/sql-server-samples/releases/tag/adventureworks) en una carpeta local (por ejemplo, 'c:\temp').  
  
2.  Restaure la copia de seguridad de la base de datos mediante [!INCLUDE[tsql](../../includes/tsql-md.md)] o [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]:  
  
    1.  Identifique la carpeta de destino y el nombre del archivo de datos, por ejemplo:  
  
         'h:\DATA\AdventureWorks2016CTP3_Data.mdf'  
  
    2.  Identifique la carpeta de destino y el nombre del archivo de registro, por ejemplo:  
  
         'i:\DATA\AdventureWorks2016CTP3_log.ldf'  
  
        1.  El archivo de registro debe colocarse en otra unidad diferente que el archivo de datos, idealmente en una unidad de baja latencia como un almacenamiento SSD o PCIe, para obtener el máximo rendimiento.  
  
     Script T-SQL de ejemplo:  
  
    ```sql
    RESTORE DATABASE [AdventureWorks2016CTP3]   
      FROM DISK = N'C:\temp\AdventureWorks2016CTP3.bak'   
        WITH FILE = 1,    
      MOVE N'AdventureWorks2016_Data' TO N'h:\DATA\AdventureWorks2016CTP3_Data.mdf',    
      MOVE N'AdventureWorks2016_Log' TO N'i:\DATA\AdventureWorks2016CTP3_log.ldf',  
      MOVE N'AdventureWorks2016CTP3_mod' TO N'h:\data\AdventureWorks2016CTP3_mod'  
     GO  
    ```  
  
3.  Para ver la carga de trabajo y scripts de ejemplo, descomprima el archivo SQLServer2016CTP3Samples.zip en una carpeta local. Para obtener instrucciones sobre cómo ejecutar la carga de trabajo, consulte el archivo In-Memory OLTP\readme.txt.  
  
##  <a name="description-of-the-sample-tables-and-procedures"></a><a name="Descriptionofthesampletablesandprocedures"></a> Descripción de las tablas y los procedimientos de ejemplo:  
 El ejemplo crea nuevas tablas para productos y pedidos de venta basadas en tablas existentes en AdventureWorks. El esquema de las nuevas tablas es similar a las tablas existentes, con algunas diferencias, como se explica a continuación.  
  
 Las nuevas tablas optimizadas para memoria llevan el sufijo "_inmem". El ejemplo también incluye las tablas correspondientes que llevan el sufijo "_ondisk"; estas tablas se pueden usar para realizar una comparación uno a uno entre el rendimiento de las tablas optimizadas para memoria y las tablas basadas en disco del sistema.  
  
 Las tablas optimizadas para memoria empleadas en la carga de trabajo para la comparación de rendimiento son totalmente durables y con registro completo. No sacrifican la durabilidad o la confiabilidad para conseguir la mejora del rendimiento.  
  
 La carga de trabajo de destino de este ejemplo es el procesamiento de pedidos de venta, donde también tenemos en cuenta la información sobre productos y descuentos. Para eso se usan las tablas SalesOrderHeader, SalesOrderDetail, Product, SpecialOffer y SpecialOfferProduct.  
  
 Se emplean dos nuevos procedimientos almacenados, Sales.usp_InsertSalesOrder_inmem y Sales.usp_UpdateSalesOrderShipInfo_inmem, para insertar pedidos de venta y actualizar la información de envío de un pedido de venta determinado.  
  
 El nuevo esquema 'Demo' contiene tablas del asistente y procedimientos almacenados para ejecutar una carga de trabajo de demostración.  
  
 En concreto, el ejemplo de OLTP en memoria agrega los siguientes objetos a AdventureWorks:  
  
### <a name="tables-added-by-the-sample"></a>Tablas agregadas por el ejemplo  
  
#### <a name="the-new-tables"></a>Las nuevas tablas  
 Sales.SalesOrderHeader_inmem  
  
-   Información de encabezado sobre los pedidos de venta. Cada pedido de venta tiene una fila en esta tabla.  
  
 Sales.SalesOrderDetail_inmem  
  
-   Detalles de los pedidos de venta. Cada artículo de un pedido de venta tiene una fila en esta tabla.  
  
 Sales.SpecialOffer_inmem  
  
-   Información sobre ofertas especiales, incluido el porcentaje de descuento asociado a cada oferta especial.  
  
 Sales.SpecialOfferProduct_inmem  
  
-   Tabla de referencia entre las ofertas especiales y los productos. Cada oferta especial puede abarcar cero o más productos, y cada producto puede estar incluido en cero o más ofertas especiales.  
  
 Production.Product_inmem  
  
-   Información sobre productos, incluido el precio de venta.  
  
 Demo.DemoSalesOrderDetailSeed  
  
-   Se usa en la carga de trabajo de demostración para generar pedidos de venta de ejemplo.  
  
 Variaciones basadas en disco de las tablas:  
  
-   Sales.SalesOrderHeader_ondisk  
  
-   Sales.SalesOrderDetail_ondisk  
  
-   Sales.SpecialOffer_ondisk  
  
-   Sales.SpecialOfferProduct_ondisk  
  
-   Production.Product_ondisk  
  
#### <a name="differences-between-original-disk-based-and-the-and-new-memory-optimized-tables"></a>Diferencias entre las tablas originales basadas en disco y las nuevas tablas optimizadas para memoria  
 En general, las nuevas tablas de este ejemplo emplean las mismas columnas y los mismos tipos de datos que las tablas originales. Sin embargo, hay algunas diferencias. A continuación se enumeran las diferencias y el motivo de los cambios.  
  
 Sales.SalesOrderHeader_inmem  
  
-   Las *restricciones DEFAULT* se admiten en las tablas optimizadas para memoria y la mayoría de las restricciones DEFAULT se migran tal cual. Sin embargo, la tabla original Sales.SalesOrderHeader contiene dos restricciones DEFAULT que recuperan la fecha actual, para las columnas OrderDate y ModifiedDate. En una carga de trabajo de procesamiento de pedidos de alto rendimiento con mucha simultaneidad, cualquier recurso global puede convertirse en un punto de contención. La hora del sistema es un recurso global y hemos observado que puede convertirse en un cuello de botella cuando se ejecuta una carga de trabajo de OLTP en memoria que inserta pedidos de venta, especialmente si es necesario recuperar la hora del sistema para varias columnas en el encabezado del pedido de venta, así como los detalles del pedido de venta. Para resolver el problema en este ejemplo se recupera la hora del sistema solo una vez para cada pedido de venta que se inserta, y se usa ese valores para las columnas datetime de SalesOrderHeader_inmem y SalesOrderDetail_inmem, en el procedimiento almacenado Sales.usp_InsertSalesOrder_inmem.  
  
-   *Tipos de datos definidos por el usuario de alias (UDT)* : la tabla original usa dos UDT de alias, dbo.OrderNumber y dbo.AccountNumber, para las columnas PurchaseOrderNumber y AccountNumber, respectivamente. [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] no admite UDT de alias para las tablas optimizadas para memoria, por lo que las tablas nuevas usan los tipos de datos del sistema nvarchar(25) y nvarchar(15), respectivamente.  
  
-   *Columnas que aceptan valores NULL en claves de índice:* en la tabla original, la columna SalesPersonID acepta valores NULL, mientras que en las tablas nuevas esa columna no acepta valores NULL y tiene una restricción DEFAULT con el valor (-1). Esta situación se debe a que los índices de las tablas optimizadas para memoria no pueden tener columnas que aceptan valores NULL en la clave de índice; -1 es un suplente para NULL en este caso.  
  
-   *Columnas calculadas:* se omiten las columnas calculadas SalesOrderNumber y TotalDue, ya que [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] no admite columnas calculadas en tablas optimizadas para memoria. La nueva vista Sales.vSalesOrderHeader_extended_inmem refleja las columnas SalesOrderNumber y TotalDue. Por tanto, puede usar esta vista si se necesitan estas columnas.  

    - **Se aplica a:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1.  
A partir de [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1, se admiten columnas calculadas en tablas e índices optimizados para memoria.

  
-   Las *restricciones FOREIGN KEY* se admiten para tablas optimizadas para memoria en [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], pero solo si las tablas de referencia también están optimizadas para memoria. Las claves externas que hagan referencia a tablas de referencia que también se migran a tablas optimizadas para memoria se conservan en esas tablas migradas, mientras que otras claves externas se omiten.  Además, SalesOrderHeader_inmem es una tabla sin interrupción en la carga de trabajo de ejemplo, y las restricciones de clave externa requieren un procesamiento adicional para todas las operaciones DML, ya que tienen que hacer búsquedas en todas las demás tablas a las que se hace referencia en estas restricciones. Por tanto, la suposición es que la aplicación garantiza la integridad referencial de la tabla Sales.SalesOrderHeader_inmem, y la integridad referencial no se valida cuando se insertan filas.  
  
-   *Rowguid:* la columna rowguid se omite. Aunque se admite uniqueidentifier en las tablas optimizadas para memoria, la opción ROWGUIDCOL no se admite en [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]. Las columnas de esta clase se suelen usar para la replicación de mezcla o para tablas que incluyen columnas FILESTREAM. En este ejemplo no se incluye ninguna de ellas.  
  
 Sales.SalesOrderDetail  
  
-   *Restricciones DEFAULT*: igual que en SalesOrderHeader, la restricción DEFAULT que requiere la fecha y la hora del sistema no se migra; en su lugar, el procedimiento almacenado que inserta pedidos de venta se encarga de insertar la fecha y la hora actuales del sistema en la primera inserción.  
  
-   *Columnas calculadas*: la columna calculada LineTotal no se ha migrado, ya que en [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] no se admiten columnas calculadas en las tablas optimizadas para memoria. Para obtener acceso a esta columna, use la vista Sales.vSalesOrderDetail_extended_inmem.  
  
-   *Rowguid:* la columna rowguid se omite. Para obtener detalles, vea la descripción de la tabla SalesOrderHeader.  
  
 Production.Product  
  
-   *UDT de alias*: en la tabla original se usa el tipo de datos definido por el usuario dbo.Flag, que equivale al tipo de datos del sistema bit. La tabla migrada usa el tipo de datos bit en su lugar.  
  
-   *Rowguid:* la columna rowguid se omite. Para obtener detalles, vea la descripción de la tabla SalesOrderHeader.  
  
 Sales.SpecialOffer  
  
-   *Rowguid:* la columna rowguid se omite. Para obtener detalles, vea la descripción de la tabla SalesOrderHeader.  
  
 Sales.SpecialOfferProduct  
  
-   *Rowguid:* la columna rowguid se omite. Para obtener detalles, vea la descripción de la tabla SalesOrderHeader.  
  
#### <a name="considerations-for-indexes-on-memory-optimized-tables"></a>Consideraciones sobre los índices de tablas optimizadas para memoria  
 El índice de línea base para las tablas optimizadas para memoria es el índice no clúster, que admite búsquedas de puntos (búsqueda de índice en predicado de igualdad), recorridos de intervalo (búsqueda de índice en predicados de desigualdad), exámenes de índice completos y exámenes ordenados. Además, los índices no clúster admiten búsquedas en las columnas iniciales de la clave de índice. De hecho, los índices no clúster optimizados para memoria admiten todas las operaciones compatibles con los índices no clúster basados en disco, con la única excepción de los exámenes hacia atrás. Por tanto, el uso de índices no clúster es una opción segura para los índices.  
  
 Se pueden usar índices hash para optimizar aún más la carga de trabajo. Están optimizados especialmente para las búsquedas de puntos y las inserciones de filas. Sin embargo, hay que tener en cuenta que no admiten recorridos de intervalo, exámenes ordenados ni búsquedas en columnas de clave de índice iniciales. Por tanto, hay que tener cuidado cuando se usen estos índices. Además, es necesario especificar el bucket_count en el momento de la creación. Normalmente se debe establecer entre una y dos veces el número de valores de clave de índice, pero la sobrestimación no suele suponer ningún problema.  
  
Para obtener más información, vea los Libros en pantalla para obtener detalles sobre las [directrices para usar índices](../indexes/guidelines-for-online-index-operations.md) y las directrices para [elegir el número correcto de depósitos](https://sqlserver-help.com/tag/bucket_count/).  

Los libros en pantalla proporcionan más información sobre los temas siguientes:
- [Instrucciones de índices](./indexes-for-memory-optimized-tables.md) <!-- On MSDN-TechNet was version sql.120 (2014), library/dn133166 -->

 Los índices de las tablas migradas se han optimizado para la carga de trabajo de procesamiento de pedidos de venta de demostración. La carga de trabajo usa inserciones y búsquedas de puntos en las tablas Sales.SalesOrderHeader_inmem y Sales.SalesOrderDetail_inmem, y también usa búsquedas de puntos en las columnas de clave principal en las tablas Production.Product_inmem y Sales.SpecialOffer_inmem.  
  
 Sales.SalesOrderHeader_inmem tiene tres índices, que son todos índices HASH por motivos de rendimiento, y porque no se necesita ningún examen ordenado o recorrido de intervalo para la carga de trabajo.  
  
-   Índice HASH de (SalesOrderID): bucket_count tiene un tamaño de 10 millones (redondeado hasta 16 millones), ya que el número esperado de pedidos de venta es 10 millones  
  
-   Índice HASH de (SalesPersonID): el bucket_count es 1 millón. El conjunto de datos proporcionado no tiene muchos vendedores. Pero este bucket_count grande permite un crecimiento futuro. Además, no paga ninguna penalización de rendimiento por las búsquedas de puntos si el elemento bucket_count es demasiado grande.  
  
-   Índice HASH de (CustomerID): el bucket_count es 1 millón. El conjunto de datos especificado no tiene muchos clientes, pero permite el crecimiento futuro.  
  
 Sales.SalesOrderDetail_inmem tiene tres índices, que son todos índices HASH por motivos de rendimiento, y porque no se necesita ningún examen ordenado o recorrido de intervalo para la carga de trabajo.  
  
-   Índice HASH de (SalesOrderID, SalesOrderDetailID): este es el índice de clave principal, y aunque las búsquedas en (SalesOrderID, SalesOrderDetailID) serán poco frecuentes, el uso de un índice hash para la clave acelera las inserciones de filas. El elemento bucket_count tiene un tamaño de 50 millones (redondeado hasta 67 millones); el número esperado de pedidos de venta es de 10 millones y tiene un tamaño promedio de cinco artículos por pedido.  
  
-   Índice HASH de (SalesOrderID): las búsquedas por pedido de venta son frecuentes; es conveniente buscar todos los artículos correspondientes a un único pedido.  bucket_count tiene un tamaño de 10 millones (redondeado hasta 16 millones), ya que el número esperado de pedidos de venta es 10 millones  
  
-   Índice HASH de (ProductID): el bucket_count es 1 millón. El conjunto de datos especificado no tiene muchos productos, pero permite el crecimiento futuro.  
  
 Production.Product_inmem tiene tres índices  
  
-   Índice HASH de (ProductID): las búsquedas de ProductID son la ruta crítica para la carga de trabajo de demostración, por lo que es un índice hash  
  
-   Índice no clúster de (Name): permitirá exámenes ordenados de los nombres de producto  
  
-   Índice no clúster de (ProductNumber): permitirá exámenes ordenados de los números de producto  
  
 Sales.SpecialOffer_inmem tiene un índice HASH en (SpecialOfferID): las búsquedas de puntos de ofertas especiales son una parte crítica de la carga de trabajo de demostración. El bucket_count tiene un tamaño de 1 millón para permitir el crecimiento futuro.  
  
 No se hace referencia a Sales.SpecialOfferProduct_inmem en la carga de trabajo de demostración, por lo que no hay necesidad de usar índices hash en esta tabla para optimizar la carga de trabajo; los índices de (SpecialOfferID, ProductID) y (ProductID) no son agrupados.  
  
 Observe que en la información anterior algunos valores de bucket_count están sobredimensionados, pero no los bucket_counts de los índices de SalesOrderHeader_inmem y SalesOrderDetail_inmem: tienen un tamaño para 10 millones de pedidos de venta. Esto se hace para permitir la instalación del ejemplo en sistemas con poca disponibilidad de memoria, aunque en esos casos la carga de trabajo de demostración producirá un error si no hay memoria suficiente. Si desea escalar el ejemplo más allá de 10 millones de pedidos de venta, no dude en aumentar los números de cubos en consecuencia.  
  
#### <a name="considerations-for-memory-utilization"></a>Consideraciones sobre el uso de memoria  
 El uso de memoria en la base de datos de ejemplo, tanto antes como después de ejecutar la carga de trabajo de demostración, se describe en la sección [Uso de memoria para las tablas optimizadas para memoria](#Memoryutilizationforthememory-optimizedtables).  
  
### <a name="stored-procedures-added-by-the-sample"></a>Procedimientos almacenados agregados por el ejemplo  
 Los dos procedimientos almacenados principales para insertar pedidos de venta y actualizar los detalles de envío son los siguientes:  
  
-   Sales.usp_InsertSalesOrder_inmem  
  
    -   Inserta un nuevo pedido de venta en la base de datos y genera el SalesOrderID para ese pedido. Como parámetros de entrada toma los detalles del encabezado del pedido de venta, así como los artículos del pedido.  
  
    -   Parámetro de salida:  
  
        -   @SalesOrderID int: el valor SalesOrderID del pedido de venta que se acaba de insertar  
  
    -   Parámetros de entrada (obligatorios):  
  
        -   @DueDate datetime2  
  
        -   @CustomerID int  
  
        -   @BillToAddressID [int]  
  
        -   @ShipToAddressID [int]  
  
        -   @ShipMethodID [int]  
  
        -   @SalesOrderDetails Sales.SalesOrderDetailType_inmem: parámetro con valores de tabla (TVP) que contiene los elementos de línea del pedido  
  
    -   Parámetros de entrada (opcionales):  
  
        -   @Status [tinyint]  
  
        -   @OnlineOrderFlag [bit]  
  
        -   @PurchaseOrderNumber [nvarchar](25\)  
  
        -   @AccountNumber [nvarchar](15\)  
  
        -   @SalesPersonID [int]  
  
        -   @TerritoryID [int]  
  
        -   @CreditCardID [int]  
  
        -   @CreditCardApprovalCode [varchar](15\)  
  
        -   @CurrencyRateID [int]  
  
        -   @Comment nvarchar(128)  
  
-   Sales.usp_UpdateSalesOrderShipInfo_inmem  
  
    -   Actualice la información de envío para un pedido de venta determinado. Esto también actualizará la información de envío de todos los artículos del pedido de venta.  
  
    -   Se trata de un procedimiento contenedor para los procedimientos almacenados compilados de forma nativa Sales.usp_UpdateSalesOrderShipInfo_native con lógica de reintento para abordar posibles conflictos (inesperados) con las transacciones simultáneas que actualizan el mismo pedido. Para obtener más información acerca de la lógica de reintento, vea [este tema](./transactions-with-memory-optimized-tables.md)en los Libros en pantalla.  
  
-   Sales.usp_UpdateSalesOrderShipInfo_native  
  
    -   Este es el procedimiento almacenado compilado de forma nativa que procesa realmente la actualización de la información de envío. Está pensado para que se le llame desde el procedimiento almacenado contenedor Sales.usp_UpdateSalesOrderShipInfo_inmem. Si el cliente puede resolver los errores e implementa lógica de reintento, puede llamar a este procedimiento directamente, en lugar de usar el procedimiento almacenado contenedor.  
  
 El procedimiento almacenado siguiente se emplea para la carga de trabajo de demostración.  
  
-   Demo.usp_DemoReset  
  
    -   Restablece la demostración vaciando y reinicializando las tablas SalesOrderHeader y SalesOrderDetail.  
  
 Los procedimientos almacenados siguientes se usan para insertar y eliminar información de las tablas optimizadas para memoria garantizando la integridad del dominio y referencial.  
  
-   Production.usp_InsertProduct_inmem  
  
-   Production.usp_DeleteProduct_inmem  
  
-   Sales.usp_InsertSpecialOffer_inmem  
  
-   Sales.usp_DeleteSpecialOffer_inmem  
  
-   Sales.usp_InsertSpecialOfferProduct_inmem  
  
 Por último, el procedimiento almacenado siguiente se usa para comprobar la integridad del dominio y referencial.  
  
1.  dbo.usp_ValidateIntegrity  
  
    -   Parámetro opcional: @object_id, identificador del objeto cuya integridad se va a validar  
  
    -   Este procedimiento usa las tablas dbo.DomainIntegrity, dbo.ReferentialIntegrity y dbo.UniqueIntegrity para las reglas de integridad que es necesario comprobar; el ejemplo rellena estas tablas según las restricciones CHECK, UNIQUE y de clave externa existentes en las tablas originales de la base de datos AdventureWorks.  
  
    -   Usa los procedimientos del asistente dbo.usp_GenerateCKCheck, dbo.usp_GenerateFKCheck y dbo.GenerateUQCheck para generar el código T-SQL necesario para realizar las comprobaciones de integridad.  
  
##  <a name="performance-measurements-using-the-demo-workload"></a><a name="PerformanceMeasurementsusingtheDemoWorkload"></a> Medidas de rendimiento con la carga de trabajo de demostración  
 Ostress es una herramienta de línea de comandos desarrollada por el equipo de soporte técnico de Microsoft CSS SQL Server. Esta herramienta se puede usar para ejecutar consultas o ejecutar procedimientos almacenados en paralelo. Puede configurar el número de subprocesos para ejecutar una instrucción T-SQL proporcionada en paralelo y puede especificar cuántas veces se debe ejecutar la instrucción en este subproceso; ostress recorrerá los subprocesos y ejecutará la instrucción en todos ellos en paralelo. Una vez que concluya la ejecución en todos los subprocesos, ostress notificará el tiempo empleado en finalizar la ejecución en todos los subprocesos.  
  
### <a name="installing-ostress"></a>Instalar ostress  
 Ostress se instala como parte de las utilidades de Report Markup Language (RML); no hay ninguna instalación independiente para ostress.  
  
 Pasos para la instalación:  
  
1.  Descargue y ejecute el paquete de instalación x64 para las utilidades de RML desde la página siguiente: [Descarga de RML para SQL Server](https://www.microsoft.com/download/details.aspx?id=4511)

2.  Si aparece un cuadro de diálogo que indica que algunos archivos están en uso, haga clic en "Continue" (Continuar).  
  
### <a name="running-ostress"></a>Ejecutar ostress  
 Ostress se ejecuta desde el símbolo del sistema. Es mejor ejecutar la herramienta desde "RML Cmd Prompt”, que se instala como parte de las utilidades de RML.  
  
 Para abrir RML Cmd Prompt, siga estas instrucciones:  
  
 En Windows Server 2012 [R2] y en Windows 8 y 8.1, haga clic en la tecla Windows para abrir el menú Inicio y escriba "rml". Haga clic en "RML Cmd Prompt", que aparecerá en la lista de resultados de la búsqueda.  
  
 Asegúrese de que el símbolo del sistema se encuentra en la carpeta de instalación de las utilidades de RML.  
  
 Las opciones de línea de comandos para ostress se pueden ver si se ejecuta ostress.exe sin ninguna opción de línea de comandos. Las opciones principales que hay que tener en cuenta para ejecutar ostress con este ejemplo son:  
  
-   -S nombre de la instancia de Microsoft SQL Server a la que conectarse  
  
-   -E usar autenticación de Windows para conectarse (valor predeterminado). Si se usa la autenticación de SQL Server, utilice las opciones -U y -P para especificar el nombre de usuario y la contraseña, respectivamente  
  
-   -d nombre de la base de datos; para este ejemplo, AdventureWorks2014  
  
-   -Q instrucción T-SQL que se va a ejecutar  
  
-   -n número de conexiones que procesan cada archivo de entrada o consulta  
  
-   -r número de iteraciones para que cada conexión ejecute cada archivo de entrada o consulta  
  
### <a name="demo-workload"></a>Carga de trabajo de demostración  
 El procedimiento almacenado principal que se usa en la carga de trabajo de demostración es Sales.usp_InsertSalesOrder_inmem/ondisk. El script siguiente crea un parámetro con valores de tabla (TVP) con datos de ejemplo y llama al procedimiento para insertar un pedido de venta con cinco artículos.  
  
 La herramienta ostress se emplea para ejecutar las llamadas a procedimientos almacenados en paralelo, con el fin de simular que los clientes insertan los pedidos de venta simultáneamente.  
  
 Restablezca la demostración después de cada prueba de esfuerzo ejecutando Demo.usp_DemoReset. Este procedimiento elimina las filas de las tablas optimizadas para memoria, trunca las tablas basadas en disco y ejecuta un punto de comprobación de la base de datos.  
  
 El script siguiente se ejecuta simultáneamente para simular una carga de trabajo de procesamiento de pedidos de venta:  
  
```sql
DECLARE   
      @i int = 0,   
      @od Sales.SalesOrderDetailType_inmem,   
      @SalesOrderID int,   
      @DueDate datetime2 = sysdatetime(),   
      @CustomerID int = rand() * 8000,   
      @BillToAddressID int = rand() * 10000,   
      @ShipToAddressID int = rand() * 10000,   
      @ShipMethodID int = (rand() * 5) + 1;   
  
INSERT INTO @od   
SELECT OrderQty, ProductID, SpecialOfferID   
FROM Demo.DemoSalesOrderDetailSeed   
WHERE OrderID= cast((rand()*106) + 1 as int);   
  
WHILE (@i < 20)   
BEGIN;   
      EXEC Sales.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @ShipMethodID, @od;   
      SET @i += 1   
END
```  
  
 Con este script, cada pedido de ejemplo que se crea se inserta 20 veces, mediante 20 procedimientos almacenados que se ejecutan en un bucle WHILE. El bucle se usa para tener en cuenta el hecho de que la base de datos se emplea para crear el pedido de ejemplo. En los entornos de producción típicos, la aplicación de nivel intermedio creará el pedido de venta que se va a insertar.  
  
 El script anterior inserta los pedidos de venta en tablas optimizadas para memoria. El script para insertar pedidos de venta en tablas basadas en disco se deriva reemplazando las dos instancias de "_inmem" por "_ondisk".  
  
 Se usará la herramienta ostress para ejecutar los scripts con varias conexiones simultáneas. Se usará el parámetro "-n" para controlar el número de conexiones y el parámetro "r" para controlar cuántas veces se ejecuta el script en cada conexión.  
  
#### <a name="running-the-workload"></a>Ejecutar la carga de trabajo  
 Para probar la escala insertamos 10 millones de pedidos de venta usando 100 conexiones. Esta prueba funciona razonablemente bien en un servidor modesto (por ejemplo, 8 núcleos físicos y 16 lógicos) y con un almacenamiento SSD básico para el registro. Si la prueba no funciona correctamente en el hardware, vea la sección [Solucionar problemas de pruebas de ejecución lenta](#Troubleshootingslow-runningtests). Si quiere reducir el nivel de esfuerzo de esta prueba, disminuya el número de conexiones cambiando el parámetro "-n". Por ejemplo, para reducir el número de conexiones a 40, cambie el parámetro "-n100" a "-n40".  
  
 Como medida de rendimiento de la carga de trabajo usamos el tiempo transcurrido notificado por ostress.exe después de ejecutar la carga de trabajo.  
  
 En las medidas e instrucciones siguientes se usa una carga de trabajo que inserta 10 millones de pedidos de ventas. Para obtener instrucciones para ejecutar una carga de trabajo reducida que inserta 1 millón de pedidos de ventas, vea las instrucciones del archivo “In-Memory OLTP\readme.txt” incluido en el archivo SQLServer2016CTP3Samples.zip.  
  
##### <a name="memory-optimized-tables"></a>Tablas optimizadas para memoria  
 Empezaremos ejecutando la carga de trabajo en las tablas optimizadas para memoria. El comando siguiente abre 100 subprocesos, cada uno de los cuales se ejecuta para 5.000 iteraciones.  Cada iteración inserta 20 pedidos de venta en transacciones diferentes. Hay 20 inserciones por iteración para compensar el hecho de que la base de datos se usa para generar los datos que se van a insertar. Esto produce un total de 20 \* 5 000 \* 100 = 10 000 000 inserciones de pedidos de venta.  
  
 Abra RML Cmd Prompt y ejecute el comando siguiente:  
  
 Haga clic en el botón Copy para copiar el comando y péguelo en el símbolo del sistema de las utilidades de RML.  
  
```console
ostress.exe -n100 -r5000 -S. -E -dAdventureWorks2016CTP3 -q -Q"DECLARE @i int = 0, @od Sales.SalesOrderDetailType_inmem, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand() * 10000, @ShipMethodID int = (rand() * 5) + 1; INSERT INTO @od SELECT OrderQty, ProductID, SpecialOfferID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*106) + 1 as int); while (@i < 20) begin; EXEC Sales.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @ShipMethodID, @od; set @i += 1 end"  
```  
  
 En un servidor de prueba con un número total de 8 núcleos físicos (16 lógicos), se tardaron 2 minutos y 5 segundos. En un segundo servidor de prueba con 24 núcleos físicos (48 lógicos), se tardó 1 minuto y 0 segundos.  
  
 Observe el uso de la CPU mientras se está ejecutando la carga de trabajo, por ejemplo con el Administrador de tareas. Verá que el uso de la CPU está cercano al 100 %. De lo contrario, tiene un cuello de botella en la E/S de registro; vea también [Solucionar problemas de pruebas de ejecución lenta](#Troubleshootingslow-runningtests).  
  
##### <a name="disk-based-tables"></a>Tablas basadas en disco  
 El comando siguiente ejecutará la carga de trabajo en tablas basadas en disco. Esta carga de trabajo puede tardar bastante tiempo en ejecutarse, lo que se debe en gran medida a la contención de bloqueos temporales del sistema. Las tablas con optimización para memoria no tienen bloqueos temporales y por tanto no experimentan este problema.  
  
 Abra RML Cmd Prompt y ejecute el comando siguiente:  
  
 Haga clic en el botón Copy para copiar el comando y péguelo en el símbolo del sistema de las utilidades de RML.  
  
```console
ostress.exe -n100 -r5000 -S. -E -dAdventureWorks2016CTP3 -q -Q"DECLARE @i int = 0, @od Sales.SalesOrderDetailType_ondisk, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand() * 10000, @ShipMethodID int = (rand() * 5) + 1; INSERT INTO @od SELECT OrderQty, ProductID, SpecialOfferID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*106) + 1 as int); while (@i < 20) begin; EXEC Sales.usp_InsertSalesOrder_ondisk @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @ShipMethodID, @od; set @i += 1 end"  
```  
  
 En un servidor de prueba con un número total de 8 núcleos físicos (16 lógicos), se tardaron 41 minutos y 25 segundos. En un segundo servidor de prueba con 24 núcleos físicos (48 lógicos), se tardaron 52 minutos y 16 segundos.  
  
 La causa principal de la diferencia de rendimiento entre las tablas optimizadas para memoria y las tablas basadas en disco en esta prueba es el hecho de que cuando se usan tablas basadas en disco, SQL Server no puede usar totalmente la CPU. El motivo es la contención de bloqueos temporales: las transacciones simultáneas intentan escribir en la misma página de datos; los bloqueos temporales se usan para asegurarse de que solo una transacción puede escribir en una página a la vez. El motor de OLTP en memoria no tiene bloqueos temporales y las filas de datos no se organizan en páginas. Por tanto, las transacciones simultáneas no bloquean las inserciones de las demás, lo que permite a SQL Server usar totalmente la CPU.  
  
 Puede observar el uso de la CPU mientras se está ejecutando la carga de trabajo, por ejemplo con el Administrador de tareas. Con las tablas basadas en disco verá que el uso de la CPU está muy alejado del 100 %. En una configuración de prueba con 16 procesadores lógicos, el uso rondaría el 24 %.  
  
 Opcionalmente, puede ver el número de esperas de bloqueos temporales por segundo mediante el Monitor de rendimiento, con el contador de rendimiento "\SQL Server:Bloqueos temporales\Esperas de bloqueos temporales/s".  
  
#### <a name="resetting-the-demo"></a>Restablecer la demostración  
 Para restablecer la demostración, abra RML Cmd Prompt y ejecute el comando siguiente:  
  
```console
ostress.exe -S. -E -dAdventureWorks2016CTP3 -Q"EXEC Demo.usp_DemoReset"  
```  
  
 Según el hardware, puede tardar unos minutos en ejecutarse.  
  
 Se recomienda restablecer la demostración tras cada ejecución. Como esta carga de trabajo solo realiza inserciones, cada ejecución consume más memoria, por lo que es necesario un restablecimiento para no quedarse sin memoria. La cantidad de memoria consumida después de una ejecución se explica en la sección [Utilización de memoria después de ejecutar la carga de trabajo](#Memoryutilizationafterrunningtheworkload).  
  
###  <a name="troubleshooting-slow-running-tests"></a><a name="Troubleshootingslow-runningtests"></a> Solucionar problemas de pruebas de ejecución lenta  
 Los resultados de prueba variarán normalmente según el hardware, y también el nivel de simultaneidad empleado en la serie de pruebas. He aquí varios aspectos que hay que examinar si los resultados no son los esperados:  
  
-   Número de transacciones simultáneas: cuando se ejecuta la carga de trabajo en un solo subproceso, el aumento del rendimiento con OLTP en memoria probablemente será menor del doble. La contención de bloqueos temporales solo supone un gran problema si hay un nivel elevado de simultaneidad.  
  
-   Pocos núcleos disponibles para SQL Server: esto significa que habrá un bajo nivel de simultaneidad en el sistema, ya que solo puede haber tantas transacciones que se ejecutan simultáneamente como núcleos disponibles haya en SQL.  
  
    -   Síntoma: si la utilización de la CPU es alta cuando se ejecuta la carga de trabajo en tablas basadas en disco, significa que no hay mucha contención, lo que apunta a una falta de simultaneidad.  
  
-   Velocidad de la unidad de registro: si la unidad de registro no puede seguir el nivel de rendimiento de transacciones del sistema, la carga de trabajo se convierte en un cuello de botella para la E/S de registro. Aunque el registro es más eficaz con OLTP en memoria, si la E/S de registro es un cuello de botella, se limita el aumento potencial de rendimiento.  
  
    -   Síntoma: si la utilización de la CPU no está cercana al 100 % o tiene muchos picos cuando se ejecuta la carga de trabajo en tablas optimizadas para memoria, es posible que haya un cuello de botella de la E/S de registro. Esto se puede confirmar abriendo el Monitor de recursos y examinando la longitud de la cola de la unidad de registro.  
  
##  <a name="memory-and-disk-space-utilization-in-the-sample"></a><a name="MemoryandDiskSpaceUtilizationintheSample"></a> Uso de memoria y de espacio en disco del ejemplo  
 A continuación se describe qué cabe esperar en cuando a uso de la memoria y del espacio en disco para la base de datos de ejemplo. También se muestran los resultados obtenidos en un servidor de prueba con 16 núcleos lógicos.  
  
###  <a name="memory-utilization-for-the-memory-optimized-tables"></a><a name="Memoryutilizationforthememory-optimizedtables"></a> Uso de memoria para las tablas optimizadas para memoria  
  
#### <a name="overall-utilization-of-the-database"></a>Utilización global de la base de datos  
 Se puede usar la consulta siguiente para obtener la utilización de memoria total para OLTP en memoria en el sistema.  
  
```sql
SELECT type  
   , name  
, pages_kb/1024 AS pages_MB   
FROM sys.dm_os_memory_clerks WHERE type LIKE '%xtp%'  
```  
  
 Instantánea justo después de crearse la base de datos:  
  
| type | name | pages_MB |
| ---- | ---- | -------- |
|MEMORYCLERK_XTP|Valor predeterminado|94|  
|MEMORYCLERK_XTP|DB_ID_5|877|  
|MEMORYCLERK_XTP|Valor predeterminado|0|  
|MEMORYCLERK_XTP|Valor predeterminado|0|  
  
 Los distribuidores de memoria predeterminados contienen estructuras de memoria de todo el sistema y son relativamente pequeños. El distribuidor de memoria para la base de datos de usuario (en este caso, la base de datos con el identificador 5) tiene unos 900 MB.  
  
#### <a name="memory-utilization-per-table"></a>Utilización de memoria por tabla  
 Se puede usar la consulta siguiente para explorar en profundidad la utilización de memoria de las tablas individuales y sus índices:  
  
```sql
SELECT object_name(t.object_id) AS [Table Name]  
     , memory_allocated_for_table_kb  
 , memory_allocated_for_indexes_kb  
FROM sys.dm_db_xtp_table_memory_stats dms JOIN sys.tables t   
ON dms.object_id=t.object_id  
WHERE t.type='U'  
```  
  
 La tabla siguiente muestra los resultados de esta consulta para una instalación nueva del ejemplo:  
  
| Nombre de la tabla | memory_allocated_for_table_kb | memory_allocated_for_indexes_kb |
| ---------- | ----------------------------- | ------------------------------- |
|SpecialOfferProduct_inmem|64|3840|  
|DemoSalesOrderHeaderSeed|1984|5504|  
|SalesOrderDetail_inmem|15316|663552|  
|DemoSalesOrderDetailSeed|64|10432|  
|SpecialOffer_inmem|3|8192|  
|SalesOrderHeader_inmem|7168|147456|  
|Product_inmem|124|12352|  

 Como puede ver, las tablas son bastante pequeñas: SalesOrderHeader_inmem tiene unos 7 MB de tamaño y SalesOrderDetail_inmem, unos 15 MB.  
  
 Lo sorprendente aquí es el tamaño de la memoria asignada para los índices, en comparación con el tamaño de los datos de tabla. Esto se debe a que los índices hash del ejemplo tienen establecido previamente un tamaño de datos mayor. Observe que los índices hash tienen un tamaño fijo y por tanto su tamaño no crece junto con el tamaño de los datos de la tabla.  
  
####  <a name="memory-utilization-after-running-the-workload"></a><a name="Memoryutilizationafterrunningtheworkload"></a> Utilización de memoria después de ejecutar la carga de trabajo  
 Después de insertar 10 millones de pedidos de venta, la utilización total de memoria es similar a lo siguiente:  
  
```sql
SELECT type  
, name  
, pages_kb/1024 AS pages_MB   
FROM sys.dm_os_memory_clerks WHERE type LIKE '%xtp%'  
```  
  
| type | name | pages_MB |
| ---- | ---- | -------- |
|MEMORYCLERK_XTP|Valor predeterminado|146|  
|MEMORYCLERK_XTP|DB_ID_5|7374|  
|MEMORYCLERK_XTP|Valor predeterminado|0|  
|MEMORYCLERK_XTP|Valor predeterminado|0|  

 Como puede ver, SQL Server usa un bit por debajo de 8 GB para las tablas y los índices optimizados para memoria de la base de datos de ejemplo.  
  
 Examinando el uso detallado de memoria por tabla después de ejecutar un ejemplo:  
  
```sql
SELECT object_name(t.object_id) AS [Table Name]  
     , memory_allocated_for_table_kb  
 , memory_allocated_for_indexes_kb  
FROM sys.dm_db_xtp_table_memory_stats dms JOIN sys.tables t   
ON dms.object_id=t.object_id  
WHERE t.type='U'  
```  
  
| Nombre de la tabla | memory_allocated_for_table_kb | memory_allocated_for_indexes_kb |
| ---------- | ----------------------------- | ------------------------------- |
|SalesOrderDetail_inmem|5113761|663552|  
|DemoSalesOrderDetailSeed|64|10368|  
|SpecialOffer_inmem|2|8192|  
|SalesOrderHeader_inmem|1575679|147456|  
|Product_inmem|111|12032|  
|SpecialOfferProduct_inmem|64|3712|  
|DemoSalesOrderHeaderSeed|1984|5504|  

 Podemos ver un total de unos 6,5 GB de datos. Observe que el tamaño de los índices de la tabla SalesOrderHeader_inmem y SalesOrderDetail_inmem es el mismo que el tamaño de los índices antes de insertar los pedidos de venta. El tamaño del índice no cambió porque ambas tablas emplean índices hash y los índices hash son estáticos.  
  
#### <a name="after-demo-reset"></a>Después de restablecer la demostración  
 Se puede usar el procedimiento almacenado Demo.usp_DemoReset para restablecer la demostración. Elimina los datos de las tablas SalesOrderHeader_inmem y SalesOrderDetail_inmem, y reinicializa los datos de las tablas originales SalesOrderHeader y SalesOrderDetail.  
  
 Ahora, aunque las filas de las tablas se han eliminado, esto no significa que la memoria se recupera inmediatamente. SQL Server recupera memoria de las filas eliminadas de las tablas optimizadas para memoria en segundo plano, según sea necesario. Verá que inmediatamente después de restablecer la demostración, sin ninguna carga de trabajo transaccional en el sistema, la memoria de las filas eliminadas aún no se ha recuperado:  
  
```sql
SELECT type  
, name  
, pages_kb/1024 AS pages_MB   
FROM sys.dm_os_memory_clerks WHERE type LIKE '%xtp%'  
```  
  
| type | name | pages_MB |
| ---- | ---- | -------- |
|MEMORYCLERK_XTP|Valor predeterminado|2261|  
|MEMORYCLERK_XTP|DB_ID_5|7396|  
|MEMORYCLERK_XTP|Valor predeterminado|0|  
|MEMORYCLERK_XTP|Valor predeterminado|0|  

 Esto es lo esperado: la memoria se recuperará cuando se ejecute la carga de trabajo transaccional.  
  
 Si inicia una segunda ejecución de la carga de trabajo de demostración, verá que la utilización de memoria disminuye inicialmente, a medida que se limpian las filas eliminadas previamente. En algún momento el tamaño de la memoria aumentará de nuevo, hasta que finalice la carga de trabajo. Después de insertar 10 millones de filas después de restablecer la demostración, el uso de memoria será muy similar al uso después de la primera ejecución. Por ejemplo:  
  
```sql
SELECT type  
, name  
, pages_kb/1024 AS pages_MB   
FROM sys.dm_os_memory_clerks WHERE type LIKE '%xtp%'  
```  
  
| type | name | pages_MB |
| ---- | ---- | -------- |
|MEMORYCLERK_XTP|Valor predeterminado|1863|  
|MEMORYCLERK_XTP|DB_ID_5|7390|  
|MEMORYCLERK_XTP|Valor predeterminado|0|  
|MEMORYCLERK_XTP|Valor predeterminado|0|  

### <a name="disk-utilization-for-memory-optimized-tables"></a>Uso de disco para las tablas optimizadas para memoria  
 El tamaño total en disco de los archivos de punto de comprobación de una base de datos en un momento dado se puede averiguar con la consulta:  
  
```sql
SELECT SUM(df.size) * 8 / 1024 AS [On-disk size in MB]  
FROM sys.filegroups f JOIN sys.database_files df   
   ON f.data_space_id=df.data_space_id  
WHERE f.type=N'FX'  
  
```  
  
#### <a name="initial-state"></a>Estado inicial  
 Cuando se crean inicialmente el grupo de archivos de ejemplo y las tablas optimizadas para memoria de ejemplo, se crean previamente varios archivos de punto de comprobación y el sistema empieza a rellenar los archivos; el número de archivos de punto de comprobación creados previamente depende del número de procesadores lógicos del sistema. Como el ejemplo es inicialmente muy pequeño, los archivos creados previamente estarán vacíos en su mayoría después de la creación inicial.  
  
 El código siguiente muestra el tamaño inicial en disco del ejemplo en un equipo con 16 procesadores lógicos:  
  
```sql
SELECT SUM(df.size) * 8 / 1024 AS [On-disk size in MB]  
FROM sys.filegroups f JOIN sys.database_files df   
   ON f.data_space_id=df.data_space_id  
WHERE f.type=N'FX'  
```  
  
| On-disk size in MB |
| ------------------ |
|2312|  

 Como puede ver, hay una gran discrepancia entre el tamaño en disco de los archivos de punto de comprobación, que es de 2,3 GB, y el tamaño de datos real, que es más cercano a 30 MB.  
  
 Para examinar más de cerca de dónde procede la utilización de espacio en disco, puede usar la consulta siguiente. El tamaño del disco devuelto por esta consulta es aproximado en el caso de los archivos que tienen el estado 5 (REQUIRED FOR BACKUP/HA), 6 (IN TRANSITION TO TOMBSTONE) o 7 (TOMBSTONE).  
  
```sql
SELECT state_desc  
 , file_type_desc  
 , COUNT(*) AS [count]  
 , SUM(CASE  
   WHEN state = 5 AND file_type=0 THEN 128*1024*1024  
   WHEN state = 5 AND file_type=1 THEN 8*1024*1024  
   WHEN state IN (6,7) THEN 68*1024*1024  
   ELSE file_size_in_bytes  
    END) / 1024 / 1024 AS [on-disk size MB]   
FROM sys.dm_db_xtp_checkpoint_files  
GROUP BY state, state_desc, file_type, file_type_desc  
ORDER BY state, file_type  
```  
  
 Para el estado inicial del ejemplo, el resultado será similar al de un servidor con 16 procesadores lógicos:  
  
| state_desc | file_type_desc | count | on-disk size MB |
| ---------- | -------------- | ----- | --------------- |
|PRECREATED|DATA|16|2048|  
|PRECREATED|DELTA|16|128|  
|UNDER CONSTRUCTION|DATA|1|128|  
|UNDER CONSTRUCTION|DELTA|1|8|  

 Como puede ver, la mayor parte del espacio la usan archivos de datos y delta creados previamente. SQL Server creó previamente un par de archivos (datos, delta) por procesador lógico. Además, los archivos de datos tienen un tamaño previo de 128 MB, y los archivos delta, de 8 MB, para que la inserción de datos en estos archivos sea más eficaz.  
  
 Los datos reales de las tablas optimizadas para memoria están en el archivo de datos.  
  
#### <a name="after-running-the-workload"></a>Después de ejecutar la carga de trabajo  
 Después de una única serie de pruebas que inserta 10 millones de pedidos de venta, el tamaño total en disco es similar al siguiente (para un servidor de prueba de 16 núcleos):  
  
```sql
SELECT SUM(df.size) * 8 / 1024 AS [On-disk size in MB]  
FROM sys.filegroups f JOIN sys.database_files df   
   ON f.data_space_id=df.data_space_id  
WHERE f.type=N'FX'  
```  
  
| On-disk size in MB |
| ------------------ |
|8828|
  
 El tamaño en disco es cercano a 9 GB, que es parecido al tamaño en memoria de los datos.  
  
 Si examinamos más de cerca los tamaños de los archivos de punto de comprobación en los distintos estados:  
  
```sql
SELECT state_desc  
 , file_type_desc  
 , COUNT(*) AS [count]  
 , SUM(CASE  
   WHEN state = 5 AND file_type=0 THEN 128*1024*1024  
   WHEN state = 5 AND file_type=1 THEN 8*1024*1024  
   WHEN state IN (6,7) THEN 68*1024*1024  
   ELSE file_size_in_bytes  
    END) / 1024 / 1024 AS [on-disk size MB]   
FROM sys.dm_db_xtp_checkpoint_files  
GROUP BY state, state_desc, file_type, file_type_desc  
ORDER BY state, file_type  
```  
  
| state_desc | file_type_desc | count | on-disk size MB |
| ---------- | -------------- | ----- | --------------- |
|PRECREATED|DATA|16|2048|  
|PRECREATED|DELTA|16|128|  
|UNDER CONSTRUCTION|DATA|1|128|  
|UNDER CONSTRUCTION|DELTA|1|8|  

 Todavía tenemos 16 pares de archivos creados previamente, listos a medida que se cierran los puntos de comprobación.  
  
 Hay un par en construcción, que se usa hasta que se cierra el punto de comprobación actual. Junto con los archivos de punto de comprobación activos, esto nos da unos 6,5 GB de uso de disco para 6,5 GB de datos en memoria. Recuerde que los índices no se conservan en disco, por lo que el tamaño total en disco es menor que el tamaño en memoria en este caso.  
  
#### <a name="after-demo-reset"></a>Después de restablecer la demostración  
 Después de restablecer la demostración, el espacio en disco no se recupera inmediatamente si no hay ninguna carga de trabajo transaccional en el sistema, y no hay ningún punto de comprobación de la base de datos. Para que los archivos de punto de comprobación pasen por sus diferentes etapas y después se descarten, tiene que haber varios puntos de comprobación y eventos de truncamiento del registro, con el fin de iniciar la combinación de los archivos de punto de comprobación e iniciar la recopilación de elementos no utilizados. Estos ocurrirá automáticamente si tiene una carga de trabajo transaccional en el sistema [y realiza copias de seguridad de registros periódicas, en caso de que esté utilizando el modelo de recuperación completa], pero no cuando el sistema está inactivo, como ocurre en un escenario de demostración.  
  
 En el ejemplo, después de restablecer la demostración, puede ver algo similar a lo siguiente:  
  
```sql
SELECT SUM(df.size) * 8 / 1024 AS [On-disk size in MB]  
FROM sys.filegroups f JOIN sys.database_files df   
   ON f.data_space_id=df.data_space_id  
WHERE f.type=N'FX'  
```  
  
| On-disk size in MB |
| ------------------ |
|11839|
  
 Con casi 12 GB, esto es mucho más significativo que los 9 GB teníamos antes de restablecer la demostración. Esto se debe a que se han iniciado algunas combinaciones de archivos de punto de comprobación, pero algunos destinos de mezcla todavía no se han instalado, y algunos de los archivos de origen de mezcla todavía no se han limpiado, como se puede ver aquí:  
  
```sql
SELECT state_desc  
 , file_type_desc  
 , COUNT(*) AS [count]  
 , SUM(CASE  
   WHEN state = 5 AND file_type=0 THEN 128*1024*1024  
   WHEN state = 5 AND file_type=1 THEN 8*1024*1024  
   WHEN state IN (6,7) THEN 68*1024*1024  
   ELSE file_size_in_bytes  
    END) / 1024 / 1024 AS [on-disk size MB]   
FROM sys.dm_db_xtp_checkpoint_files  
GROUP BY state, state_desc, file_type, file_type_desc  
ORDER BY state, file_type  
```  
  
| state_desc | file_type_desc | count | on-disk size MB |
| ---------- | -------------- | ----- | --------------- |
|PRECREATED|DATA|16|2048|  
|PRECREATED|DELTA|16|128|  
|ACTIVO|DATA|38|5152|  
|ACTIVO|DELTA|38|1331|  
|MERGE TARGET|DATA|7|896|  
|MERGE TARGET|DELTA|7|56|  
|MERGED SOURCE|DATA|13|1772|  
|MERGED SOURCE|DELTA|13|455|  

 Los destinos de mezcla se instalan y el origen de mezcla se limpia mientras la realiza actividad transaccional en el sistema.  
  
 Después de una segunda ejecución de la carga de trabajo de demostración, insertando 10 millones de pedidos de venta después de restablecer la demostración, verá que los archivos creados durante la primera ejecución de la carga de trabajo se han limpiado. Si ejecuta la consulta anterior varias veces mientras se está ejecutando la carga de trabajo, puede ver que los archivos de punto de comprobación pasaron por las distintas fases.  
  
 Después de que la segunda ejecución de la carga de trabajo inserta 10 millones de pedidos de venta, verá un uso de disco muy similar, aunque no necesariamente igual después de la primera ejecución, porque el sistema es dinámico por naturaleza. Por ejemplo:  
  
```sql
SELECT state_desc  
 , file_type_desc  
 , COUNT(*) AS [count]  
 , SUM(CASE  
   WHEN state = 5 AND file_type=0 THEN 128*1024*1024  
   WHEN state = 5 AND file_type=1 THEN 8*1024*1024  
   WHEN state IN (6,7) THEN 68*1024*1024  
   ELSE file_size_in_bytes  
    END) / 1024 / 1024 AS [on-disk size MB]   
FROM sys.dm_db_xtp_checkpoint_files  
GROUP BY state, state_desc, file_type, file_type_desc  
ORDER BY state, file_type  
```  
  
| state_desc | file_type_desc | count | on-disk size MB |
| ---------- | -------------- | ----- | --------------- |
|PRECREATED|DATA|16|2048|  
|PRECREATED|DELTA|16|128|  
|UNDER CONSTRUCTION|DATA|2|268|  
|UNDER CONSTRUCTION|DELTA|2|16|  
|ACTIVO|DATA|41|5608|  
|ACTIVO|DELTA|41|328|  

 En este caso, hay dos pares de archivos de punto de comprobación en el estado "under construction", lo que significa que varios pares de archivos pasaron por el estado "under construction", probablemente debido al elevado nivel de simultaneidad de la carga de trabajo. Varios subprocesos simultáneos necesitaron un nuevo par de archivos al mismo tiempo, por lo que se movió un par de "precreated" a "under construction".  
  
## <a name="see-also"></a>Consulte también

[OLTP en memoria &#40;optimización en memoria&#41;](in-memory-oltp-in-memory-optimization.md)