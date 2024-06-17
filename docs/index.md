# Documentação Dashboard TIM

## Objetivo

O Dashboard tem como objetivo trazer as informações pertinentes sobre os números da parceria com a TIM e também possibilita uma visão detalhada sobre a operação no dia a dia. 

## Detalhamento do Dashboard

### Página 1 - Vendas Líquidas

#### Filtros:
    1. Regional / Franquia: 
        - Mostra as Regionais e a Franquias;
    2. Tipo de Movimento: 
        - Mostra qual foi a operação feita;
    3. Tipo de Filiado:
        - Mostra qual é o perfil do cliente;
    4. Tipo de Pagamento:
        - Mostra as formas de pagamento;
    5. Tipo de Fluxo:
        - Mostra os canais de filiação;
    6. Período:
        - Mostra o Mês/Ano;

#### Gráficos:
    1. **Bignumber (Cartão): Cashback**
        - Esse cartão exibe o total de cashback fornecido;
    2. **Bignumber (Cartão): TPV**
        - Esse cartão exibe o total de TPV (valor total faturado em planos);
    3. **Bignumber (Cartão): TKM**
        - Esse cartão exibe o ticket médio que é dado, pelo valor total faturado em planos, pela quantidade de transações;
    4. **Bignumber (Cartão): Volume**
        - O cartão exibe a quantidade de pedidos distintos, a quantidade de clientes distintos e a quantidade de transações;
    5. **Seleção de Parametros (Lista): Período**
        - A lista oferece um parametro que alterna o eixo dos gráficos seguintes entre: Ano, Mês/Ano, Semana e Dia;
    6. **Seleção de Parametros (Lista): Valor**
        - A lista oferece um parametro que alterna os valores dos gráficos seguintes entre:
            - Valor Cashback que é o valor total de cashback;
            - Valor Plano que é o valor total faturado;
            - Quant. de Pedidos que é a quantidade de pedidos distintos realizados;
            - Quant. de Clientes que é a quantidade de clientes distintos;
    7. **Seleção de Parametros (Lista): Local**
        - A lista oferece um parametro que alterna o eixo do gráfico 'Ranking' entre Regional e Franquia;
    8. **Gráfico de Linhas: Evolução Acumulada**
        - O gráfico mostra a evolução acumulada de acordo com o parametro de período selecionado e também o parametro de valor selecionado;
    9. **Gráfico de Colunas: Evolução**
        - O gráfico mostra a evolução de acordo com o parametro de período selecionado e também o parametro de valor selecionado; 
        - Esse gráfico conta com um botão que leva a visão detalhada sendo a página dois desse dashboard; 
    10. **Gráfico de Barras: Ranking**
        - O gráfico mostra o total em valor de acordo com o parametro valor selecionado e tem o parametro próprio (parametro local);

### Página 2 - Detalhamento

#### Filtros:
    1. Regional / Franquia: 
        - Mostra as Regionais e a Franquias;
    2. Tipo de Movimento: 
        - Mostra qual foi a operação feita;
    3. Tipo de Filiado:
        - Mostra qual é o perfil do cliente;
    4. Tipo de Pagamento:
        - Mostra as formas de pagamento;
    5. Tipo de Fluxo:
        - Mostra os canais de filiação;
    6. Período:
        - Mostra o Mês/Ano;
    
#### Gráficos:
    1. **Tabela Matriz: Detalhamento por Regional**
        - A tabela traz as colunas, regional, faturamento, total em cashback, ticket médio, quantidade de clientes distintos, quantidade de pedidos distintos, quantidade de transações e SLA. A matriz visa trazer o detalhamento agrupado por regional, franquia, mês/ano, tipo de produto e plano;
    2. **Tabela Matriz: Detalhamento por Período**
        - A tabela traz as colunas, regional, faturamento, total em cashback, ticket médio, quantidade de clientes distintos, quantidade de pedidos distintos, quantidade de transações e SLA. A matriz visa trazer o detalhamento agrupado por perído, regional, franquia, mês/ano, tipo de produto e plano;

## Detalhamento de Regras de Negócio

### Filtros aplicados
    1. Não é considerado nenhum valor onde o campo 'partner' seja igual a 'teste' na tabela fonte das operações TIM.
    2. Para trazer informações sobre 'tipo de filiado' e a 'titularidade' foi feito uma junção que traz as informações de outras tabelas através do CPF, nessa tabela, é apenas considerado os registros atuais e o com status de ativos através dos campos, 'registro_atual' e 'id_status_atual_filiado', respectivamente.
    3. Para trazer as informações sobre a data de primeiro e último cashback, é feito junção com outra tabela onde o tipo de transação precisa ser de cashin, o parceiro seja nacional e também o nome ser igual a 'tim', através do campos, 'id_tipo_transacao', 'id_nv01' e nv02_parceiro', respectivamente.
    
