---
title: Interface ICLRMetaHostPolicy
ms.date: 03/30/2017
api_name:
- ICLRMetaHostPolicy
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRMetaHostPolicy
helpviewer_keywords:
- ICLRMetaHostPolicy interface [.NET Framework hosting]
ms.assetid: 1bdeccb6-0698-4c97-ad69-eae2b69e59f1
topic_type:
- apiref
ms.openlocfilehash: 79b62a5e2aad9cfcd14ba40c1abf0342bfe57a4b
ms.sourcegitcommit: 0926684d8d34f4c6b5acce58d2193db093cb9cf2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83703535"
---
# <a name="iclrmetahostpolicy-interface"></a>Interface ICLRMetaHostPolicy
Fornece o método [GetRequestedRuntime](iclrmetahostpolicy-getrequestedruntime-method.md) , que retorna um ponteiro para uma interface Common Language Runtime (CLR) com base em critérios de política, assembly gerenciado, versão e arquivo de configuração.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método GetRequestedRuntime](iclrmetahostpolicy-getrequestedruntime-method.md)|Fornece uma interface CLR preferida com base em critérios de política, assembly gerenciado, versão e arquivo de configuração.|  
  
## <a name="remarks"></a>Comentários  
 Você pode obter uma referência a essa interface chamando a função [CLRCreateInstance](clrcreateinstance-function.md) , conforme mostrado no código a seguir:  
  
```cpp  
ICLRMetaHostPolicy *pMetaHostPolicy = NULL;  
HRESULT hr = CLRCreateInstance(CLSID_CLRMetaHostPolicy,  
                   IID_ICLRMetaHostPolicy, (LPVOID*)&pMetaHostPolicy);  
```  
  
> [!NOTE]
> Essa interface, na verdade, não carrega nem ativa o CLR, mas simplesmente retorna a versão do CLR preferida com base nas versões disponíveis que estão instaladas ou carregadas.  
  
 A API de hospedagem do .NET Framework 4 consolida políticas para que os hosts com necessidades específicas possam usar a funcionalidade básica sem incorrer em penalidades involuntárias. Por exemplo, muitas das exportações de MSCorEE. dll serão vinculadas a um CLR específico, embora um método possa não exigi-la logicamente. A enumeração de [METAHOST_POLICY_FLAGS](metahost-policy-flags-enumeration.md) fornece políticas de associação que são comuns à maioria dos hosts.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MetaHost. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Veja também

- [Interfaces de hospedagem CLR adicionadas ao .NET Framework 4 e 4.5](clr-hosting-interfaces-added-in-the-net-framework-4-and-4-5.md)
- [Interfaces de hospedagem](hosting-interfaces.md)
- [Hospedagem](index.md)
