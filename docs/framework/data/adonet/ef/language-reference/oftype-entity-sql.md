---
title: DOTIPO (Entity SQL)
ms.date: 03/30/2017
ms.assetid: 6d259ca7-bbf0-40f8-a154-181d25c0d67e
ms.openlocfilehash: f1dd5ba92c7b1eaf7117c9732a78e04e5d5a317a
ms.sourcegitcommit: 628e8147ca10187488e6407dab4c4e6ebe0cac47
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72319452"
---
# <a name="oftype-entity-sql"></a>DOTIPO (Entity SQL)
Retorna uma coleção de objetos de uma expressão de consulta que é de um tipo específico.  
  
## <a name="syntax"></a>Sintaxe  
  
```sql  
OFTYPE ( expression, [ONLY] test_type )  
```  
  
## <a name="arguments"></a>Arguments  
 `expression`  
 Qualquer expressão de consulta válida que retorna uma coleção de objetos.  
  
 `test_type`  
 O tipo para testar cada objeto retornado por `expression` contra. O tipo deve ser qualificado por um namespace.  
  
## <a name="return-value"></a>Valor retornado  
 Uma coleção de objetos que são do tipo `test_type`, ou um tipo base ou um tipo derivado de `test_type`. Se for especificado SOMENTE, somente as instâncias de `test_type` ou de uma coleção vazia serão retornadas.  
  
## <a name="remarks"></a>Comentários  
 Uma expressão de `OFTYPE` especifica uma expressão que é emitida para executar um teste de tipo versus cada elemento de uma coleção.  A expressão de `OFTYPE` gerencia uma nova coleção do tipo especificado que contém somente os elementos que foram qualquer equivalente a esse tipo ou um subpropriedades tipo deles.  
  
 Uma expressão de `OFTYPE` é uma abreviação de expressão de consulta a seguir:  
  
```sql  
select value treat(t as T) from ts as t where t is of (T)  
```  
  
 Já que um gerente é um subtipo de funcionários, a seguinte expressão gerenciar uma coleção somente de gerentes de uma coleção de funcionários:  
  
```sql  
OfType(employees, NamespaceName.Manager)  
```  
  
 Também é possível gerar a conversão uma coleção usando o filtro de tipo:  
  
```sql
OfType(executives, NamespaceName.Manager)  
```  
  
 Desde que os executivos são gerentes, a coleção resultante ainda contém todos os executivos originais, embora a coleção é digitada agora como uma coleção de gerentes.  
  
 A tabela a seguir mostra o comportamento do operador de `OFTYPE` sobre alguns padrões. Todas as exceções são geradas do lado do cliente antes que o provedor foi chamado:  
  
|Padrão|Comportamento|  
|-------------|--------------|  
|OFTYPE (coleção (EntityType), EntityType)|Coleção (EntityType)|  
|OFTYPE (coleção (ComplexType), ComplexType)|Gera|  
|OFTYPE (coleção (RowType), RowType)|Gera|  
  
## <a name="example"></a>Exemplo  
 A consulta [!INCLUDE[esql](../../../../../../includes/esql-md.md)] a seguir usa o operador OFTYPE para retornar uma coleção de objetos OnsiteCourse de uma coleção de objetos Course. A consulta é baseada no [modelo escolar](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb896300(v=vs.100)).  
  
 [!code-sql[DP EntityServices Concepts#OFTYPE](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#oftype)]  
  
## <a name="see-also"></a>Consulte também

- [Referência de Entity SQL](entity-sql-reference.md)
