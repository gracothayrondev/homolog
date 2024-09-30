# TESTES WEBPACK PDV

### Novo Orçamento

1. Iniciei a criação de um novo orçamento
2. Inclui alguns produtos
2.1 Testei também a remoção e edição dos produtos
3. Testei a impressão do orçamento (gerei o PDF)
4. Reabri o orçamento pela tela **Pesquisar Venda**.<br>
4.1 Efetuei alterações nos produtos<br>
4.2 Inclui um observação<br>
4.3 Alterei o cliente<br>
4.4 testei novamente a impressão<br>
5. Testei a abertura do modal de **Análise Lucro** e **Inf. Adic.**

Todo esse fluxo ocorreu sem nenhum problema
Resultado do teste: <span style="color:green;"><b> OK </b></span>

---
### Novo Pedido

1. Iniciei a criação de um novo pedido
2. Efetuei algumas alterações:
    - Cliente
    - Vendedor
3. Inclui diversos produtos
    - Exclui alguns produtos
    - Editei alguns outros
4. Inclui valor de frete pela aba de **Configurações** do pedido
5. Informei pagamento no pedido
6. Testei a impressão do pedido
7. Testei a abertura do modal de **Análise Lucro** e **Inf. Adic.**

Todos esses procedimentos se seguiram sem erros.
Resultado do teste: <span style="color:green;"><b> OK </b></span>

---
### Novo Cupom

1. Iniciei a criação de um novo cupom
2. Efetuei algumas alterações:
    - Cliente
    - Vendedor
3. Tentei incluir um produto sem NCM e o sistema barrou a inclusão corretamente
4. Fiz os mesmos testes que as demais telas. Inclui, editei e exclui produtos
5. Testei a abertura do modal de **Análise Lucro** e **Inf. Adic.**
6. Informei pagamento
7. Tentei gerar um cupom em homologação<br>
7.1 Deu erro de duplicidade, então salvei o cupom e alterei a numeração do último cupom
8. Reabri o orçamento salvo, encontrando-o pela tela **Pesquisar Venda**<br>
8.1 Cliquei em **Faturar**<br>
8.2 Selecionei **Novo Cupom**<br>
8.3 Cliquei em **Registrar** o pagamento novamente para prosseguir com a emissão
9. Emissão finalizada com sucesso

Resultado do teste: <span style="color:green;"><b> OK </b></span>

---
### Nova NF-e

1. Iniciei a criação de uma nova nf-e
2. Efetuei algumas alterações:
    - Cliente
    - Vendedor
3. Inclui os produtos e informei **Descrição complementar** em um deles
4. Apliquei descontos e acréscimos na venda
5. Informei as formas de pagamento
6. Prossegui com a emissão da NF-e
6.1 Deu erro de duplicidade, então salvei a nota e alterei a numeração da último nota
7. Reabri o orçamento salvo, encontrando-o pela tela **Pesquisar Venda**<br>
7.1 Cliquei em **Faturar**<br>
7.2 Selecionei **Nova NF-e**<br>
7.3 Cliquei em **Registrar** o pagamento novamente para prosseguir com a emissão
8. Emissão finalizada com sucesso

Resultado do teste: <span style="color:green;"><b> OK </b></span>

---
### Pesquisar Venda

