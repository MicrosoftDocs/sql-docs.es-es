---
description: Compatibilidad con las reglas, desencadenadores, valores predeterminados y los procedimientos almacenados (controlador ODBC de Visual FoxPro)
title: Compatibilidad con reglas, desencadenadores, valores predeterminados y procedimientos almacenados | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- Visual FoxPro ODBC driver [ODBC], stored procedures
- FoxPro ODBC driver [ODBC], commands and functions
- commands for FoxPro ODBC driver [ODBC]
- FoxPro ODBC driver [ODBC], default values
- FoxPro ODBC driver [ODBC], triggers
- stored procedures [ODBC], Visual FoxPro ODBC driver
- Visual FoxPro ODBC driver [ODBC], rules
- FoxPro commands and functions [ODBC]
- rules [ODBC]
- Visual FoxPro ODBC driver [ODBC], default values
- FoxPro ODBC driver [ODBC], rules
- functions [ODBC], Visual FoxPro
- default values [ODBC]
- Visual FoxPro ODBC driver [ODBC], commands and functions
- Visual FoxPro ODBC driver [ODBC], triggers
- FoxPro ODBC driver [ODBC], stored procedures
- Visual FoxPro commands and functions [ODBC]
ms.assetid: e449de20-d6ca-4902-9f8e-814eb6e86650
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ea801de44e1fce175ac549402e17b9fcc2e9f020
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061130"
---
# <a name="support-for-rules-triggers-default-values-and-stored-procedures-visual-foxpro-odbc-driver"></a>Compatibilidad con las reglas, desencadenadores, valores predeterminados y los procedimientos almacenados (controlador ODBC de Visual FoxPro)
No se pueden crear reglas, desencadenadores, valores predeterminados o procedimientos almacenados de Visual FoxPro mediante el controlador ODBC de Visual FoxPro. Sin embargo, la aplicación podría interactuar con las reglas, los desencadenadores, los valores predeterminados o los procedimientos almacenados existentes a medida que inserta, actualiza o elimina los datos de Visual FoxPro almacenados en una base de datos.  
  
 En la tabla siguiente se enumeran los comandos y las funciones de Visual FoxPro admitidos por el controlador ODBC de Visual FoxPro cuando los comandos o las funciones existen en reglas, desencadenadores, valores predeterminados o procedimientos almacenados.  
  
 Si la aplicación interactúa con datos cuyas reglas, desencadenadores, valores predeterminados o procedimientos almacenados llaman a cualquier otra función o comando de Visual FoxPro, el controlador genera un error. Consulte [funciones y comandos de Visual FoxPro no compatibles](../../odbc/microsoft/unsupported-visual-foxpro-commands-and-functions-visual-foxpro-odbc-driver.md) para obtener una lista de comandos y funciones no admitidos por el controlador.  
  
> [!TIP]  
>  Si desea insertar código condicional en las reglas, los desencadenadores o los procedimientos almacenados que determinan los comandos que se ejecutan cuando el controlador los llama, puede usar la función **version ()** . La función **version ()** devuelve "controlador ODBC de Visual FoxPro *\<version>* " cuando lo llama el controlador.  
  
## <a name="visual-foxpro-commands-and-functions-supported-in-rules-triggers-default-values-and-stored-procedures"></a>Comandos y funciones de Visual FoxPro admitidos en reglas, desencadenadores, valores predeterminados y procedimientos almacenados  

:::row:::
    :::column:::
        $ (Operador)  
        Operador %  
    :::column-end:::
    :::column:::
        Comando &  
        Comando &&   
    :::column-end:::
    :::column:::
        \* Command  
        = (Comando)  
    :::column-end:::
:::row-end:::

## <a name="a"></a>A  

:::row:::
    :::column:::
        ABS () (función)  
        ACOPY () (función)  
        ACOS () (función)  
        ADATABASES () (función)  
        ADBOBJECTS () (función)  
        Agregar tabla (comando)  
        ADEL () (función)  
        AELEMENT () (función)  
        AERROR () (función)  
        AFIELDS () (función)  
        AINS () (función)  
        ALEN () (función)  
        Función ALIAS ()  
    :::column-end:::
    :::column:::
        ALLTRIM () (función)  
        Modificar tabla - comando SQL  
        AND (operador)  
        APPEND (comando)  
        ANEXAr desde (comando)  
        ANEXAr de la matriz (comando)  
        ANEXAr comando GENERAL  
        APPEND (comando de memorando)  
        Comando APPEND PROCEDUREs  
        ASC () (función)  
        ASCAN () (función)  
        ASIN () (función)  
        ASORT () (función)  
    :::column-end:::
    :::column:::
        ASUBSCRIPT () (función)  
        AT () (función)  
        AT_C () (función)  
        ATAN () (función)  
        ATC () (función)  
        ATCC () (función)  
        ATCLINE () (función)  
        ATLINE () (función)  
        ATN2 () (función)  
        AUSED () (función)  
        PROMEDIO (comando)  
    :::column-end:::
