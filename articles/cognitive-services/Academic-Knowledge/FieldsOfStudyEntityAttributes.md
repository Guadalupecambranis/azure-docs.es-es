---
title: 'Atributos de la entidad de campo de estudio: Academic Knowledge API'
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre los atributos que puede usar con la entidad de campo de estudio en Academic Knowledge API.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/31/2017
ms.author: alch
ms.openlocfilehash: e9d6badf76efd03c0520a728af7b3e47b25f200a
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55878959"
---
# <a name="field-of-study-entity"></a>Entidad de campo de estudio

<sub> *Los atributos siguientes son específicos de la entidad de campo de estudio. (Ty = '6') </sub>

NOMBRE    |DESCRIPCIÓN                            |Type       | Operaciones
------- | ------------------------------------- | --------- | ----------------------------
Id      |El identificador de entidad                              |Int64      |Equals
FN      |Nombre normalizado de campo de estudio         |string     |Equals
DFN     |Nombre para mostrar de campo de estudio            |string     |None
CC      |Recuento de citas total de campo de estudio    |Int32      |None  
ECC     |Recuento de citas estimado total de campo de estudio|Int32      |None
FL      |Nivel en jerarquía de campos de estudio     |Int32      |Equals, <br/>IsBetween
FP.FN   |Nombre de campo de estudio primario             |string     |Equals
FP.FId  |Identificador de campo de estudio primario               |Int64      |Equals
FC.FN   |Nombre de campo de estudio secundario              |string     |Equals
FC.FId  |Identificador de campo de estudio secundario                |Int64      |Equals
