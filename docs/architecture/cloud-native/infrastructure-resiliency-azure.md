---
title: Resiliência da plataforma Azure
description: Arquitetando aplicativos .NET nativos da nuvem para o Azure | Resiliência de infraestrutura de nuvem com o Azure
ms.date: 06/30/2019
ms.openlocfilehash: 7f148588be97fa6bf8a055f5f5bed8e23908277f
ms.sourcegitcommit: 56f1d1203d0075a461a10a301459d3aa452f4f47
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71214203"
---
# <a name="azure-platform-resiliency"></a><span data-ttu-id="0c07b-103">Resiliência da plataforma Azure</span><span class="sxs-lookup"><span data-stu-id="0c07b-103">Azure platform resiliency</span></span>

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

<span data-ttu-id="0c07b-104">Criar um aplicativo confiável na nuvem é diferente do desenvolvimento de aplicativos local tradicional.</span><span class="sxs-lookup"><span data-stu-id="0c07b-104">Building a reliable application in the cloud is different from traditional on-premises application development.</span></span> <span data-ttu-id="0c07b-105">Embora historicamente você adquiriu um hardware de extremidade superior para escalar verticalmente, em um ambiente de nuvem, você escala horizontalmente. Em vez de tentar evitar falhas, o objetivo é minimizar seus efeitos e manter o sistema estável.</span><span class="sxs-lookup"><span data-stu-id="0c07b-105">While historically you purchased higher-end hardware to scale up, in a cloud environment you scale out. Instead of trying to prevent failures, the goal is to minimize their effects and keep the system stable.</span></span>

<span data-ttu-id="0c07b-106">Dito isso, os aplicativos de nuvem confiáveis exibem características distintas:</span><span class="sxs-lookup"><span data-stu-id="0c07b-106">That said, reliable cloud applications display distinct characteristics:</span></span>

- <span data-ttu-id="0c07b-107">Eles são resilientes, recuperam-se normalmente de problemas e continuam a funcionar.</span><span class="sxs-lookup"><span data-stu-id="0c07b-107">They're resilient, recover gracefully from problems, and continue to function.</span></span>
- <span data-ttu-id="0c07b-108">Eles são altamente disponíveis (HA) e são executados conforme projetados em um estado íntegro sem tempo de inatividade significativo.</span><span class="sxs-lookup"><span data-stu-id="0c07b-108">They're highly available (HA) and run as designed in a healthy state with no significant downtime.</span></span>

<span data-ttu-id="0c07b-109">Entender como essas características funcionam em conjunto e como elas afetam o custo-é essencial para a criação de um aplicativo de nuvem confiável.</span><span class="sxs-lookup"><span data-stu-id="0c07b-109">Understanding how these characteristics work together - and how they affect cost - is essential to building a reliable cloud-native application.</span></span> <span data-ttu-id="0c07b-110">Em seguida, veremos como você pode criar resiliência e disponibilidade em seus aplicativos nativos de nuvem aproveitando os recursos da nuvem do Azure.</span><span class="sxs-lookup"><span data-stu-id="0c07b-110">We'll next look at ways that you can build resiliency and availability into your cloud-native applications leveraging features from the Azure cloud.</span></span>

## <a name="design-with-redundancy"></a><span data-ttu-id="0c07b-111">Design com redundância</span><span class="sxs-lookup"><span data-stu-id="0c07b-111">Design with redundancy</span></span>

<span data-ttu-id="0c07b-112">As falhas variam no escopo do impacto.</span><span class="sxs-lookup"><span data-stu-id="0c07b-112">Failures vary in scope of impact.</span></span> <span data-ttu-id="0c07b-113">Uma falha de hardware, como um disco com falha, pode afetar um único nó em um cluster.</span><span class="sxs-lookup"><span data-stu-id="0c07b-113">A hardware failure, such as a failed disk, can affect a single node in a cluster.</span></span> <span data-ttu-id="0c07b-114">Um comutador de rede com falha pode afetar um rack de servidor inteiro.</span><span class="sxs-lookup"><span data-stu-id="0c07b-114">A failed network switch could affect an entire server rack.</span></span> <span data-ttu-id="0c07b-115">Falhas menos comuns, como perda de energia, podem interromper um datacenter inteiro.</span><span class="sxs-lookup"><span data-stu-id="0c07b-115">Less common failures, such as loss of power, could disrupt a whole datacenter.</span></span> <span data-ttu-id="0c07b-116">Raramente, uma região inteira fica indisponível.</span><span class="sxs-lookup"><span data-stu-id="0c07b-116">Rarely, an entire region becomes unavailable.</span></span>