:::row-end:::

## <a name="b"></a>B  

:::row:::
    :::column:::
        Comando BEGIN TRANSACTION  
        BETWEEN () (función)  
        BITAND () (función)  
        BITCLEAR () (función)  
        BIT a bit () (función)  
    :::column-end:::
    :::column:::
        BITNOT () (función)  
        BITOR () (función)  
        BITRSHIFT () (función)  
        BITSET () (función)  
        Función DEBITTEST ()  
    :::column-end:::
    :::column:::
        Función BITXOR ()  
        Comando en blanco  
        BOF () (función)  
    :::column-end:::
:::row-end:::

## <a name="c"></a>C  

:::row:::
    :::column:::
        Comando CALCULAte  
        Función CANDIDAte ()  
        CDOW () (función)  
        CDX () (función)  
        Función CEILING ()  
        CHR () (función)  
        CHRTRAN () (función)  
        CHRTRANC () (función)  
        CERRAR comandos  
        CMONTH () (función)  
    :::column-end:::
    :::column:::
        Comando continuar  
        Comando Copiar índices  
        Comando Copiar procedimientos  
        COPIAR estructura (comando)  
        COPIAR estructura (comando extendido)  
        Comando Copiar etiqueta  
        COPIAR a (comando)  
        COPIAR en matriz (comando)  
        COS () (función)  
        Recuento (comando)  
    :::column-end:::
    :::column:::
        CPCONVERT () (función)  
        CPCURRENT () (función)  
        CPDBF () (función)  
        CTOD () (función)  
        CTOT () (función)  
        CURSORGETPROP () (función)  
        CURSORSETPROP () (función)  
        Función CURVAl ()  
    :::column-end:::
:::row-end:::

## <a name="d"></a>D  

:::row:::
    :::column:::
        DATE () (función)  
        DATETIME () (función)  
        DAY () (función)  
        DBC () (función)  
        DBF () (función)  
        DBGETPROP () (función)  
        DBUSED () (función)  
        ELIMINAR, comando SQL  
    :::column-end:::
    :::column:::
        ELIMINAR comando  
        Eliminar etiqueta, comando  
        Función DELETEd ()  
        Descending () (función)  
        DIFFERENCE () (función)  
        Comando de dimensión  
        Función de espacio en la ()  
        DMY () (función)  
    :::column-end:::
    :::column:::
        DO (comando)  
        CASO... Comando ENDCASE  
        DO WHILE... Comando ENDDO  
        DOW () (función)  
        DTOC () (función)  
        DTOR () (función)  
        DTO () (función)  
        DTOT () (función)  
    :::column-end:::
:::row-end:::

## <a name="e"></a>E  

:::row:::
    :::column:::
        EMPTY () (función)  
        END TRANSACTION (comando)  
        EOF () (función)  
    :::column-end:::
    :::column:::
        ERROR () (función)  
        EVALUAte () (función)  
        EXIT (comando)  
    :::column-end:::
    :::column:::
        EXP () (función)  
    :::column-end:::
:::row-end:::

## <a name="f"></a>F  

:::row:::
    :::column:::
        FCOUNT () (función)  
        FDATE () (función)  
        FIELD () (función)  
        FILE () (función)  
        FILTER () (función)  
        FLDLIST () (función)  
    :::column-end:::
    :::column:::
        Función de rebaño ()  
        Función FLOOR ()  
        VACIAr comando  
        Función FOR ()  
        PARA... Comando ENDFOR  
        FOUND () (función)  
    :::column-end:::
    :::column:::
        Comando de tabla libre  
        FSIZE () (función)  
        FTIME () (función)  
        FULLPATH () (función)  
        Comando de función  
        FV () (función)  
    :::column-end:::
:::row-end:::

## <a name="g"></a>G  

