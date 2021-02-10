---
title: Subexpresión común
description: Muestra la mejora de consultas de ejemplo que se presentó en Analytics Platform System CU 7.3.
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 12/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
monikerRange: '>= aps-pdw-2016-au7'
ms.openlocfilehash: d1d60b85bec6ec810cc506e1002dbc8ecc60ed50
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052796"
---
# <a name="common-subexpression-elimination-explained"></a>Explicación de la eliminación de subexpresiones comunes

APS CU 7.3 mejora el rendimiento de las consultas con una eliminación común de subexpresiones en el optimizador de consultas de SQL. La mejora mejora las consultas de dos maneras. La primera ventaja es que la capacidad de identificar y eliminar estas expresiones ayuda a reducir el tiempo de compilación de SQL. La segunda ventaja y más importante son las operaciones de movimiento de datos para estas subexpresiones redundantes que se eliminan, por lo que el tiempo de ejecución de las consultas es más rápido.

```sql
select top 100 asceding.rnk, i1.i_product_name best_performing, i2.i_product_name worst_performing
  from(select *
       from (select item_sk,rank() over (order by rank_col asc) rnk
             from (select ss_item_sk item_sk,avg(ss_net_profit) rank_col
                   from store_sales ss1
                   where ss_store_sk = 8
                   group by ss_item_sk
                   having avg(ss_net_profit) > 0.9*(select avg(ss_net_profit) rank_col
                                                    from store_sales
                                                    where ss_store_sk = 8
                                                      and ss_hdemo_sk is null
                                                    group by ss_store_sk))V1)V11
       where rnk  < 11) asceding,
      (select *
       from (select item_sk,rank() over (order by rank_col desc) rnk
             from (select ss_item_sk item_sk,avg(ss_net_profit) rank_col
                   from store_sales ss1
                   where ss_store_sk = 8
                   group by ss_item_sk
                   having avg(ss_net_profit) > 0.9*(select avg(ss_net_profit) rank_col
                                                    from store_sales
                                                    where ss_store_sk = 8
                                                      and ss_hdemo_sk is null
                                                    group by ss_store_sk))V2)V21
       where rnk  < 11) descending,
  item i1,
  item i2
  where asceding.rnk = descending.rnk
    and i1.i_item_sk=asceding.item_sk
    and i2.i_item_sk=descending.item_sk
  order by asceding.rnk
  ;
```
Considere la consulta anterior de las herramientas de pruebas comparativas TPC-DS.  En la consulta anterior, la subconsulta es la misma, pero la cláusula order by con la función Rank () sobre se ordena de dos maneras diferentes. Anterior a CU 7.3, esta subconsulta se evaluará y se ejecutará dos veces, una para el orden ascendente y otra para el orden descendente, incurriendo en dos operaciones de movimiento de datos. Después de instalar APS CU 7.3, la parte de la subconsulta se evaluará una vez, lo que reduce el movimiento de datos y finaliza la consulta más rápidamente.

Hemos introducido un [modificador de características](appliance-feature-switch.md) denominado ' OptimizeCommonSubExpressions ' que le permitirá probar la característica incluso después de actualizar a APS cu 7.3. La característica está activada de forma predeterminada, pero se puede desactivar. 

> [!NOTE] 
> Los cambios en los valores del modificador de características requieren un reinicio del servicio.

Puede probar la consulta de ejemplo creando las tablas siguientes en el entorno de prueba y evaluando el plan de explicación de la consulta mencionada anteriormente. 

```sql
CREATE TABLE [dbo].[store_sales] (
    [ss_sold_date_sk] int NULL, 
    [ss_sold_time_sk] int NULL, 
    [ss_item_sk] int NOT NULL, 
    [ss_customer_sk] int NULL, 
    [ss_cdemo_sk] int NULL, 
    [ss_hdemo_sk] int NULL, 
    [ss_addr_sk] int NULL, 
    [ss_store_sk] int NULL, 
    [ss_promo_sk] int NULL, 
    [ss_ticket_number] int NOT NULL, 
    [ss_quantity] int NULL, 
    [ss_wholesale_cost] decimal(7, 2) NULL, 
    [ss_list_price] decimal(7, 2) NULL, 
    [ss_sales_price] decimal(7, 2) NULL, 
    [ss_ext_discount_amt] decimal(7, 2) NULL, 
    [ss_ext_sales_price] decimal(7, 2) NULL, 
    [ss_ext_wholesale_cost] decimal(7, 2) NULL, 
    [ss_ext_list_price] decimal(7, 2) NULL, 
    [ss_ext_tax] decimal(7, 2) NULL, 
    [ss_coupon_amt] decimal(7, 2) NULL, 
    [ss_net_paid] decimal(7, 2) NULL, 
    [ss_net_paid_inc_tax] decimal(7, 2) NULL, 
    [ss_net_profit] decimal(7, 2) NULL
)
WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = HASH([ss_item_sk]),  PARTITION ([ss_sold_date_sk] RANGE RIGHT FOR VALUES (2450815, 2451180, 2451545, 2451911, 2452276, 2452641, 2453006)));

CREATE TABLE [dbo].[item] (
    [i_item_sk] int NOT NULL, 
    [i_item_id] char(16) COLLATE Latin1_General_100_CI_AS_KS_WS NOT NULL, 
    [i_rec_start_date] date NULL, 
    [i_rec_end_date] date NULL, 
    [i_item_desc] varchar(200) COLLATE Latin1_General_100_CI_AS_KS_WS NULL, 
    [i_current_price] decimal(7, 2) NULL, 
    [i_wholesale_cost] decimal(7, 2) NULL, 
    [i_brand_id] int NULL, 
    [i_brand] char(50) COLLATE Latin1_General_100_CI_AS_KS_WS NULL, 
    [i_class_id] int NULL, 
    [i_class] char(50) COLLATE Latin1_General_100_CI_AS_KS_WS NULL, 
    [i_category_id] int NULL, 
    [i_category] char(50) COLLATE Latin1_General_100_CI_AS_KS_WS NULL, 
    [i_manufact_id] int NULL, 
    [i_manufact] char(50) COLLATE Latin1_General_100_CI_AS_KS_WS NULL, 
    [i_size] char(20) COLLATE Latin1_General_100_CI_AS_KS_WS NULL, 
    [i_formulation] char(20) COLLATE Latin1_General_100_CI_AS_KS_WS NULL, 
    [i_color] char(20) COLLATE Latin1_General_100_CI_AS_KS_WS NULL, 
    [i_units] char(10) COLLATE Latin1_General_100_CI_AS_KS_WS NULL, 
    [i_container] char(10) COLLATE Latin1_General_100_CI_AS_KS_WS NULL, 
    [i_manager_id] int NULL, 
    [i_product_name] char(50) COLLATE Latin1_General_100_CI_AS_KS_WS NULL
)
WITH (CLUSTERED INDEX ( [i_item_sk] ASC ), DISTRIBUTION = REPLICATE);
```
Si echa un vistazo al plan explicado de la consulta, verá que antes de la CU 7.3 (o cuando el modificador de características está desactivado), la consulta tiene 17 número total de operaciones y después de CU 7.3 (o con el modificador de características activado) la misma consulta muestra 9 número total de operaciones. Si solo cuenta las operaciones de movimiento de datos, verá que el plan anterior tiene cuatro operaciones de movimiento frente a dos operaciones de movimiento en el nuevo plan. El nuevo optimizador de consultas pudo reducir dos operaciones de movimiento de datos mediante la reutilización de la tabla temporal que ya se creó con el nuevo plan, lo que reduce el tiempo de ejecución de la consulta. 


