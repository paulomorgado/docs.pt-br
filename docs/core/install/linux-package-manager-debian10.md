---
title: Instalar o .NET Core no Debian 10-Package Manager-.NET Core
description: Use um Gerenciador de pacotes para instalar SDK do .NET Core e tempo de execução no Debian 10.
author: thraka
ms.author: adegeo
ms.date: 11/06/2019
ms.openlocfilehash: 1280758e7ea9300d83fa01532f3b051c6e1c0c67
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74451069"
---
# <a name="debian-10-package-manager---install-net-core"></a><span data-ttu-id="db798-103">Gerenciador de pacotes do Debian 10 – instalar o .NET Core</span><span class="sxs-lookup"><span data-stu-id="db798-103">Debian 10 Package Manager - Install .NET Core</span></span>

[!INCLUDE [package-manager-switcher](./includes/package-manager-switcher.md)]

<span data-ttu-id="db798-104">Este artigo descreve como usar um Gerenciador de pacotes para instalar o .NET Core no Debian 10.</span><span class="sxs-lookup"><span data-stu-id="db798-104">This article describes how to use a package manager to install .NET Core on Debian 10.</span></span> <span data-ttu-id="db798-105">Se você estiver instalando o tempo de execução, sugerimos que instale o [ASP.NET Core Runtime](#install-the-aspnet-core-runtime), pois ele inclui o .NET Core e ASP.NET Core Runtimes.</span><span class="sxs-lookup"><span data-stu-id="db798-105">If you're installing the runtime, we suggest you install the [ASP.NET Core runtime](#install-the-aspnet-core-runtime), as it includes both .NET Core and ASP.NET Core runtimes.</span></span>

## <a name="register-microsoft-key-and-feed"></a><span data-ttu-id="db798-106">Registrar chave e feed da Microsoft</span><span class="sxs-lookup"><span data-stu-id="db798-106">Register Microsoft key and feed</span></span>

<span data-ttu-id="db798-107">Antes de instalar o .NET, você precisará:</span><span class="sxs-lookup"><span data-stu-id="db798-107">Before installing .NET, you'll need to:</span></span>

- <span data-ttu-id="db798-108">Registrar a chave da Microsoft</span><span class="sxs-lookup"><span data-stu-id="db798-108">Register the Microsoft key</span></span>
- <span data-ttu-id="db798-109">registrar o repositório do produto</span><span class="sxs-lookup"><span data-stu-id="db798-109">register the product repository</span></span>
- <span data-ttu-id="db798-110">Instalar dependências necessárias</span><span class="sxs-lookup"><span data-stu-id="db798-110">Install required dependencies</span></span>

<span data-ttu-id="db798-111">Isso só precisa ser feito uma vez por computador.</span><span class="sxs-lookup"><span data-stu-id="db798-111">This only needs to be done once per machine.</span></span>

<span data-ttu-id="db798-112">Abra um terminal e execute os comandos a seguir.</span><span class="sxs-lookup"><span data-stu-id="db798-112">Open a terminal and run the following commands.</span></span>

```bash
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.asc.gpg
sudo mv microsoft.asc.gpg /etc/apt/trusted.gpg.d/
wget -q https://packages.microsoft.com/config/debian/10/prod.list
sudo mv prod.list /etc/apt/sources.list.d/microsoft-prod.list
sudo chown root:root /etc/apt/trusted.gpg.d/microsoft.asc.gpg
sudo chown root:root /etc/apt/sources.list.d/microsoft-prod.list
```

## <a name="install-the-net-core-sdk"></a><span data-ttu-id="db798-113">Instalar o SDK do .NET Core</span><span class="sxs-lookup"><span data-stu-id="db798-113">Install the .NET Core SDK</span></span>

<span data-ttu-id="db798-114">Atualize os produtos disponíveis para instalação e, em seguida, instale o SDK do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="db798-114">Update the products available for installation, then install the .NET Core SDK.</span></span> <span data-ttu-id="db798-115">Em seu terminal, execute os comandos a seguir.</span><span class="sxs-lookup"><span data-stu-id="db798-115">In your terminal, run the following commands.</span></span>

```bash
sudo apt-get update
sudo apt-get install apt-transport-https
sudo apt-get update
sudo apt-get install dotnet-sdk-3.0
```

## <a name="install-the-aspnet-core-runtime"></a><span data-ttu-id="db798-116">Instalar o ASP.NET Core Runtime</span><span class="sxs-lookup"><span data-stu-id="db798-116">Install the ASP.NET Core runtime</span></span>

<span data-ttu-id="db798-117">Atualize os produtos disponíveis para instalação e, em seguida, instale o tempo de execução do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="db798-117">Update the products available for installation, then install the ASP.NET runtime.</span></span> <span data-ttu-id="db798-118">Em seu terminal, execute os comandos a seguir.</span><span class="sxs-lookup"><span data-stu-id="db798-118">In your terminal, run the following commands.</span></span>

```bash
sudo apt-get update
sudo apt-get install apt-transport-https
sudo apt-get update
sudo apt-get install aspnetcore-runtime-3.0
```

## <a name="install-the-net-core-runtime"></a><span data-ttu-id="db798-119">Instalar o tempo de execução do .NET Core</span><span class="sxs-lookup"><span data-stu-id="db798-119">Install the .NET Core runtime</span></span>

<span data-ttu-id="db798-120">Atualize os produtos disponíveis para instalação e, em seguida, instale o tempo de execução do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="db798-120">Update the products available for installation, then install the .NET Core runtime.</span></span> <span data-ttu-id="db798-121">Em seu terminal, execute os comandos a seguir.</span><span class="sxs-lookup"><span data-stu-id="db798-121">In your terminal, run the following commands.</span></span>

```bash
sudo apt-get update
sudo apt-get install apt-transport-https
sudo apt-get update
sudo apt-get install dotnet-runtime-3.0
```

## <a name="how-to-install-other-versions"></a><span data-ttu-id="db798-122">Como instalar outras versões</span><span class="sxs-lookup"><span data-stu-id="db798-122">How to install other versions</span></span>

[!INCLUDE [package-manager-switcher](./includes/package-manager-heading-hack-pkgname.md)]