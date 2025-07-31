# Instruções de Código Python para o Pagar.me Billing

## Princípios Gerais

- Priorize legibilidade, clareza e coesão em cada script.
- Cada arquivo `.py` deve representar uma fatia vertical completa de uma funcionalidade de negócio (Vertical Slice Architecture).
- Mantenha cada módulo independente, focado em uma única responsabilidade.
- Utilize comentários claros para explicar decisões de design e fluxos de dados.
- Sempre documente integrações externas (S3, Postgres, Oracle, APIs) e explique o papel de cada dependência.

## Convenções de Código Python

- Siga o guia de estilo PEP 8 para formatação e organização do código.
- Use 4 espaços para indentação e limite linhas a 79 caracteres.
- Nomeie funções, variáveis e arquivos de forma descritiva e consistente.
- Utilize type hints e o módulo `typing` para anotações de tipos.
- Escreva docstrings em conformidade com PEP 257 para funções, classes e módulos.
- Separe funções e classes com linhas em branco para facilitar a leitura.

## Estrutura de Scripts (Vertical Slice)

Cada script deve conter:
- Toda a lógica de negócio referente à feature.
- Funções auxiliares e utilitários locais à feature.
- Integração direta com fontes de dados necessárias (S3, Postgres, etc).

## Fluxo de Dados e Integrações

- Utilize arquivos CSV no S3 como principal meio de comunicação entre módulos.
- Scripts devem ler entradas do S3, processar e salvar saídas no S3.
- Para integrações com bancos de dados ou APIs, documente endpoints, queries e formatos esperados.

## Tratamento de Erros e Edge Cases

- Sempre trate entradas vazias, tipos inválidos e exceções comuns.
- Inclua comentários explicando o tratamento de casos de borda.

## Exemplo de Documentação de Função

```python
def calcular_receita(valor: float, taxa: float) -> float:
    """
    Calcula a receita líquida a partir de um valor bruto e uma taxa.

    Parâmetros:
    valor (float): Valor bruto da transação.
    taxa (float): Taxa percentual a ser descontada.

    Retorna:
    float: Valor líquido após desconto da taxa.
    """
    return valor * (1 - taxa)
```
