---
title: Analisador de API do .NET
description: Saiba como o analisador de API do .NET pode ajudar a detectar problemas de compatibilidade de plataforma e de APIs preteridas.
author: oliag
ms.date: 02/20/2020
ms.technology: dotnet-standard
ms.openlocfilehash: e214c91f2beebc7f3b3324f4879deba9a5623f86
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2020
ms.locfileid: "78156128"
---
# <a name="net-api-analyzer"></a>Analisador de API do .NET

O analisador do API .NET é um analisador Roslyn que descobre riscos potenciais de compatibilidade para APIs em C# em diferentes plataformas e detecta chamadas a APIs preteridas. Pode ser útil para todos os desenvolvedores de C# em qualquer estágio do desenvolvimento.

O Analisador de API é fornecido como um pacote NuGet [Microsoft.DotNet.Analyzers.Compatibility](https://www.nuget.org/packages/Microsoft.DotNet.Analyzers.Compatibility/). Depois que você faz referência a ele em um projeto, ele automaticamente monitora o código e indica um problema de uso de API. Você também pode obter sugestões sobre possíveis correções clicando na lâmpada. O menu suspenso inclui uma opção para suprimir os avisos.

> [!NOTE]
> O analisador do .NET API ainda é uma versão de pré-lançamento.

## <a name="prerequisites"></a>Pré-requisitos

- Visual Studio 2017 e versões posteriores ou Visual Studio para Mac (todas as versões).

## <a name="discover-deprecated-apis"></a>Descubra APIs depreciadas

### <a name="what-are-deprecated-apis"></a>Quais são as APIs preteridas?

A família .NET é um conjunto de produtos grandes que são atualizados constantemente para atender melhor às necessidades dos clientes. É natural substituir algumas APIs e substituí-las por novas. Uma API é considerada preterida quando existe uma alternativa melhor. Uma maneira de informar que uma API foi preterida e não deve ser usada é marcá-la com o atributo <xref:System.ObsoleteAttribute>. A desvantagem dessa abordagem é que há apenas uma ID de diagnóstico para todas as APIs preteridas (para C#, [CS0612](../../csharp/misc/cs0612.md)). Isso significa que:

- É impossível ter documentos dedicados para cada caso.
- É impossível suprimir determinada categoria de avisos. Você pode suprimir todos ou nenhum deles.
- Para informar os usuários de uma nova preterição, um assembly referenciado ou um pacote de direcionamento deve ser atualizado.

O Analisador de API usa códigos de erro específicos de API que começam com DE (que representa Erro de Preterição), o que permite controlar a exibição de avisos individuais. As APIs preteridas identificadas pelo analisador são definidas no repositório [dotnet/platform-compat](https://github.com/dotnet/platform-compat).

### <a name="add-the-api-analyzer-to-your-project"></a>Adicione o Analisador de API ao seu projeto

1. Abra o Visual Studio.
2. Abra o projeto que deseja executar o analisador.
3. No **Solution Explorer,** clique com o botão direito do mouse no projeto e escolha **Gerenciar pacotes NuGet**. (Esta opção também está disponível no menu **Projeto**.)
4. Na guia NuGet Package Manager:
   1. Selecione "nuget.org" como fonte do Pacote.
   2. Vá para a guia **Procurar.**
   3. Selecione **Incluir pré-lançamento**.
   4. Procure **microsoft.DotNet.Analyzers.Compatibility**.
   5. Selecione esse pacote na lista.
   6. Selecione o botão **Instalar**.
   7. Selecione o botão **OK** na caixa de diálogo **Visualizar Alterações** e selecione o botão **Aceito** na caixa de diálogo **Aceitação da Licença**, se concordar com o termos de licença para os pacotes listados.

### <a name="use-the-api-analyzer"></a>Use o analisador de API

Quando uma API preterida, como <xref:System.Net.WebClient>, é usada em um código, o Analisador de API a realça com uma linha irregular verde. Quando você focaliza a chamada de API, uma lâmpada é exibida com informações sobre a substituição de API, como no seguinte exemplo:

!["Captura de tela da API WebClient com linha irregular verde e lâmpada à esquerda"](media/api-analyzer/green-squiggle.jpg)

A janela **Lista de Erros** contém avisos com uma ID exclusiva por API preterida, conforme mostrado no seguinte exemplo (`DE004`):

!["Captura de tela da janela Lista de Erros mostrando a ID e a descrição do aviso"](media/api-analyzer/warnings-id-and-descriptions.jpg "Janela lista de erros que inclui avisos.")

Clicando na ID, você vai para uma página da Web com informações detalhadas sobre por que a API foi preterida e sugestões sobre APIs alternativas que podem ser usadas.

Os avisos podem ser suprimidos clicando com o botão direito do mouse no membro realçado e selecionando **Suprimir \<ID de diagnóstico>**. Há duas maneiras de suprimir avisos:

- [localmente (no código-fonte)](#suppress-warnings-locally)
- [globalmente (em um arquivo de supressão)](#suppress-warnings-globally) ‒ recomendado

### <a name="suppress-warnings-locally"></a>Suprimir avisos localmente

Para suprimir avisos localmente, clique com o botão direito do mouse no membro para o que deseja suprimir avisos e, em seguida, selecione **Ações rápidas e refatorações** > **Suprimir *iD*\<de diagnóstico de id de diagnóstico>**  > na **Fonte**. A política de pré-processamento de aviso [#pragma](../../csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning.md) é adicionada ao código-fonte no escopo definido: !["Captura de tela de código enquadrado com #pragma warning disable"](media/api-analyzer/suppress-in-source.jpg)

### <a name="suppress-warnings-globally"></a>Suprimir avisos globalmente

Para suprimir avisos globalmente, clique com o botão direito do mouse no membro para o que deseja suprimir avisos e, em seguida, selecione **Ações rápidas e refatorações** > **Suprimir *iD*\<de diagnóstico de diagnóstico>**  > no Arquivo de **Supressão**.

!["Captura de tela da API WebClient com linha irregular verde e lâmpada à esquerda"](media/api-analyzer/suppress-in-sup-file.jpg)

Um arquivo *GlobalSuppressions.cs* é adicionado ao projeto após a primeira supressão. Novo supressões globais são anexadas a este arquivo.

!["Captura de tela da API WebClient com linha irregular verde e lâmpada à esquerda"](media/api-analyzer/suppression-file.jpg)

A supressão global é a maneira recomendada de garantir a consistência do uso da API em projetos.

## <a name="discover-cross-platform-issues"></a>Descubra problemas entre plataformas

De forma semelhante a APIs preteridas, o analisador identifica todas as APIs que não são entre plataformas. Por exemplo, <xref:System.Console.WindowWidth?displayProperty=nameWithType> funciona no Windows, mas não no Linux ou no macOS. A ID de diagnóstico é mostrada na janela **Lista de Erros**. Você pode suprimir esse aviso clicando e selecionando **Ações Rápidas e Refatorações**. Diferentemente de casos de preterição em que você tem duas opções (continuar usando o membro preterido e suprimir avisos ou não o utilizar), aqui, se estiver desenvolvendo o código apenas para algumas plataformas, você poderá suprimir todos os avisos para todas as outras plataformas em que não planejar executar o código. Para fazer isso, basta editar o arquivo de projeto e adicionar a propriedade `PlatformCompatIgnore` que lista todas as plataformas a serem ignoradas. Os valores aceitos são: `Linux`, `macOS` e `Windows`.

```xml
<PropertyGroup>
    <PlatformCompatIgnore>Linux;macOS</PlatformCompatIgnore>
</PropertyGroup>
```

Se o código for voltado para várias plataformas e você desejar aproveitar uma API que não tem suporte em algumas delas, poderá proteger essa parte do código com uma instrução `if`:

```csharp
if (RuntimeInformation.IsOSPlatform(OSPlatform.Windows))
{
     var w = Console.WindowWidth;
     // More code
}
```

Você também pode compilar condicionalmente por estrutura/sistema operacional de destino, mas, no momento, precisa fazer isso [manualmente](../frameworks.md#how-to-specify-target-frameworks).

## <a name="supported-diagnostics"></a>Diagnóstico com suporte

Atualmente, o analisador trata dos seguintes casos:

- Uso de uma API padrão do .NET que gera <xref:System.PlatformNotSupportedException> (PC001).
- Uso de uma API padrão do .NET que não está disponível no .NET Framework 4.6.1 (PC002).
- Uso de uma API nativa que não existe no UWP (PC003).
- Uso das APIs Delegate.BeginInvoke e EndInvoke (PC004).
- Uso de uma API que está marcada como preterida (DEXXXX).

## <a name="ci-machine"></a>Máquina de CI

Todos esses diagnósticos estão disponíveis não só no IDE, mas também na linha de comando, como parte da criação do projeto, o que inclui o servidor de CI.

## <a name="configuration"></a>Configuração

O usuário decide como o diagnóstico deve ser tratado: como avisos, erros, sugestões ou ser desligado. Por exemplo, como arquiteto, você pode decidir que problemas de compatibilidade devem ser tratados como erros, chamadas a algumas APIs preteridas geram avisos, enquanto outras só geram sugestões. Você pode configurar isso separadamente por ID de diagnóstico e por projeto. Para fazer isso no **Gerenciador de Soluções**, navegue até o nó **Dependências** em seu projeto. Expanda os árdeis **Dependências** > **Analyzers** > **Microsoft.DotNet.Analyzers.Compatibility**. Clique com o botão direito do mouse na ID de diagnóstico, selecione **Definir Severidade de Conjunto de Regras** e selecione a opção desejada.

!["Captura de tela do Gerenciador de Soluções mostrando o diagnóstico e a caixa de diálogo pop-up com Severidade de Conjunto de Regras"](media/api-analyzer/disable-notifications.jpg)

## <a name="see-also"></a>Confira também

- Postagem de blog [Introduzindo o analisador de API](https://devblogs.microsoft.com/dotnet/introducing-api-analyzer/).
- Vídeo de demonstração no YouTube [Analisador de API](https://youtu.be/eeBEahYXGd0).
