# TESTES WEBPACK TRUECLOUDWEB pt.2

> **Testes realizados na Org 131 (vcan)**

Utilizei como base os testes feito na [pt.1](https://github.com/gracothayrondev/homolog/tree/main/test/webpack-truecloudweb). Testei também as telas que sofreram alterações na Sprint 103.

Foquei os testes apenas nas novas funcionalidades e nas telas onde deram erro no primeiro teste.

---
### TELA PRODUTOS

Testei o novo botão **Ações** e o seu funcionamento se deu sem nenhum problema. 

Simulei a tela em modo responsivo e tentei gerar a etiqueta e não foi. Mas, em testes em outra vesão, constatei que é u merro já existente. Testei também em meu celular e todas as telas de impressão não funcionam.

---
### TELA IMPORTAÇÃO

1. Verifiquei que o problema no detalhamento de grade foi resolvido. Consegui detalhar corretamente a grade, sem nenhum problema.
2. Naveguei por todas as abas da tela sem detectar nenhum problema.
3. Finalizei a importação da nota de teste corretamente.

Resultado do teste: <span style="color:green;"><b> OK </b></span>

---
### TELA DESPESAS E RECEITAS

O erro nessa tela era referente a Busca Avançada. 
Testei utilizando apenas a Busca avançada e funcionou. Testei também a busca utilizando a busca rápida junto com a Busca avançada e também funcionou corretamente.


Resultado do teste: <span style="color:green;"><b> OK </b></span>

---
### TELA CLIENTES

Assim como na [TELA DESPESAS E RECEITAS](#tela-despesas-e-receitas), essa tela também apresentava erros em sua busca avançada. Fiz os mesmos testes e obtive resultados positivos.

Resultado do teste: <span style="color:green;"><b> OK </b></span>


### TELA FORNECEDORES

Os testes foram idênticos ao da [TELA DE CLIENTES](#tela-clientes), com o resultado positivo também.

Resultado do teste: <span style="color:green;"><b> OK </b></span>


Todas as tela de CRUD do que se refere à Pessoa, o problema na busca avançada foi resolvido. Sendo assim: **Clientes, Fornecedores, Funcionários, Transportadores e Vendedores** estão funcionando.

---
### TELA TIPO DE PAGAMENTO

Apresentou o mesmo erro do primeiro teste. A tela ainda não abre e dá o erro no console:
        
       GET https://d2towid6322pn6.cloudfront.net/vcan/js/modules/picklist/pickListModule.js?bust=2.0.1-2 net::ERR_ABORTED 404 (Not Found)


Além de não abrir a tela, a **Visão Geral** do sistema perde seus atalhos e indicadores, exibindo apenas uma tela vazia.


Resultado do teste: <span style="color:red;"><b> ERRO </b></span>

---
### TELA REMESSA BOLETOS

Abriu corretamente sem nenhum problema.

Resultado do teste: <span style="color:green;"><b> OK </b></span>

---
### TELA RETORNO BOLETOS

Assim como à [TELA REMESSA BOLETOS](#tela-remessa-boletos) o resultado foi conforme esperado. A tela abriu sem problemas.

Resultado do teste: <span style="color:green;"><b> OK </b></span>

---
### TELA CONDIÇÕES PGTO

Mesmo problema que a [TELA TIPO DE PAGAMENTO](#tela-tipo-de-pagamento). Não abre a tela e deixa em branco a tela **Visão Geral**.

Resultado do teste: <span style="color:red;"><b> ERRO </b></span>

---
### TELA EMPRESA

Corrigido a busca avançada nessa tela.

Resultado do teste: <span style="color:green;"><b> OK </b></span>

---
### TELA INVENTÁRIO

Essa tela apresentava o mesmo comportamento que [TELA TIPO DE PAGAMENTO](#tela-tipo-de-pagamento), porém foi resolvido nesse teste.

Resultado do teste: <span style="color:green;"><b> OK </b></span>