:::row:::
    :::column:::
        RECOPILAr (comando)  
        GETCP () (función)  
        GETENV () (función)  
    :::column-end:::
    :::column:::
        GETFLDSTATE () (función)  
        GETNEXTMODIFIED () (función)  
        GO/GOTO (comando)  
    :::column-end:::
    :::column:::
        GOMONTH () (función)  
    :::column-end:::
:::row-end:::

## <a name="h"></a>H  

:::row:::
    :::column:::
        HEADER () (función)
    :::column-end:::
    :::column:::
        HOUR () (función)
    :::column-end:::
:::row-end:::

## <a name="i"></a>I  

:::row:::
    :::column:::
        IDXCOLLATE () (función)  
        IF... ENDIF (comando)  
        IIF () (función)  
        INDBC () (función)  
        Comando de índice  
        Inlist () (función)  
    :::column-end:::
    :::column:::
        Comando INSERT-SQL  
        INT () (función)  
        ISALPHA () (función)  
        Función esblanco ()  
        ISDIGIT () (función)  
        ISEXCLUSIVE () (función)  
    :::column-end:::
    :::column:::
        ISLEADBYTE () (función)  
        ISLOWER () (función)  
        ISNULL () (función)  
        ISREADONLY () (función)  
        ISUPPER () (función)  
    :::column-end:::
:::row-end:::

## <a name="k"></a>K  

:::row:::
    :::column:::
        KEY () (función)
    :::column-end:::
    :::column:::
        KEYMATCH () (función)
    :::column-end:::
:::row-end:::

## <a name="l"></a>L  

:::row:::
    :::column:::
        LEFT () (función)  
        LEFTC () (función)  
        LEN () (función)  
        LENC () (función)  
        LIKE () (función)  
        LIKEC () (función)  
    :::column-end:::
    :::column:::
        Comando LOCAL  
        LOCATE (comando)  
        LOCK () (función)  
        Función LOG ()  
        LOG10 () (función)  
        LOOKUP () (función)  
    :::column-end:::
    :::column:::
        LOWER () (función)  
        Comando LPARAMETERS  
        LTRIM () (función)  
        LUPDATE () (función)  
    :::column-end:::
:::row-end:::

## <a name="m"></a>M  

:::row:::
    :::column:::
        MAX () (función)  
        MDX () (función)  
        MDA () (función)  
        MEMLINES () (función)  
    :::column-end:::
    :::column:::
        MESSAGE () (función)  
        MIN () (función)  
        MINUTE () (función)  
        _MLINE variable de memoria del sistema  
    :::column-end:::
    :::column:::
        MLINE () (función)  
        MOD () (función)  
        MONTH () (función)  
        MTON () (función)  
    :::column-end:::
:::row-end:::

## <a name="n"></a>N  

:::row:::
    :::column:::
        NDX () (función)  
        NORMALIZE () (función)  
    :::column-end:::
    :::column:::
        NOT (operador)  
        NOTE (comando)  
    :::column-end:::
    :::column:::
        NTOM () (función)  
        NVL () (función)  
    :::column-end:::
:::row-end:::

## <a name="o"></a>O  

:::row:::
    :::column:::
        Función produce ()  
        OLDVAL () (función)  
        ON () (función)  
    :::column-end:::
    :::column:::
        ON ERROR (comando)  
        ON KEY (comando)  
        Comando abrir base de datos  
    :::column-end:::
    :::column:::
        Operador OR  
        ORDER () (función)  
        Función OS ()  
    :::column-end:::
:::row-end:::

## <a name="p"></a>P  

:::row:::
    :::column:::
        Comando PACK  
        PADL () &#124; PADR () &#124; funciones PADC ()  
        PARÁMETROS (comando)  
        Parameters () (función)  
        PAYMENT () (función)  
    :::column-end:::
    :::column:::
        PI () (función)  
        PRIMAry () (función)  
        Comando privado  
        PROCEDIMIENTO (comando)  
        Función PROGRAM ()  
    :::column-end:::
    :::column:::
        NOMPROPIO () (función)  
        Comando público  
        PV () (función)  
    :::column-end:::
:::row-end:::

## <a name="r"></a>R  

