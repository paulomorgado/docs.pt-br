---
title: .NET Core introdução e visão geral
description: .NET Core é uma implementação modular e de alto desempenho do .NET para criar aplicativos Windows, Linux e macOS. Saiba mais sobre o .NET Core para começar.
author: richlander
ms.date: 03/26/2020
ms.custom: updateeachrelease
ms.openlocfilehash: f99b446bbd38b2b814c13afa13ab34cd5c9aa086
ms.sourcegitcommit: 62285ec11fa8e8424bab00511a90760c60e63c95
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/20/2020
ms.locfileid: "81645518"
---
# <a name="introduction-to-net-core"></a>Introdução ao .NET Core

[.NET Core](about.md) é uma plataforma de desenvolvimento [de código aberto](https://github.com/dotnet/runtime/blob/master/LICENSE.TXT)e de uso geral. Você pode criar aplicativos .NET Core para Windows, macOS e Linux para processadores x64, x86, ARM32 e ARM64 usando várias linguagens de programação. Estruturas e APIs são fornecidas para [nuvem,](/aspnet/core/) [IoT,](/archive/msdn-magazine/2019/august/net-core-cross-platform-iot-programming-with-net-core-3-0) [ui cliente](../desktop-wpf/overview/index.md)e [machine learning](/dotnet/machine-learning/).

[Baixe o .NET Core SDK](https://dotnet.microsoft.com/download) para experimentar o .NET Core na sua máquina. A versão mais recente é [.NET Core 3.1](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-1/).

## <a name="download-net-core"></a>Baixe o .NET Core

Você pode obter o .NET Core das seguintes maneiras:

* [Instaladores para Windows e macOS](https://dotnet.microsoft.com/download)
* [Pacotes do Linux](https://docs.microsoft.com/dotnet/core/install/linux-package-managers)
* [Contêineres do Docker](https://hub.docker.com/_/microsoft-dotnet-core/)
* [Zips e tarballs](https://dotnet.microsoft.com/download/dotnet-core/3.1)
* [Instalar scripts](https://dotnet.microsoft.com/download/dotnet-core/scripts)
* [Notas de Versão](https://github.com/dotnet/core/tree/master/release-notes)

## <a name="create-your-first-application"></a>Criar seu primeiro aplicativo

Após instalar o SDK do .NET Core, abra um prompt de comando. Use os seguintes comandos para criar e executar um aplicativo:

```dotnetcli
dotnet new console
dotnet run
```

Você deve ver o seguinte resultado:

```output
Hello World!
```

## <a name="contribute"></a>Contribuir

.NET Core é uma plataforma aberta. Todos podem participar.

* Filie problemas e perguntas sobre produtos na [Comunidade de Desenvolvedores](https://developercommunity.visualstudio.com/spaces/61/index.html).
* As contribuições dos produtos devem ser feitas em um dos repositórios do projeto, tais como [dotnet/runtime,](https://github.com/dotnet/runtime) [dotnet/sdk,](https://github.com/dotnet/sdk) [dotnet/rosyln](https://github.com/dotnet/roslyn)ou [aspnetcore](https://github.com/dotnet/aspnetcore). Para obter mais informações, consulte [os repos .NET Core](https://github.com/dotnet/core/blob/master/Documentation/core-repos.md).

## <a name="support"></a>Suporte

.NET Core é suportado pela Microsoft no Windows, macOS e Linux e pela Red Hat no Red Hat Enterprise Linux.

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Tutoriais do .NET Core](tutorials/index.md)

> [!div class="nextstepaction"]
> [Experimente o .NET Core no seu navegador](../csharp/tutorials/intro-to-csharp/numbers-in-csharp.yml)
