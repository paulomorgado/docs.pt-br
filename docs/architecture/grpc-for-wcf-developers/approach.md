---
title: Como o gRPC se aproxima de RPC-gRPC para desenvolvedores do WCF
description: Comparando os principais recursos do WCF para o gRPC.
author: markrendle
ms.date: 09/02/2019
ms.openlocfilehash: 65d61c8246569d81dfec3aeb8e3df4bea26258dc
ms.sourcegitcommit: 55f438d4d00a34b9aca9eedaac3f85590bb11565
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71184599"
---
# <a name="how-grpc-approaches-rpc"></a><span data-ttu-id="4585e-103">Como o gRPC se aproxima de RPC</span><span class="sxs-lookup"><span data-stu-id="4585e-103">How gRPC approaches RPC</span></span>

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

<span data-ttu-id="4585e-104">Windows Communication Foundation (WCF) e gRPC são implementações do padrão RPC ( *chamada de procedimento remoto* ), que visa fazer chamadas para serviços em execução em um computador diferente, ou em um processo diferente, trabalhar de forma direta como se fosse apenas chamadas de método no aplicativo cliente.</span><span class="sxs-lookup"><span data-stu-id="4585e-104">Windows Communication Foundation (WCF) and gRPC are both implementations of the *Remote Procedure Call* (RPC) pattern, which aims to make calls to services running on a different machine, or in a different process, seamlessly work as though they were just method calls in the client application.</span></span> <span data-ttu-id="4585e-105">Embora os objetivos do WCF e do gRPC sejam os mesmos, os detalhes da implementação são bastante diferentes.</span><span class="sxs-lookup"><span data-stu-id="4585e-105">While the aims of WCF and gRPC are the same, the details of the implementation are quite different.</span></span>

<span data-ttu-id="4585e-106">A tabela a seguir define como os principais recursos do WCF se relacionam com o gRPC e onde você pode encontrar explicações mais detalhadas no restante do livro.</span><span class="sxs-lookup"><span data-stu-id="4585e-106">The following table sets out how the key features of WCF relate to gRPC and where you can find more detailed explanations in the rest of the book.</span></span>

