# TESTES WEBPACK TRUECLOUDWEB

> **Testes realizados na Org 131 (vcan)**

---
### TELA PRODUTOS

1. Testado as pesquisas (busca genérica e busca avançada)<br>
1.1 Busca avançada com marca, grupo, subgrupo e categoria 1. Tudo funcionou perfeitamente. <br>
1.2 Efetuei as buscas marcando o checkbox _Exibir inativos_ e também ocorreu tudo normal. <br>
1.3 Pesquisei também utilizando a função de exibição de estoque e também obtive sucesso. <br>
2. Consegui adicionar foto e excluir foto no produto.

Não encontrei nenhum comportamento inadequado nesta tela.<br>
Resultado do teste: <span style="color:green;"><b> OK </b></span>

---
### TELA NOTAS DE ENTRADA E SAÍDA

1. Testei a inclusão de um novo registro, bem como a inclusão de produtos na mesma.<br>
1.1 Consegui salvar o registro sem problemas<br>
1.2 Chamei o processo de impressão do relatório de Movimentação Detalhada.<br>
1.3 Fiz o fechamento da movimentação<br>
1.4 Inseri pagamentos ao registro<br>
1.5 Consegui realizar a reabertura, e inclui outro produto.<br>
1.6 Efetuei o fechamento novamente, o sistema perguntou se eu desejava cadastrar os pagamentos, cliquei em Sim e o sistema não me levou para a aba de pagamentos.<br>
1.7 Prossegui com o cancelamento do registro normalmente.<br>

Não encontrei nenhum comportamento inadequado nesta tela.<br>
Resultado do teste: <span style="color:green;"><b> OK </b></span>

---
### TELA AJUSTE DE ESTOQUE

1. Iniciei a inclusão de um novo registro<br>
1.1 Adicionei um produto<br>
1.2 Consegui salvar e finalizar a movimentação<br>
1.3 Testei a geração de etiquetas e tudo ocorreu normalmente<br>
1.4 Consegui realizar a reabertura, adicionar um novo item, salvar e finalizar novamente sem erros<br>
1.5 Por fim, prossegui com o cancelamento do ajuste<br>
2. Realizei buscas genéricas e avançadas<br>
2.1 A busca genérica pesquisa apenas pelo campo **Num. Doc.**<br>
2.2 Na busca avançada, utilizei todos os seus filtros e a pesquisa ocorreu sem nenhum problema<br>

Não encontrei nenhum comportamento inadequado nesta tela.<br>
Resultado do teste: <span style="color:green;"><b> OK </b></span>

---
### TELA IMPORTAÇÃO

1. Iniciei a importação através de um XML.  
1.1 Verfiquei uma lentidão considerável ao tentar enviar o XML para iniciar a importação  
2. Na aba **Produtos**, ao tentar detalhar grade observei que o modal não é aberto. A tela simplesmente fica mais opaca e o usuário é obrigado a recarregar a página para voltar a ter controle sob a tela.<br>
2.1 Verfiquei também que o modal de seleção de produtos existentes (catálogo) funciona normal, ou seja, o problema não é referente ao componente do modal - ao que tudo indica.<br>
3. Desconsiderando o problema do detalhamento da grade, consegui replicar normalmente os grupos nos itens<br>
4. Na aba **Preço Venda**, apliquei margem de lucro para todos os itens e os checkbox's (Atualizar preço) foram marcados corretamente.<br>
4.1 O botão na coluna Detalhes também está funcionando.<br>
4.2 Verifiquei que a troca de Tabelas de Preço também está ocorrendo corretamente.<br>
5. A aba **Pagamento** tudo está ok. Verifiquei a troca de vencimento da conta, a alteração da forma de pgto e até a geração de novas contas.<br>
6. Ao finalizar a importação ocorreu a mensagem de que a importação estava demorando muito, porém logo após a mesma foi finalizada com sucesso!  


A tela apresentou lentidão para abrir o XML e para finalizar a importação com 30 itens<br>
A tela apresentou problema para abrir o modal de detalhamento de grade<br>


Resultado do teste: <span style="color:red;"><b> ERRO </b></span>

---
### TELA MANIFESTO AVULSO

1. Criei um novo registro de Manifesto Avulso
1.1 Indiquei Veículo, Motorista, Transportador, Município de Carregamento e Estado de Destino sem nenhum problema
2. Na aba **Documentos Fiscais** incluir uma nota sem nenhum problema.
3. Prossegui com a emissão, deu erro apenas porque a chave que utilizei era muito antiga e a SEFAZ deu rejeição. A tela em si passou no meu teste