:::row:::
    :::column:::
        RAND () (función)  
        RAT () (función)  
        RATC () (función)  
        RATLINE () (función)  
        Comando de recuperación  
        RECCOUNT () (función)  
        RECNO () (función)  
        RECSIZE () (función)  
    :::column-end:::
    :::column:::
        Comando REGIONAL  
        Función Relation ()  
        Comando quitar tabla  
        REEMPLAZAR (comando)  
        REEMPLAZAR de la matriz (comando)  
        REPLICAte () (función)  
        Reintentar (comando)  
        Comando Return  
    :::column-end:::
    :::column:::
        RIGHT () (función)  
        RIGHTC () (función)  
        RLOCK () (función)  
        ROLLBACK (comando)  
        ROUND () (función)  
        RTOD () (función)  
        RTRIM () (función)  
    :::column-end:::
:::row-end:::

## <a name="s"></a>S  

:::row:::
    :::column:::
        EXAMINAR... Comando ENDSCAN  
        DISPERSIÓN (comando)  
        SEC () (función)  
        SECONDs () (función)  
        Comando de búsqueda  
        SEEK () (función)  
        SELECCIONAR comando  
        SELECT () (función)  
        Comando SELECT-SQL  
        SET () (función)  
        Comando BLOCKSIZE Set  
        ESTABLECER comando de transporte  
        Comando SET CENTURY  
        Comando COLLATE Set  
        ESTABLECER base de datos (comando)  
        ESTABLECER fecha (comando)  
        ESTABLECER comando predeterminado  
        Comando de eliminaciones de Set  
        Comando exacto de conjunto  
        Comando exclusivo de Set  
        Comando SET FDOW  
    :::column-end:::
    :::column:::
        Comando SET FIELDs  
        ESTABLECER filtro (comando)  
        ESTABLECER comando fijo  
        SET FULLPATH (comando)  
        Comando SET FWEEK  
        ESTABLECER horas (comando)  
        Comando SET INDEX  
        ESTABLECER comando de bloqueo  
        ESTABLECER el comando de multilocks  
        ESTABLECER NEAR (comando)  
        Comando SET NOCPTRANS  
        Comando SET NOTIFY  
        Comando NULL del conjunto  
        Comando SET OPTIMIZe  
        Comando SET ORDER  
        Comando de ruta de acceso set  
        Comando SET PROCEDURE  
        Comando SET Relation  
        Comando establecer relación desactivada  
        CONJUNTO de comandos de volver a procesar  
        SET SKIP (comando)  
    :::column-end:::
    :::column:::
        Comando SET UDFPARMS  
        CONJUNTO único comando  
        Comando SET VOLUME  
        SETFLDSTATE () (función)  
        SIGN () (función)  
        SIN () (función)  
        SKIP (comando)  
        SORT (comando)  
        SPACE () (función)  
        SQRT () (función)  
        Comando de almacenamiento  
        STR () (función)  
        STRCONV () (función)  
        STRTRAN () (función)  
        Función material ()  
        STUFFC () (función)  
        SUBSTR () (función)  
        SUBSTRC () (función)  
        SUM (comando)  
        SYS (2011) (función)  
    :::column-end:::
:::row-end:::

## <a name="t"></a>T  

:::row:::
    :::column:::
        TABLEREVERT () (función)  
        TABLEUPDATE () (función)  
        TAG () (función)  
        TAGCOUNT () (función)  
        TAGNO () (función)  
        _TALLY variable de memoria del sistema  
    :::column-end:::
    :::column:::
        TAN () (función)  
        TARGET () (función)  
        TIME () (función)  
        TOTAL (comando)  
        _TRIGGERLEVEL variable de memoria del sistema  
        TRIM () (función)  
    :::column-end:::
    :::column:::
        TTOC () (función)  
        TTOD () (función)  
        TXNLEVEL () (función)  
        TYPE () (función)  
    :::column-end:::
:::row-end:::

## <a name="u"></a>U  

:::row:::
    :::column:::
        UNIQUE () (función)  
        DESBLOQUEAR (comando)  
        Comando UPDATE  
    :::column-end:::
    :::column:::
        Comando de SQL de actualización  
        UPPER () (función)  
        USAR comando  
    :::column-end:::
    :::column:::
        Función USED ()  
    :::column-end:::
:::row-end:::

## <a name="v"></a>V  

:::row:::
    :::column:::
        VAL () (función)
    :::column-end:::
    :::column:::
        VERSION () (función)
    :::column-end:::
:::row-end:::

## <a name="w"></a>W  

WEEK () (función)

## <a name="y"></a>Y  

YEAR () (función)

## <a name="z"></a>Z  

ZAP (comando)
