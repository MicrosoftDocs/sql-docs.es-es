---
description: Modos DE SQL (MySQLToSQL)
title: Modos SQL (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: d840ee51-b863-4e77-84aa-37d3f094bfed
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: e09a38ce70b4f144f6da324525a7fee925d586a9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074477"
---
# <a name="sql-modes-mysqltosql"></a>Modos DE SQL (MySQLToSQL)
SSMA para MySQL puede funcionar en diferentes modos SQL y puede aplicar estos modos de manera diferente para los distintos clientes.  
  
Los modos definen la sintaxis SQL que debe admitir MySQL y el tipo de comprobaciones de validación de datos que debe realizar. Esto facilita el uso de MySQL en entornos diferentes y el uso de MySQL con SQL Server.  
  
## <a name="sql-modes-grid"></a>Cuadrícula modos SQL:  
  
-   La cuadrícula modos SQL en el nivel raíz contiene las columnas siguientes: **nombre del modo SQL**, **modos SQL cargados** y **modos SQL efectivos**.  
  
-   Cuadrícula de modos SQL en bases de datos categoría, base de datos, categoría de tabla, categoría de instrucciones, categoría de vistas, tabla, vista, funciones, procedimientos, UDF y nivel de objeto de evento contiene las columnas siguientes: **nombre del modo SQL**, **modos SQL heredados** y **modos efectivos SQL**.  
  
-   La cuadrícula modos SQL en el procedimiento almacenado, la función almacenada y el nivel de desencadenador contiene las columnas siguientes: **nombre del modo SQL**,  **modos SQL originales** y **modos efectivos SQL**.  
  
> [!NOTE]  
> Los modos de grupo se mostrarán en negrita, en la columna "nombre del modo SQL".  
  
## <a name="loaded-sql-modes"></a>Modos SQL cargados  
Estos son los modos SQL, que se establecen en el nivel de sesión o raíz. Los modos SQL una vez cargados en la base de datos de destino no se pueden editar ni modificar.  
  
## <a name="inherited-sql-modes"></a>Modos SQL heredados  
Estos son los modos SQL, que se heredan del nodo primario correspondiente.  
  
Excepto en el caso de la categoría de funciones, la categoría de procedimientos, la categoría de eventos y los desencadenadores, estos modos SQL están presentes en todos los niveles (base de datos, categoría de tabla, categoría de instrucciones, categoría de vistas, tabla, vista, funciones, procedimientos, UDF y objeto de evento).  
  
> [!NOTE]  
> Al activar la casilla **heredar de primario** , los modos SQL heredados se pueden heredar del nodo primario. De forma predeterminada, esta casilla permanece activada.  
  
## <a name="original-sql-modes"></a>Modos SQL originales  
Estos son los modos SQL presentes solo en los niveles de función, procedimiento y desencadenador.  
  
> [!NOTE]  
> Al activar la casilla **usar original** , se pueden usar los modos SQL que se usaron originalmente en la función, el procedimiento o el desencadenador correspondiente. De forma predeterminada, esta casilla permanece activada.  
  
## <a name="effective-sql-modes"></a>Modos SQL efectivos  
Los modos de SQL efectivos se pueden definir en distintos niveles de la siguiente manera:  
  
-   **En el nivel de sesión:**  
  
    1.  Se puede llamar a todos los modos SQL cargados, "modos SQL efectivos".  
  
    2.  En este nivel, los modos SQL efectivos se pueden modificar directamente y de forma explícita.  
  
    3.  El modo de SQL efectivo que se establece explícitamente no se refleja como modo de SQL cargado y, finalmente, se aplica al objeto.  
  
-   **En el nivel de función o procedimiento o desencadenador:**  
  
    1.  Se puede llamar a todos los modos SQL originales, "modos SQL efectivos".  
  
    2.  En este nivel, el modo SQL efectivo solo se puede modificar de forma explícita si la casilla **usar original** está desactivada.  
  
    3.  El modo de SQL efectivo que se establece explícitamente no se refleja como el modo SQL original y, finalmente, se aplica al objeto.  
  
-   **En nodos distintos del nivel de función o procedimiento o desencadenador:**  
  
    1.  Se puede llamar a todos los modos SQL heredados, "modos SQL efectivos".  
  
    2.  En este nivel, el modo SQL efectivo solo se puede modificar de forma explícita cuando la casilla **heredar de elemento primario** está desactivada.  
  
    3.  El modo de SQL efectivo que se establece explícitamente no se refleja como modo SQL heredado y, por último, se aplica al objeto.  
  
