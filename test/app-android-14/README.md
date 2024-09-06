# Testes do APP no Android 14.

### Criação de orçamento, faturamento e emissão de notas

##### Resultado: OK

* Criação de orçamento deu certo. Consegui criar e enviar para o servidor sem problemas.
* Com o mesmo orçamento, prossegui com o seu faturamento como emissão posterior e o mesmo também deu sucesso.
* Fiz outro pedido e emiti uma nota em homologação.
* Também testei o cancelamento das vendas e o teste também deu ok.

### Inventário

##### Resultado: OK

* Bipe sonoro funcionando.
* Pop-up sendo aberto no caso de produtos não encontrados.

### Relatórios

##### Resultado: OK

Relatórios financeiros e de venda gerados com sucesso!

### Fotos dos produtos

##### Resultado: OK

* Testei a funcionalidade de produtos com uma foto, várias fotos e sem nenhuma foto e todos deram ok.
* O teste ocorreu tanto na tela de CRUD de produto quanto na inclusão de um novo produto na venda.

### Sincronização

##### Resultado: OK

* Teste de sincronização de venda offline deu certo.
* Fiz a alteração de um produto no TC Web e efetuei a sincronização no APP e veio as informações atualizadas corretas.
* Cadastrei um cliente de forma normal e a sincronização foi feita.
* Logo após, desativei o Wifi do celular e cadastrei um cliente offline. Reconectei na internet e efetuei a sincronização. No TC Web, no CRUD de cliente, o cliente apareceu corretamente.

### Buscas de clientes e produtos

##### Resultado: OK

* Testei a busca de cliente pelo CRUD no app, tanto por nome quanto por CPF/CNPJ e as pesquisas foram todas corretas.

* A busca dos produtos também foram feitas com sucesso usando: código de barras, descrição, código interno e ref de fabricante.

---

## Sistema offline

> **Para iniciar esses testes, desabilitei a internet do celular**

### Realizações de vendas offline
##### Resultado: OK

1. Criei um orçamento, adicionei um produto e, ao tentar **salvar**, percebi que o sistema já percebeu que estava offline e já salvou a venda dessa forma, sem necessitar de perguntar se eu desejava salvar a venda de forma Local ou no Servidor.

2. Para faturar um pedido o sistema já informa que estamos offline e não será possível. Mas, logo após restaurar a conexão, conseguimos prosseguir com a emissão sem nenhum problema.

---

### Busca e o Uso de filtros nas pesquisas

* Na pesquisa de cliente deu tudo ok, conforme mostrado no teste de busca acima.
* Testei a funcionalidade de filtragem nas pesquisas de produtos dentro da venda e funcionou tudo corretamente.
* O uso dos filtros no módulo financeiro também não apresentou problemas.


---
## Módulo Financeiro
##### Resultado: OK

* Tanto a receita quanto a despesa foram criadas sem nenhum problema. Ambas, ao serem baixadas, impactavam no extrato corretamente.

Vi uma possível melhoria de UX/UI na tela de extrato quanto a atualização da tela:

- A lista do extrato é sempre do mais antigo para o mais atual.
    - Ao efetuar alguma alteração nas receitas/despesas, como baixa, estorno ou cancelamento, o usuário precisa rolar o scroll novamente até a parte de cima da lista para conseguir realizar a sincronização que vai verificar a atualização.
        - Acredito que, caso tenha um botão até mesmo na parte do topo (ao lado do botão de relatório), para sincronizar, essa usabilidade dessa tela seja melhorada.

---

## Multiempresa
##### Resultado: 2 Erros encontrados

> Ambiente de teste: Usuário **Multiorganização** em uma empresa **Multiempresa**.
> **OrgId: 850** **HOMOLOGAÇÃO**
> Usuário: thayron@pg.com
> Senha: 123

### Passos de Reprodução

1. A configuração de Multiempresa estava totalmente marcada como **Empresa**.
2. Logado no APP e no TC Web, fiz a alteração nas configurações para serem compartilhadas via **Organização** as seguintes entidades:
    - Localizacoes
    - Precos de Produto
    - Produtos
    - Localizacao dos Produtos
    - Tabelas Preço
3. Fiz o logoff e efetuei o login, digitando o meu usuário.
4. No APP, ao navegar até o CRUD de produto, percebi que na empresa 1 - HOMOLOGACAO - THAYRON os produtos permaneceram sendo mostrado com a configuração de **Empresa**. Já na empresa 17 - Rm Guedes Beltrao Ltda, os produtos ficaram como **Organizacao**, mostrando todos os produtos cadastrados nas empresas.
5. Efetuei o logoff do usuário e o login novamente, mas o caso persistiu.
6. Nielson percebeu que o problema estava justamente nesse login. Efetuando o login, a população dos produtos só ocorre na empresa atual que está selecionada pelo TC Web.
7. Esse último ponto também mostrou um outro problema:
    - Ao efetuar a alteração de empresa no APP, no TC Web deveria ser alterado também a empresa atual e isto não ocorre. Apenas do TC Web para o APP.




