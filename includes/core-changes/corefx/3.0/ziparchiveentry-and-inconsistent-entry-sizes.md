---
ms.openlocfilehash: 9520f8c6b6671917f5694bc602293a00e2dab82d
ms.sourcegitcommit: 79a2d6a07ba4ed08979819666a0ee6927bbf1b01
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74568245"
---
### <a name="ziparchiveentry-no-longer-handles-archives-with-inconsistent-entry-sizes"></a><span data-ttu-id="fb6e0-101">O ZipArchiveEntry não lida mais com os arquivos mortos com tamanhos de entrada inconsistentes</span><span class="sxs-lookup"><span data-stu-id="fb6e0-101">ZipArchiveEntry no longer handles archives with inconsistent entry sizes</span></span>

<span data-ttu-id="fb6e0-102">Os arquivos zip listam Tamanho compactado e Tamanho descompactado no diretório central e no cabeçalho local.</span><span class="sxs-lookup"><span data-stu-id="fb6e0-102">Zip archives list both compressed size and uncompressed size in the central directory and local header.</span></span>  <span data-ttu-id="fb6e0-103">Os próprios dados de entrada também indicam seu tamanho.</span><span class="sxs-lookup"><span data-stu-id="fb6e0-103">The entry data itself also indicates its size.</span></span>  <span data-ttu-id="fb6e0-104">No .NET Core 2,2 e versões anteriores, esses valores nunca foram verificados quanto à consistência.</span><span class="sxs-lookup"><span data-stu-id="fb6e0-104">In .NET Core 2.2 and earlier versions, these values were never checked for consistency.</span></span> <span data-ttu-id="fb6e0-105">A partir do .NET Core 3,0, eles agora são.</span><span class="sxs-lookup"><span data-stu-id="fb6e0-105">Starting with .NET Core 3.0, they now are.</span></span>

#### <a name="change-description"></a><span data-ttu-id="fb6e0-106">Alterar descrição</span><span class="sxs-lookup"><span data-stu-id="fb6e0-106">Change description</span></span>

<span data-ttu-id="fb6e0-107">No .NET Core 2,2 e versões anteriores, <xref:System.IO.Compression.ZipArchiveEntry.Open?displayProperty=nameWithType> é bem sucedido mesmo que o cabeçalho local desconcorde com o cabeçalho central do arquivo zip.</span><span class="sxs-lookup"><span data-stu-id="fb6e0-107">In .NET Core 2.2 and earlier versions, <xref:System.IO.Compression.ZipArchiveEntry.Open?displayProperty=nameWithType> succeeds even if the local header disagrees with the central header of the zip file.</span></span> <span data-ttu-id="fb6e0-108">Os dados são descompactados até que o final do fluxo compactado seja atingido, mesmo que seu comprimento exceda o tamanho do arquivo descompactado listado no cabeçalho do diretório central/local.</span><span class="sxs-lookup"><span data-stu-id="fb6e0-108">Data is decompressed until the end of the compressed stream is reached, even if its length exceeds the uncompressed file size listed in the central directory/local header.</span></span>

<span data-ttu-id="fb6e0-109">A partir do .NET Core 3,0, o método <xref:System.IO.Compression.ZipArchiveEntry.Open?displayProperty=nameWithType> verifica se o cabeçalho local e o cabeçalho central concordam em tamanhos compactados e descompactados de uma entrada.</span><span class="sxs-lookup"><span data-stu-id="fb6e0-109">Starting with .NET Core 3.0, the <xref:System.IO.Compression.ZipArchiveEntry.Open?displayProperty=nameWithType> method checks that local header and central header agree on compressed and uncompressed sizes of an entry.</span></span>  <span data-ttu-id="fb6e0-110">Se não tiverem, o método lançará um <xref:System.IO.InvalidDataException> se os tamanhos de lista de cabeçalho local e/ou descritor de dados do arquivo que discordarem com o diretório central do arquivo zip.</span><span class="sxs-lookup"><span data-stu-id="fb6e0-110">If they do not, the method throws an <xref:System.IO.InvalidDataException> if the archive's local header and/or data descriptor list sizes that disagree with the central directory of the zip file.</span></span> <span data-ttu-id="fb6e0-111">Ao ler uma entrada, os dados descompactados são truncados para o tamanho de arquivo descompactado listado no cabeçalho.</span><span class="sxs-lookup"><span data-stu-id="fb6e0-111">When reading an entry, decompressed data is truncated to the uncompressed file size listed in the header.</span></span>

<span data-ttu-id="fb6e0-112">Essa alteração foi feita para garantir que um <xref:System.IO.Compression.ZipArchiveEntry> represente corretamente o tamanho de seus dados e que apenas essa quantidade de dados seja lida.</span><span class="sxs-lookup"><span data-stu-id="fb6e0-112">This change was made to ensure that a <xref:System.IO.Compression.ZipArchiveEntry> correctly represents the size of its data and that only that amount of data is read.</span></span>

#### <a name="version-introduced"></a><span data-ttu-id="fb6e0-113">Versão introduzida</span><span class="sxs-lookup"><span data-stu-id="fb6e0-113">Version introduced</span></span>

<span data-ttu-id="fb6e0-114">3.0</span><span class="sxs-lookup"><span data-stu-id="fb6e0-114">3.0</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="fb6e0-115">Ação recomendada</span><span class="sxs-lookup"><span data-stu-id="fb6e0-115">Recommended action</span></span>

<span data-ttu-id="fb6e0-116">Empacote novamente qualquer arquivo zip que exiba esses problemas.</span><span class="sxs-lookup"><span data-stu-id="fb6e0-116">Repackage any zip archive that exhibits these problems.</span></span>

#### <a name="category"></a><span data-ttu-id="fb6e0-117">Categoria</span><span class="sxs-lookup"><span data-stu-id="fb6e0-117">Category</span></span>

<span data-ttu-id="fb6e0-118">CoreFx</span><span class="sxs-lookup"><span data-stu-id="fb6e0-118">CoreFx</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="fb6e0-119">APIs afetadas</span><span class="sxs-lookup"><span data-stu-id="fb6e0-119">Affected APIs</span></span>

- <xref:System.IO.Compression.ZipArchiveEntry.Open?displayProperty=nameWithType>
- <xref:System.IO.Compression.ZipFileExtensions.ExtractToDirectory%2A?displayProperty=nameWithType>
- <xref:System.IO.Compression.ZipFileExtensions.ExtractToFile%2A?displayProperty=nameWithType>
- <xref:System.IO.Compression.ZipFile.ExtractToDirectory%2A?displayProperty=nameWithType>

<!--

### Affected APIs

`M:System.IO.Compression.ZipArchiveEntry.Open`
`Overload:System.IO.Compression.ZipFileExtensions.ExtractToDirectory%2A`
`Overload:System.IO.Compression.ZipFileExtensions.ExtractToFile%2A`
`Overload:System.IO.Compression.ZipFile.ExtractToDirectory%2A`

-->