# Criação de permissão 'Alterar Data de Movimento' para o PDV

O objetivo foi criar uma permissão para ser utilizada no PDV que possibilita ao usuário a alteração da data de movimento do pedido a ser faturado.

Como essa função no sistema é muito crítica, a implementação dessa permissão é para que a mesma venha por padrão como **NEGADA**. Sendo assim, no builder de criação de uma nova empresa e consequentemente um novo usuário, coloquei essa condição para que essa permissão fique sempre desmarcada em todo e qualquer perfil (Administrador, Suporte, Vendedor, Convidado).

Fiz o testes e deu tudo certo, sem nenhum problema. Criei uma empresa nova e observei que nos usuários padrão que o sistema cria (Suporte e Administrador), essa permissão veio desmarcada.