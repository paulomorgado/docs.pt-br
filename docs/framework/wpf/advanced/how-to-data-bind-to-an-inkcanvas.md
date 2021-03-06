---
title: 'Como: Associar dados a um InkCanvas'
ms.date: 03/30/2017
helpviewer_keywords:
- InkCanvas [WPF], binding data to
- binding data [WPF], to InkCanvas
ms.assetid: 8d6b4d9e-ea7f-4412-ba83-3feccec5a515
ms.openlocfilehash: 5d3fc0ed7b6176d7bc68bf20af42c311b5563908
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61776461"
---
# <a name="how-to-data-bind-to-an-inkcanvas"></a>Como: Associar dados a um InkCanvas
## <a name="example"></a>Exemplo  
 O exemplo a seguir demonstra como associar o <xref:System.Windows.Controls.InkCanvas.Strokes%2A> propriedade de um <xref:System.Windows.Controls.InkCanvas> para outro <xref:System.Windows.Controls.InkCanvas>.  
  
 [!code-xaml[InkCanvasBindingSnippet#2](~/samples/snippets/csharp/VS_Snippets_Wpf/InkCanvasBindingSnippet/CS/Window2.xaml#2)]  
  
 O exemplo a seguir demonstra como associar o <xref:System.Windows.Controls.InkCanvas.DefaultDrawingAttributes%2A> propriedade a uma fonte de dados.  
  
 [!code-xaml[InkCanvasBindingSnippet#3](~/samples/snippets/csharp/VS_Snippets_Wpf/InkCanvasBindingSnippet/CS/Window2.xaml#3)]  
[!code-xaml[InkCanvasBindingSnippet#4](~/samples/snippets/csharp/VS_Snippets_Wpf/InkCanvasBindingSnippet/CS/Window2.xaml#4)]  
  
 O exemplo a seguir declara dois <xref:System.Windows.Controls.InkCanvas> objetos no XAML e estabelece a associação de dados entre eles e outras fontes de dados.  A primeira <xref:System.Windows.Controls.InkCanvas>, chamado `ic`, está associado a duas fontes de dados.  O <xref:System.Windows.Controls.InkCanvas.EditingMode%2A> e <xref:System.Windows.Controls.InkCanvas.DefaultDrawingAttributes%2A> as propriedades no `ic` são associados a <xref:System.Windows.Controls.ListBox> objetos, que por sua vez são associados a matrizes definidas no XAML.  O <xref:System.Windows.Controls.InkCanvas.EditingMode%2A>, <xref:System.Windows.Controls.InkCanvas.DefaultDrawingAttributes%2A>, e <xref:System.Windows.Controls.InkCanvas.Strokes%2A> propriedades da segunda <xref:System.Windows.Controls.InkCanvas> são vinculados ao primeiro <xref:System.Windows.Controls.InkCanvas>, `ic`.  
  
 [!code-xaml[InkCanvasBindingSnippet#1](~/samples/snippets/csharp/VS_Snippets_Wpf/InkCanvasBindingSnippet/CS/Window1.xaml#1)]
