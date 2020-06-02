# Como autenticar um usuário

Para que a autenticação aconteça, todo o canal de comunicação deve ser realizado com o protocolo HTTPS. Será feito um redirecionamento para uma URL de autorização do Acesso Cidadão e, após a autenticação ser concluída, retornará um código de autenticação para a aplicação cliente com intuito de adquirir um ticket de acesso para os serviços protegidos.

A utilização da autenticação do Acesso Cidadão depende dos seguintes passos:
   1. Ao requisitar autenticação via Provedor, o mesmo verifica se o usuário está logado. Caso o usuário não esteja logado o provedor redireciona para a página de login.
   2. A requisição é feita através de um GET para o endereço https://acessocidadao.es.gov.br/is/connect/authorize passando as seguintes informações:

| **Variavél**|   	   **Descrição**| 
| -----------------| :----------------------------------------------------------------------:| 
| **response_type**| Especifica para o tipo de autenticação sendo utilizado. Neste caso será **code id_token**| 
| **client_id**      | Chave de acesso, que identifica o serviço consumidor fornecido pelo Acesso Cidadão para a aplicação cadastrada| 
| **scope**          | Especifica os recursos que o serviço consumidor quer obter. Um ou mais escopos inseridos para a aplicação cadastrada. Informação mínima a ser preenchida por padrão: **openid profile**. Mais informações [aqui](scopes.md)| 
| **redirect_uri**  |  URI de retorno cadastrada para a aplicação cliente no formato *URL Encode*. Este parâmetro não pode conter caracteres especiais conforme consta na especificação 'auth 2.0 Redirection Endpoint'| 
| **nonce**          | Sequência de caracteres usado para associar uma sessão do serviço consumidor a um *Token* de ID e para atenuar os ataques de repetição. Pode ser um valor aleatório, mas que não seja de fácil dedução. **Item obrigatório**.| 
| **state**|           Valor usado para manter o estado entre a solicitação e o retorno de chamada. Item não obrigatório. | 


Exemplo de **URL da requisição**:

    https://acessocidadao.es.gov.br/is/connect/authorize?response_type=code%20id_token&client_id=[CLIENT_ID_DA_APLICAÇÃO]&scope=openid%20profile&redirect_uri=https%3A%2F%2Fappcliente.com.br%2Floginacessocidadao&nonce=[NONCE_GERADO]&state=[STATE_GERADO]

   3. Após a autorização, a requisição é retornada para a URL especificada na variável redirect_uri da url de requisição (acima), enviando os parâmetros:


|**Variavél**   |   **Descrição**   |
|---------------|:-----------------:|
|**code**       |Código de autenticação gerado pelo provedor. Será utilizado para obtenção do Token de Resposta. Possui tempo de expiração e só pode ser utilizado uma única vez.|
|**state**      |*State* passado anteriormente na url de requisição (para https://acessocidadao.es.gov.br/is/connect/authorize) que pode ser utilizado para controle da aplicação cliente. Pode correlacionar com o *code* gerado.| 

   4. Após autenticado, o provedor redireciona para a página de autorização. O usuário habilitará o consumidor no sistema para os escopos solicitados. Caso o usuário da solicitação autorize o acesso, é gerado um "ticket de acesso", conforme demonstrado na especificação OpenID Connect;
   5. Para obter o ticket de acesso, o consumidor deve fazer uma requisição POST para o endereço https://acessocidadao.es.gov.br/is/connect/token passando as seguintes informações:

Parâmetros que devem ser adicionados no Header da requisição Post https://acessocidadao.es.gov.br/is/connect/token

|**Variavél**   |   **Descrição**|
|---------------|:--------------:|
|**Content-Type**|Tipo do conteúdo da requisição que está sendo enviada. Nesse caso estamos enviando como um formulário application/x-www-form-urlencoded.|
|**Authorization**|Informação codificada em Base64, no seguinte formato: **CLIENT_ID:CLIENT_SECRET** (utilizar [codificador para Base64](https://www.base64decode.org) para gerar codificação). A palavra Basic deve está antes da informação. Exemplo: O resultado da codificação em Base64 do texto CLIENT_ID:CLIENT_SECRET é Q0xJRU5UX0lEOkNMSUVOVF9TRUNSRVQ= [Referência](https://tools.ietf.org/html/rfc7617#page-4)|

Exemplo de header:

Content-Type: application/x-www-form-urlencoded
Authorization: Basic Q0xJRU5UX0lEOkNMSUVOVF9TRUNSRVQ=

Parâmetros que devem ser colocados no Body da requisição Post https://acessocidadao.es.gov.br/is/connect/token

|**Variavél**   |   **Descrição**   |
|---------------|:-----------------:|
|**grant_type**|Especifica para o provedor o tipo de autorização. Neste caso será **authorization_code**|
|**code**|Código retornado pela requisição anterior|
|**redirect_uri**|URI de retorno cadastrada para a aplicação cliente no formato *URL Encode*. Este parâmetro não pode conter caracteres especiais conforme consta na especificação 'auth 2.0 Redirection Endpoint'. Deve ser igual ao usado na URL da requisição inicial|

Exemplo da chamada HTTP:



Exemplo da chamada cURL:



O serviço retornará, em caso de sucesso, no formato JSON, as informações conforme exemplo:
{
        "access_token": "(Token de acesso a recursos protegidos do autenticador.)",
        "id_token": "(Token de autenticação com informações básicas do usuário.)",
        "token_type": "(O tipo do token gerado. Padrão: Bearer)",
        "expires_in": "(Tempo de vida do token em segundos.)"
}