1. Testei a busca genérica<br>
1.1 Busquei pelo número do pedido e pelo número do cupom/nota
2. Testei a busca avançada selecionando todos os filtros
3. Selecionei o cupom fiscal gerado no [Novo Cupom](#novo-cupom) e solicitei a emissão da nota de cupom do mesmo clicando no botão **Ações**<br>
3.1 A emissão da nota prosseguiu corretamente, no entanto, ao fechar o PDF gerado e retornar para o sistema, toda a tela fica em branco. Sendo necessário o cliente clicar em alguma outra tela na barra de ferramentas para retornar ao uso do sistema ou até mesmo induzir o cliente a reiniciar o PDV.
4. Selecionei o orçamento e o pedido gerado no [Novo Orçamento](#novo-orçamento) e [Novo Pedido](#novo-pedido), respectivamente, e solicitei a mesclagem dos mesmos clicando no botão **Ações**<br>
4.1 Na mesclagem ocorreu tudo normal, exceto pelo o erro que foi apontado [aqui](#erro-nos-valores-da-mesclagem)
5. Solicitei a impressão do pedido direto por esta tela e funcionou corretamente <br>

Devido ao problema na emissão da nota de cupom, irei indicar um erro nesta tela

Resultado do teste: <span style="color:red;"><b> ERRO </b></span>

---
### Catálogo de Produtos

Nessa tela consegui buscar os produtos usando todo os filtros disponíveis e não obtive nenhum resultado negativo

Cliquei no botão para "edição" do **Valor Unit.** e o modal abriu sem nenhum problema.

Resultado do teste: <span style="color:green;"><b> OK </b></span>

---
### Operações Caixa

1. Já havia realizado o procedimento de **Abertura de Caixa** ao iniciar os testes, prosseguiu tudo de maneira adequada
2. Realizei um **Suprimento** e deu tudo certo.
3. Efetuei uma **Sangria** do valor que havia adicionado no passo 2 e tudo ocorreu certo
4. Fiz uma **Sangria total** para fechar o caixa com o saldo zerado
5. Prossegui com o **Fechamento Parcial** para verificar se conseguiria reabrir o caixa normalmente
6. Reabri o caixa informando um **Suprimento inicial**
7. Efetuei o **Fechamento Total** marcando o checkbox **Efetuar sangria total**

Todos esses procedimentos ocorreram sem nenhum problema
Resultado do teste: <span style="color:green;"><b> OK </b></span>

---
### Cadastrar Cliente

1. Iniciei a inclusão de um novo cliente
2. Selecionei o mesmo como Pessoa Jurídica<br>
2.1 Testei a inserção automática dos dados com base no CNPJ


---
### Contingência Manual

Nesta tela, como é bem simples, não encontrei nenhum problema em seu uso. Coloquei o sistema em contingência no NF-e e NFC-e e tudo foi feito de maneira adequada.

Depois retornei o sistema para a normalidade e tudo deu ok novamente.

Resultado do teste: <span style="color:green;"><b> OK </b></span>

---
### Devolução/Troca

As buscas pelas vendas pelo **Número**, **CPF/CNPJ Cliente** e **Nome/Razão Social Cliente** funcionaram perfeitamente!

1. Selecionei uma venda
2. Dos dois produtos existentes na venda, selecionei apenas um para ser devolvido
3. Iniciei a devolução e prossegui com a emissão da nota
4. Após a emissão da nota o sistema perguntou se eu desejava iniciar uma nova venda a partir da devolução. Cliquei que **Sim**
5. Selecionei **Novo Cupom**
6. Adicionei um produto cujo o valor era maior que o crédito gerado pela devolução
7. Em pagamento informei uma parte com o **Créd. Devolução** e o restante em **Dinheiro**.
8. A emissão do cupom fiscal de venda foi feita com sucesso

Testei também a criação de uma devolução sem venda de origem

1. Selecionei o cliente
2. Informei o produto da devolução
3. Emiti a nota fiscal
4. Iniciei uma nova venda a partir da devolução e selecionei **Novo Cupom**
5. Adicionei um produto na venda de mesmo valor do cŕedito gerado
6. Informei o pagamento e gerei o cupom corretamente

Resultado do teste: <span style="color:green;"><b> OK </b></span>

---
### Nf-e Último Cupom

Cliquei no botão e a nota fiscal do último cupom emitido foi realizada com sucesso

Resultado do teste: <span style="color:green;"><b> OK </b></span>

---
### Configurações

Naveguei pela tela de configurações por todas as suas abas sem observar nenhum erro na tela

Resultado do teste: <span style="color:green;"><b> OK </b></span>

---
### Impressão e TEF

Foi realizado o teste de impressão da venda pela impressora EPSON TM-T20x e o uso do pinpad via DLL. Ambos os testes deram ok.

Resultado do teste: <span style="color:green;"><b> OK </b></span>

---
## Novos testes unitários
###### Adicionados no dia 30/09/2024

Testar:

* Abertura caixa
* Ajustes tela de suprimento
* Desc complementar / Complemento obrigatorio
* Comprovante de suprimento

### Abertura Caixa

Testei a abertura de caixa utilizando a nova funcionalidade de suprimento com outros tipos de pagamento além do dinheiro. O caixa foi aberto utilizando **Dinheiro** e **PIX** como suprimentos iniciais. Tudo funcionou corretamente, inclusive a impressão do comprovante.

Resultado do teste: <span style="color:green;"><b> OK </b></span>

### Ajustes tela de suprimento / Comprovante de suprimento

Na aba **Suprimento** na tela **Operações Caixa**, efetuei a inclusão de um suprimento com a forma de pgto PIX.

Resultado do teste: <span style="color:green;"><b> OK </b></span>

### Desc complementar / Complemento obrigatorio

1. Marquei o produto como **true** no parâmetro **Complemento Obrigatório**.
2. Sincronizei o PDV
3. Iniciei a criação de uma nova venda
4. Inclui o produto
5. O sistema abriu o modal solicitando a digitação do complemento/descrição complementar do produto

O produto só é adicionado na venda caso o usuário digite no mínimo 1 caracter no input.

Resultado do teste: <span style="color:green;"><b> OK </b></span>

---
## ERROS GERAIS

#### Estarão inclusos nessa seção os erros que ocorrem tanto na versão do Webpack quanto na versão atual do sistema

### ERRO DE ABERTURA DO VÍDEO

1. Tentei emitir um cupom fiscal com o certificado digital expirado e foi retornado a mensagem de erro com o link do vídeo para a solução do problema
2. Ao clicar no link o sistema abre outra janela da aplicação e não o browser.
3. Na tela que se abre, aparece o loading em loop e não abre o vídeo para que o usuário o assista

### ERRO NOS VALORES DA MESCLAGEM

1. Fiz uma mesclagem de dois pedidos que estavam da mesma forma:
    - Com apenas 1 produto no valor de R$ 100,00
    - Com **Valor Frete**, **Outras Despesas**, **Valor Seguro** e **Desconto** no valor de R$ 100,00
    - Com **Acréscimo** no valor de R$ 200,00
2. Os pedidos, dessa forma, ficavam com o valor final de R$ 500,00
3. Ao solicitar a mesclagem de ambos, observei que o valor final da mesclagem foi de R$ 0,00, porque a mesclagem não leva em conta o **Valor Frete**, **Outras Despesas**, **Valor Seguro** e **Acréscimo**. Só leva para o pedido criado o **Desconto**.