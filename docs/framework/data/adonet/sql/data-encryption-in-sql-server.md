---
title: Criptografia de dados no SQL Server
ms.date: 03/30/2017
ms.assetid: 83b992f7-b351-4678-b4b9-f4ffd58134cc
ms.openlocfilehash: 1d185dd121336b62bd66a11bf0cc4253b45ae47e
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/07/2019
ms.locfileid: "70794254"
---
# <a name="data-encryption-in-sql-server"></a>Criptografia de dados no SQL Server
O SQL Server fornece funções para criptografar e descriptografar dados usando um certificado, uma chave assimétrica ou uma chave simétrica. Ele gerencia todos eles em um repositório de certificados interno. O repositório usa uma hierarquia de criptografia que protege os certificados e as chaves em um nível com a camada acima deles na hierarquia. Essa área de recurso do SQL Server é chamada Armazenamento Secreto.  
  
 O modo mais rápido de criptografia suportado por funções de criptografia é criptografia de chave simétrica. Este modo é apropriado para tratar grandes volumes de dados. As chaves simétricas podem ser criptografadas por certificados, senhas ou outras chaves simétricas.  
  
## <a name="keys-and-algorithms"></a>Chaves e algoritmos  
 O SQL Server oferece suporte a vários algoritmos de criptografia de chave simétrica, incluindo DES, Triple DES, RC2, RC4, RC4 de 128 bits, DESX, AES de 128 bits, AES de 192 bits e AES de 256 bits. Os algoritmos são implementados usando a API de criptografia do Windows.  
  
 No escopo de uma conexão de banco de dados, o SQL Server pode manter várias chaves simétricas abertas. Uma chave aberta é recuperada do repositório e está disponível para descriptografar dados. Quando uma parte de dados é descriptografada, não há necessidade de especificar a chave simétrica a ser usada. Cada valor criptografado contém o identificador da chave (GUID) da chave usada para criptografa. O mecanismo corresponde ao fluxo de bytes criptografado com uma chave simétrica aberta, se a chave correta tiver sido descriptografada e estiver aberta. Essa chave é usada para executar a descriptografia e retornar os dados. Se a chave correta não estiver aberta, NULL será retornado.  
  
 Para obter um exemplo que mostra como trabalhar com dados criptografados em um banco de dados, consulte [Encrypt a Column of data](/sql/relational-databases/security/encryption/encrypt-a-column-of-data).
  
## <a name="external-resources"></a>Recursos externos  
 Para obter mais informações sobre criptografia de dados, consulte os seguintes recursos.  
  
|Recurso|Descrição|  
|-|-|  
|[SQL Server criptografia](/sql/relational-databases/security/encryption/sql-server-encryption)|Fornece uma visão geral de criptografia no SQL Server. Este tópico inclui links para artigos adicionais.|  
|[Hierarquia de criptografia](/sql/relational-databases/security/encryption/encryption-hierarchy)|Fornece uma visão geral de criptografia no SQL Server. Este tópico fornece links para artigos adicionais.|  
  
## <a name="see-also"></a>Consulte também

- [Securing ADO.NET Applications](../securing-ado-net-applications.md) (Protegendo aplicativos ADO.NET)
- [Cenários de segurança do aplicativo no SQL Server](application-security-scenarios-in-sql-server.md)
- [Autenticação no SQL Server](authentication-in-sql-server.md)
- [Servidor e funções de banco de dados no SQL Server](server-and-database-roles-in-sql-server.md)
- [Propriedade e separação do esquema de usuário no SQL Server](ownership-and-user-schema-separation-in-sql-server.md)
- [Autorização e permissões no SQL Server](authorization-and-permissions-in-sql-server.md)
- [ADO.NET Overview](../ado-net-overview.md) (Visão geral do ADO.NET)
