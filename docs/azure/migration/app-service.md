---
title: Migrar seu aplicativo Web .NET ou serviço para o Serviço de Aplicativo do Azure
description: Saiba mais sobre como migrar um aplicativo Web ou serviço do .NET do local para o serviço Azure App.
ms.topic: conceptual
ms.date: 08/11/2018
ms.openlocfilehash: 8761642469b6f3d3c93d2e2e0fa7e02dbf3de6d7
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84446998"
---
# <a name="migrate-your-net-web-app-or-service-to-azure-app-service"></a>Migrar seu aplicativo Web .NET ou serviço para o Serviço de Aplicativo do Azure

O [serviço de aplicativo](https://docs.microsoft.com/azure/app-service/overview) é um serviço de plataforma de computação totalmente gerenciado que é otimizado para hospedar sites e aplicativos Web escalonáveis. Este artigo fornece informações sobre como mover e deslocar um aplicativo existente para Azure App serviço, modificações a serem consideradas e recursos adicionais para [mudar para a nuvem](https://azure.microsoft.com/migration/web-applications/). A maioria dos sites ASP.NET (formulários da Web, MVC) e serviços (API da Web, WCF) podem migrar diretamente para o Serviço de Aplicativo do Azure sem alterações. Alguns podem precisar de pequenas alterações enquanto outros talvez precisem de alguma refatoração.

Pronto para começar? [Publique seu aplicativo ASP.NET + SQL para o Serviço de Aplicativo do Azure](https://tutorials.visualstudio.com/azure-webapp-migrate/intro).

## <a name="considerations"></a>Considerações

### <a name="on-premises-resources-including-sql-server"></a>Recursos locais (incluindo o SQL Server)

Verifique o acesso aos recursos locais, conforme precisem ser migrados ou alterados. Veja a seguir opções para atenuar o acesso a recursos locais:

* Crie uma VPN que conecta o Serviço de Aplicativo aos recursos locais usando as [Redes Virtuais do Azure](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet).
* Exponha com segurança os serviços locais na nuvem sem alterações de firewall usando a [Retransmissão do Azure](https://docs.microsoft.com/azure/service-bus-relay/relay-what-is-it).
* Migre dependências como [banco de dados SQL](https://go.microsoft.com/fwlink/?linkid=863217) para o Azure.
* Use as ofertas de plataforma como serviço na nuvem para reduzir as dependências. Por exemplo, em vez de se conectar a um servidor de email local, considere o uso de [SendGrid](https://docs.microsoft.com/azure/sendgrid-dotnet-how-to-send-email).

### <a name="port-bindings"></a>Associações de Porta

O Serviço de Aplicativo do Azure dá suporte à porta 80 para HTTP e à porta 443 para tráfego HTTPS.

Para o WCF, há suporte para as seguintes associações:

Associação | Observações
--------|--------
BasicHttp |
WSHttp |
WSDualHttpBinding | [O suporte de soquete da Web](https://docs.microsoft.com/azure/app-service/web-sites-configure) deve ser habilitado.
NetHttpBinding | [O suporte de soquete da Web](https://docs.microsoft.com/azure/app-service/web-sites-configure) deve estar habilitado para contratos duplex.
NetHttpsBinding | [O suporte de soquete da Web](https://docs.microsoft.com/azure/app-service/web-sites-configure) deve estar habilitado para contratos duplex.
BasicHttpContextBinding |
WebHttpBinding |
WSHttpContextBinding |

### <a name="authentication"></a>Autenticação

O Serviço de Aplicativo do Azure dá suporte à autenticação anônima por padrão e autenticação de formulários quando pretendido. A autenticação do Windows pode ser usada somente com a integração com o Microsoft Azure Active Directory e o ADFS. [Saiba mais sobre como integrar seus diretórios locais no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

### <a name="assemblies-in-the-gac-global-assembly-cache"></a>Instale assemblies no cache de assembly global (GAC)

Não há suporte para isso. Considere copiar os assemblies necessários para a pasta *\Bin* do aplicativo. Os arquivos *. msi* personalizados instalados no servidor (por exemplo, geradores de PDF) não podem ser usados.

### <a name="iis-settings"></a>Configurações do IIS

Tudo configurado tradicionalmente via applicationHost.config em seu aplicativo, agora pode ser configurado com o portal do Azure. Isso se aplica ao AppPool bit a bit, habilitar/desabilitar WebSockets, versão do pipeline gerenciado, versão do .NET Framework (2.0/4.0) e assim por diante. Para modificar as [configurações do aplicativo](https://docs.microsoft.com/azure/app-service/web-sites-configure), navegue até o [portal do Azure](https://portal.azure.com), abra a folha de seu aplicativo Web, em seguida, selecione a guia **Configurações do Aplicativo**.

#### <a name="iis5-compatibility-mode"></a>Modo de Compatibilidade do IIS5

Não há suporte para o Modo de Compatibilidade do IIS5. No serviço Azure App, cada aplicativo Web e todos os aplicativos sob ele são executados no mesmo processo de trabalho com um conjunto específico de [pools de aplicativos](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc735247(v=ws.10)).

#### <a name="iis7-schema-compliance"></a>IIS7+ conformidade de esquema

Alguns elementos e atributos não são definidos no esquema do IIS do Serviço de Aplicativo do Azure. Se você encontrar problemas, considere o uso de [transformações XDT](https://azure.microsoft.com/documentation/articles/web-sites-transform-extend/).

#### <a name="single-application-pool-per-site"></a>Pool de aplicativos único por site

No serviço Azure App, cada aplicativo Web e todos os aplicativos sob ele são executados no mesmo pool de aplicativos. Considere estabelecer um único pool de aplicativos com configurações comuns ou criar um aplicativo Web separado para cada aplicativo.

### <a name="com-and-com-components"></a>Componentes COM e COM+

O Serviço de Aplicativo do Azure permite o registro de componentes COM na plataforma. Se seu aplicativo utiliza qualquer componente COM, eles precisam ser reescritos em código gerenciado e implantados com o site ou aplicativo.

### <a name="physical-directories"></a>Diretórios físicos

O Serviço de Aplicativo do Azure não permite acesso à unidade física. Talvez seja necessário usar [os arquivos do Azure](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) para acessar arquivos via SMB. [O armazenamento de Blobs do Azure](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) pode armazenar arquivos para acesso via HTTPS.

### <a name="isapi-filters"></a>Filtros ISAPI

O Serviço de Aplicativo do Azure pode dar suporte ao uso de filtros ISAPI, no entanto, a DLL ISAPI deve ser implantada com seu site e registrada por meio do web.config.

### <a name="https-bindings-and-ssl"></a>Associações de HTTPS e SSL

Associações HTTPS não são migradas, nem os certificados SSL associados aos seus sites. [Certificados SSL podem ser carregados manualmente](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-ssl), no entanto, somente após a migração do site.

### <a name="sharepoint-and-frontpage"></a>SharePoint e FrontPage

Não há suporte para o SharePoint e as Extensões de Servidor do FrontPage (FPSE).

### <a name="web-site-size"></a>Tamanho do site da Web

Sites gratuitos têm um limite de tamanho de 1 GB de conteúdo. Se seu site for maior que 1 GB, você deve atualizar para uma SKU paga. Consulte [preços do serviço de aplicativo](https://azure.microsoft.com/pricing/details/app-service/windows/).

### <a name="database-size"></a>Tamanho do banco de dados

Para bancos de dados do SQL Server, verifique os [preços do Banco de Dados SQL do Microsoft Azure](https://azure.microsoft.com/pricing/details/sql-database) atuais.

### <a name="azure-active-directory-aad-integration"></a>Integração do Azure Active Directory (AAD)

O AAD não funciona com aplicativos gratuitos. Para usar o AAD, você deve atualizar o SKU de aplicativo. Consulte [preços do serviço de aplicativo](https://azure.microsoft.com/pricing/details/app-service/windows/).

### <a name="monitoring-and-diagnostics"></a>Monitoramento e diagnóstico

As atuais soluções locais para o monitoramento e diagnóstico provavelmente não funcionarão na nuvem. No entanto, o Azure fornece ferramentas para o registro em log, monitoramento e diagnóstico, para que você possa identificar e depurar os problemas nos aplicativos Web. Você pode habilitar facilmente o diagnóstico para seu aplicativo Web na configuração e pode exibir os logs registrados no Azure Application Insights. [Saiba mais sobre como habilitar o log de diagnóstico para os aplicativos Web](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log).

### <a name="connection-strings-and-application-settings"></a>Configurações do aplicativo e cadeias de conexão

Considere usar o [KeyVault do Azure](https://docs.microsoft.com/azure/key-vault/), um serviço que armazena com segurança as informações confidenciais usadas em seu aplicativo. Como alternativa, você pode armazenar esses dados como uma configuração do Serviço de Aplicativo.

### <a name="dns"></a>DNS

Talvez seja necessário atualizar as configurações de DNS com base nos requisitos de seu aplicativo. Essas configurações de DNS podem ser definidas nas [configurações de domínio personalizadas](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-domain) do Serviço de Aplicativo.

## <a name="azure-app-service-with-windows-containers"></a>Serviço de Aplicativo do Azure com contêineres do Windows

Se seu aplicativo não pode ser migrado diretamente para o Serviço de Aplicativo, considere usar o Serviço de Aplicativo usando contêineres do Windows, que permite o uso do GAC, componentes COM, MSIs, o acesso completo a APIs do .NET FX, DirectX e muito mais.

## <a name="see-also"></a>Confira também

* [Como determinar se seu aplicativo se qualifica para o Serviço de Aplicativo](https://appmigration.microsoft.com/)
* [Movendo o banco de dados para a nuvem](sql.md)
* [Detalhes e restrições da área restrita do aplicativo Web do Azure](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)
