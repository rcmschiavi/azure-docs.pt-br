---
title: "Manutenção planejada para VMs Linux no Azure | Microsoft Docs"
description: "Entenda o que é a manutenção planejada do Azure e como ela afeta as máquinas virtuais Windows em execução no Azure."
services: virtual-machines-linux
documentationcenter: 
author: zivr
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/27/2017
ms.author: zivr
ms.translationtype: Human Translation
ms.sourcegitcommit: 43aab8d52e854636f7ea2ff3aae50d7827735cc7
ms.openlocfilehash: f5e52f4642b85a75f0c9f1892fbf0e565aa8b35b
ms.contentlocale: pt-br
ms.lasthandoff: 06/03/2017


---

# <a name="planned-maintenance-for-linux-virtual-machines"></a>Manutenção planejada para máquinas virtuais Linux 

O Microsoft Azure realiza atualizações periodicamente em todo o mundo para aumentar a confiabilidade, o desempenho e a segurança da infraestrutura de host subjacente a máquinas virtuais. Essas atualizações vão desde a aplicação de patch de componentes de software no ambiente de hospedagem (sistema operacional, hipervisor e vários agentes implantados no host), a atualização de componentes de rede até o encerramento de hardware.

A maioria dessas atualizações é realizada sem nenhum impacto às máquinas virtuais hospedadas ou aos serviços de nuvem.

No entanto, há casos em que as atualizações afetam as máquinas virtuais hospedadas:

-   A manutenção para preservar a VM com o uso da migração de VM in-loco descreve uma classe de atualizações em que as máquinas virtuais não são reinicializadas durante a manutenção.

-   A manutenção de reinicialização de VM que exige uma reinicialização ou reimplantação em máquinas virtuais hospedadas.

Observe que esta página descreve como o Microsoft Azure realiza a manutenção planejada. Para obter mais informações sobre eventos não planejados (interrupções), consulte [Gerenciar a disponibilidade das máquinas virtuais](../linux/manage-availability.md).

