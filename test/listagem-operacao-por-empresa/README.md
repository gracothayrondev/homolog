# Criação de Operação de Estoque por Empresa

## Resultado esperado

Poder incluir uma Operação de Estoque para ser utilizada apenas por uma determinada empresa, em casos de organizações multiempresa.

Dessa forma, poderemos incluir no PDV apenas as operações que não possuem empresa_id definida ou que a empresa_id seja a da empresa logada. E no CRUD do Truecloud WEB mesma situação.

Para possibilitar tal feito, foi incluída uma nova coluna (empresa_id) na tabela operacao_estoque.

## Resultados dos testes

1. Inclui uma nova operação de estoque em uma empresa de uma organização multiempresa
2. Verifiquei que a persistência do id da empresa que criou essa operação foi correta.
3. No CRUD no Truecloud WEB aparecem todas as operações que não possuem empresa_id indicada (operações globais para as empresas) e aparece junto com as anteriores as operações que a empresa_id é o mesmo do id da empresa logada.
4. Testei no PDV, alternando várias vezes entre as empresas e o resultado também foi conforme o esperado.
