# TESTES SOBRE ATUALIZAÇÃO DO REACT NATIVE

---
## TELA VENDAS

1. Iniciei uma nova venda
2. Adicionei os produtos<br>
2.1 Consegui pesquisar através da leitura de código de barras<br>
2.2 Fiz a alteração de preço no produto<br>
2.3 Fiz a pesquisa utilizando os filtros<br>
3. Alterei o cliente da venda
4. Inclui pagamento
5. Salvei no servidor a venda <br>
5.1 Consegui também faturar como **Emissão Posterior** <br>
5.2 Também consegui cancelar a venda pelo app.

Não encontrei nenhum problema nessa tela em meus testes.

Resultado do teste: <span style="color:green;"><b> OK </b></span>

---
## TELA FINANCEIRO

Erro ao tentar cadastrar nova receita ou despesas

No logcat em meu celular o erro foi o:
~~~
[AUX]GuiExtAuxCheckAuxPath
~~~

Já utilizando o Android Studio, Lucas verificou que o problema está no onPressOut do componente.

Resultado do teste: <span style="color:red;"><b> ERRO </b></span>

---
## TELA CLIENTES

1. Realizei um cadastro sem nenhum problema <br>
1.1 Testei o componente de pesquisa de cidade <br>
1.2 Testei a pesquisa de endereço por CEP
2. Consegui buscar um cliente utilizando a pesquisa da tela sem problemas
 
Resultado do teste: <span style="color:green;"><b> OK </b></span>

---
## TELA INVENTÁRIO

1. Utilizei a câmera do celular como leitor sem problemas.
2. Inclui um produto manualmente, também sem qualquer problema
3. Exclui um item da contagem
4. Editei um item da contagem
5. Criei uma nova contagem

Resultado do teste: <span style="color:green;"><b> OK </b></span>

---
## TELA FORNECEDORES

Mesmo comportamento positivo da [TELA DE CLIENTES](#tela-clientes).

Resultado do teste: <span style="color:green;"><b> OK </b></span>

---
## TELA PRODUTOS

Na versão que está em produção, ao tentar alterar um preço de um produto que **não possui** nenhum preço, ao clicar em salvar, o app segue o fluxo como se houvesse a persistência dessa alteração.

Já na versão do React Native 0.63, ao clicar em salvar, o loading fica girando em loop até que o usuário saia da tela. Também não há persistência de alteração.

Estou documentando aqui apenas a diferença de comportamento entre as versões.

Resultado do teste: <span style="color:green;"><b> OK </b></span>