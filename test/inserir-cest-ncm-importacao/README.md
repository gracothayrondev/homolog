# Inserção de novos NCM's e/ou CEST's caso não existam na base

## Resultado final

Os NCM's e/ou CEST's que estão no XML da importação, ao ser verificado que os mesmos não estão em nossa base de dados, serão verificados se estão no formato padrão, tanto de NCM quanto CEST através de um Regex.

Caso estejam no formato padrão, a persistência na base de dados será prosseguida, criando o NCM e o CEST com as descrições 'NOVO NCM' e 'NOVO CEST', respectivamente.

#### Regex utilizado
> [0-9]{7} - CEST
> [0-9]{8} - NCM

### Como foram feitos os testes?

1. Alterei manualmente um XML colocando NCM's e CEST's inexistentes na base e que seguem o padrão estabelecido pelo Regex para que os mesmos fossem cadastrados.
2. Coloquei também manualmente no XML NCM's e CEST's com caracteres especiais como **_#,%,@_** e verifiquei que o sistema, ao identificar que os mesmos estão foram do padrão, prossegue com a importação anulando o NCM/CEST. Então o produto é cadastrado nulo.
3. Testei com um XML que tinham os dois casos acima, mas que também possuia NCM's e CEST's já cadastrados no sistema para me certificar que o sistema não irá tentar cadastrar esses já cadastrados ocasionando duplicidade. Esse teste também ocorreu corretamente, o sistema apenas cadastra os NCM's e/ou CEST's que respeitem o Regex e que não possuem cadastro em nossa base de dados.