### Regras de Negócio aplicada em colunas:

#### Coluna 'tipo_filiado'
A coluna 'tipo_filiado' é uma coluna condicional dada através dos seguintes casos:

    1. Caso quando o campo 'cashback_rule' for igual a 'employee' então, mostre 'colaborador';
    2. Caso quando o campo 'cashback_rule' for igual a 'default' then 'filiado'
    3. Caso nenhuma das condicionais sejam atendidas, a coluna retorna o valor que está em 'cashback_rule';

#### Coluna 'tipo_filiado'
Coluna 'tipo_filiado' seguinte, é um novo tratamento feito a partir do tratamento anterior, sendo criada a partir das condionais seguintes:

    1. Caso quando o valor for igual a 'colaborador', então, mostre 'colaborador';
    2. Caso quando o valor for igual a 'filiado' e o campo 'titularidade' for igual a 'titular', então, mostre 'filiado - titular';
    3. Caso quando o valor for igual a 'filiado' e o campo 'titularidade' for igual a 'dependente', então, mostre 'filiado - dependente';
    4. Caso nenhuma das condicionais sejam atendidas, o valor mostra o que tem no campo 'tipo_filiado';

#### Coluna 'flg_colaborador'
Coluna 'flg_colaborador' é uma coluna condicional dada através dos seguintes casos:

    1. Caso quando o campo 'flg_colaborador' for vazio ou igual a '0' e o 'tipo_filiado' (do primeiro tratamento) for igual a 'colaborador', então, mostre '1';
    2. Caso quando o campo 'flg_colaborador' for vazio e o 'tipo_filiado' (do primeiro tratamento) for igual a 'filiado', então, mostre '0'
    3. Caso nenhuma das condicionais sejam atendidas, mostre o valor já existente em 'flg_colaborador';

Esse tratamento é feito pois existe uma divergência entre o que é considerado filiado e colaborador entre a base MaisTODOS e os dados recebidos pela TIM.

#### Coluna 'vlr_plano'
A coluna 'vlr_plano' é uma coluna condicional dada através dos seguintes casos: 

    1. Caso quando o campo 'flg_colaborador' (tratado anteriormente) seja '1' e a data de ativação seja menor que '2024-05-05' e o plano seja igual a 'tim controle smart 5 0' ou 'tim controle b express 5 1', então, o valor do plano é R$46.99;
    2. Caso quando o campo 'flg_colaborador' (tratado anteriormente) seja '1' e a data de ativação seja maior que '2024-05-06' e o plano seja igual a 'tim controle smart', 'tim controle b express 5 1', 'tim controle j express 6 0' ou 'tim controle smart 5 0', então, o valor do plano é R$52.99;
    3. Caso quando o campo 'flg_colaborador' (tratado anteriormente) seja '1' e nenhuma das condições anteriores sejam cumpridas, o valor do plano será R$52.99;
    4. Caso quando o campo 'flg_colaborador' (tratado anteriormente) seja '0' e a data de ativação seja menor que '2024-05-15' e o plano seja igual a 'tim controle smart 5 0', então, o valor do plano é R$52.99;
    5. Caso quando o campo 'flg_colaborador' (tratado anteriormente) seja '0' e a data de ativação seja maior que '2024-05-16' e o plano seja igual a 'tim controle smart', então, o valor do plano é R$57.99;
    6. Caso quando o campo 'flg_colaborador' (tratado anteriormente) seja '0' e a data de ativação seja maior que '2024-05-16' e o plano seja igual a 'tim controle redes sociais' ou 'tim controle l express 6 0', então, o valor do plano é R$64.99;
    7. Caso quando o campo 'flg_colaborador' (tratado anteriormente) seja '0' e a data de ativação seja maior que '2024-05-16' e o plano seja igual a 'tim controle j express 6 0', então, o valor do plano é R$54.99;

#### Coluna 'sla'
A coluna SLA é advida da diferença da data do primeiro cashback e a data de ativação;

#### Coluna 'quant_cashback_devido'
A coluna quantidade de cashback devido é a quantidade de meses entre a data de ativação o mês atual;
