---
title: Definindo o aplicativo de vários contêineres com o docker-compose.yml
description: Como especificar a composição de microsserviços para um aplicativo de vários contêineres com o docker-compose.yml.
ms.date: 10/02/2018
ms.openlocfilehash: 6f526a951f50bad673a44cfd6c53664a13211a32
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68675953"
---
# <a name="defining-your-multi-container-application-with-docker-composeyml"></a><span data-ttu-id="d4782-103">Definindo o aplicativo de vários contêineres com o docker-compose.yml</span><span class="sxs-lookup"><span data-stu-id="d4782-103">Defining your multi-container application with docker-compose.yml</span></span>

<span data-ttu-id="d4782-104">Neste guia, o arquivo [docker-compose.yml](https://docs.docker.com/compose/compose-file/) foi apresentado na seção [Etapa 4. Definir os serviços no docker-compose.yml ao compilar um aplicativo de vários contêineres do Docker](../docker-application-development-process/docker-app-development-workflow.md#step-4-define-your-services-in-docker-composeyml-when-building-a-multi-container-docker-application).</span><span class="sxs-lookup"><span data-stu-id="d4782-104">In this guide, the [docker-compose.yml](https://docs.docker.com/compose/compose-file/) file was introduced in the section [Step 4. Define your services in docker-compose.yml when building a multi-container Docker application](../docker-application-development-process/docker-app-development-workflow.md#step-4-define-your-services-in-docker-composeyml-when-building-a-multi-container-docker-application).</span></span> <span data-ttu-id="d4782-105">No entanto, há outras maneiras de usar os arquivos docker-compose e vale a pena explorá-las com mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="d4782-105">However, there are additional ways to use the docker-compose files that are worth exploring in further detail.</span></span>

<span data-ttu-id="d4782-106">Por exemplo, você pode descrever explicitamente como deseja implantar o aplicativo de vários contêineres no arquivo docker-compose.yml.</span><span class="sxs-lookup"><span data-stu-id="d4782-106">For example, you can explicitly describe how you want to deploy your multi-container application in the docker-compose.yml file.</span></span> <span data-ttu-id="d4782-107">Opcionalmente, você também pode descrever como vai criar as imagens personalizadas do Docker.</span><span class="sxs-lookup"><span data-stu-id="d4782-107">Optionally, you can also describe how you are going to build your custom Docker images.</span></span> <span data-ttu-id="d4782-108">(As imagens personalizadas do Docker também podem ser criadas com a CLI do Docker).</span><span class="sxs-lookup"><span data-stu-id="d4782-108">(Custom Docker images can also be built with the Docker CLI.)</span></span>

<span data-ttu-id="d4782-109">Basicamente, você define cada um dos contêineres que deseja implantar, além de determinadas características para cada implantação de contêiner.</span><span class="sxs-lookup"><span data-stu-id="d4782-109">Basically, you define each of the containers you want to deploy plus certain characteristics for each container deployment.</span></span> <span data-ttu-id="d4782-110">Com um arquivo de descrição de implantação de vários contêineres pronto, você poderá implantar toda a solução por meio de uma única ação orquestrada pelo comando da CLI [docker-compose up](https://docs.docker.com/compose/overview/) ou poderá implantá-la de forma transparente no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d4782-110">Once you have a multi-container deployment description file, you can deploy the whole solution in a single action orchestrated by the [docker-compose up](https://docs.docker.com/compose/overview/) CLI command, or you can deploy it transparently from Visual Studio.</span></span> <span data-ttu-id="d4782-111">Caso contrário, você precisará usar a CLI do Docker para implantar contêiner por contêiner, em várias etapas, usando o comando `docker run` na linha de comando.</span><span class="sxs-lookup"><span data-stu-id="d4782-111">Otherwise, you would need to use the Docker CLI to deploy container-by-container in multiple steps by using the `docker run` command from the command line.</span></span> <span data-ttu-id="d4782-112">Portanto, cada serviço definido no docker-compose.yml deve especificar exatamente uma imagem ou build.</span><span class="sxs-lookup"><span data-stu-id="d4782-112">Therefore, each service defined in docker-compose.yml must specify exactly one image or build.</span></span> <span data-ttu-id="d4782-113">As outras chaves são opcionais e são semelhantes aos correspondentes de `docker run` na linha de comando.</span><span class="sxs-lookup"><span data-stu-id="d4782-113">Other keys are optional, and are analogous to their `docker run` command-line counterparts.</span></span>

<span data-ttu-id="d4782-114">O código YAML a seguir é a definição de um arquivo docker-compose.yml possivelmente global, mas único, do exemplo eShopOnContainers.</span><span class="sxs-lookup"><span data-stu-id="d4782-114">The following YAML code is the definition of a possible global but single docker-compose.yml file for the eShopOnContainers sample.</span></span> <span data-ttu-id="d4782-115">Esse não é o verdadeiro arquivo docker-compose do eShopOnContainers.</span><span class="sxs-lookup"><span data-stu-id="d4782-115">This is not the actual docker-compose file from eShopOnContainers.</span></span> <span data-ttu-id="d4782-116">Em vez disso, ele é uma versão simplificada e consolidada em um único arquivo, o que não é a melhor maneira de se trabalhar com arquivos docker-compose, como será explicado posteriormente.</span><span class="sxs-lookup"><span data-stu-id="d4782-116">Instead, it is a simplified and consolidated version in a single file, which is not the best way to work with docker-compose files, as will be explained later.</span></span>

```yml
version: '3.4'

services:
  webmvc:
    image: eshop/webmvc
    environment:
      - CatalogUrl=http://catalog.api
      - OrderingUrl=http://ordering.api
      - BasketUrl=http://basket.api
    ports:
      - "5100:80"
    depends_on:
      - catalog.api
      - ordering.api
      - basket.api

  catalog.api:
    image: eshop/catalog.api
    environment:
      - ConnectionString=Server=sql.data;Initial Catalog=CatalogData;User Id=sa;Password=your@password
    expose:
      - "80"
    ports:
      - "5101:80"
    #extra hosts can be used for standalone SQL Server or services at the dev PC
    extra_hosts:
      - "CESARDLSURFBOOK:10.0.75.1"
    depends_on:
      - sql.data

  ordering.api:
    image: eshop/ordering.api
    environment:
      - ConnectionString=Server=sql.data;Database=Services.OrderingDb;User Id=sa;Password=your@password
    ports:
      - "5102:80"
    #extra hosts can be used for standalone SQL Server or services at the dev PC
    extra_hosts:
      - "CESARDLSURFBOOK:10.0.75.1"
    depends_on:
      - sql.data

  basket.api:
    image: eshop/basket.api
    environment:
      - ConnectionString=sql.data
    ports:
      - "5103:80"
    depends_on:
      - sql.data

  sql.data:
    environment:
      - SA_PASSWORD=your@password
      - ACCEPT_EULA=Y
    ports:
      - "5434:1433"

  basket.data:
    image: redis
```

<span data-ttu-id="d4782-117">A chave de raiz desse arquivo é services.</span><span class="sxs-lookup"><span data-stu-id="d4782-117">The root key in this file is services.</span></span> <span data-ttu-id="d4782-118">Sob essa chave, você define os serviços que deseja implantar e executar ao executar o comando `docker-compose up` ou ao implantar por meio do Visual Studio usando esse arquivo docker-compose.yml.</span><span class="sxs-lookup"><span data-stu-id="d4782-118">Under that key you define the services you want to deploy and run when you execute the `docker-compose up` command or when you deploy from Visual Studio by using this docker-compose.yml file.</span></span> <span data-ttu-id="d4782-119">Nesse caso, o arquivo docker-compose.yml tem vários serviços definidos, conforme descrito na tabela a seguir.</span><span class="sxs-lookup"><span data-stu-id="d4782-119">In this case, the docker-compose.yml file has multiple services defined, as described in the following table.</span></span>

| <span data-ttu-id="d4782-120">Nome do serviço</span><span class="sxs-lookup"><span data-stu-id="d4782-120">Service name</span></span> | <span data-ttu-id="d4782-121">Descrição</span><span class="sxs-lookup"><span data-stu-id="d4782-121">Description</span></span> |
|--------------|-------------|
| <span data-ttu-id="d4782-122">webmvc</span><span class="sxs-lookup"><span data-stu-id="d4782-122">webmvc</span></span>       | <span data-ttu-id="d4782-123">Contêiner, incluindo o aplicativo MVC ASP.NET Core, que consome os microsserviços do C\# do lado do servidor</span><span class="sxs-lookup"><span data-stu-id="d4782-123">Container including the ASP.NET Core MVC application consuming the microservices from server-side C\#</span></span>|
| <span data-ttu-id="d4782-124">catalog.api</span><span class="sxs-lookup"><span data-stu-id="d4782-124">catalog.api</span></span>  | <span data-ttu-id="d4782-125">Contêiner, incluindo o microsserviço de API Web do ASP.NET Core de Catálogo</span><span class="sxs-lookup"><span data-stu-id="d4782-125">Container including the Catalog ASP.NET Core Web API microservice</span></span> |
| <span data-ttu-id="d4782-126">ordering.api</span><span class="sxs-lookup"><span data-stu-id="d4782-126">ordering.api</span></span> | <span data-ttu-id="d4782-127">Contêiner, incluindo o microsserviço de API Web do ASP.NET Core de Pedidos</span><span class="sxs-lookup"><span data-stu-id="d4782-127">Container including the Ordering ASP.NET Core Web API microservice</span></span> |
| <span data-ttu-id="d4782-128">sql.data</span><span class="sxs-lookup"><span data-stu-id="d4782-128">sql.data</span></span>     | <span data-ttu-id="d4782-129">Contêiner que executa o SQL Server para Linux armazenando os bancos de dados de microsserviços</span><span class="sxs-lookup"><span data-stu-id="d4782-129">Container running SQL Server for Linux, holding the microservices databases</span></span> |
| <span data-ttu-id="d4782-130">basket.api</span><span class="sxs-lookup"><span data-stu-id="d4782-130">basket.api</span></span>   | <span data-ttu-id="d4782-131">Contêiner com o microsserviço de API Web do ASP.NET Core de Cesta</span><span class="sxs-lookup"><span data-stu-id="d4782-131">Container with the Basket ASP.NET Core Web API microservice</span></span> |
| <span data-ttu-id="d4782-132">basket.data</span><span class="sxs-lookup"><span data-stu-id="d4782-132">basket.data</span></span>  | <span data-ttu-id="d4782-133">Contêiner que executa o serviço de Cache Redis, com o banco de dados da cesta como um Cache Redis</span><span class="sxs-lookup"><span data-stu-id="d4782-133">Container running the REDIS cache service, with the basket database as a REDIS cache</span></span> |

### <a name="a-simple-web-service-api-container"></a><span data-ttu-id="d4782-134">Um contêiner de API de serviço Web simples</span><span class="sxs-lookup"><span data-stu-id="d4782-134">A simple Web Service API container</span></span>

<span data-ttu-id="d4782-135">Concentrando-se em um único contêiner, o microsserviço de contêiner catalog.api tem uma definição simples:</span><span class="sxs-lookup"><span data-stu-id="d4782-135">Focusing on a single container, the catalog.api container-microservice has a straightforward definition:</span></span>

```yml
  catalog.api:
    image: eshop/catalog.api
    environment:
      - ConnectionString=Server=sql.data;Initial Catalog=CatalogData;User Id=sa;Password=your@password
    expose:
      - "80"
    ports:
      - "5101:80"
    #extra hosts can be used for standalone SQL Server or services at the dev PC
    extra_hosts:
      - "CESARDLSURFBOOK:10.0.75.1"
    depends_on:
      - sql.data
```

<span data-ttu-id="d4782-136">Esse serviço em contêiner tem a seguinte configuração básica:</span><span class="sxs-lookup"><span data-stu-id="d4782-136">This containerized service has the following basic configuration:</span></span>

- <span data-ttu-id="d4782-137">Ele se baseia na imagem eshop/catalog.api personalizada.</span><span class="sxs-lookup"><span data-stu-id="d4782-137">It is based on the custom eshop/catalog.api image.</span></span> <span data-ttu-id="d4782-138">Por questões de simplicidade, não há nenhuma definição de chave build: no arquivo.</span><span class="sxs-lookup"><span data-stu-id="d4782-138">For simplicity’s sake, there is no build: key setting in the file.</span></span> <span data-ttu-id="d4782-139">Isso significa que a imagem precisa ser previamente compilada (com docker build) ou ser baixada (com o comando docker pull) de qualquer registro do Docker.</span><span class="sxs-lookup"><span data-stu-id="d4782-139">This means that the image must have been previously built (with docker build) or have been downloaded (with the docker pull command) from any Docker registry.</span></span>

- <span data-ttu-id="d4782-140">Ele define uma variável de ambiente denominada ConnectionString, com a cadeia de conexão a ser usada pelo Entity Framework para acessar a instância do SQL Server que contém o modelo de dados de catálogo.</span><span class="sxs-lookup"><span data-stu-id="d4782-140">It defines an environment variable named ConnectionString with the connection string to be used by Entity Framework to access the SQL Server instance that contains the catalog data model.</span></span> <span data-ttu-id="d4782-141">Nesse caso, o mesmo contêiner do SQL Server está mantendo vários bancos de dados.</span><span class="sxs-lookup"><span data-stu-id="d4782-141">In this case, the same SQL Server container is holding multiple databases.</span></span> <span data-ttu-id="d4782-142">Dessa forma, você precisará de menos memória no computador de desenvolvimento do Docker.</span><span class="sxs-lookup"><span data-stu-id="d4782-142">Therefore, you need less memory in your development machine for Docker.</span></span> <span data-ttu-id="d4782-143">No entanto, você também poderia implantar um contêiner do SQL Server para cada banco de dados de microsserviço.</span><span class="sxs-lookup"><span data-stu-id="d4782-143">However, you could also deploy one SQL Server container for each microservice database.</span></span>

- <span data-ttu-id="d4782-144">O nome do SQL Server é sql.data, que é o mesmo nome usado pelo contêiner que está executando a Instância do SQL Server para Linux.</span><span class="sxs-lookup"><span data-stu-id="d4782-144">The SQL Server name is sql.data, which is the same name used for the container that is running the SQL Server instance for Linux.</span></span> <span data-ttu-id="d4782-145">Isso é conveniente: a possibilidade de usar essa resolução de nome (interna ao host do Docker) resolverá o endereço de rede, então você não precisará saber o IP interno dos contêineres que está acessando de outros contêineres.</span><span class="sxs-lookup"><span data-stu-id="d4782-145">This is convenient; being able to use this name resolution (internal to the Docker host) will resolve the network address so you don’t need to know the internal IP for the containers you are accessing from other containers.</span></span>

<span data-ttu-id="d4782-146">Como a cadeia de conexão é definida por uma variável de ambiente, você pode definir essa variável por meio de um mecanismo diferente e em outro momento.</span><span class="sxs-lookup"><span data-stu-id="d4782-146">Because the connection string is defined by an environment variable, you could set that variable through a different mechanism and at a different time.</span></span> <span data-ttu-id="d4782-147">Por exemplo, você pode definir uma cadeia de conexão diferente durante a implantação em produção nos hosts finais ou fazer isso através dos pipelines de CI/CD no Azure DevOps Services ou no sistema de DevOps de sua preferência.</span><span class="sxs-lookup"><span data-stu-id="d4782-147">For example, you could set a different connection string when deploying to production in the final hosts, or by doing it from your CI/CD pipelines in Azure DevOps Services or your preferred DevOps system.</span></span>

- <span data-ttu-id="d4782-148">Ele expõe a porta 80 para acesso interno ao serviço catalog.api dentro do host do Docker.</span><span class="sxs-lookup"><span data-stu-id="d4782-148">It exposes port 80 for internal access to the catalog.api service within the Docker host.</span></span> <span data-ttu-id="d4782-149">Atualmente o host é uma VM do Linux, porque ele se baseia em uma imagem do Docker para Linux, mas você também pode configurar o contêiner para ser executado em uma imagem do Windows.</span><span class="sxs-lookup"><span data-stu-id="d4782-149">The host is currently a Linux VM because it is based on a Docker image for Linux, but you could configure the container to run on a Windows image instead.</span></span>

- <span data-ttu-id="d4782-150">Ele encaminha a porta 80, exposta no contêiner, para a porta 5101 no computador host do Docker (a VM do Linux).</span><span class="sxs-lookup"><span data-stu-id="d4782-150">It forwards the exposed port 80 on the container to port 5101 on the Docker host machine (the Linux VM).</span></span>

- <span data-ttu-id="d4782-151">Ele vincula o serviço Web ao serviço sql.data (a instância do SQL Server para o banco de dados do Linux em execução em um contêiner).</span><span class="sxs-lookup"><span data-stu-id="d4782-151">It links the web service to the sql.data service (the SQL Server instance for Linux database running in a container).</span></span> <span data-ttu-id="d4782-152">Ao especificar essa dependência, o contêiner catalog.api não será iniciado até que o contêiner sql.data seja iniciado. Isso é importante porque o catalog.api precisa que o banco de dados do SQL Server esteja em execução.</span><span class="sxs-lookup"><span data-stu-id="d4782-152">When you specify this dependency, the catalog.api container will not start until the sql.data container has already started; this is important because catalog.api needs to have the SQL Server database up and running first.</span></span> <span data-ttu-id="d4782-153">No entanto, esse tipo de dependência de contêiner não é suficiente em muitos casos, porque o Docker verifica apenas no nível de contêiner.</span><span class="sxs-lookup"><span data-stu-id="d4782-153">However, this kind of container dependency is not enough in many cases, because Docker checks only at the container level.</span></span> <span data-ttu-id="d4782-154">Às vezes, o serviço (o SQL Server, neste caso) poderá ainda não estar pronto, portanto, é aconselhável implementar uma lógica de repetição com retirada exponencial no microsserviço cliente.</span><span class="sxs-lookup"><span data-stu-id="d4782-154">Sometimes the service (in this case SQL Server) might still not be ready, so it is advisable to implement retry logic with exponential backoff in your client microservices.</span></span> <span data-ttu-id="d4782-155">Dessa forma, se um contêiner de dependência não estiver pronto durante um breve período de tempo, o aplicativo continuará resiliente.</span><span class="sxs-lookup"><span data-stu-id="d4782-155">That way, if a dependency container is not ready for a short time, the application will still be resilient.</span></span>

- <span data-ttu-id="d4782-156">Ele é configurado para permitir o acesso a servidores externos: a configuração extra\_hosts permite que você acesse servidores externos ou computadores fora do host do Docker (ou seja, fora da VM do Linux padrão, que é um host do Docker para desenvolvimento), como uma Instância do SQL Server local em seu computador de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="d4782-156">It is configured to allow access to external servers: the extra\_hosts setting allows you to access external servers or machines outside of the Docker host (that is, outside the default Linux VM which is a development Docker host), such as a local SQL Server instance on your development PC.</span></span>

<span data-ttu-id="d4782-157">Também há outras configurações mais avançadas do docker-compose.yml que discutiremos nas próximas seções.</span><span class="sxs-lookup"><span data-stu-id="d4782-157">There are also other, more advanced docker-compose.yml settings that we will discuss in the following sections.</span></span>

### <a name="using-docker-compose-files-to-target-multiple-environments"></a><span data-ttu-id="d4782-158">Usando arquivos docker-compose para destinar-se a vários ambientes</span><span class="sxs-lookup"><span data-stu-id="d4782-158">Using docker-compose files to target multiple environments</span></span>

<span data-ttu-id="d4782-159">Os arquivos docker-compose.yml são arquivos de definição e podem ser usados por várias infraestruturas que entendem esse formato.</span><span class="sxs-lookup"><span data-stu-id="d4782-159">The docker-compose.yml files are definition files and can be used by multiple infrastructures that understand that format.</span></span> <span data-ttu-id="d4782-160">A ferramenta mais simples é o comando docker-compose.</span><span class="sxs-lookup"><span data-stu-id="d4782-160">The most straightforward tool is the docker-compose command.</span></span>

<span data-ttu-id="d4782-161">Portanto, ao usar o comando docker-compose você pode ter os seguintes principais cenários como destino.</span><span class="sxs-lookup"><span data-stu-id="d4782-161">Therefore, by using the docker-compose command you can target the following main scenarios.</span></span>

#### <a name="development-environments"></a><span data-ttu-id="d4782-162">Ambientes de desenvolvimento</span><span class="sxs-lookup"><span data-stu-id="d4782-162">Development environments</span></span>

<span data-ttu-id="d4782-163">Ao desenvolver aplicativos, é importante ser capaz de executar um aplicativo em um ambiente de desenvolvimento isolado.</span><span class="sxs-lookup"><span data-stu-id="d4782-163">When you develop applications, it is important to be able to run an application in an isolated development environment.</span></span> <span data-ttu-id="d4782-164">Você pode usar o comando de CLI docker-compose para criar esse ambiente ou usar o Visual Studio, que usa o docker-compose nos bastidores.</span><span class="sxs-lookup"><span data-stu-id="d4782-164">You can use the docker-compose CLI command to create that environment or use Visual Studio which uses docker-compose under the covers.</span></span>

<span data-ttu-id="d4782-165">O arquivo docker-compose.yml permite que você configure e documente todas as dependências de serviço do seu aplicativo (outros serviços, cache, bancos de dados, filas, etc.).</span><span class="sxs-lookup"><span data-stu-id="d4782-165">The docker-compose.yml file allows you to configure and document all your application’s service dependencies (other services, cache, databases, queues, etc.).</span></span> <span data-ttu-id="d4782-166">Usando o comando da CLI docker-compose, você pode criar e iniciar um ou mais contêineres para cada dependência com um único comando (docker-compose up).</span><span class="sxs-lookup"><span data-stu-id="d4782-166">Using the docker-compose CLI command, you can create and start one or more containers for each dependency with a single command (docker-compose up).</span></span>

<span data-ttu-id="d4782-167">Os docker-compose.yml são arquivos de configuração interpretados pelo mecanismo do Docker, mas também servem como arquivos de documentação convenientes, com informações sobre a composição de seu aplicativo para vários contêineres.</span><span class="sxs-lookup"><span data-stu-id="d4782-167">The docker-compose.yml files are configuration files interpreted by Docker engine but also serve as convenient documentation files about the composition of your multi-container application.</span></span>

#### <a name="testing-environments"></a><span data-ttu-id="d4782-168">Ambientes de teste</span><span class="sxs-lookup"><span data-stu-id="d4782-168">Testing environments</span></span>

<span data-ttu-id="d4782-169">Uma parte importante de qualquer processo de CD (implantação contínua) ou CI (integração contínua) são os testes de unidade e testes de integração.</span><span class="sxs-lookup"><span data-stu-id="d4782-169">An important part of any continuous deployment (CD) or continuous integration (CI) process are the unit tests and integration tests.</span></span> <span data-ttu-id="d4782-170">Esses testes automatizados exigem um ambiente isolado para que não sejam afetados por usuários ou qualquer outra alteração nos dados do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d4782-170">These automated tests require an isolated environment so they are not impacted by the users or any other change in the application’s data.</span></span>

<span data-ttu-id="d4782-171">Com o Docker Compose, você pode criar e destruir esse ambiente isolado facilmente por meio de alguns comandos no prompt de comando ou por meio de scripts, como os comandos a seguir:</span><span class="sxs-lookup"><span data-stu-id="d4782-171">With Docker Compose you can create and destroy that isolated environment very easily in a few commands from your command prompt or scripts, like the following commands:</span></span>

```console
docker-compose -f docker-compose.yml -f docker-compose-test.override.yml up -d
./run_unit_tests
docker-compose -f docker-compose.yml -f docker-compose.test.override.yml down
```

#### <a name="production-deployments"></a><span data-ttu-id="d4782-172">Implantações de produção</span><span class="sxs-lookup"><span data-stu-id="d4782-172">Production deployments</span></span>

<span data-ttu-id="d4782-173">Você também pode usar o Compose para implantar em um mecanismo remoto do Docker.</span><span class="sxs-lookup"><span data-stu-id="d4782-173">You can also use Compose to deploy to a remote Docker Engine.</span></span> <span data-ttu-id="d4782-174">Um caso comum é a implantação em uma única instância de host do Docker (como uma VM ou servidor de produção, provisionados com o [Docker Machine](https://docs.docker.com/machine/overview/)).</span><span class="sxs-lookup"><span data-stu-id="d4782-174">A typical case is to deploy to a single Docker host instance (like a production VM or server provisioned with [Docker Machine](https://docs.docker.com/machine/overview/)).</span></span>

<span data-ttu-id="d4782-175">Se estiver usando qualquer outro orquestrador (Azure Service Fabric, Kubernetes, etc.), precisará adicionar definições de configuração de instalação e metadados, como aquelas em docker-compose.yml, mas no formato exigido pelo outro orquestrador.</span><span class="sxs-lookup"><span data-stu-id="d4782-175">If you are using any other orchestrator (Azure Service Fabric, Kubernetes, etc.), you might need to add setup and metadata configuration settings like those in docker-compose.yml, but in the format required by the other orchestrator.</span></span>

<span data-ttu-id="d4782-176">De qualquer maneira, o docker-compose é uma ferramenta conveniente e um formato de metadados para fluxos de trabalho de desenvolvimento, teste e produção, embora o fluxo de trabalho de produção possa variar no orquestrador que você está usando.</span><span class="sxs-lookup"><span data-stu-id="d4782-176">In any case, docker-compose is a convenient tool and metadata format for development, testing and production workflows, although the production workflow might vary on the orchestrator you are using.</span></span>

### <a name="using-multiple-docker-compose-files-to-handle-several-environments"></a><span data-ttu-id="d4782-177">Usando vários arquivos docker-compose para lidar com diversos ambientes</span><span class="sxs-lookup"><span data-stu-id="d4782-177">Using multiple docker-compose files to handle several environments</span></span>

<span data-ttu-id="d4782-178">Ao destinar-se a ambientes diferentes, você deve usar vários arquivos compose.</span><span class="sxs-lookup"><span data-stu-id="d4782-178">When targeting different environments, you should use multiple compose files.</span></span> <span data-ttu-id="d4782-179">Isso permite que você crie diversas variantes de configuração, dependendo do ambiente.</span><span class="sxs-lookup"><span data-stu-id="d4782-179">This lets you create multiple configuration variants depending on the environment.</span></span>

#### <a name="overriding-the-base-docker-compose-file"></a><span data-ttu-id="d4782-180">Substituindo o arquivo base docker-compose</span><span class="sxs-lookup"><span data-stu-id="d4782-180">Overriding the base docker-compose file</span></span>

<span data-ttu-id="d4782-181">Você pode usar um único arquivo docker-compose.yml, como nos exemplos simplificados mostrados nas seções anteriores.</span><span class="sxs-lookup"><span data-stu-id="d4782-181">You could use a single docker-compose.yml file as in the simplified examples shown in previous sections.</span></span> <span data-ttu-id="d4782-182">No entanto, isso não é recomendável para a maioria dos aplicativos.</span><span class="sxs-lookup"><span data-stu-id="d4782-182">However, that is not recommended for most applications.</span></span>

<span data-ttu-id="d4782-183">Por padrão, o Compose lê dois arquivos, um docker-compose.yml e um arquivo docker-compose.override.yml opcional.</span><span class="sxs-lookup"><span data-stu-id="d4782-183">By default, Compose reads two files, a docker-compose.yml and an optional docker-compose.override.yml file.</span></span> <span data-ttu-id="d4782-184">Conforme mostrado na Figura 6-11, quando você estiver usando o Visual Studio e habilitar o suporte ao Docker, o Visual Studio também criará um arquivo docker-compose.vs.debug.g.yml adicional para depurar o aplicativo. Dê uma olhada nesse arquivo na pasta obj\\Docker\\ da pasta da solução principal.</span><span class="sxs-lookup"><span data-stu-id="d4782-184">As shown in Figure 6-11, when you are using Visual Studio and enabling Docker support, Visual Studio also creates an additional docker-compose.vs.debug.g.yml file for debugging the application, you can take a look at this file in folder obj\\Docker\\ in the main solution folder.</span></span>

![Estrutura do arquivo de projeto de docker-compose: .dockerignore, para ignorar arquivos; docker-compose.yml, para compor microsserviços; docker-compose.override.yml, para configurar o ambiente de microsserviços.](./media/image12.png)

<span data-ttu-id="d4782-186">**Figura 6-11**.</span><span class="sxs-lookup"><span data-stu-id="d4782-186">**Figure 6-11**.</span></span> <span data-ttu-id="d4782-187">Arquivos docker-compose no Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="d4782-187">docker-compose files in Visual Studio 2017</span></span>

<span data-ttu-id="d4782-188">Edite os arquivos docker-compose com qualquer editor, como o Visual Studio Code ou o Sublime, e execute o aplicativo com o comando docker-compose up.</span><span class="sxs-lookup"><span data-stu-id="d4782-188">You can edit the docker-compose files with any editor, like Visual Studio Code or Sublime, and run the application with the docker-compose up command.</span></span>

<span data-ttu-id="d4782-189">Por convenção, o arquivo docker-compose.yml contém sua configuração base e outras configurações estáticas.</span><span class="sxs-lookup"><span data-stu-id="d4782-189">By convention, the docker-compose.yml file contains your base configuration and other static settings.</span></span> <span data-ttu-id="d4782-190">Isso significa que a configuração do serviço não deve ser alterada de acordo com o ambiente de implantação que você tem como destino.</span><span class="sxs-lookup"><span data-stu-id="d4782-190">That means that the service configuration should not change depending on the deployment environment you are targeting.</span></span>

<span data-ttu-id="d4782-191">O arquivo docker-compose.override.yml, como o próprio nome sugere, contém definições de configuração que substituem a configuração base, como a configuração que depende do ambiente de implantação.</span><span class="sxs-lookup"><span data-stu-id="d4782-191">The docker-compose.override.yml file, as its name suggests, contains configuration settings that override the base configuration, such as configuration that depends on the deployment environment.</span></span> <span data-ttu-id="d4782-192">Você também pode ter vários arquivos de substituição com nomes diferentes.</span><span class="sxs-lookup"><span data-stu-id="d4782-192">You can have multiple override files with different names also.</span></span> <span data-ttu-id="d4782-193">Os arquivos de substituição geralmente contêm informações adicionais, exigidas pelo aplicativo, bem como as informações específicas de um ambiente ou uma implantação.</span><span class="sxs-lookup"><span data-stu-id="d4782-193">The override files usually contain additional information needed by the application but specific to an environment or to a deployment.</span></span>

#### <a name="targeting-multiple-environments"></a><span data-ttu-id="d4782-194">Destinando-se a vários ambientes</span><span class="sxs-lookup"><span data-stu-id="d4782-194">Targeting multiple environments</span></span>

<span data-ttu-id="d4782-195">Um caso de uso típico é aquele em que você define vários arquivos compose para destinar-se a vários ambientes, como de produção, de preparo, de CI ou de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="d4782-195">A typical use case is when you define multiple compose files so you can target multiple environments, like production, staging, CI, or development.</span></span> <span data-ttu-id="d4782-196">Para dar suporte a essas diferenças, você pode dividir a configuração do Compose em vários arquivos, conforme mostrado na Figura 6-12.</span><span class="sxs-lookup"><span data-stu-id="d4782-196">To support these differences, you can split your Compose configuration into multiple files, as shown in Figure 6-12.</span></span>

![Você pode combinar vários arquivos docker-compose\*.fml para lidar com diferentes ambientes.](./media/image13.png)

<span data-ttu-id="d4782-198">**Figura 6-12**.</span><span class="sxs-lookup"><span data-stu-id="d4782-198">**Figure 6-12**.</span></span> <span data-ttu-id="d4782-199">Vários arquivos docker-compose substituindo valores no arquivo base docker-compose.yml</span><span class="sxs-lookup"><span data-stu-id="d4782-199">Multiple docker-compose files overriding values in the base docker-compose.yml file</span></span>

<span data-ttu-id="d4782-200">Você inicia com o arquivo base docker-compose.yml.</span><span class="sxs-lookup"><span data-stu-id="d4782-200">You start with the base docker-compose.yml file.</span></span> <span data-ttu-id="d4782-201">Esse arquivo base deve conter as definições de configuração base ou estáticas que não se alteram de acordo com o ambiente.</span><span class="sxs-lookup"><span data-stu-id="d4782-201">This base file has to contain the base or static configuration settings that do not change depending on the environment.</span></span> <span data-ttu-id="d4782-202">Por exemplo, o eShopOnContainers tem o arquivo docker-compose.yml a seguir (simplificado com menos serviços) como o arquivo base.</span><span class="sxs-lookup"><span data-stu-id="d4782-202">For example, the eShopOnContainers has the following docker-compose.yml file (simplified with less services) as the base file.</span></span>

```yml
#docker-compose.yml (Base)
version: '3.4'
services:
  basket.api:
    image: eshop/basket.api:${TAG:-latest}
    build:
      context: .
      dockerfile: src/Services/Basket/Basket.API/Dockerfile
    depends_on:
      - basket.data
      - identity.api
      - rabbitmq

  catalog.api:
    image: eshop/catalog.api:${TAG:-latest}
    build:
      context: .
      dockerfile: src/Services/Catalog/Catalog.API/Dockerfile
    depends_on:
      - sql.data
      - rabbitmq

  marketing.api:
    image: eshop/marketing.api:${TAG:-latest}
    build:
      context: .
      dockerfile: src/Services/Marketing/Marketing.API/Dockerfile
    depends_on:
      - sql.data
      - nosql.data
      - identity.api
      - rabbitmq

  webmvc:
    image: eshop/webmvc:${TAG:-latest}
    build:
      context: .
      dockerfile: src/Web/WebMVC/Dockerfile
    depends_on:
      - catalog.api
      - ordering.api
      - identity.api
      - basket.api
      - marketing.api

  sql.data:
    image: microsoft/mssql-server-linux:2017-latest

  nosql.data:
    image: mongo

  basket.data:
    image: redis

  rabbitmq:
    image: rabbitmq:3-management

```

<span data-ttu-id="d4782-203">Os valores do arquivo base docker-compose.yml não devem se alterar devido aos diferentes ambientes de implantação de destino.</span><span class="sxs-lookup"><span data-stu-id="d4782-203">The values in the base docker-compose.yml file should not change because of different target deployment environments.</span></span>

<span data-ttu-id="d4782-204">Se você se concentrar na definição do serviço webmvc, por exemplo, verá como essa informação é muito semelhante independentemente do ambiente ao qual você se destina.</span><span class="sxs-lookup"><span data-stu-id="d4782-204">If you focus on the webmvc service definition, for instance, you can see how that information is much the same no matter what environment you might be targeting.</span></span> <span data-ttu-id="d4782-205">Você tem as seguintes informações:</span><span class="sxs-lookup"><span data-stu-id="d4782-205">You have the following information:</span></span>

- <span data-ttu-id="d4782-206">O nome do serviço: webmvc.</span><span class="sxs-lookup"><span data-stu-id="d4782-206">The service name: webmvc.</span></span>

- <span data-ttu-id="d4782-207">A imagem personalizada do contêiner: eshop/webmvc.</span><span class="sxs-lookup"><span data-stu-id="d4782-207">The container’s custom image: eshop/webmvc.</span></span>

- <span data-ttu-id="d4782-208">O comando para compilar a imagem personalizada do Docker, que indica qual Dockerfile usar.</span><span class="sxs-lookup"><span data-stu-id="d4782-208">The command to build the custom Docker image, indicating which Dockerfile to use.</span></span>

- <span data-ttu-id="d4782-209">As dependências de outros serviços, para que este contêiner não se inicie até que os outros contêineres de dependência sejam iniciados.</span><span class="sxs-lookup"><span data-stu-id="d4782-209">Dependencies on other services, so this container does not start until the other dependency containers have started.</span></span>

<span data-ttu-id="d4782-210">Você pode ter outras configurações, mas o ponto importante é que, no arquivo base docker-compose.yml, é necessário definir apenas as informações que são comuns entre ambientes.</span><span class="sxs-lookup"><span data-stu-id="d4782-210">You can have additional configuration, but the important point is that in the base docker-compose.yml file, you just want to set the information that is common across environments.</span></span> <span data-ttu-id="d4782-211">Então, no docker-compose.override.yml ou em arquivos semelhantes de produção ou de preparo, você deve colocar a configuração específica para cada ambiente.</span><span class="sxs-lookup"><span data-stu-id="d4782-211">Then in the docker-compose.override.yml or similar files for production or staging, you should place configuration that is specific for each environment.</span></span>

<span data-ttu-id="d4782-212">Normalmente, o docker-compose.override.yml é usado para o ambiente de desenvolvimento, como no exemplo a seguir, do eShopOnContainers:</span><span class="sxs-lookup"><span data-stu-id="d4782-212">Usually, the docker-compose.override.yml is used for your development environment, as in the following example from eShopOnContainers:</span></span>

```yml
#docker-compose.override.yml (Extended config for DEVELOPMENT env.)
version: '3.4'

services:
# Simplified number of services here:

  basket.api:
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=http://0.0.0.0:80
      - ConnectionString=${ESHOP_AZURE_REDIS_BASKET_DB:-basket.data}
      - identityUrl=http://identity.api
      - IdentityUrlExternal=http://${ESHOP_EXTERNAL_DNS_NAME_OR_IP}:5105
      - EventBusConnection=${ESHOP_AZURE_SERVICE_BUS:-rabbitmq}
      - EventBusUserName=${ESHOP_SERVICE_BUS_USERNAME}
      - EventBusPassword=${ESHOP_SERVICE_BUS_PASSWORD}
      - AzureServiceBusEnabled=False
      - ApplicationInsights__InstrumentationKey=${INSTRUMENTATION_KEY}
      - OrchestratorType=${ORCHESTRATOR_TYPE}
      - UseLoadTest=${USE_LOADTEST:-False}

    ports:
      - "5103:80"

  catalog.api:
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=http://0.0.0.0:80
      - ConnectionString=${ESHOP_AZURE_CATALOG_DB:-Server=sql.data;Database=Microsoft.eShopOnContainers.Services.CatalogDb;User Id=sa;Password=Pass@word}
      - PicBaseUrl=${ESHOP_AZURE_STORAGE_CATALOG_URL:-http://localhost:5202/api/v1/catalog/items/[0]/pic/}
      - EventBusConnection=${ESHOP_AZURE_SERVICE_BUS:-rabbitmq}
      - EventBusUserName=${ESHOP_SERVICE_BUS_USERNAME}
      - EventBusPassword=${ESHOP_SERVICE_BUS_PASSWORD}
      - AzureStorageAccountName=${ESHOP_AZURE_STORAGE_CATALOG_NAME}
      - AzureStorageAccountKey=${ESHOP_AZURE_STORAGE_CATALOG_KEY}
      - UseCustomizationData=True
      - AzureServiceBusEnabled=False
      - AzureStorageEnabled=False
      - ApplicationInsights__InstrumentationKey=${INSTRUMENTATION_KEY}
      - OrchestratorType=${ORCHESTRATOR_TYPE}
    ports:
      - "5101:80"

  marketing.api:
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=http://0.0.0.0:80
      - ConnectionString=${ESHOP_AZURE_MARKETING_DB:-Server=sql.data;Database=Microsoft.eShopOnContainers.Services.MarketingDb;User Id=sa;Password=Pass@word}
      - MongoConnectionString=${ESHOP_AZURE_COSMOSDB:-mongodb://nosql.data}
      - MongoDatabase=MarketingDb
      - EventBusConnection=${ESHOP_AZURE_SERVICE_BUS:-rabbitmq}
      - EventBusUserName=${ESHOP_SERVICE_BUS_USERNAME}
      - EventBusPassword=${ESHOP_SERVICE_BUS_PASSWORD}
      - identityUrl=http://identity.api
      - IdentityUrlExternal=http://${ESHOP_EXTERNAL_DNS_NAME_OR_IP}:5105
      - CampaignDetailFunctionUri=${ESHOP_AZUREFUNC_CAMPAIGN_DETAILS_URI}
      - PicBaseUrl=${ESHOP_AZURE_STORAGE_MARKETING_URL:-http://localhost:5110/api/v1/campaigns/[0]/pic/}
      - AzureStorageAccountName=${ESHOP_AZURE_STORAGE_MARKETING_NAME}
      - AzureStorageAccountKey=${ESHOP_AZURE_STORAGE_MARKETING_KEY}
      - AzureServiceBusEnabled=False
      - AzureStorageEnabled=False
      - ApplicationInsights__InstrumentationKey=${INSTRUMENTATION_KEY}
      - OrchestratorType=${ORCHESTRATOR_TYPE}
      - UseLoadTest=${USE_LOADTEST:-False}
    ports:
      - "5110:80"

  webmvc:
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=http://0.0.0.0:80
      - PurchaseUrl=http://webshoppingapigw
      - IdentityUrl=http://10.0.75.1:5105
      - MarketingUrl=http://webmarketingapigw
      - CatalogUrlHC=http://catalog.api/hc
      - OrderingUrlHC=http://ordering.api/hc
      - IdentityUrlHC=http://identity.api/hc
      - BasketUrlHC=http://basket.api/hc
      - MarketingUrlHC=http://marketing.api/hc
      - PaymentUrlHC=http://payment.api/hc
      - SignalrHubUrl=http://${ESHOP_EXTERNAL_DNS_NAME_OR_IP}:5202
      - UseCustomizationData=True
      - ApplicationInsights__InstrumentationKey=${INSTRUMENTATION_KEY}
      - OrchestratorType=${ORCHESTRATOR_TYPE}
      - UseLoadTest=${USE_LOADTEST:-False}
    ports:
      - "5100:80"
  sql.data:
    environment:
      - SA_PASSWORD=Pass@word
      - ACCEPT_EULA=Y
    ports:
      - "5433:1433"
  nosql.data:
    ports:
      - "27017:27017"
  basket.data:
    ports:
      - "6379:6379"
  rabbitmq:
    ports:
      - "15672:15672"
      - "5672:5672"

```

<span data-ttu-id="d4782-213">Neste exemplo, a configuração de substituição de desenvolvimento expõe algumas portas para o host, define variáveis de ambiente com URLs de redirecionamento e especifica cadeias de conexão para o ambiente de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="d4782-213">In this example, the development override configuration exposes some ports to the host, defines environment variables with redirect URLs, and specifies connection strings for the development environment.</span></span> <span data-ttu-id="d4782-214">Todas essas configurações são apenas para o ambiente de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="d4782-214">These settings are all just for the development environment.</span></span>

<span data-ttu-id="d4782-215">Quando você executa `docker-compose up` (ou o inicia no Visual Studio), o comando lerá as substituições automaticamente como se estivesse mesclando os dois arquivos.</span><span class="sxs-lookup"><span data-stu-id="d4782-215">When you run `docker-compose up` (or launch it from Visual Studio), the command reads the overrides automatically as if it were merging both files.</span></span>

<span data-ttu-id="d4782-216">Suponha que você deseja outro arquivo do Compose para o ambiente de produção, com valores de configuração, de portas ou de cadeias de conexão diferentes.</span><span class="sxs-lookup"><span data-stu-id="d4782-216">Suppose that you want another Compose file for the production environment, with different configuration values, ports or connection strings.</span></span> <span data-ttu-id="d4782-217">Você pode criar outro arquivo de substituição, como um arquivo chamado `docker-compose.prod.yml`, com diferentes configurações e variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="d4782-217">You can create another override file, like file named `docker-compose.prod.yml` with different settings and environment variables.</span></span> <span data-ttu-id="d4782-218">Esse arquivo poderá ser armazenado em outro repositório GIT ou gerenciado e protegido por uma equipe diferente.</span><span class="sxs-lookup"><span data-stu-id="d4782-218">That file might be stored in a different Git repo or managed and secured by a different team.</span></span>

#### <a name="how-to-deploy-with-a-specific-override-file"></a><span data-ttu-id="d4782-219">Como implantar com um arquivo de substituição específico</span><span class="sxs-lookup"><span data-stu-id="d4782-219">How to deploy with a specific override file</span></span>

<span data-ttu-id="d4782-220">Para usar vários arquivos de substituição, ou um arquivo de substituição com um nome diferente, use a opção -f com o comando docker-compose e especifique os arquivos.</span><span class="sxs-lookup"><span data-stu-id="d4782-220">To use multiple override files, or an override file with a different name, you can use the -f option with the docker-compose command and specify the files.</span></span> <span data-ttu-id="d4782-221">O Compose mescla os arquivos na ordem em que eles são especificados na linha de comando.</span><span class="sxs-lookup"><span data-stu-id="d4782-221">Compose merges files in the order they are specified on the command line.</span></span> <span data-ttu-id="d4782-222">O exemplo a seguir mostra como implantar com arquivos de substituição.</span><span class="sxs-lookup"><span data-stu-id="d4782-222">The following example shows how to deploy with override files.</span></span>

```console
docker-compose -f docker-compose.yml -f docker-compose.prod.yml up -d
```

#### <a name="using-environment-variables-in-docker-compose-files"></a><span data-ttu-id="d4782-223">Usando variáveis de ambiente em arquivos docker-compose</span><span class="sxs-lookup"><span data-stu-id="d4782-223">Using environment variables in docker-compose files</span></span>

<span data-ttu-id="d4782-224">É conveniente, especialmente em ambientes de produção, ter a possibilidade de obter informações de configuração de variáveis de ambiente, como mostramos em exemplos anteriores.</span><span class="sxs-lookup"><span data-stu-id="d4782-224">It is convenient, especially in production environments, to be able to get configuration information from environment variables, as we have shown in previous examples.</span></span> <span data-ttu-id="d4782-225">Referencie uma variável de ambiente nos arquivos docker-compose usando a sintaxe ${MY\_VAR}.</span><span class="sxs-lookup"><span data-stu-id="d4782-225">You can reference an environment variable in your docker-compose files using the syntax ${MY\_VAR}.</span></span> <span data-ttu-id="d4782-226">A seguinte linha de um arquivo docker-compose.prod.yml mostra como referenciar o valor de uma variável de ambiente.</span><span class="sxs-lookup"><span data-stu-id="d4782-226">The following line from a docker-compose.prod.yml file shows how to reference the value of an environment variable.</span></span>

```yml
IdentityUrl=http://${ESHOP_PROD_EXTERNAL_DNS_NAME_OR_IP}:5105
```

<span data-ttu-id="d4782-227">As variáveis de ambiente são criadas e inicializadas de diferentes maneiras, dependendo do ambiente de host (Linux, Windows, cluster de nuvem, etc.).</span><span class="sxs-lookup"><span data-stu-id="d4782-227">Environment variables are created and initialized in different ways, depending on your host environment (Linux, Windows, Cloud cluster, etc.).</span></span> <span data-ttu-id="d4782-228">Entretanto, uma abordagem conveniente é usar um arquivo .env.</span><span class="sxs-lookup"><span data-stu-id="d4782-228">However, a convenient approach is to use an .env file.</span></span> <span data-ttu-id="d4782-229">Os arquivos docker-compose são compatíveis com a declaração de variáveis de ambiente padrão no arquivo .env.</span><span class="sxs-lookup"><span data-stu-id="d4782-229">The docker-compose files support declaring default environment variables in the .env file.</span></span> <span data-ttu-id="d4782-230">Esses valores das variáveis de ambiente são os valores padrão.</span><span class="sxs-lookup"><span data-stu-id="d4782-230">These values for the environment variables are the default values.</span></span> <span data-ttu-id="d4782-231">Mas elas podem ser substituídas pelos valores que você definiu em cada um dos ambientes (sistema operacional de host ou variáveis de ambiente do cluster).</span><span class="sxs-lookup"><span data-stu-id="d4782-231">But they can be overridden by the values you might have defined in each of your environments (host OS or environment variables from your cluster).</span></span> <span data-ttu-id="d4782-232">Coloque esse arquivo .env na pasta em que o comando docker-compose é executado.</span><span class="sxs-lookup"><span data-stu-id="d4782-232">You place this .env file in the folder where the docker-compose command is executed from.</span></span>

<span data-ttu-id="d4782-233">O exemplo a seguir mostra um arquivo .env parecido com o [.env](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/.env) do aplicativo eShopOnContainers.</span><span class="sxs-lookup"><span data-stu-id="d4782-233">The following example shows an .env file like the [.env](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/.env) file for the eShopOnContainers application.</span></span>

```
# .env file

ESHOP_EXTERNAL_DNS_NAME_OR_IP=localhost

ESHOP_PROD_EXTERNAL_DNS_NAME_OR_IP=10.121.122.92
```

<span data-ttu-id="d4782-234">O docker-compose espera que cada linha de um arquivo .env esteja no formato \<variável\>=\<valor\>.</span><span class="sxs-lookup"><span data-stu-id="d4782-234">Docker-compose expects each line in an .env file to be in the format \<variable\>=\<value\>.</span></span>

<span data-ttu-id="d4782-235">Observe que os valores definidos no ambiente do tempo de execução sempre substituem os valores definidos no arquivo .env.</span><span class="sxs-lookup"><span data-stu-id="d4782-235">Note that the values set in the runtime environment always override the values defined inside the .env file.</span></span> <span data-ttu-id="d4782-236">De maneira semelhante, os valores passados por meio de argumentos de linha de comando também substituem os valores padrão definidos no arquivo .env.</span><span class="sxs-lookup"><span data-stu-id="d4782-236">In a similar way, values passed via command-line command arguments also override the default values set in the .env file.</span></span>

#### <a name="additional-resources"></a><span data-ttu-id="d4782-237">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="d4782-237">Additional resources</span></span>

- <span data-ttu-id="d4782-238">**Visão geral do Docker Compose** </span><span class="sxs-lookup"><span data-stu-id="d4782-238">**Overview of Docker Compose** </span></span>\
    <https://docs.docker.com/compose/overview/>

- <span data-ttu-id="d4782-239">**Vários arquivos Compose** </span><span class="sxs-lookup"><span data-stu-id="d4782-239">**Multiple Compose files** </span></span>\
    [https://docs.docker.com/compose/extends/\#multiple-compose-files](https://docs.docker.com/compose/extends/#multiple-compose-files)

### <a name="building-optimized-aspnet-core-docker-images"></a><span data-ttu-id="d4782-240">Criando imagens do Docker do ASP.NET Core otimizadas</span><span class="sxs-lookup"><span data-stu-id="d4782-240">Building optimized ASP.NET Core Docker images</span></span>

<span data-ttu-id="d4782-241">Se estiver explorando o Docker e o .NET Core em fontes na Internet, você encontrará Dockerfiles que demonstram a simplicidade da criação de uma imagem do Docker por meio da cópia da fonte em um contêiner.</span><span class="sxs-lookup"><span data-stu-id="d4782-241">If you are exploring Docker and .NET Core on sources on the Internet, you will find Dockerfiles that demonstrate the simplicity of building a Docker image by copying your source into a container.</span></span> <span data-ttu-id="d4782-242">Esses exemplos sugerem que, ao usar uma configuração simples, você pode ter uma imagem do Docker com o ambiente empacotado com seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d4782-242">These examples suggest that by using a simple configuration, you can have a Docker image with the environment packaged with your application.</span></span> <span data-ttu-id="d4782-243">O exemplo a seguir mostra um Dockerfile simples neste sentido.</span><span class="sxs-lookup"><span data-stu-id="d4782-243">The following example shows a simple Dockerfile in this vein.</span></span>

```Dockerfile
FROM mcr.microsoft.com/dotnet/core/sdk:2.2
WORKDIR /app
ENV ASPNETCORE_URLS http://+:80
EXPOSE 80
COPY . .
RUN dotnet restore
ENTRYPOINT ["dotnet", "run"]
```

<span data-ttu-id="d4782-244">Um Dockerfile como esse funcionará.</span><span class="sxs-lookup"><span data-stu-id="d4782-244">A Dockerfile like this will work.</span></span> <span data-ttu-id="d4782-245">No entanto, você pode otimizar substancialmente suas imagens, especialmente as imagens de produção.</span><span class="sxs-lookup"><span data-stu-id="d4782-245">However, you can substantially optimize your images, especially your production images.</span></span>

<span data-ttu-id="d4782-246">No modelo de contêiner e microsserviços, você está constantemente iniciando contêineres.</span><span class="sxs-lookup"><span data-stu-id="d4782-246">In the container and microservices model, you are constantly starting containers.</span></span> <span data-ttu-id="d4782-247">O modo comum de uso de contêineres não reinicia um contêiner em suspensão porque o contêiner é descartável.</span><span class="sxs-lookup"><span data-stu-id="d4782-247">The typical way of using containers does not restart a sleeping container, because the container is disposable.</span></span> <span data-ttu-id="d4782-248">Orquestradores (como o Kubernetes e o Azure Service Fabric) simplesmente criam instâncias de imagens.</span><span class="sxs-lookup"><span data-stu-id="d4782-248">Orchestrators (like Kubernetes and Azure Service Fabric) simply create new instances of images.</span></span> <span data-ttu-id="d4782-249">Isso significa que você precisará otimizar por meio da pré-compilação do aplicativo quando ele for criado, fazendo com que o processo de instanciação seja mais rápido.</span><span class="sxs-lookup"><span data-stu-id="d4782-249">What this means is that you would need to optimize by precompiling the application when it is built so the instantiation process will be faster.</span></span> <span data-ttu-id="d4782-250">Quando o contêiner é iniciado, ele deve estar pronto para executar.</span><span class="sxs-lookup"><span data-stu-id="d4782-250">When the container is started, it should be ready to run.</span></span> <span data-ttu-id="d4782-251">Você não deve restaurar nem compilar em tempo de execução usando os comandos `dotnet restore` e `dotnet build` na CLI do dotnet, como visto em várias postagens no blog sobre o .NET Core e o Docker.</span><span class="sxs-lookup"><span data-stu-id="d4782-251">You should not restore and compile at run time, using `dotnet restore` and `dotnet build` commands from the dotnet CLI that, as you see in many blog posts about .NET Core and Docker.</span></span>

<span data-ttu-id="d4782-252">A equipe do .NET está realizando um trabalho importante para tornar o .NET Core e o ASP.NET Core uma estrutura otimizada para contêineres.</span><span class="sxs-lookup"><span data-stu-id="d4782-252">The .NET team has been doing important work to make .NET Core and ASP.NET Core a container-optimized framework.</span></span> <span data-ttu-id="d4782-253">O .NET Core não é apenas uma estrutura leve com um volume de memória pequeno; a equipe se concentrou em imagens do Docker otimizadas para três cenários principais e as publicou no Registro no Hub do Docker em *dotnet/core*, começando na versão 2.1:</span><span class="sxs-lookup"><span data-stu-id="d4782-253">Not only is .NET Core a lightweight framework with a small memory footprint; the team has focused on optimized Docker images for three main scenarios and published them in the Docker Hub registry at *dotnet/core*, beginning with version 2.1:</span></span>

1. <span data-ttu-id="d4782-254">**Desenvolvimento**: quando a prioridade é a capacidade de iterar e depurar alterações rapidamente e o tamanho é secundário.</span><span class="sxs-lookup"><span data-stu-id="d4782-254">**Development**: Where the priority is the ability to quickly iterate and debug changes, and where size is secondary.</span></span>

2. <span data-ttu-id="d4782-255">**Build**: a prioridade é compilar o aplicativo e inclui os binários e outras dependências para otimizar os binários.</span><span class="sxs-lookup"><span data-stu-id="d4782-255">**Build**: The priority is compiling the application and includes binaries and other dependencies to optimize binaries.</span></span>

3. <span data-ttu-id="d4782-256">**Produção**: quando o foco é implantar e iniciar contêineres rapidamente. Assim, essas imagens são limitadas aos binários e ao conteúdo necessário para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d4782-256">**Production**: Where the focus is fast deploying and starting of containers, so these images are limited to the binaries and the content needed to run the application.</span></span>

<span data-ttu-id="d4782-257">Para conseguir isso, a equipe do .NET fornece quatro variantes básicas em [dotnet/core](https://hub.docker.com/_/microsoft-dotnet-core/) (no Hub do Docker):</span><span class="sxs-lookup"><span data-stu-id="d4782-257">To achieve this, the .NET team is providing four basic variants in [dotnet/core](https://hub.docker.com/_/microsoft-dotnet-core/) (at Docker Hub):</span></span>

1. <span data-ttu-id="d4782-258">**sdk**: para cenários de desenvolvimento e build</span><span class="sxs-lookup"><span data-stu-id="d4782-258">**sdk**: for development and build scenarios</span></span>
1. <span data-ttu-id="d4782-259">**aspnet**: para cenários de produção do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d4782-259">**aspnet**: for ASP.NET production scenarios</span></span>
1. <span data-ttu-id="d4782-260">**runtime**: para cenários de produção do .NET</span><span class="sxs-lookup"><span data-stu-id="d4782-260">**runtime**: for .NET production scenarios</span></span>
1. <span data-ttu-id="d4782-261">**runtime-deps**: para cenários de produção de [aplicativos autossuficientes](../../../core/deploying/index.md#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="d4782-261">**runtime-deps**: for production scenarios of [self-contained applications](../../../core/deploying/index.md#self-contained-deployments-scd).</span></span>

<span data-ttu-id="d4782-262">Para uma inicialização mais rápida, as imagens de tempo de execução também definem automaticamente aspnetcore\_urls para a porta 80 e usam o Ngen para criar um cache de imagens nativas de assemblies.</span><span class="sxs-lookup"><span data-stu-id="d4782-262">For faster startup, runtime images also automatically set aspnetcore\_urls to port 80 and use Ngen to create a native image cache of assemblies.</span></span>

#### <a name="additional-resources"></a><span data-ttu-id="d4782-263">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="d4782-263">Additional resources</span></span>

- <span data-ttu-id="d4782-264">**Criando imagens otimizadas do Docker com o ASP.NET Core** </span><span class="sxs-lookup"><span data-stu-id="d4782-264">**Building Optimized Docker Images with ASP.NET Core** </span></span>\
    <https://blogs.msdn.microsoft.com/stevelasker/2016/09/29/building-optimized-docker-images-with-asp-net-core/>

- <span data-ttu-id="d4782-265">**Criando imagens do Docker para aplicativos do .NET Core** </span><span class="sxs-lookup"><span data-stu-id="d4782-265">**Building Docker Images for .NET Core Applications** </span></span>\
    [https://docs.microsoft.com/dotnet/core/docker/building-net-docker-images](../../../core/docker/building-net-docker-images.md)

> [!div class="step-by-step"]
> <span data-ttu-id="d4782-266">[Anterior](data-driven-crud-microservice.md)
> [Próximo](database-server-container.md)</span><span class="sxs-lookup"><span data-stu-id="d4782-266">[Previous](data-driven-crud-microservice.md)
[Next](database-server-container.md)</span></span>