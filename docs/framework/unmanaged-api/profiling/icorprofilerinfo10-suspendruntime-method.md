---
title: ICorProfilerInfo10::SuspendRuntime
ms.date: 08/06/2019
dev_langs:
- cpp
api_name:
- ICorProfilerInfo10.SuspendRuntime
api_location:
- mscorwks.dll
api_type:
- COM
author: davmason
ms.author: davmason
ms.openlocfilehash: 1d0e1914ac13b5004f9e66a6e6dc2d4c914c6fd5
ms.sourcegitcommit: a97ecb94437362b21fffc5eb3c38b6c0b4368999
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68973996"
---
# <a name="icorprofilerinfo10suspendruntime-method"></a><span data-ttu-id="1ff7d-102">Método ICorProfilerInfo10:: SuspendRuntime</span><span class="sxs-lookup"><span data-stu-id="1ff7d-102">ICorProfilerInfo10::SuspendRuntime Method</span></span>
  
 <span data-ttu-id="1ff7d-103">Suspende o tempo de execução sem executar um GC.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-103">Suspends the runtime without performing a GC.</span></span>   
  
## <a name="syntax"></a><span data-ttu-id="1ff7d-104">Sintaxe</span><span class="sxs-lookup"><span data-stu-id="1ff7d-104">Syntax</span></span>  
  
```cpp
HRESULT SuspendRuntime();
```  

## <a name="requirements"></a><span data-ttu-id="1ff7d-105">Requisitos</span><span class="sxs-lookup"><span data-stu-id="1ff7d-105">Requirements</span></span>  
 <span data-ttu-id="1ff7d-106">**Compatíveis** Consulte [sistemas operacionais com suporte do .NET Core](../../../core/windows-prerequisites.md#net-core-supported-operating-systems).</span><span class="sxs-lookup"><span data-stu-id="1ff7d-106">**Platforms:** See [.NET Core supported operating systems](../../../core/windows-prerequisites.md#net-core-supported-operating-systems).</span></span>  
  
 <span data-ttu-id="1ff7d-107">**Cabeçalho:** CorProf.idl, CorProf.h</span><span class="sxs-lookup"><span data-stu-id="1ff7d-107">**Header:** CorProf.idl, CorProf.h</span></span>  
  
 <span data-ttu-id="1ff7d-108">**Biblioteca** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="1ff7d-108">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="1ff7d-109">**Versões do .net:** [!INCLUDE[net_core_22](../../../../includes/net-core-30-md.md)]</span><span class="sxs-lookup"><span data-stu-id="1ff7d-109">**.NET Versions:** [!INCLUDE[net_core_22](../../../../includes/net-core-30-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="1ff7d-110">Consulte também</span><span class="sxs-lookup"><span data-stu-id="1ff7d-110">See also</span></span>
- [<span data-ttu-id="1ff7d-111">Interface ICorProfilerInfo10</span><span class="sxs-lookup"><span data-stu-id="1ff7d-111">ICorProfilerInfo10 Interface</span></span>](../../../../docs/framework/unmanaged-api/profiling/icorprofilerinfo10-interface.md)
