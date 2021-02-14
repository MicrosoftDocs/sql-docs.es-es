---
title: Wide World Importers-base de datos de ejemplo para SQL | Microsoft Docs
description: Obtenga información sobre cómo las bases de datos de ejemplo WideWorldImporters admiten los flujos de trabajo de la empresa ficticia WideWorldImporters.
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.custom: ''
ms.date: 04/04/2018
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: f844299a392f9521afe3af7d72bc72272304d3a5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354042"
---
# <a name="wide-world-importers-sample-databases-for-microsoft-sql"></a>Bases de datos de ejemplo de Wide World Importers para Microsoft SQL
[!INCLUDE [SQL Server Azure SQL Database](../includes/applies-to-version/sql-asdb.md)]
Se trata de información general de los importadores de World Wide World de la empresa ficticia y de los flujos de trabajo que se tratan en las bases de datos de ejemplo de WideWorldImporters para SQL Server y Azure SQL Database.  

Wide World Importers (WWI) es un importador y distribuidor de productos de primera categoría, que funciona desde el área de la bahía de San Francisco.

Como mayorista, los clientes de WWI son principalmente empresas que revenden a individuos. WWI vende a los clientes de venta al por menor en el Estados Unidos como tiendas especializadas, supermercados, almacenes informáticos, tiendas de atracción turística y otras personas. WWI también vende a otros mayoristas a través de una red de agentes que promocionan los productos en el nombre de WWI. A pesar de que todos los clientes de WWI se basan actualmente en el Estados Unidos, la empresa está intentando impulsar la expansión en otros países.

WWI compra mercancías de proveedores, incluidos los fabricantes de la novedad y el juguete, y otros mayoristas. Almacenan las mercancías en el almacén de WWI y reordenan a los proveedores según sea necesario para satisfacer los pedidos de los clientes. También compran grandes volúmenes de materiales de embalaje y los venden en cantidades más pequeñas, por comodidad para los clientes.

Recientemente, WWI comenzó a vender una variedad de Novelties comestibles, como chocolates de Chile.  La compañía no tenía que administrar los artículos refrigerados previamente. Ahora, para satisfacer los requisitos de tratamiento de los alimentos, deben supervisar la temperatura de su habitación refrigerante y cualquiera de sus camiones que tengan secciones más frías.

## <a name="workflow-for-warehouse-stock-items"></a>Flujo de trabajo para los elementos de almacén

El flujo típico de cómo se almacenan y distribuyen los elementos es el siguiente:
- WWI crea pedidos de compra y envía los pedidos a los proveedores.
- Los proveedores envían los elementos, WWI los recibe y los almacena en su almacén.
- Los clientes ordenan los elementos de WWI
- WWI rellena el pedido del cliente con los artículos de las existencias en el almacén y, cuando no tienen existencias suficientes, ordenan las existencias adicionales de los proveedores.
- Algunos clientes no quieren esperar los elementos que no están en existencias. Si indican cinco elementos bursátiles diferentes y cuatro están disponibles, quieren recibir los cuatro elementos y hacer un pedido pendiente del elemento restante. Después, el elemento se enviaría posteriormente en un envío independiente.
- WWI factura a los clientes por los artículos de acciones, normalmente convirtiendo el pedido en una factura.
- Los clientes pueden solicitar artículos que no están en existencias. Estos elementos están ordenados.
- WWI entrega productos a los clientes a través de sus propios furgonetas de entrega, o bien a través de otros métodos de envío o transporte.
- Los clientes pagan las facturas a WWI.
- Periódicamente, WWI paga a los proveedores los artículos que se encontraban en los pedidos de compra. Esto suele ser un momento después de haber recibido los productos.

## <a name="data-warehouse-and-analysis-workflow"></a>Flujo de trabajo de análisis y almacenamiento de datos

Aunque el equipo de WWI usa SQL Server Reporting Services para generar informes operativos de la base de datos de WideWorldImporters, también deben realizar análisis sobre sus datos y tener que generar informes estratégicos. El equipo ha creado un modelo de datos dimensionales en una base de datos WideWorldImportersDW. Esta base de datos se rellena con un paquete de Integration Services.

SQL Server Analysis Services se usa para crear modelos de datos analíticos a partir de los datos del modelo de datos dimensionales. SQL Server Reporting Services se usa para generar informes estratégicos directamente desde el modelo de datos dimensionales y también desde el modelo analítico. Power BI se usa para crear paneles a partir de los mismos datos. Los paneles se usan en sitios web y en teléfonos y tabletas. *Nota: estos modelos de datos e informes todavía no están disponibles*

## <a name="additional-workflows"></a>Flujos de trabajo adicionales

Estos son flujos de trabajo adicionales.
- WWI emite notas de crédito cuando un cliente no recibe la buena por alguna razón o cuando las mercancías están defectuosas. Se tratan como facturas negativas.
- WWI cuenta periódicamente las cantidades de existencias disponibles para asegurarse de que las cantidades de existencias que se muestran como disponibles en su sistema son precisas. (El proceso de hacer esto se denomina stocktake).
- Temperaturas del salón frío. Las mercancías perecederos se almacenan en salones frigoríficos. Los datos de los sensores de estos salones se ingestan en la base de datos con fines de supervisión y análisis.
- Seguimiento de la ubicación del vehículo. Los vehículos que transporten mercancías para WWI incluyen sensores que realizan el seguimiento de la ubicación. Esta ubicación se ingesta de nuevo en la base de datos para la supervisión y análisis adicionales.

## <a name="fiscal-year"></a>Año fiscal

La empresa opera con un año financiero que comienza el 1 de noviembre.

## <a name="terms-of-use"></a>Términos de uso

La licencia para la base de datos de ejemplo y el código de ejemplo se describen aquí: [license.txt](https://github.com/Microsoft/sql-server-samples/blob/master/license.txt)

La base de datos de ejemplo incluye datos públicos que se han cargado desde data.gov y natural EarthData. Los términos de uso están aquí: [https://www.naturalearthdata.com/about/terms-of-use/](https://www.naturalearthdata.com/about/terms-of-use/)