<span data-ttu-id="0c07b-117">A [redundância](https://docs.microsoft.com/azure/architecture/guide/design-principles/redundancy) é uma maneira de fornecer resiliência do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0c07b-117">[Redundancy](https://docs.microsoft.com/azure/architecture/guide/design-principles/redundancy) is one way to provide application resilience.</span></span> <span data-ttu-id="0c07b-118">O nível exato de redundância necessário depende de seus requisitos de negócios e afetará o custo e a complexidade do seu sistema.</span><span class="sxs-lookup"><span data-stu-id="0c07b-118">The exact level of redundancy needed depends on your business requirements and will affect both cost and complexity of your system.</span></span> <span data-ttu-id="0c07b-119">Por exemplo, uma implantação de várias regiões é mais cara e mais complexa para ser gerenciada do que uma implantação de região única.</span><span class="sxs-lookup"><span data-stu-id="0c07b-119">For example, a multi-region deployment is more expensive and more complex to manage than a single-region deployment.</span></span> <span data-ttu-id="0c07b-120">Você precisará de procedimentos operacionais para gerenciar failover e failback.</span><span class="sxs-lookup"><span data-stu-id="0c07b-120">You'll need operational procedures to manage failover and failback.</span></span> <span data-ttu-id="0c07b-121">O custo adicional e a complexidade podem ser justificados para alguns cenários de negócios e não para outros.</span><span class="sxs-lookup"><span data-stu-id="0c07b-121">The additional cost and complexity might be justified for some business scenarios and not others.</span></span>

<span data-ttu-id="0c07b-122">Para arquitetar a redundância, você precisa identificar os caminhos críticos em seu aplicativo e, em seguida, determinar se há redundância em cada ponto do caminho?</span><span class="sxs-lookup"><span data-stu-id="0c07b-122">To architect redundancy, you need to identify the critical paths in your application, and then determine if there's redundancy at each point in the path?</span></span> <span data-ttu-id="0c07b-123">Se um subsistema falhar, o aplicativo fará failover para outra coisa?</span><span class="sxs-lookup"><span data-stu-id="0c07b-123">If a subsystem should fail, will the application fail over to something else?</span></span> <span data-ttu-id="0c07b-124">Por fim, você precisa de uma compreensão clara desses recursos incorporados à plataforma de nuvem do Azure que você pode aproveitar para atender aos seus requisitos de redundância.</span><span class="sxs-lookup"><span data-stu-id="0c07b-124">Finally, you need a clear understanding of those features built into the Azure cloud platform that you can leverage to meet your redundancy requirements.</span></span> <span data-ttu-id="0c07b-125">Aqui estão as recomendações para a arquitetura da redundância:</span><span class="sxs-lookup"><span data-stu-id="0c07b-125">Here are recommendations for architecting redundancy:</span></span>

- <span data-ttu-id="0c07b-126">*Implantar várias instâncias de serviços.*</span><span class="sxs-lookup"><span data-stu-id="0c07b-126">*Deploy multiple instances of services.*</span></span> <span data-ttu-id="0c07b-127">Se seu aplicativo depende de uma única instância de um serviço, ele cria um ponto único de falha.</span><span class="sxs-lookup"><span data-stu-id="0c07b-127">If your application depends on a single instance of a service, it creates a single point of failure.</span></span> <span data-ttu-id="0c07b-128">O provisionamento de várias instâncias melhora a resiliência e a escalabilidade.</span><span class="sxs-lookup"><span data-stu-id="0c07b-128">Provisioning multiple instances improves both resiliency and scalability.</span></span> <span data-ttu-id="0c07b-129">Ao hospedar no serviço kubernetes do Azure, você pode configurar declarativamente instâncias redundantes (conjuntos de réplicas) no arquivo de manifesto kubernetes.</span><span class="sxs-lookup"><span data-stu-id="0c07b-129">When hosting in Azure Kubernetes Service, you can declaratively configure redundant instances (replica sets) in the Kubernetes manifest file.</span></span> <span data-ttu-id="0c07b-130">O valor da contagem de réplicas pode ser gerenciado programaticamente, no portal ou por meio de recursos de dimensionamento automático, que serão discutidos posteriormente.</span><span class="sxs-lookup"><span data-stu-id="0c07b-130">The replica count value can be managed programmatically, in the portal or through autoscaling features, which will be discussed later on.</span></span>

- <span data-ttu-id="0c07b-131">*Aproveitando um balanceador de carga.*</span><span class="sxs-lookup"><span data-stu-id="0c07b-131">*Leveraging a load balancer.*</span></span> <span data-ttu-id="0c07b-132">O balanceamento de carga distribui as solicitações do aplicativo para instâncias de serviço íntegra e remove automaticamente instâncias não íntegras da rotação.</span><span class="sxs-lookup"><span data-stu-id="0c07b-132">Load-balancing distributes your application's requests to healthy service instances and automatically removes unhealthy instances from rotation.</span></span> <span data-ttu-id="0c07b-133">Ao implantar no kubernetes, o balanceamento de carga pode ser especificado no arquivo de manifesto kubernetes na seção serviços.</span><span class="sxs-lookup"><span data-stu-id="0c07b-133">When deploying to Kubernetes, load balancing can be specified in the Kubernetes manifest file in the Services section.</span></span>

- <span data-ttu-id="0c07b-134">*Planejar a implantação em multiregião.*</span><span class="sxs-lookup"><span data-stu-id="0c07b-134">*Plan for multiregion deployment.*</span></span> <span data-ttu-id="0c07b-135">Se seu aplicativo for implantado em uma única região e a região ficar indisponível, seu aplicativo também ficará indisponível.</span><span class="sxs-lookup"><span data-stu-id="0c07b-135">If your application is deployed to a single region, and the region becomes unavailable, your application will also become unavailable.</span></span> <span data-ttu-id="0c07b-136">Isso pode ser inaceitável nos termos dos contratos de nível de serviço de seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0c07b-136">This may be unacceptable under the terms of your application's service level agreements.</span></span> <span data-ttu-id="0c07b-137">Em vez disso, considere implantar seu aplicativo e seus serviços em várias regiões.</span><span class="sxs-lookup"><span data-stu-id="0c07b-137">Instead, consider deploying your application and its services across multiple regions.</span></span> <span data-ttu-id="0c07b-138">Por exemplo, um cluster AKS (serviço de kubernetes do Azure) é implantado em uma única região.</span><span class="sxs-lookup"><span data-stu-id="0c07b-138">For example, an Azure Kubernetes Service (AKS) cluster is deployed to a single region.</span></span> <span data-ttu-id="0c07b-139">Para proteger seu sistema de uma falha regional, você pode implantar seu aplicativo em vários clusters AKS em regiões diferentes e usar o recurso de [regiões emparelhadas](https://buildazure.com/2017/01/06/azure-region-pairs-explained/) para coordenar as atualizações da plataforma e priorizar os esforços de recuperação.</span><span class="sxs-lookup"><span data-stu-id="0c07b-139">To protect your system from a regional failure, you might deploy your application to multiple AKS clusters across different regions and use the [Paired Regions](https://buildazure.com/2017/01/06/azure-region-pairs-explained/) feature to coordinate platform updates and prioritize recovery efforts.</span></span>

- <span data-ttu-id="0c07b-140">*Habilite [a replicação geográfica](https://docs.microsoft.com/azure/sql-database/sql-database-active-geo-replication).*</span><span class="sxs-lookup"><span data-stu-id="0c07b-140">*Enable [geo-replication](https://docs.microsoft.com/azure/sql-database/sql-database-active-geo-replication).*</span></span> <span data-ttu-id="0c07b-141">A replicação geográfica para serviços, como o banco de dados SQL do Azure e o Cosmos DB criará réplicas secundárias de sua data em várias regiões.</span><span class="sxs-lookup"><span data-stu-id="0c07b-141">Geo-replication for services such as Azure SQL Database and Cosmos DB will create secondary replicas of your data across multiple regions.</span></span> <span data-ttu-id="0c07b-142">Embora os dois serviços repliquem automaticamente os dados na mesma região, a replicação geográfica protege você contra uma interrupção regional, permitindo o failover para uma região secundária.</span><span class="sxs-lookup"><span data-stu-id="0c07b-142">While both services will automatically replicate data within the same region, geo-replication protects you against a regional outage by enabling you to fail over to a secondary region.</span></span> <span data-ttu-id="0c07b-143">Outra prática recomendada para os centros de replicação geográfica em relação ao armazenamento de imagens de contêiner.</span><span class="sxs-lookup"><span data-stu-id="0c07b-143">Another best practice for geo-replication centers around storing container images.</span></span> <span data-ttu-id="0c07b-144">Para implantar um serviço no AKS, você precisa armazenar e efetuar pull da imagem de um repositório.</span><span class="sxs-lookup"><span data-stu-id="0c07b-144">To deploy a service in AKS, you need to store and pull the image from a repository.</span></span> <span data-ttu-id="0c07b-145">O registro de contêiner do Azure integra-se com o AKS e pode armazenar imagens de contêiner com segurança.</span><span class="sxs-lookup"><span data-stu-id="0c07b-145">Azure Container Registry integrates with AKS and can securely store container images.</span></span> <span data-ttu-id="0c07b-146">Para melhorar o desempenho e a disponibilidade, considere a replicação geográfica de suas imagens para um registro em cada região em que você tenha um cluster AKS.</span><span class="sxs-lookup"><span data-stu-id="0c07b-146">To improve performance and availability, consider geo-replicating your images to a registry in each region where you have an AKS cluster.</span></span> <span data-ttu-id="0c07b-147">Cada cluster AKS, em seguida, efetua pull das imagens de contêiner do registro de contêiner local em sua região, conforme mostrado na Figura 6-6:</span><span class="sxs-lookup"><span data-stu-id="0c07b-147">Each AKS cluster then pulls container images from the local container registry in its region as shown in Figure 6-6:</span></span>

![Recursos replicados entre regiões](./media/replicated-resources.png)

<span data-ttu-id="0c07b-149">**Figura 6-6**.</span><span class="sxs-lookup"><span data-stu-id="0c07b-149">**Figure 6-6**.</span></span> <span data-ttu-id="0c07b-150">Recursos replicados entre regiões</span><span class="sxs-lookup"><span data-stu-id="0c07b-150">Replicated resources across regions</span></span>

- <span data-ttu-id="0c07b-151">*Implemente um balanceador de carga de tráfego DNS.*</span><span class="sxs-lookup"><span data-stu-id="0c07b-151">*Implement a DNS traffic load balancer.*</span></span> <span data-ttu-id="0c07b-152">O [Gerenciador de tráfego do Azure](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) fornece alta disponibilidade para aplicativos críticos pelo balanceamento de carga no nível do DNS.</span><span class="sxs-lookup"><span data-stu-id="0c07b-152">[Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) provides high-availability for critical applications by load-balancing at the DNS level.</span></span> <span data-ttu-id="0c07b-153">Ele pode rotear o tráfego para diferentes regiões com base na geografia, no tempo de resposta do cluster e até mesmo na integridade do ponto de extremidade do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0c07b-153">It can route traffic to different regions based on geography, cluster response time, and even application endpoint health.</span></span> <span data-ttu-id="0c07b-154">Por exemplo, o Gerenciador de tráfego do Azure pode direcionar os clientes para a instância de aplicativo e cluster AKS mais próxima.</span><span class="sxs-lookup"><span data-stu-id="0c07b-154">For example, Azure Traffic Manager can direct customers to the closest AKS cluster and application instance.</span></span> <span data-ttu-id="0c07b-155">Se você tiver vários clusters AKS em regiões diferentes, use o Gerenciador de tráfego para controlar como o tráfego flui para os aplicativos que são executados em cada cluster.</span><span class="sxs-lookup"><span data-stu-id="0c07b-155">If you have multiple AKS clusters in different regions, use Traffic Manager to control how traffic flows to the applications that run in each cluster.</span></span> <span data-ttu-id="0c07b-156">A Figura 6-7 mostra esse cenário.</span><span class="sxs-lookup"><span data-stu-id="0c07b-156">Figure 6-7 shows this scenario.</span></span>

![AKS e Gerenciador de tráfego do Azure](./media/aks-traffic-manager.png)

<span data-ttu-id="0c07b-158">**Figura 6-7**.</span><span class="sxs-lookup"><span data-stu-id="0c07b-158">**Figure 6-7**.</span></span> <span data-ttu-id="0c07b-159">AKS e Gerenciador de tráfego do Azure</span><span class="sxs-lookup"><span data-stu-id="0c07b-159">AKS and Azure Traffic Manager</span></span>

## <a name="design-for-scalability"></a><span data-ttu-id="0c07b-160">Design para escalabilidade</span><span class="sxs-lookup"><span data-stu-id="0c07b-160">Design for scalability</span></span>

<span data-ttu-id="0c07b-161">A nuvem prospera no dimensionamento.</span><span class="sxs-lookup"><span data-stu-id="0c07b-161">The cloud thrives on scaling.</span></span> <span data-ttu-id="0c07b-162">A capacidade de aumentar/diminuir os recursos do sistema para lidar com a carga crescente/decrescente do sistema é um princípio fundamental da nuvem do Azure.</span><span class="sxs-lookup"><span data-stu-id="0c07b-162">The ability to increase/decrease system resources to address increasing/decreasing system load is a key tenet of the Azure cloud.</span></span> <span data-ttu-id="0c07b-163">Mas, para dimensionar um aplicativo com eficiência, você precisa compreender os recursos de dimensionamento de cada serviço do Azure que você inclui em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0c07b-163">But, to effectively scale an application, you need an understanding of the scaling features of each Azure service that you include in your application.</span></span>  <span data-ttu-id="0c07b-164">Aqui estão recomendações para implementar efetivamente o dimensionamento em seu sistema.</span><span class="sxs-lookup"><span data-stu-id="0c07b-164">Here are recommendations for effectively implementing scaling in your system.</span></span>

- <span data-ttu-id="0c07b-165">*Design para dimensionamento.*</span><span class="sxs-lookup"><span data-stu-id="0c07b-165">*Design for scaling.*</span></span> <span data-ttu-id="0c07b-166">Um aplicativo deve ser projetado para dimensionamento.</span><span class="sxs-lookup"><span data-stu-id="0c07b-166">An application must be designed for scaling.</span></span> <span data-ttu-id="0c07b-167">Para iniciar, os serviços devem ser sem estado para que as solicitações possam ser roteadas para qualquer instância.</span><span class="sxs-lookup"><span data-stu-id="0c07b-167">To start, services should be stateless so that requests can be routed to any instance.</span></span> <span data-ttu-id="0c07b-168">Ter serviços sem estado também significa que a adição ou remoção de uma instância não afeta negativamente os usuários atuais.</span><span class="sxs-lookup"><span data-stu-id="0c07b-168">Having stateless services also means that adding or removing an instance doesn't adversely impact current users.</span></span>

- <span data-ttu-id="0c07b-169">*Cargas de trabalho de partição*.</span><span class="sxs-lookup"><span data-stu-id="0c07b-169">*Partition workloads*.</span></span> <span data-ttu-id="0c07b-170">A decomposição de domínios em microserviços independentes e autônomos permite que cada serviço seja dimensionado de forma independente dos outros.</span><span class="sxs-lookup"><span data-stu-id="0c07b-170">Decomposing domains into independent, self-contained microservices enable each service to scale independently of others.</span></span> <span data-ttu-id="0c07b-171">Normalmente, os serviços terão necessidades e requisitos de escalabilidade diferentes.</span><span class="sxs-lookup"><span data-stu-id="0c07b-171">Typically, services will have different scalability needs and requirements.</span></span> <span data-ttu-id="0c07b-172">O particionamento permite que você dimensione apenas o que precisa ser dimensionado sem o custo desnecessário de dimensionamento de um aplicativo inteiro.</span><span class="sxs-lookup"><span data-stu-id="0c07b-172">Partitioning enables you to scale only what needs to be scaled without the unnecessary cost of scaling an entire application.</span></span>

- <span data-ttu-id="0c07b-173">*Favorecer a expansão.* Os aplicativos baseados em nuvem favorecem a expansão dos recursos em oposição ao dimensionamento.</span><span class="sxs-lookup"><span data-stu-id="0c07b-173">*Favor scale-out.* Cloud-based applications favor scaling out resources as opposed to scaling up.</span></span> <span data-ttu-id="0c07b-174">A expansão (também conhecida como escala horizontal) envolve a adição de mais recursos de serviço a um sistema existente para atender e compartilhar um nível de desempenho desejado.</span><span class="sxs-lookup"><span data-stu-id="0c07b-174">Scaling out (also known as horizontal scaling) involves adding more service resources to an existing system to meet and share a desired level of performance.</span></span> <span data-ttu-id="0c07b-175">O dimensionamento (também conhecido como dimensionamento vertical) envolve a substituição de recursos existentes por hardware mais potente (mais núcleos de disco, memória e processamento).</span><span class="sxs-lookup"><span data-stu-id="0c07b-175">Scaling up (also known as vertical scaling) involves replacing existing resources with more powerful hardware (more disk, memory, and processing cores).</span></span> <span data-ttu-id="0c07b-176">A expansão pode ser invocada automaticamente com os recursos de dimensionamento automático disponíveis em alguns recursos de nuvem do Azure.</span><span class="sxs-lookup"><span data-stu-id="0c07b-176">Scaling out can be invoked automatically with the autoscaling features available in some Azure cloud resources.</span></span> <span data-ttu-id="0c07b-177">Escalar horizontalmente em vários recursos também adiciona redundância ao sistema geral.</span><span class="sxs-lookup"><span data-stu-id="0c07b-177">Scaling out across multiple resources also adds redundancy to the overall system.</span></span> <span data-ttu-id="0c07b-178">Por fim, a expansão de um único recurso é normalmente mais cara do que escalar horizontalmente em muitos recursos menores.</span><span class="sxs-lookup"><span data-stu-id="0c07b-178">Finally scaling up a single resource is typically more expensive than scaling out across many smaller resources.</span></span> <span data-ttu-id="0c07b-179">A Figura 6-8 mostra as duas abordagens:</span><span class="sxs-lookup"><span data-stu-id="0c07b-179">Figure 6-8 shows the two approaches:</span></span>

![Escalar verticalmente versus escalar horizontalmente](./media/scale-up-scale-out.png)

<span data-ttu-id="0c07b-181">**Figura 6-8.**</span><span class="sxs-lookup"><span data-stu-id="0c07b-181">**Figure 6-8.**</span></span> <span data-ttu-id="0c07b-182">Escalar verticalmente versus escalar horizontalmente</span><span class="sxs-lookup"><span data-stu-id="0c07b-182">Scale up versus scale out</span></span>

- <span data-ttu-id="0c07b-183">*Dimensionar proporcionalmente.*</span><span class="sxs-lookup"><span data-stu-id="0c07b-183">*Scale proportionally.*</span></span> <span data-ttu-id="0c07b-184">Ao dimensionar um serviço, considere em termos de *conjuntos de recursos*.</span><span class="sxs-lookup"><span data-stu-id="0c07b-184">When scaling a service, think in terms of *resource sets*.</span></span> <span data-ttu-id="0c07b-185">Se você fosse expandir drasticamente um serviço específico, que impacto teria em armazenamentos de dados de back-end, em caches e serviços dependentes?</span><span class="sxs-lookup"><span data-stu-id="0c07b-185">If you were to dramatically scale out a specific service, what impact would that have on back-end data stores, caches and dependent services?</span></span> <span data-ttu-id="0c07b-186">Alguns recursos, como Cosmos DB, podem ser expandidos proporcionalmente, enquanto muitos outros não podem.</span><span class="sxs-lookup"><span data-stu-id="0c07b-186">Some resources such as Cosmos DB can scale out proportionally, while many others can't.</span></span> <span data-ttu-id="0c07b-187">Você deseja garantir que não expanda um recurso para um ponto em que ele esgotará outros recursos associados.</span><span class="sxs-lookup"><span data-stu-id="0c07b-187">You want to ensure that you don't scale out a resource to a point where it will exhaust other associated resources.</span></span>

- <span data-ttu-id="0c07b-188">*Evite a afinidade.*</span><span class="sxs-lookup"><span data-stu-id="0c07b-188">*Avoid affinity.*</span></span> <span data-ttu-id="0c07b-189">Uma prática recomendada é garantir que um nó não exija afinidade local, geralmente chamado de *sessão adesiva*.</span><span class="sxs-lookup"><span data-stu-id="0c07b-189">A best practice is to ensure a node doesn't require local affinity, often referred to as a *sticky session*.</span></span> <span data-ttu-id="0c07b-190">Uma solicitação deve ser capaz de rotear para qualquer instância.</span><span class="sxs-lookup"><span data-stu-id="0c07b-190">A request should be able to route to any instance.</span></span> <span data-ttu-id="0c07b-191">Se você precisar manter o estado, ele deve ser salvo em um cache distribuído, como o [cache Redis do Azure](https://azure.microsoft.com/services/cache/).</span><span class="sxs-lookup"><span data-stu-id="0c07b-191">If you need to persist state, it should be saved to a distributed cache, such as [Azure Redis cache](https://azure.microsoft.com/services/cache/).</span></span>

- <span data-ttu-id="0c07b-192">*Aproveite os recursos de dimensionamento automático da plataforma.*</span><span class="sxs-lookup"><span data-stu-id="0c07b-192">*Take advantage of platform autoscaling features.*</span></span> <span data-ttu-id="0c07b-193">Use recursos internos de dimensionamento automático sempre que possível, em vez de mecanismos personalizados ou de terceiros.</span><span class="sxs-lookup"><span data-stu-id="0c07b-193">Use built-in autoscaling features whenever possible, rather than custom or third-party mechanisms.</span></span> <span data-ttu-id="0c07b-194">Sempre que possível, use regras de dimensionamento agendadas para garantir que os recursos estejam disponíveis sem um atraso de inicialização, mas adicione o dimensionamento automático reativo às regras conforme apropriado, para lidar com alterações inesperadas na demanda.</span><span class="sxs-lookup"><span data-stu-id="0c07b-194">Where possible, use scheduled scaling rules to ensure that resources are available without a startup delay, but add reactive autoscaling to the rules as appropriate, to cope with unexpected changes in demand.</span></span> <span data-ttu-id="0c07b-195">Para obter mais informações, consulte [diretrizes de dimensionamento](https://docs.microsoft.com/azure/architecture/best-practices/auto-scaling)automático.</span><span class="sxs-lookup"><span data-stu-id="0c07b-195">For more information, see [Autoscaling guidance](https://docs.microsoft.com/azure/architecture/best-practices/auto-scaling).</span></span>

- <span data-ttu-id="0c07b-196">*Escalar verticalmente de forma agressiva.*</span><span class="sxs-lookup"><span data-stu-id="0c07b-196">*Scale-up aggressively.*</span></span> <span data-ttu-id="0c07b-197">Uma prática final seria escalar verticalmente de forma agressiva para que você possa atender rapidamente a picos imediatos de tráfego sem perder negócios.</span><span class="sxs-lookup"><span data-stu-id="0c07b-197">A final practice would be to scale up aggressively so that you can quickly meet immediate spikes in traffic without losing business.</span></span> <span data-ttu-id="0c07b-198">E, em seguida, reduzir verticalmente (ou seja, remover recursos desnecessários) de forma conservadora para manter o sistema estável.</span><span class="sxs-lookup"><span data-stu-id="0c07b-198">And, then scale down (that is, remove unneeded resources) conservatively to keep the system stable.</span></span> <span data-ttu-id="0c07b-199">Uma maneira simples de implementar isso é definir o período de resfriamento, que é o tempo de espera entre as operações de dimensionamento, cinco minutos para adicionar recursos e até 15 minutos para remover instâncias.</span><span class="sxs-lookup"><span data-stu-id="0c07b-199">A simple way to implement this is to set the cool down period, which is the time to wait between scaling operations, to five minutes for adding resources and up to 15 minutes for removing instances.</span></span>

## <a name="built-in-retry-in-services"></a><span data-ttu-id="0c07b-200">Repetição interna em serviços</span><span class="sxs-lookup"><span data-stu-id="0c07b-200">Built-in retry in services</span></span>

<span data-ttu-id="0c07b-201">Incentivamos a prática recomendada de implementar operações de repetição programáticas em uma seção anterior.</span><span class="sxs-lookup"><span data-stu-id="0c07b-201">We encouraged the best practice of implementing programmatic retry operations in an earlier section.</span></span> <span data-ttu-id="0c07b-202">Tenha em mente que muitos serviços do Azure e seus SDKs de cliente correspondentes também incluem mecanismos de repetição.</span><span class="sxs-lookup"><span data-stu-id="0c07b-202">Keep in mind that many Azure services and their corresponding client SDKs also include retry mechanisms.</span></span> <span data-ttu-id="0c07b-203">A lista a seguir resume os recursos de repetição nos muitos dos serviços do Azure que são discutidos neste livro:</span><span class="sxs-lookup"><span data-stu-id="0c07b-203">The following list summarizes retry features in the many of the Azure services that are discussed in this book:</span></span>

- <span data-ttu-id="0c07b-204">*Azure Cosmos DB.*</span><span class="sxs-lookup"><span data-stu-id="0c07b-204">*Azure Cosmos DB.*</span></span> <span data-ttu-id="0c07b-205">A <xref:Microsoft.Azure.Documents.Client.DocumentClient> classe da API do cliente desativa automaticamente as tentativas com falha.</span><span class="sxs-lookup"><span data-stu-id="0c07b-205">The <xref:Microsoft.Azure.Documents.Client.DocumentClient> class from the client API automatically retires failed attempts.</span></span> <span data-ttu-id="0c07b-206">O número de repetições e o tempo de espera máximo são configuráveis.</span><span class="sxs-lookup"><span data-stu-id="0c07b-206">The number of retries and maximum wait time are configurable.</span></span> <span data-ttu-id="0c07b-207">As exceções geradas pela API do cliente são solicitações que excedem a política de repetição ou erros não transitórios.</span><span class="sxs-lookup"><span data-stu-id="0c07b-207">Exceptions thrown by the client API are either requests that exceed the retry policy or non-transient errors.</span></span>

- <span data-ttu-id="0c07b-208">*Cache Redis do Azure.*</span><span class="sxs-lookup"><span data-stu-id="0c07b-208">*Azure Redis Cache.*</span></span> <span data-ttu-id="0c07b-209">O cliente Redis StackExchange usa uma classe de Gerenciador de conexões que inclui novas tentativas em tentativas com falha.</span><span class="sxs-lookup"><span data-stu-id="0c07b-209">The Redis StackExchange client uses a connection manager class that includes retries on failed attempts.</span></span> <span data-ttu-id="0c07b-210">O número de repetições, a política de repetição específica e o tempo de espera são todos configuráveis.</span><span class="sxs-lookup"><span data-stu-id="0c07b-210">The number of retries, specific retry policy and wait time are all configurable.</span></span>

- <span data-ttu-id="0c07b-211">*Barramento de serviço do Azure.*</span><span class="sxs-lookup"><span data-stu-id="0c07b-211">*Azure Service Bus.*</span></span> <span data-ttu-id="0c07b-212">O cliente do barramento de serviço expõe uma [classe RetryPolicy](xref:Microsoft.ServiceBus.RetryPolicy) que pode ser configurada com um intervalo de retirada, contagem <xref:Microsoft.ServiceBus.RetryExponential.TerminationTimeBuffer>de repetição e, que especifica o tempo máximo que uma operação pode tomar.</span><span class="sxs-lookup"><span data-stu-id="0c07b-212">The Service Bus client exposes a [RetryPolicy class](xref:Microsoft.ServiceBus.RetryPolicy) that can be configured with a back-off interval, retry count, and <xref:Microsoft.ServiceBus.RetryExponential.TerminationTimeBuffer>, which specifies the maximum time an operation can take.</span></span> <span data-ttu-id="0c07b-213">A política padrão é de nove tentativas de repetição máximas com um período de retirada de 30 segundos entre as tentativas.</span><span class="sxs-lookup"><span data-stu-id="0c07b-213">The default policy is nine maximum retry attempts with a 30-second backoff period between attempts.</span></span>

- <span data-ttu-id="0c07b-214">*Banco de dados SQL do Azure.*</span><span class="sxs-lookup"><span data-stu-id="0c07b-214">*Azure SQL Database.*</span></span> <span data-ttu-id="0c07b-215">O suporte de repetição é fornecido ao usar a biblioteca de [Entity Framework Core](https://docs.microsoft.com/ef/core/miscellaneous/connection-resiliency) .</span><span class="sxs-lookup"><span data-stu-id="0c07b-215">Retry support is provided when using the [Entity Framework Core](https://docs.microsoft.com/ef/core/miscellaneous/connection-resiliency) library.</span></span>

- <span data-ttu-id="0c07b-216">*Armazenamento do Azure.*</span><span class="sxs-lookup"><span data-stu-id="0c07b-216">*Azure Storage.*</span></span> <span data-ttu-id="0c07b-217">A biblioteca de cliente de armazenamento dá suporte a operações de repetição.</span><span class="sxs-lookup"><span data-stu-id="0c07b-217">The storage client library support retry operations.</span></span> <span data-ttu-id="0c07b-218">As estratégias variam em tabelas de armazenamento do Azure, BLOBs e filas.</span><span class="sxs-lookup"><span data-stu-id="0c07b-218">The strategies vary across Azure storage tables, blobs, and queues.</span></span> <span data-ttu-id="0c07b-219">Assim, as repetições alternativas alternam entre locais de serviços de armazenamento primários e secundários quando o recurso de redundância geográfica está habilitado.</span><span class="sxs-lookup"><span data-stu-id="0c07b-219">As well, alternate retries switch between primary and secondary storage services locations when the geo-redundancy feature is enabled.</span></span>

- <span data-ttu-id="0c07b-220">*Hubs de eventos do Azure.*</span><span class="sxs-lookup"><span data-stu-id="0c07b-220">*Azure Event Hubs.*</span></span> <span data-ttu-id="0c07b-221">A biblioteca de cliente do hub de eventos apresenta uma propriedade RetryPolicy, que inclui um recurso de retirada exponencial configurável.</span><span class="sxs-lookup"><span data-stu-id="0c07b-221">The Event Hub client library features a RetryPolicy property, which includes a configurable exponential backoff feature.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="0c07b-222">[Anterior](application-resiliency-patterns.md)
>[Próximo](resilient-communications.md)</span><span class="sxs-lookup"><span data-stu-id="0c07b-222">[Previous](application-resiliency-patterns.md)
[Next](resilient-communications.md)</span></span>