---
title: Segurança e geração de código durante a execução
description: Gerar código em nome de código menos confiável que é executado em uma confiança mais alta é uma preocupação de segurança, especialmente quando um chamador pode influenciar a geração de código.
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- code security, on-the-fly code generation
- on-the-fly code generation
- security [.NET Framework], on-the-fly code generation
- secure coding, on-the-fly code generation
ms.assetid: 6d221724-bb21-4d76-90c3-0ee2a2e69be2
ms.openlocfilehash: 7e5168aa9305c559cf5ea2fb197b2c23ce2a05b0
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84291026"
---
# <a name="security-and-on-the-fly-code-generation"></a>Segurança e geração de código durante a execução
Algumas bibliotecas operam gerando código e executando-o para executar alguma operação para o chamador. O problema básico é gerar código em nome de código menos confiável e executá-lo em uma relação de confiança maior. O problema piora quando o chamador pode influenciar a geração de código, portanto, você deve garantir que apenas o código considerado seguro seja gerado.  
  
 Você precisa saber exatamente o código que está gerando em todos os momentos. Isso significa que você deve ter controles estritos sobre quaisquer valores obtidos de um usuário, sejam eles cadeias de caracteres entre aspas (que devem ser precedidas para que não possam incluir elementos de código inesperados), identificadores (que devem ser verificados para verificar se são identificadores válidos) ou qualquer outra coisa. Os identificadores podem ser perigosos porque um assembly compilado pode ser modificado para que seus identificadores contenham caracteres estranhos, o que provavelmente irá quebrá-lo (embora isso raramente seja uma vulnerabilidade de segurança).  
  
 É recomendável que você gere código com emissão de reflexo, o que geralmente ajuda a evitar muitos desses problemas.  
  
 Ao compilar o código, considere se há alguma maneira como um programa mal-intencionado pode modificá-lo. Há uma pequena janela de tempo durante a qual o código mal-intencionado pode alterar o código-fonte no disco antes que o compilador o leia ou antes que seu código carregue o arquivo. dll? Nesse caso, você deve proteger o diretório que contém esses arquivos, usando uma lista de controle de acesso no sistema de arquivos, conforme apropriado.  
  
## <a name="see-also"></a>Veja também

- [Diretrizes de codificação segura](secure-coding-guidelines.md)
