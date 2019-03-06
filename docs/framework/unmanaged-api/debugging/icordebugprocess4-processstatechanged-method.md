---
title: Método ICorDebugProcess4::ProcessStateChanged
ms.date: 02/07/2019
api_name:
- ICorDebugProcess4::ProcessStateChanged
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugProcess4::ProcessStateChanged
helpviewer_keywords:
- ICorDebugProcess4::ProcessStateChanged method [.NET Framework debugging]
topic_type:
- apiref
author: hoyosjs
ms.author: juhoyosa
ms.openlocfilehash: b77dd1277e7d23729f30d9d495c5417055a22759
ms.sourcegitcommit: 0c48191d6d641ce88d7510e319cf38c0e35697d0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57378971"
---
# <a name="icordebugprocess4processstatechanged-method"></a><span data-ttu-id="7a81a-102">Método ICorDebugProcess4::ProcessStateChanged</span><span class="sxs-lookup"><span data-stu-id="7a81a-102">ICorDebugProcess4::ProcessStateChanged Method</span></span>

<span data-ttu-id="7a81a-103">Notifica o pipeline ICorDebug que fora do depurador de processo está continuando a execução do depurado.</span><span class="sxs-lookup"><span data-stu-id="7a81a-103">Notifies the ICorDebug pipeline that the out of process debugger is continuing the debugee's execution.</span></span>

## <a name="syntax"></a><span data-ttu-id="7a81a-104">Sintaxe</span><span class="sxs-lookup"><span data-stu-id="7a81a-104">Syntax</span></span>

```
HRESULT ProcessStateChanged(
    [in] CorDebugStateChange change
);
```

## <a name="parameters"></a><span data-ttu-id="7a81a-105">Parâmetros</span><span class="sxs-lookup"><span data-stu-id="7a81a-105">Parameters</span></span>

 `eChange`\
<span data-ttu-id="7a81a-106">[in] Um membro do [enumeração CorDebugStateChange](cordebugstatechange-enumeration.md) que descreve uma alteração no estado de execução do processo.</span><span class="sxs-lookup"><span data-stu-id="7a81a-106">[in] A member of the [CorDebugStateChange enumeration](cordebugstatechange-enumeration.md) describing a change in the process's execution state.</span></span>

## <a name="remarks"></a><span data-ttu-id="7a81a-107">Comentários</span><span class="sxs-lookup"><span data-stu-id="7a81a-107">Remarks</span></span>

<span data-ttu-id="7a81a-108">O método fornecido é parte do `ICorDebugProcess4` de interface e corresponde ao slot de quarto da tabela de método virtual.</span><span class="sxs-lookup"><span data-stu-id="7a81a-108">The provided method is part of the `ICorDebugProcess4` interface and corresponds to the fourth slot of the virtual method table.</span></span>

## <a name="requirements"></a><span data-ttu-id="7a81a-109">Requisitos</span><span class="sxs-lookup"><span data-stu-id="7a81a-109">Requirements</span></span>

 <span data-ttu-id="7a81a-110">**Plataformas:** Confira [Requisitos de sistema](../../../../docs/framework/get-started/system-requirements.md).</span><span class="sxs-lookup"><span data-stu-id="7a81a-110">**Platforms:** See [System Requirements](../../../../docs/framework/get-started/system-requirements.md).</span></span>

 <span data-ttu-id="7a81a-111">**Cabeçalho:** Nenhum</span><span class="sxs-lookup"><span data-stu-id="7a81a-111">**Header:** None</span></span>

 <span data-ttu-id="7a81a-112">**Biblioteca:** Nenhum</span><span class="sxs-lookup"><span data-stu-id="7a81a-112">**Library:** None</span></span>
 
 <span data-ttu-id="7a81a-113">**Versões do .NET Framework:** [!INCLUDE[net_current_v45plus](../../../../includes/net-current-v20plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="7a81a-113">**.NET Framework Versions:** [!INCLUDE[net_current_v45plus](../../../../includes/net-current-v20plus-md.md)]</span></span>

## <a name="see-also"></a><span data-ttu-id="7a81a-114">Consulte também</span><span class="sxs-lookup"><span data-stu-id="7a81a-114">See also</span></span>

- [<span data-ttu-id="7a81a-115">Interface ICorDebugProcess4</span><span class="sxs-lookup"><span data-stu-id="7a81a-115">ICorDebugProcess4 Interface</span></span>](icordebugprocess4-interface.md)
- [<span data-ttu-id="7a81a-116">Depurando interfaces</span><span class="sxs-lookup"><span data-stu-id="7a81a-116">Debugging Interfaces</span></span>](debugging-interfaces.md)
- [<span data-ttu-id="7a81a-117">Depuração</span><span class="sxs-lookup"><span data-stu-id="7a81a-117">Debugging</span></span>](index.md)