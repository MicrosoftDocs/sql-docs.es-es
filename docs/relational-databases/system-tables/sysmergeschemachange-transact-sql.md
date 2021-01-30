---
description: sysmergeschemachange (Transact-SQL)
title: sysmergeschemachange (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysmergeschemachange_TSQL
- sysmergeschemachange
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergeschemachange system table
ms.assetid: ae9ce16e-6ee9-4c7c-8210-a9bf2c7efdf0
author: cawrites
ms.author: chadam
ms.openlocfilehash: e4fcb1360495a72ca5650abc1c76e7c7dd224f2e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209208"
---
# <a name="sysmergeschemachange-transact-sql"></a>sysmergeschemachange (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene información acerca de los artículos publicados generados por el Agente de instantáneas. Esta tabla se almacena en las bases de datos de publicación y de suscripciones.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**pubid**|**uniqueidentifier**|Id. de la publicación.|  
|**artid**|**uniqueidentifier**|Id. del artículo.|  
|**schemaversion**|**int**|Número del último cambio de esquema.|  
|**schemaguid**|**uniqueidentifier**|Id. único del último esquema.|  
|**schematype**|**int**|Tipo de cambio de esquema:<br /><br /> **-1** = no es válido.<br /><br /> **1** = comando SQL.<br /><br /> **2** = script de esquema.<br /><br /> **3** = utilidad de programa de copia masiva (BCP) nativa.<br /><br /> **4** = BCP de caracteres.<br /><br /> **5** = última generación grabada.<br /><br /> **6** = última generación enviada.<br /><br /> **7** = directorio.<br /><br /> **8** = prioridad.<br /><br /> **9** = período de retención.<br /><br /> **10** = script de desencadenador.<br /><br /> **11** = ALTER TABLE.<br /><br /> **12** = reinicializar todo.<br /><br /> **13** = ALTER TABLE (no [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ).<br /><br /> **14** = reinicializar con carga.<br /><br /> **15** = script de restricción e índice.<br /><br /> **16** = limpieza de metadatos.<br /><br /> **17** = actualización de la última generación enviada.<br /><br /> **18** = nivel de compatibilidad con versiones anteriores.<br /><br /> **19** = validar la información del suscriptor.<br /><br /> **20** = bien particionado.<br /><br /> **21** = solucionador personalizado.<br /><br /> **22** = orden de procesamiento del artículo.<br /><br /> **23** = publicado en la publicación transaccional.<br /><br /> **24** = compensar los errores.<br /><br /> **25** = carpeta de instantáneas alternativa.<br /><br /> **26** = solo descarga.<br /><br /> **27** = eliminar seguimiento.<br /><br /> **40** = script de instantánea de creación previa.<br /><br /> **45** = script de instantánea posterior a la creación.<br /><br /> **46** = script de usuario a petición.<br /><br /> **50** = inicio del encabezado de la instantánea.<br /><br /> **51** = final del encabezado de la instantánea.<br /><br /> **52** = finalizador de instantáneas.<br /><br /> **53** = dirección de protocolo de transferencia de archivos (FTP).<br /><br /> **54** = puerto FTP.<br /><br /> **55** = subdirectorio FTP.<br /><br /> **56** = instantánea comprimida.<br /><br /> **57** = inicio de sesión FTP.<br /><br /> **58** = contraseña FTP.<br /><br /> **60** = script previo a la creación del sistema.<br /><br /> **61** = esquema de procedimiento almacenado.<br /><br /> **62** = esquema de vista.<br /><br /> **63** = esquema de la función definida por el usuario.<br /><br /> **64** = ver índices.<br /><br /> **65** = propiedades extendidas.<br /><br /> **66** = Validate.<br /><br /> **67** = comando SQL anterior a la instantánea.<br /><br /> **71** = validación de instantáneas dinámicas.<br /><br /> **80** = BCP 9,0 nativa de la tabla del sistema.<br /><br /> **81** = BCP 9,0 de caracteres de tabla del sistema.<br /><br /> **82** = BCP 9,0 nativo de la tabla del sistema (solo global).<br /><br /> **83** = BCP 9,0 de caracteres de tabla del sistema (solo global).<br /><br /> **84** = BCP 9,0 (ligera) nativa de la tabla del sistema.<br /><br /> **85** = carácter de tabla del sistema BCP 9,0 (ligera).<br /><br /> **128** = BCP dinámico (bit).<br /><br /> **131** = BCP nativo dinámico.<br /><br /> **132** = BCP de carácter dinámico.<br /><br /> **208** = BCP 9,0 nativa de la tabla del sistema dinámica.<br /><br /> **209** = BCP 9,0 de caracteres de tabla del sistema dinámico.<br /><br /> **210** = BCP nativo de tabla del sistema dinámica 9,0 (solo global).<br /><br /> **211** = BCP 9,0 de caracteres de tabla del sistema dinámica (solo global).<br /><br /> **212** = tabla dinámica del sistema BCP 9,0 (ligera).<br /><br /> **213** = BCP 9,0 (ligera) de caracteres de tabla del sistema dinámico.<br /><br /> **300** = acciones del lenguaje de definición de datos (DDL).<br /><br /> **1024** = control incremental de instantáneas.<br /><br /> **1049** = carpeta de instantáneas incrementales.<br /><br /> **1074** = encabezado de inicio de la instantánea incremental.<br /><br /> **1075** = encabezado de finalización de la instantánea incremental.<br /><br /> **1076** = finalizador de instantáneas incrementales.<br /><br /> **1077** = dirección ftp incremental.<br /><br /> **1078** = puerto FTP incremental.<br /><br /> **1079** = subdirectorio FTP incremental.<br /><br /> **1080** = instantánea comprimida incremental.<br /><br /> **1081** = inicio de sesión FTP incremental.<br /><br /> **1082** = contraseña de FTP incremental.|  
|**schematext**|**nvarchar (2000)**|El nombre del archivo de script o un comando que incluye un nombre de archivo.|  
|**schemastatus**|**tinyint**|Indica si hay pendiente algún cambio de esquema para el artículo. Puede ser uno de estos valores:<br /><br /> **0** = inactivo.<br /><br /> **1** = activo.<br /><br /> Cuando hay un cambio de esquema pendiente, este valor se establece en **1**.|  
|**schemasubtype**|**int**|El subtipo de cambio de esquema:<br /><br /> **1** = ADDCOLUMN<br /><br /> **2** = DROPCOLUMN<br /><br /> **3** = ALTERCOLUMN<br /><br /> **4** = ADDPRIMARYKEY<br /><br /> **5** = ADDUNIQUE<br /><br /> **6** = ADDREFERENCE<br /><br /> **7** = DROPCONSTRAINT<br /><br /> **8** = ADDDEFAULT<br /><br /> **9** = ADDCHECK<br /><br /> **10** = DISABLETRIGGER<br /><br /> **11** = ENABLETRIGGER<br /><br /> **12** = DISABLETRIGGER<br /><br /> **13** = ENABLETRIGGER<br /><br /> **14** = ENABLECONSTRAINT<br /><br /> **15** = DISABLECONSTRAINT<br /><br /> **16** = ENABLECONSTRAINT<br /><br /> **17** = DISABLECONSTRAINT|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
