# Como solicitar acesso de desenvolvedor

Para fazer integrações com os sistemas disponibilizados pelo Governo do Estado do Espírito Santo é necessário que haja um registro desse sistema integrador no  AC Admin. Essa registro ocorre por meio de Solicitação de Atendimento enviada para o email atendimento@prodest.es.gov.br.

Após o registro o responsável pelo sistema integrador será configurado com proprietário do novo sistema sendo responsável pelas informações ali contidas e pelo cadastramento (e descadastramento) de todos os desenvolvedores com acesso as informações e configurações daquele sistema.

## Criando um App

Após o registro do sistema conforme descrito anteriormente é necessário criar os Apps para obter um Client Id e Client Secret necessários para o consumo de qualquer serviço.

Um App poderá ter dois tipos de fluxo:
  - **Hybrid:** Fluxo que deverá ser utilizado quando se deseja obter o Access Token através da autenticação do usuário. Conforme explicado [aqui](./AutenticacaoUsuarios/AutenticacaoUsuarios.md)
  - **ClientCredentials:** Fluxo que deverá ser utilizado quando se deseja obter o Access Token através da autenticação da aplicação. Conforme explicado [aqui](/AutenticacaoSistemas/AutenticacaoSistemas.md) 

