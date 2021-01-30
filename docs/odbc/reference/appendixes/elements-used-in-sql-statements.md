---
description: Elementos que se usan en instrucciones SQL
title: Elementos usados en instrucciones SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQL statements [ODBC], elements supported
- minimum SQL syntax supported [ODBC]
- ODBC drivers [ODBC], minimum SQL syntax supported
ms.assetid: 85777525-1555-4731-8309-63a464c6b43a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5911de3ce17fc0557b9fad885fa7cdc6ce5024ac
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194872"
---
# <a name="elements-used-in-sql-statements"></a>Elementos que se usan en instrucciones SQL
Los elementos siguientes se utilizan en las instrucciones SQL mencionadas anteriormente.  
  
## <a name="element"></a>Elemento  
 *identificador de tabla base* :: = *nombre definido por el usuario*  
  
 *nombre de tabla base* :: = *identificador de tabla base*  
  
 *Boolean-factor* :: = [Not] *Boolean-Primary*  
  
 *Boolean-Primary* :: = Comparison *-Predicate* &#124; ( *Search-Condition* )  
  
 *Boolean-term* :: = *factor booleano* [y *Boolean-term*]  
  
 *carácter-cadena-literal* :: = ' ' {*carácter*}... ' ' (el *carácter* es cualquier carácter del juego de caracteres del controlador o el origen de datos. Para incluir un solo carácter de comilla literal (' ') en un literal de cadena de caracteres, use dos caracteres de Comillas literales [' ' ' '].)  
  
 *Column-Identifier* :: = *nombre definido por el usuario*  
  
 *Column-Name* :: = [*nombre de tabla*] *identificador de columna*  
  
 *Comparison-Operator* :: = < &#124; > &#124; \<= &#124; > = &#124; = &#124; <>  
  
 *Comparison-Predicate* : *: = Expression* Comparison-expresión de operador  
  
 *tipo de datos* :: = *carácter-cadena-tipo* (*carácter-cadena-tipo* es cualquier tipo de datos para el que la columna "data_type" del conjunto de resultados devuelto por SQLGetTypeInfo es SQL_CHAR o SQL_VARCHAR).  
  
 *digit* :: = 0 &#124; 1 &#124; 2 &#124; 3 &#124; 4 &#124; 5 &#124; 6 &#124; 7 &#124; 8 &#124; 9  
  
 *parámetro dinámico* :: =?  
  
 *expresión* :: = término &#124; expresión {+&#124;-} término  
  
 *factor* :: = [ *+*&#124;*-* ]*principal*  
  
 *Insert-Value* :: =  
  
 *parámetro dinámico*  
  
 *Literal* de &#124;  
  
 &#124; NULL  
  
 &#124; USUARIO  
  
 *Letter* :: = *letra minúscula &#124;* letra mayúscula.  
  
 *literal* :: = *carácter-cadena-literal*  
  
 *lower-case-Letter* :: = a &#124; b &#124; c &#124; d &#124; e &#124; f &#124; g &#124; h &#124; i &#124; j &#124; k &#124; &#124; &#124; m &#124; n &#124; o &#124; p &#124; q &#124; r &#124; s &#124; t &#124; u &#124; v &#124; w &#124; x &#124; y z  
  
 *order-by-clause* :: = order by *Sort-Specification* [, *Sort-Specification*]...  
  
 *Primary* :: = *Column-Name*  
  
 &#124; *Dynamic-Parameter*  
  
 *Literal* de &#124;  
  
 &#124; ( *expresión* )  
  
 *Search-Condition* :: = *Boolean-term* [o *la condición de búsqueda*]  
  
 *Select-List* :: = \* &#124; *Select-subList* [, *Select-subList*]...  (*Select-List* no puede contener parámetros).  
  
 *Select-subList* :: = *expresión*  
  
 *Sort-Specification* :: = {*unsigned-Integer &#124; Column-Name*} [*ASC &#124; DESC*]  
  
 *TABLE-Identifier* :: = *nombre definido por el usuario*  
  
 *TABLE-Name* :: = *identificador de tabla*  
  
 *tabla-referencia* :: = *nombre de tabla*  
  
 *TABLE-Reference-List* :: = *tabla-referencia* [,*referencia de tabla*]...  
  
 *term* :: = *factor* &#124; *term* { \*&#124;*/* } *factor*  
  
 *unsigned-Integer* :: = {*digit*}  
  
 *mayúsculas-minúsculas* :: = *a &#124; B &#124; C &#124; D &#124; E &#124; F &#124; G &#124; H &#124; i &#124; J &#124; K &#124; &#124; &#124; M &#124; N &#124; O &#124; P &#124; Q &#124; R &#124; S &#124; T &#124; U &#124; V &#124; W &#124; X &#124; Y Z*  
  
 *nombre definido por el usuario* :: = *letra*[*dígito* &#124; *letra* &#124; *_*]...
