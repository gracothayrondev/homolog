# Resultados do teste

### Regime Normal

1 - Feito a importação utilizando o CST **10**.
2 - Verificado que o produto recebeu essa informação persistida no banco de dados.
3- Logo após, foi feito outra importação, colocando o CST **60** no produto.
4 - Analisei novamente que a informação fora persistida no banco de dados corretamente.
5 - Efetuei o cancelamento da última importação e percebi que o produto recebeu o retorno da informação para o CST **10** corretamente.

### Regime Simples

1 - A importação do simples nacional recebeu o CSOSN **102** no primeiro caso.
2 - Analisei no banco de dados e a informação foi persistida corretamente, sem nenhum problema.
3 - Prossegui com a segunda importação que alterou o CSOSN do produto para **900**
4 - Efetuei o cancelamento da última importação e constatei que houve o comportamento esperado, o produto retornou para o CST **102**.

#### Erros encontrados

###### 03/09/2024
Verifiquei que na tabela produto_trib não ocorreu o regresso dos campos icms_csosn e icms_cst.