| <span data-ttu-id="4585e-107">Recursos</span><span class="sxs-lookup"><span data-stu-id="4585e-107">Features</span></span> | <span data-ttu-id="4585e-108">WCF</span><span class="sxs-lookup"><span data-stu-id="4585e-108">WCF</span></span> | <span data-ttu-id="4585e-109">gRPC</span><span class="sxs-lookup"><span data-stu-id="4585e-109">gRPC</span></span> |
| -------- | --- | ---- |
| <span data-ttu-id="4585e-110">Objetivo</span><span class="sxs-lookup"><span data-stu-id="4585e-110">Objective</span></span> | <span data-ttu-id="4585e-111">Separar código comercial da implementação de rede</span><span class="sxs-lookup"><span data-stu-id="4585e-111">Separate business code from networking implementation</span></span> | <span data-ttu-id="4585e-112">Separe o código comercial da definição de interface e da implementação de rede</span><span class="sxs-lookup"><span data-stu-id="4585e-112">Separate business code from interface definition and networking implementation</span></span> |
| <span data-ttu-id="4585e-113">Definir serviços e mensagens (capítulo 3-4)</span><span class="sxs-lookup"><span data-stu-id="4585e-113">Define Services and messages (chapter 3-4)</span></span>  | <span data-ttu-id="4585e-114">Contrato de serviço, contrato de operação e contrato de dados</span><span class="sxs-lookup"><span data-stu-id="4585e-114">Service Contract, Operation Contract, and Data Contract</span></span> | <span data-ttu-id="4585e-115">Usa o arquivo proto para declarar serviços e mensagens</span><span class="sxs-lookup"><span data-stu-id="4585e-115">Uses proto file to declare services and messages</span></span> |
| <span data-ttu-id="4585e-116">Idioma (capítulo 3-5)</span><span class="sxs-lookup"><span data-stu-id="4585e-116">Language (chapter 3-5)</span></span> | <span data-ttu-id="4585e-117">Contratos escritos no C# ou Visual Basic</span><span class="sxs-lookup"><span data-stu-id="4585e-117">Contracts written in C# or Visual Basic</span></span> | <span data-ttu-id="4585e-118">Idioma do buffer de protocolo</span><span class="sxs-lookup"><span data-stu-id="4585e-118">Protocol Buffer language</span></span> |
| <span data-ttu-id="4585e-119">Formato de conexão (capítulo 3)</span><span class="sxs-lookup"><span data-stu-id="4585e-119">Wire format (chapter 3)</span></span> | <span data-ttu-id="4585e-120">Configurável, incluindo SOAP/XML, XML simples, JSON, binário do .NET e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="4585e-120">Configurable, including SOAP/XML, Plain XML, JSON, .NET Binary, and so on.</span></span> | <span data-ttu-id="4585e-121">Formato binário de buffer de protocolo (embora seja possível usar outros formatos).</span><span class="sxs-lookup"><span data-stu-id="4585e-121">Protocol Buffer binary format (although it's possible to use other formats).</span></span>
| <span data-ttu-id="4585e-122">Interoperabilidade (capítulo 4)</span><span class="sxs-lookup"><span data-stu-id="4585e-122">Interoperability (chapter 4)</span></span> | <span data-ttu-id="4585e-123">Ao usar SOAP sobre HTTP</span><span class="sxs-lookup"><span data-stu-id="4585e-123">When using SOAP over HTTP</span></span> | <span data-ttu-id="4585e-124">Suporte oficial: .NET, Java, Python, JavaScript, C/C++, go, Rust, Ruby, Swift, Dart, php.</span><span class="sxs-lookup"><span data-stu-id="4585e-124">Official support: .NET, Java, Python, JavaScript, C/C++, Go, Rust, Ruby, Swift, Dart, PHP.</span></span> <span data-ttu-id="4585e-125">Suporte não oficial para outros idiomas da Comunidade.</span><span class="sxs-lookup"><span data-stu-id="4585e-125">Unofficial support for other languages from the community.</span></span> |
| <span data-ttu-id="4585e-126">Rede (capítulo 4)</span><span class="sxs-lookup"><span data-stu-id="4585e-126">Networking (chapter 4)</span></span> | <span data-ttu-id="4585e-127">Configurado em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="4585e-127">Configured at runtime.</span></span> <span data-ttu-id="4585e-128">Alterne entre TCP, HTTP, MSMQ e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="4585e-128">Switch between TCP, HTTP, MSMQ, and so on.</span></span> | <span data-ttu-id="4585e-129">Sempre HTTP/2</span><span class="sxs-lookup"><span data-stu-id="4585e-129">Always HTTP/2</span></span> |
| <span data-ttu-id="4585e-130">Abordagem (capítulo 4)</span><span class="sxs-lookup"><span data-stu-id="4585e-130">Approach (chapter 4)</span></span> | <span data-ttu-id="4585e-131">Geração de tempo de execução de/Deserialization de serialização e código de rede em classes base</span><span class="sxs-lookup"><span data-stu-id="4585e-131">Runtime generation of serialization /deserialization and networking code in base classes</span></span> | <span data-ttu-id="4585e-132">Geração de tempo de compilação de/Deserialization de serialização e código de rede em classes base</span><span class="sxs-lookup"><span data-stu-id="4585e-132">Build-time generation of serialization /deserialization and networking code in base classes</span></span> |
| <span data-ttu-id="4585e-133">Segurança (capítulo 6)</span><span class="sxs-lookup"><span data-stu-id="4585e-133">Security (chapter 6)</span></span> | <span data-ttu-id="4585e-134">Autenticação, WS-Security, criptografia de mensagem</span><span class="sxs-lookup"><span data-stu-id="4585e-134">Authentication, WS-Security, message encryption</span></span> | <span data-ttu-id="4585e-135">Credenciais, segurança de ASP.NET Core, rede TLS</span><span class="sxs-lookup"><span data-stu-id="4585e-135">Credentials, ASP.NET Core security, TLS networking</span></span> |

>[!div class="step-by-step"]
><span data-ttu-id="4585e-136">[Anterior](grpc-overview.md)
>[Próximo](interface-definition-language.md)</span><span class="sxs-lookup"><span data-stu-id="4585e-136">[Previous](grpc-overview.md)
[Next](interface-definition-language.md)</span></span>