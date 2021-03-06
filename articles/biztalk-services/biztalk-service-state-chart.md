---
title: Tareas permitidas en diferentes condiciones o estados de BizTalk Services | Microsoft Docs
description: 'Las acciones o las operaciones admitidas en distintos estados de MABS: detener, iniciar, reiniciar, suspender, reanudar, eliminar, escalar, actualizar configuración y realizar copias de seguridad.'
services: biztalk-services
documentationcenter: ''
author: MandiOhlinger
manager: anneta
editor: ''
ms.assetid: aea738f3-ec76-4099-a41b-e17fea9e252f
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2016
ms.author: mandia
ms.openlocfilehash: bbe1288a42db307001ac778394ac410206f1df21
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51228205"
---
# <a name="what-you-can-and-cant-do-using-the-biztalk-service-state"></a>Qué puede y no puede hacer con el estado del servicio de BizTalk

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

En función del estado real del servicio de BizTalk, habrá operaciones que pueda realizar o no en el servicio de BizTalk.

Por ejemplo, supongamos que aprovisiona un nuevo servicio de BizTalk. Cuando finalice correctamente, el servicio de BizTalk se encuentra en estado `active`. En el estado activo, puede detener, suspender y eliminar el servicio de BizTalk. Si detiene el servicio de BizTalk, y se produce un error en la detención, entonces el servicio de BizTalk se convierte en un estado `StopFailed`. Puede reiniciar el servicio de BizTalk si se encuentra en el estado `StopFailed`. Si intenta realizar una operación no permitida, como la reanudación, se produce el error siguiente:

`Operation not allowed`

## <a name="view-the-possible-states"></a>Ver los estados posibles

Las tablas siguientes incluyen las operaciones o acciones que se pueden realizar cuando el servicio de BizTalk se encuentra en un estado concreto. Una marca ✔ significa que se permite la operación mientras esté en ese estado. y la entrada vacía indica que la operación no se puede realizar en ese estado.

| Estado de servicio | Iniciar | Detención | Reinicio | Suspensión | Reanudación | Eliminar | Escala | Actualizar <br/> Configuración | Copia de seguridad |
| --- | --- | --- | --- | --- | --- | --- |--- | --- | --- |
| Active |  | ✔ | ✔ | ✔ |  | ✔ |✔ |✔ |✔ |
| Disabled |  |  |  |  |  | ✔ | |  |  | 
| Suspended |  |  |  |  | ✔ | ✔ | |  | ✔ |
| Stopped | ✔ |  | ✔ |  |  | ✔ | |  | ✔ |
| Service Update Failed |  |  |  |  |  | ✔ | |  |  | 
| DisableFailed |  |  |  |  |  | ✔ | |  |  | 
| EnableFailed |  |  |  |  |  | ✔ | |  |  | 
| StartFailed <br/> StopFailed <br/> RestartFailed | ✔ | ✔ | ✔ |  |  | ✔ | | ✔ | |
| SuspendedFailed <br/> ResumeFailed|  |  |  | ✔ | ✔ | ✔ | |  |  | 
| CreatedFailed <br/> RestoreFailed |  |  |  |  |  | ✔ | |  |  | 
| ConfigUpdateFailed  |  |  | ✔ |  |  | ✔ | |✔ | |
| ScaleFailed |  |  |  |  |  | ✔ |✔ | |  |  | 



## <a name="see-also"></a>Otras referencias
* [Revise las pestañas Panel, Monitor, Escala, Configurar y Conexiones híbridas](https://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [BizTalk Services: gráfico de ediciones](https://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [BizTalk Services: copias de seguridad y restauración](https://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [BizTalk Services: limitaciones](https://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
* [BizTalk Services: nombre del emisor y clave del emisor](https://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
* [¿Cómo puedo comenzar a utilizar el SDK de Azure BizTalk Services?](https://go.microsoft.com/fwlink/p/?LinkID=302335)

