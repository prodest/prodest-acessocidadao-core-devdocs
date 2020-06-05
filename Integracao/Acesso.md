# Como solicitar acesso de desenvolvedor

Para fazer integrações com os sistemas disponibilizados pelo Governo do Estado do Espírito Santo é necessário que haja um registro desse sistema integrador no AC Admin. Esse registro ocorre por meio de Solicitação de Atendimento enviada para o email atendimento@prodest.es.gov.br por pessoa cadastrado como responsavél por abertura de chamados do orgão solicitante.

Após o registro o responsável pelo sistema integrador será configurado como proprietário do novo sistema sendo responsável pelas informações ali contidas e pelo cadastramento (e descadastramento) de todos os desenvolvedores com acesso as informações e configurações daquele sistema.

## Criando um App

Após o registro do sistema conforme descrito anteriormente é necessário criar os Apps para obter um Client Id e Client Secret necessários para o consumo de qualquer serviço.

Um App poderá ter dois tipos de fluxo:
  - **ClientCredentials:** Fluxo que deverá ser utilizado quando se deseja obter o Access Token através da autenticação da aplicação. Conforme explicado [aqui](/AutorizacaoSistemas/ComoGerarToken.md) 
  - **Hybrid:** Fluxo que deverá ser utilizado quando se deseja obter o Access Token através da autenticação do usuário. Conforme explicado [aqui](/AutenticacaoUsuarios/ComoGerarToken.md)
  
## Criando um App ClientCredentials

A criação do App Client Credentials depende dos seguintes passos:
   1. Acessar o Acesso Cidadão Admin (https://sistemas.es.gov.br/prodest/acessocidadao.admin).
   2. Selecionar o item Sistemas do menu à esquerda.
   3. Procurar o sistema que deseja criar o App.
   4. Selecionar o sistema que deseja criar o App.
   5. Selecionar o item App do menu à esquerda.
   6. Conferir se está no ambiente onde se deseja criar o app e alterar caso necessário.
   7. Clicar no botão "Adicionar Legacy".
   8. Preencher as informações do App, neste caso o fluxo a ser selecionado deve ser o "Client Credentials".
   9. Clicar no botão Salvar.

Após a criação do App será possível obter o Client Id e o Client Secret clicando no botão representado por um cadeado fechado (Visualizar senha) referente ao registro criado.

## Criando um App Hybrid

A criação do App Hybrid depende dos seguintes passos:
   1. Acessar o Acesso Cidadão Admin (https://sistemas.es.gov.br/prodest/acessocidadao.admin).
   2. Selecionar o item Sistemas do menu à esquerda.
   3. Procurar o sistema onde deseja criar o App.
   4. Selecionar o sistema onde deseja criar o App.
   5. Selecionar o item App do menu à esquerda.
   6. Conferir se está no ambiente onde se deseja criar o app e alterar caso necessário.
   7. Clicar no botão "Adicionar Legacy".
   8. Preencher as informações básicas do App, neste caso o fluxo a ser selecionado deve ser o Hybrid.
   9. Clicar no botão Salvar.

Após a criação do App será possível obter o Client Id e o Client Secret clicando no botão representado por um cadeado fechado (Visualizar senha) referente ao registro criado. Também é importante cadastrar os scopes de usuário que serão usados pelo app, conforme explicado [aqui](/AutenticacaoUsuarios/Scopes.md).
Antes de usar um app Hybrid é necessário se cadastrar as urls de retorno permitida e que são usadas no protocolo de autenticação. Conforme explicado [aqui](/AutenticacaoUsuarios/ComoGerarToken.md)

    Não é recomendado fazer cadastro de urls do tipo localhost 
    nos ambiente de produção. A nova versão do AC Admin não vai permitir
    esse tipo de cadastro a não ser no ambiente de desenvolvimento.
    Urls do tipo http não vão ser permitidas.


