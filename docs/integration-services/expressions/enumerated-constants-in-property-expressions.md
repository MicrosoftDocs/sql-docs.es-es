---
description: Constantes enumeradas en expresiones de propiedad
title: Constantes enumeradas en expresiones de propiedad | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- enumerators [Integration Services]
- packages [Integration Services], expressions
- dynamic properties
- updating package properties
- enumerated constants [Integration Services]
- property expressions [Integration Services]
ms.assetid: a4418315-38e2-4ad3-8784-576163b25d6f
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 692e2e19b86b5d47d207c32c3024365675603c07
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123257"
---
# <a name="enumerated-constants-in-property-expressions"></a>Constantes enumeradas en expresiones de propiedad

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Si las expresiones de propiedad incluyen valores de una lista de miembros enumeradores, la expresión debe utilizar el valor numérico del miembro enumerador en lugar del nombre descriptivo del miembro. Por ejemplo, si una expresión establece la propiedad **LoggingMode** , debe utilizar el valor 2 en lugar del nombre descriptivo Deshabilitado.  
  
 Este tema enumera solo los valores numéricos equivalentes a los nombres descriptivos de los enumeradores cuyos miembros se utilizan generalmente en expresiones de propiedad. El modelo de objetos [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] incluye varios enumeradores adicionales que puede utilizar cuando programa el modelo de objetos para generar paquetes mediante programación o elementos de paquete de código personalizado tales como tareas y componentes de flujo de datos.  
  
 Además de las propiedades personalizadas de los paquetes y objetos de paquetes, la ventana Propiedades de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] incluye un conjunto de propiedades disponibles para paquetes, tareas, y los contenedores de secuencias, de bucles Foreach y de bucles For. Las propiedades comunes establecidas por valores de enumeradores (**ForceExecutionResult**, **LoggingMode**, **IsolationLevel** y **TransactionOption**) se enumeran en la sección “Propiedades comunes”.  
  
 Las siguientes secciones proporcionan información sobre constantes enumeradas:  
  
 [Paquete](#Package)  
  
 [Enumeradores de bucle Foreach](#Foreach)  
  
 [Tareas](#Tasks)  
  
 [Tareas del plan de mantenimiento](#MaintenancePlanTasks)  
  
 [Common Properties](#CommonProperties)  
  
##  <a name="package"></a><a name="Package"></a> Paquete  
 Las siguientes tablas enumeran los nombres descriptivos y los equivalentes de valores numéricos de las propiedades de paquetes que se establecen utilizando valores de un enumerador.  
  
 Propiedad **PackageType**: se establece mediante el uso de valores de la enumeración **DTSPackageType**.  
  
|Nombre descriptivo en DTSPackageType|Valor numérico|  
|-------------------------------------|-------------------|  
|Valor predeterminado|0|  
|DTSWizard|1|  
|DTSDesigner|2|  
|SQLReplication|3|  
|DTSDesigner100|5|  
|SQLDBMaint|6|  
  
 Propiedad **CheckpointUsage**: se establece mediante el uso de valores de la enumeración **DTSCheckpointUsage**.  
  
|Nombre descriptivo en DTSCheckpointUsage|Valor numérico|  
|-----------------------------------------|-------------------|  
|Nunca|0|  
|IfExists|1|  
|Siempre|2|  
  
 Propiedad **PackagePriorityClass**: se establece mediante el uso de valores de la enumeración **DTSPriorityClass**.  
  
|Nombre descriptivo en DTSPriorityClass|Valor numérico|  
|---------------------------------------|-------------------|  
|Valor predeterminado|0|  
|AboveNormal|1|  
|Normal|2|  
|BelowNormal|3|  
|Inactivo|4|  
  
 Propiedad **ProtectionLevel**: se establece mediante el uso de valores de la enumeración **DTSProtectionLevel**.  
  
|Nombre descriptivo en DTSProtectionLevel|Valor numérico|  
|-----------------------------------------|-------------------|  
|DontSaveSensitive|0|  
|EncryptSensitiveWithUserKey|1|  
|EncryptSensitiveWithPassword|2|  
|EncryptAllWithPassword|3|  
|EncryptAllWithUserKey|4|  
|ServerStorage|5|  
  
##  <a name="precedence-constraints"></a><a name="PrecedenceConstraints"></a> Restricciones de precedencia  
 Propiedad **EvalOp**: se establece mediante el uso de valores de la enumeración **DTSPrecedenceEvalOp**.  
  
|Nombre descriptivo en DTSPrecedenceEvalOp|Valor numérico|  
|------------------------------------------|-------------------|  
|Expression|1|  
|Restricción|2|  
|ExpressionAndConstraint|3|  
|ExpressionOrConstraint|4|  
  
 Propiedad **Value**: se establece mediante el uso de valores de la enumeración **DTSExecResult**.  
  
|Nombre descriptivo|Valor numérico|  
|-------------------|-------------------|  
|Correcto|0|  
|Error|1|  
|Completion|2|  
|Canceled|3|  
  
##  <a name="foreach-loop-enumerators"></a><a name="Foreach"></a> Enumeradores de bucle Foreach  
 El bucle Foreach incluye un conjunto de enumeradores con propiedades que se pueden establecer a partir de expresiones de propiedad.  
  
### <a name="foreach-ado-enumerator"></a>Enumerador de ADO para Foreach  
 Propiedad **Type**: se establece mediante el uso de valores de la enumeración **ADOEnumerationType**.  
  
|Nombre descriptivo en ADOEnumerationType|Valor numérico|  
|-----------------------------------------|-------------------|  
|EnumerateTables|0|  
|EnumerateAllRows|1|  
|EnumerateRowsInFirstTable|2|  
  
### <a name="foreach-nodelist-enumerator"></a>Enumerador de lista de nodos para Foreach  
 Propiedades **SourceDocumentType**, **InnerXPathStringSourceType** y **OuterXPathStringSourceType**: se establecen mediante el uso de valores de la enumeración **SourceType**.  
  
|Nombre descriptivo en SourceType|Valor numérico|  
|---------------------------------|-------------------|  
|FileConnection|0|  
|Variable|1|  
|DirectInput|2|  
  
 Propiedad **EnumerationType**: se establece mediante el uso de valores de la enumeración **EnumerationType**.  
  
|Nombre descriptivo en EnumerationType|Valor numérico|  
|--------------------------------------|-------------------|  
|Navegador|0|  
|Nodo|1|  
|NodeText|2|  
|ElementCollection|3|  
  
 Propiedad **InnerElementType**: se establece mediante el uso de valores de la enumeración **InnerElementType**.  
  
|Nombre descriptivo en InnerElementType|Valor numérico|  
|---------------------------------------|-------------------|  
|Navegador|0|  
|Nodo|1|  
|NodeText|2|  
  
##  <a name="tasks"></a><a name="Tasks"></a> Tareas  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] incluye numerosas tareas con propiedades que se establecen a partir de expresiones de propiedad.  
  
### <a name="analysis-services-execute-ddl-task"></a>Tarea Ejecutar DDL de Analysis Services  
 Propiedad **SourceType**: se establece mediante el uso de valores de la enumeración **DDLSourceType**.  
  
|Nombre descriptivo en DDLSourceType|Valor numérico|  
|------------------------------------|-------------------|  
|DirectInput|0|  
|FileConnection|1|  
|Variable|2|  
  
### <a name="bulk-insert-task"></a>Inserción masiva, tarea  
 Propiedad **DataFileType**: se establece mediante el uso de valores de la enumeración **DTSBulkInsert_DataFileType**.  
  
|Nombre descriptivo en DTSBulkInsert_DataFileType|Valor numérico|  
|--------------------------------------------------|-------------------|  
|DTSBulkInsert_DataFileType_Char|0|  
|DTSBulkInsert_DataFileType_Native|1|  
|DTSBulkInsert_DataFileType_WideChar|2|  
|DTSBulkInsert_DataFileType_WideNative|3|  
  
### <a name="execute-sql-task"></a>Tarea Ejecutar SQL  
 Propiedad **ResultSetType**: se establece mediante el uso de valores de la enumeración **ResultSetType**.  
  
|Nombre descriptivo en ResultSetType|Valor numérico|  
|------------------------------------|-------------------|  
|ResultSetType_None|1|  
|ResultSetType_SingleRow|2|  
|ResultSetType_Rowset|3|  
|ResultSetType_XML|4|  
  
 Propiedad **SqlStatementSourceType**: se establece mediante el uso de valores de la enumeración **SqlStatementSourceType**.  
  
|Nombre descriptivo en SqlStatementSourceType|Valor numérico|  
|---------------------------------------------|-------------------|  
|DirectInput|1|  
|FileConnection|2|  
|Variable|3|  
  
### <a name="file-system-task"></a>Tarea Sistema de archivos  
 Propiedad **Operation**: se establece mediante el uso de valores de la enumeración **DTSFileSystemOperation**.  
  
|Nombre descriptivo en DTSFileSystemOperation|Valor numérico|  
|---------------------------------------------|-------------------|  
|CopyFile|0|  
|MoveFile|1|  
|DeleteFile|2|  
|RenameFile|3|  
|SetAttributes|4|  
|CreateDirectory|5|  
|CopyDirectory|6|  
|MoveDirectory|7|  
|DeleteDirectory|8|  
|DeleteDirectoryContent|9|  
  
 Propiedad **Attributes**: se establece mediante el uso de valores de la enumeración **DTSFileSystemAttributes**.  
  
|Nombre descriptivo en DTSFileSystemAttributes|Valor numérico|  
|----------------------------------------------|-------------------|  
|Normal|0|  
|Archivar|1|  
|Hidden|2|  
|ReadOnly|4|  
|Sistema|8|  
  
### <a name="ftp-task"></a>Tarea FTP  
 Propiedad **Operation**: se establece mediante el uso de valores de la enumeración **DTSFTPOp**.  
  
|Nombre descriptivo en DTSFTPOp|Valor numérico|  
|-------------------------------|-------------------|  
|Envío|0|  
|Recepción|1|  
|DeleteLocal|2|  
|DeleteRemote|3|  
|MakeDirLocal|4|  
|MakeDirRemote|5|  
|RemoveDirLocal|6|  
|RemoveDirRemote|7|  
  
### <a name="message-queue-task"></a>Message Queue Task  
 Propiedad **MessageType**: se establece mediante el uso de valores de la enumeración **MQMessageType**.  
  
|Nombre descriptivo en MQMessageType|Valor numérico|  
|------------------------------------|-------------------|  
|DTSMQMessageType_String|0|  
|DTSMQMessageType_DataFile|1|  
|DTSMQMessageType_Variables|2|  
|DTSMQMessagType_StringMessageToVariable|3|  
  
 Propiedad **StringCompareType**: se establece mediante el uso de valores de la enumeración **MQStringMessageCompare**.  
  
|Nombre descriptivo en MQStringMessageCompare|Valor numérico|  
|---------------------------------------------|-------------------|  
|DTSMQStringMessageCompare_None|0|  
|DTSMQStringMessageCompare_Exact|1|  
|DTSMQStringMessageCompare_IgnoreCase|2|  
|DTSMQStringMessageCompare_Contains|3|  
  
 Propiedad **TaskType**: se establece mediante el uso de valores de la enumeración **MQType**.  
  
|Nombre descriptivo en MQType|Valor numérico|  
|-----------------------------|-------------------|  
|DTSMQType_Sender|0|  
|DTSMQType_Receiver|1|  
  
### <a name="send-mail-task"></a>Enviar correo, tarea  
 Propiedad **MessageSourceType**: se establece mediante el uso de valores de la enumeración **SendMailMessageSourceType**.  
  
|Nombre descriptivo en SendMailMessageSourceType|Valor numérico|  
|------------------------------------------------|-------------------|  
|DirectInput|0|  
|FileConnection|1|  
|Variable|2|  
  
 Propiedad **Priority**: se establece mediante el uso de valores de la enumeración **MailPriority**.  
  
|Nombre descriptivo en MailPriority|Valor numérico|  
|-----------------------------------|-------------------|  
|Alto|1|  
|Normal|3|  
|Bajo|5|  
  
### <a name="transfer-database-task"></a>Tarea Transferir bases de datos  
 Propiedad **Action**: se establece mediante el uso de valores de la enumeración **TransferAction**.  
  
|Nombre descriptivo en TransferAction|Valor numérico|  
|-------------------------------------|-------------------|  
|Copiar|0|  
|Move|1|  
  
 Propiedad **Method**: se establece mediante el uso de valores de la enumeración **TransferMethod**.  
  
|Nombre descriptivo en TransferMethod|Valor numérico|  
|-------------------------------------|-------------------|  
|DatabaseOffline|0|  
|DatabaseOnline|1|  
  
### <a name="transfer-error-messages-task"></a>Tarea Transferir mensajes de error  
 Propiedad **IfObjectExists**: se establece mediante el uso de valores de la enumeración **IfObjectExists**.  
  
|Nombre descriptivo en IfObjectExists|Valor numérico|  
|-------------------------------------|-------------------|  
|FailTask|0|  
|Sobrescribir|1|  
|Omitir|2|  
  
### <a name="transfer-jobs-task"></a>Tarea Transferir trabajos  
 Propiedad **IfObjectExists**: se establece mediante el uso de valores de la enumeración **IfObjectExists**.  
  
|Nombre descriptivo en IfObjectExists|Valor numérico|  
|-------------------------------------|-------------------|  
|FailTask|0|  
|Sobrescribir|1|  
|Omitir|2|  
  
### <a name="transfer-logins-task"></a>Tarea Transferir inicios de sesión  
 Propiedad **IfObjectExists**: se establece mediante el uso de valores de la enumeración **IfObjectExists**.  
  
|Nombre descriptivo en IfObjectExists|Valor numérico|  
|-------------------------------------|-------------------|  
|FailTask|0|  
|Sobrescribir|1|  
|Omitir|2|  
  
 Propiedad **LoginsToTransfer**: se establece mediante el uso de valores de la enumeración **LoginsToTransfer**.  
  
|Nombre descriptivo en LoginsToTransfer|Valor numérico|  
|---------------------------------------|-------------------|  
|AllLogins|0|  
|SelectedLogins|1|  
|AllLoginsFromSelectedDatabases|2|  
  
### <a name="transfer-master-stored-procedures-task"></a>Tarea Transferir procedimientos almacenados principales  
 Propiedad **IfObjectExists**: se establece mediante el uso de valores de la enumeración **IfObjectExists**.  
  
|Nombre descriptivo en IfObjectExists|Valor numérico|  
|-------------------------------------|-------------------|  
|FailTask|0|  
|Sobrescribir|1|  
|Omitir|2|  
  
### <a name="transfer-sql-server-objects-task"></a>Tarea Transferir objetos de SQL Server  
 Propiedad **ExistingData**: se establece mediante el uso de valores de la enumeración **ExistingData**.  
  
|Nombre descriptivo en ExistingData|Valor numérico|  
|-----------------------------------|-------------------|  
|Replace|0|  
|Anexar|1|  
  
### <a name="web-service-task"></a>Tarea Servicio web  
 Propiedad **OutputType**: se establece mediante el uso de valores de la enumeración **DTSOutputType**.  
  
|Nombre descriptivo en DTSOutputType|Valor numérico|  
|------------------------------------|-------------------|  
|Archivo|0|  
|Variable|1|  
  
### <a name="wmi-data-reader-task"></a>Tarea Lector de datos WMI  
 Propiedad **OverwriteDestination**: se establece mediante el uso de valores de la enumeración **OverwriteDestination**.  
  
|Nombre descriptivo en OverwriteDestination|Valor numérico|  
|-------------------------------------------|-------------------|  
|OverwriteDestination|0|  
|AppendToDestination|1|  
|KeepOriginal|2|  
  
 Propiedad **OutputType**: se establece mediante el uso de valores de la enumeración **OutputType**.  
  
|Nombre descriptivo en OutputType|Valor numérico|  
|---------------------------------|-------------------|  
|DataTable|0|  
|PropertyValue|1|  
|PropertyNameAndValue|2|  
  
 Propiedad **DestinationType**: se establece mediante el uso de valores de la enumeración **DestinationType**.  
  
|Nombre descriptivo en DestinationType|Valor numérico|  
|--------------------------------------|-------------------|  
|FileConnection|0|  
|Variable|1|  
  
 Propiedad **WqlQuerySourceType**: se establece mediante el uso de valores de la enumeración **QuerySourceType**.  
  
|Nombre descriptivo en QuerySourceType|Valor numérico|  
|--------------------------------------|-------------------|  
|FileConnection|0|  
|DirectInput|1|  
|Variable|2|  
  
 Propiedad **ActionAtEvent** del Monitor de eventos WMI: se establece mediante el uso de valores de la enumeración **ActionAtEvent**.  
  
|Nombre descriptivo en ActionAtEvent|Valor numérico|  
|------------------------------------|-------------------|  
|LogTheEventAndFireDTSEvent|0|  
|LogTheEvent|1|  
  
 Propiedad **ActionAtTimeout**: se establece mediante el uso de valores de la enumeración **ActionAtTimeout**.  
  
|Nombre descriptivo en ActionAtTimeout|Valor numérico|  
|--------------------------------------|-------------------|  
|LogTimeoutAndFireDTSEvent|0|  
|LogTimeout|1|  
  
 Propiedad **AfterEvent**: se establece mediante el uso de valores de la enumeración **AfterEvent**.  
  
|Nombre descriptivo en AfterEvent|Valor numérico|  
|---------------------------------|-------------------|  
|ReturnWithSuccess|0|  
|ReturnWithFailure|1|  
|WatchfortheEventAgain|2|  
  
 Propiedad **AfterTimeout**: se establece mediante el uso de valores de la enumeración **AfterTimeout**.  
  
|Nombre descriptivo en AfterTimeout|Valor numérico|  
|-----------------------------------|-------------------|  
|ReturnWithSuccess|0|  
|ReturnWithFailure|1|  
|WatchfortheEventAgain|2|  
  
 Propiedad **WqlQuerySourceType**: se establece mediante el uso de valores de la enumeración **QuerySourceType**.  
  
|Nombre descriptivo en QuerySourceType|Valor numérico|  
|--------------------------------------|-------------------|  
|FileConnection|0|  
|DirectInput|1|  
|Variable|2|  
  
### <a name="xml-task"></a>Tarea XML  
 Propiedad **OperationType**: se establece mediante el uso de valores de la enumeración **DTSXMLOperation**.  
  
|Nombre descriptivo en DTSXMLOperation|Valor numérico|  
|--------------------------------------|-------------------|  
|Validación|0|  
|XSLT|1|  
|XPATH|2|  
|Fusionar mediante combinación|3|  
|Diferencias|4|  
|Revisión|5|  
  
 Propiedades **SourceType**, **SecondOperandType** y **XPathSourceType**: se establecen mediante el uso de valores de la enumeración **DTSXMLSourceType**.  
  
|Nombre descriptivo en DTSXMLSourceType|Valor numérico|  
|---------------------------------------|-------------------|  
|FileConnection|0|  
|Variable|1|  
|DirectInput|2|  
  
 Propiedades **DestinationType** y **DiffGramDestinationType**: se establecen mediante el uso de valores de la enumeración **DTSXMLSaveResultTo**.  
  
|Nombre descriptivo en DTSXMLSaveResultTo|Valor numérico|  
|-----------------------------------------|-------------------|  
|FileConnection|0|  
|Variable|1|  
  
 Propiedad **ValidationType**: se establece mediante el uso de valores de la enumeración **DTSXMLValidationType**.  
  
|Nombre descriptivo en DTSXMLValidationType|Valor numérico|  
|-------------------------------------------|-------------------|  
|DTD|0|  
|XSD|1|  
  
 Propiedad **XPathOperation**: se establece mediante el uso de valores de la enumeración **DTSXMLXPathOperation**.  
  
|Nombre descriptivo en DTSXMLXPathOperation|Valor numérico|  
|-------------------------------------------|-------------------|  
|Evaluación|0|  
|Valores|1|  
|NodeList|2|  
  
 Propiedad **DiffOptions**: se establece mediante el uso de valores de la enumeración **DTSXMLDiffOptions**. Las opciones de este enumerador no se excluyen mutualmente. Para utilizar varias opciones, proporcione una lista separada por comas de las opciones que se deben aplicar.  
  
|Nombre descriptivo en DTSXMLDiffOptions|Valor numérico|  
|----------------------------------------|-------------------|  
|None|0|  
|IgnoreChildOrder|1|  
|IgnoreComments|2|  
|IgnorePI|4|  
|IgnoreWhitespace|8|  
|IgnoreNameSpaces|16|  
|IgnorePrefixes|32|  
|IgnoreXmlDecl|64|  
|IgnoreDtd|128|  
  
 Propiedad **DiffAlgorithm**: se establece mediante el uso de valores de la enumeración **DTSXMLDiffAlgorithm**.  
  
|Nombre descriptivo en DTSXMLDiffAlgorithm|Valor numérico|  
|------------------------------------------|-------------------|  
|Auto|0|  
|Fast (rápido)|1|  
|Preciso|2|  
  
##  <a name="maintenance-plan-tasks"></a><a name="MaintenancePlanTasks"></a> Tareas del plan de mantenimiento  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] incluye un conjunto de tareas que realiza tareas de SQL Server para utilizar en planes de mantenimiento y paquetes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no admite el trabajo con estas tareas mediante programación y la documentación de referencia de programación no incluye documentación de API de estas tareas y sus enumeradores.  
  
### <a name="all-maintenance-tasks"></a>Todas las tareas de mantenimiento  
 Todas las tareas de mantenimiento utilizan las siguientes enumeraciones para establecer las propiedades especificadas.  
  
 Propiedad **DatabaseSelectionType**: se establece mediante el uso de valores de la enumeración **DatabaseSelection**.  
  
|Nombre descriptivo en DatabaseSelection|Valor numérico|  
|----------------------------------------|-------------------|  
|None|0|  
|Todo|1|  
|Sistema|2|  
|Usuario|3|  
|Específico|4|  
  
 Propiedad **TableSelectionType**: se establece mediante el uso de valores de la enumeración **TableSelection**.  
  
|Nombre descriptivo en TableSelection|Valor numérico|  
|-------------------------------------|-------------------|  
|None|0|  
|Todo|1|  
|Específico|2|  
  
 Propiedad **ObjectTypeSelection**: se establece mediante el uso de valores de la enumeración **ObjectType**.  
  
|Nombre descriptivo en ObjectType|Valor numérico|  
|---------------------------------|-------------------|  
|Tabla|0|  
|Ver|1|  
|TableView|2|  
  
### <a name="back-up-database-task"></a>Tarea Copia de seguridad de la base de datos  
 Propiedad **DestinationCreationType**: se establece mediante el uso de valores de la enumeración **DestinationType**.  
  
|Nombre descriptivo en DestinationType|Valor numérico|  
|--------------------------------------|-------------------|  
|Auto|0|  
|Manual|1|  
  
 Propiedad **ExistingBackupsAction**: se establece mediante el uso de valores de la enumeración **ActionForExistingBackups**.  
  
|Nombre descriptivo en ActionForExistingBackups|Valor numérico|  
|-----------------------------------------------|-------------------|  
|Anexar|0|  
|Sobrescribir|1|  
  
 Propiedad **BackupAction**: se establece mediante el uso de valores de la enumeración **BackupTaskType**. Esta propiedad trabaja con la propiedad **BackupIsIncremental** para definir el tipo de copia de seguridad que realiza la tarea.  
  
|Nombre descriptivo en BackupTaskType|Valor numérico|  
|-------------------------------------|-------------------|  
|Base de datos|0|  
|Archivos|1|  
|Log|2|  
  
 Propiedad **BackupDevice**: se establece mediante el uso de valores de la enumeración **DeviceType** de los objetos de administración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SMO).  
  
|Nombre descriptivo en DeviceType|Valor numérico|  
|---------------------------------|-------------------|  
|LogicalDevice|0|  
|Cinta|1|  
|Archivo|2|  
|Pipe|3|  
|VirtualDevice|4|  
  
### <a name="maintenance-cleanup-task"></a>tarea, Limpieza de mantenimiento  
 Propiedad **FileTypeSelected**: se establece mediante el uso de valores de la enumeración **FileType**.  
  
|Nombre descriptivo en FileType|Valor numérico|  
|-------------------------------|-------------------|  
|FileBackup|0|  
|FileReport|1|  
  
 Propiedad **OlderThanTimeUnitType**: se establece mediante el uso de valores de la enumeración **TimeUnitType**.  
  
|Nombre descriptivo en TimeUnitType|Valor numérico|  
|-----------------------------------|-------------------|  
|Día|0|  
|Semana|1|  
|Month|2|  
|Year|3|  
  
### <a name="update-statistics-task"></a>Tarea Actualizar estadísticas  
 Propiedad **UpdateType**: se establece mediante el uso de valores de la enumeración **StatisticsTarget** de los objetos de administración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SMO).  
  
|Nombre descriptivo en StatisticsTarget|Valor numérico|  
|---------------------------------------|-------------------|  
|Columna|1|  
|Índice|2|  
|Todo|3|  
  
##  <a name="common-properties"></a><a name="CommonProperties"></a> Propiedades comunes  
 Los paquetes, tareas, y los contenedores de secuencias, de bucles Foreach y de bucles For pueden utilizar las siguientes enumeraciones para establecer las propiedades especificadas.  
  
 Propiedad **ForceExecutionResult**: se establece mediante el uso de valores de la enumeración **DTSForcedExecResult**.  
  
|Nombre descriptivo en DTSForcedExecResult|Valor numérico|  
|------------------------------------------|-------------------|  
|Ninguno|-1|  
|Correcto|0|  
|Error|1|  
|Completion|2|  
  
 Propiedad **IsolationLevel**: se establece mediante la enumeración **IsolationLevel** de .NET Framework. Para obtener más información, vea la biblioteca de clases de .NET Framework. en [MSDN Library](https://go.microsoft.com/fwlink?LinkId=17313).  
  
 Propiedad **LoggingMode**: se establece mediante el uso de valores de la enumeración **DTSLoggingMode**.  
  
|Nombre descriptivo en DTSLoggingMode|Valor numérico|  
|-------------------------------------|-------------------|  
|UseParentSetting|0|  
|habilitado|1|  
|Disabled|2|  
  
 Propiedad **TransactionOption**: se establece mediante el uso de valores de la enumeración **DTSTransactionOption**.  
  
|Nombre descriptivo en DTSTransactionOption|Valor numérico|  
|-------------------------------------------|-------------------|  
|NotSupported|0|  
|Compatible|1|  
|Obligatorio|2|  
  
## <a name="related-tasks"></a>Related Tasks  
 [Agregar o cambiar una expresión de propiedad](../../integration-services/expressions/add-or-change-a-property-expression.md)  
  
## <a name="see-also"></a>Vea también  
 [Usar expresiones de propiedad en paquetes](../../integration-services/expressions/use-property-expressions-in-packages.md)   
 [Paquetes de Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-packages.md)   
 [Contenedores de Integration Services](../../integration-services/control-flow/integration-services-containers.md)   
 [Tareas de Integration Services](../../integration-services/control-flow/integration-services-tasks.md)   
 [Restricciones de precedencia](../../integration-services/control-flow/precedence-constraints.md)  
  
  