Não encontrei nenhum comportamento inadequado nesta tela.<br>
Resultado do teste: <span style="color:green;"><b> OK </b></span>

---
### TELA DESPESAS E RECEITAS

Testei todas as funcionalidades, exceto Geração de boletos, pois o meu ambiente de teste não estava configurado para tal função.

Funcionalidades testadas:
- Criação de novo registro
    - Nessa criação testei também a funcionalidade de repetir a conta
- Alteração do Cliente/Fornecedor em conta existente
- Alteração da Conta de Movimentação em conta existente
- Alteração da Categoria em conta existente
- Alteração do valor em conta existente
- Alteração de vencimento em conta existente
- Baixa da conta
    - Testei a baixa com a seleção de várias contas também
- Estorno da conta
    - Testei o estorno com a seleção de várias contas também
- Cancelamento da conta
    - Testei o cancelamento com a seleção de várias contas também
- Busca genérica
- Busca avançada
    - Nessa busca avançada, percebi que os filtros não estão funcionando. Testei em outra Organização que está na versão sem o Webpack e está funcionando perfeitamente.

A tela apresentou problema na busca avançada com seus filtros<br>


Resultado do teste: <span style="color:red;"><b> ERRO </b></span>

---
### TELA EXTRATO FINANCEIRO

Testei as funcionalidades dessa tela e não encontrei nenhum problema.

- Consegui realizar todas as ações:
    - Transferência entre contas
    - Definição de saldo inicial
    - Ajuste do saldo da conta
    - Fechamento de período
    - Consultar fechamentos

Vi que a navegação entre **Conta / Banco** está ocorrendo sem nenhum problema, bem como a navegação entre as data também.

Fiz a utilização dos Filtros e também não obtive nenhum problema!

Não encontrei nenhum comportamento inadequado nesta tela.<br>
Resultado do teste: <span style="color:green;"><b> OK </b></span>

---
### TELA MESCLAGEM CONTAS

Consegui realizar todo os procedimentos nessa tela. 

- Exclusão
- Inclusão
- Geração das contas
- Reabertura da mesclagem
- Cancelamento da mesclagem
- Exportação da tabela

Só não consegui realizar a busca. Sempre aparece o erro **NOT FOUND**, mas verifiquei que o erro ocorre também na outra versão do build.

A tela apresentou apenas este problema na busca, todavia é um erro que ocorre na versão atual. Sendo assim, irei marcá-la como ERRO PARCIAL<br>


Resultado do teste: <span style="color:red;"><b> ERRO PARCIAL</b></span>

---
### TELA CLIENTES

Assim como na [TELA DESPESAS E RECEITAS](#tela-despesas-e-receitas), essa tela também apresentou erros em sua busca avançada. Verifiquei na outra versão e não ocorre esse problema.

As demais funcionalidades, inclusive a busca genérica não apresentou erros.

A tela apresentou problema na busca avançada com seus filtros<br>

Resultado do teste: <span style="color:red;"><b> ERRO </b></span>


### TELA FORNECEDORES

Os testes foram idênticos ao da [TELA DE CLIENTES](#tela-clientes), inclusive com o mesmo erro.

A tela apresentou problema na busca avançada com seus filtros<br>

Resultado do teste: <span style="color:red;"><b> ERRO </b></span>


Foi verificado que, no CRUD do que se refere à Pessoa, existe problema na busca avançada. Sendo assim: **Clientes, Fornecedores, Funcionários, Transportadores e Vendedores** possuem o mesmo comportamento de erro.

---
### TELA TIPO DE PAGAMENTO

A tela não abre na versão do Webpack. Me certifiquei de que a mesma consegue abrir normalmente na versão atual do sistema.

Além de não abrir a tela, a **Visão Geral** do sistema perde seus atalhos e indicadores, exibindo apenas uma tela vazia.


Resultado do teste: <span style="color:red;"><b> ERRO </b></span>

---
### TELA REMESSA BOLETOS

Teve um comportamento parecido com a [TELA TIPO DE PAGAMENTO](#tela-tipo-de-pagamento). A tela nesse caso até abre, aparece o título, porém sua tabela com as funções de listagem e geração de nova remessa não é exibida.

Verifiquei em outra versão e a tela abre normalmente.

Resultado do teste: <span style="color:red;"><b> ERRO </b></span>

---
### TELA RETORNO BOLETOS

Comportamento idêntico à [TELA REMESSA BOLETOS](#tela-remessa-boletos). Abre a tela, mas sem exibição de nenhuma ferramenta.

Resultado do teste: <span style="color:red;"><b> ERRO </b></span>