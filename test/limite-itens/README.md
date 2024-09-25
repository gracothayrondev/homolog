# Limite de itens no catálogo de produtos

## Resultado esperado

O resultado esperado ao fim do teste é que a consulta no banco de dados seja limitada de acordo com o tamanho (parâmetro t) enviado pelo FrontEnd.

**Endpoint**: api/produtos/catalogo

---

Testei pelo Insomnia a chamada desse endpoint informando os parâmetros:
- t
- valor
- localizacaoId
- tabelaPrecoId
- grupoId

O comportamento foi conforme esperado, a consulta foi limitada e trouxe apenas a quantidade de itens que foi pedida pelo parâmetro **t**.

---

#### Teste integrado

Os testes integrados do limite não foram possíveis, porque atualmente o frontend não envia esse parâmetro. Verifiquei na tela de catálogo de produtos na tela de importação, na tela de inventário. 

No PDV, tanto no catálogo quanto na inserção de um novo item na venda, não apontam a sua requisição para o Endpoint do nosso teste.

Sendo assim, os testes foram realizados pelo _**Insomnia**_.

