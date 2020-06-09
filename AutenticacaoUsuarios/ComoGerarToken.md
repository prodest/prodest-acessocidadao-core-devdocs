# Como autenticar um usuário

Para que a autenticação aconteça, todo o canal de comunicação deve ser realizado com o protocolo HTTPS. Será feito um redirecionamento para uma URL de autorização do Acesso Cidadão e, após a autenticação ser concluída, retornará um código de autenticação para a aplicação(app) cliente com intuito de adquirir um token de acesso para os serviços protegidos.

A utilização da autenticação do Acesso Cidadão depende dos seguintes passos:

1\. Ao requisitar autenticação na sua aplicação cliente a mesma verifica se o usuário está logado. Caso o usuário não esteja logado a sua aplicação deve redirecionar para a página de login no Acesso Cidadão.

2\. A requisição é iniciada através de um GET para o endereço **https://acessocidadao.es.gov.br/is/connect/authorize** passando as seguintes informações:

   | **Variavél**|   	   **Descrição**| 
   |-----------------|----------------------------------------------------------------------| 
   | **response_type**| Especifica para o tipo de autenticação sendo utilizado. Neste caso será **code id_token**| 
   | **client_id**      | Chave de acesso, que identifica o app cliente fornecido pelo Acesso Cidadão para cada app cadastrada| 
   | **scope**          | Especifica os recursos que o app cliente quer obter. Um ou mais escopos inseridos para a app cadastrada. Informação mínima a ser preenchida por padrão: **openid profile**. Mais informações [aqui](./Scopes.md)| 
   | **redirect_uri**  |  URI de retorno cadastrada para a app cliente no formato *URL Encode*. Este parâmetro não pode conter caracteres especiais conforme consta na especificação 'auth 2.0 Redirection Endpoint'| 
   | **nonce**          | Sequência de caracteres usado para associar uma sessão do serviço consumidor a um *Token* de ID e para atenuar os ataques de repetição. Pode ser um valor aleatório, mas que não seja de fácil dedução. **Item obrigatório**.| 
   | **state**|           Valor usado para manter o estado entre a solicitação e o retorno de chamada. Item não obrigatório. | 

``` code-block::
    :caption: Exemplo de **URL da requisição**

    https://acessocidadao.es.gov.br/is/connect/authorize?response_type=code%20id_token&client_id=[CLIENT_ID_DA_APLICAÇÃO]&scope=openid%20profile&redirect_uri=https%3A%2F%2Fappcliente.com.br%2Floginacessocidadao&nonce=[NONCE_GERADO]&state=[STATE_GERADO]
```
   
``` important:: Qualquer mensagem de erro no momento desse primeiro redirect deve ser debugado a partir da url de requisição. Na maioria das vezes o erro é relativo ao uso de um redirect_uri (ou scopes), não autorizados. Esses itens são autorizados no AC Admin. Você não pode usar redirect_uri ou scopes não cadastrados no AC Admin!
```

3\. Após autenticado, o AC redireciona para um página de autorização. O usuário deve permitir que app cliente acesse os escopos solicitados. Caso o usuário da solicitação autorize o acesso, é gerado um "token de acesso", conforme demonstrado na especificação OpenID Connect;


``` important:: Essa tela de autorização é opcional na versão atual do AC. Ela pode ser removida mediante requisição ao responsável pelo sistema. Na nova versão do AC essa tela será obrigatória para todos os sistemas.
```

4\. Após a autorização, a requisição é retornada para a URL especificada na variável redirect_uri da url de requisição (acima), enviando os parâmetros abaixo via post:

|**Variavél**   |   **Descrição**   |
|---------------|-----------------|
|**code**       |Código de autenticação gerado pelo AC. Será utilizado para obtenção do Token de Acesso. Possui tempo de expiração e só pode ser utilizado uma única vez.|
|**state**      |*State* passado anteriormente na url de requisição (para https://acessocidadao.es.gov.br/is/connect/authorize) que pode ser utilizado para controle da aplicação cliente. Pode correlacionar com o *code* gerado.| 

5\. Para obter o token de acesso, o app cliente deve fazer uma requisição POST para o endereço https://acessocidadao.es.gov.br/is/connect/token passando as seguintes informações:

Parâmetros que devem ser adicionados no Header da requisição Post https://acessocidadao.es.gov.br/is/connect/token

|**Variavél**   |   **Descrição**|
|---------------|--------------|
|**Content-Type**|Tipo do conteúdo da requisição que está sendo enviada. Nesse caso estamos enviando como um formulário application/x-www-form-urlencoded.|
|**Authorization**|Informação codificada em Base64, no seguinte formato: **CLIENT_ID:CLIENT_SECRET** (utilizar [codificador para Base64](https://www.base64decode.org) para gerar codificação). A palavra Basic deve está antes da informação. Exemplo: O resultado da codificação em Base64 do texto CLIENT_ID:CLIENT_SECRET é Q0xJRU5UX0lEOkNMSUVOVF9TRUNSRVQ= [Referência](https://tools.ietf.org/html/rfc7617#page-4)|


``` code-block::
    :caption: Exemplo de header

    Content-Type: application/x-www-form-urlencoded
    Authorization: Basic Q0xJRU5UX0lEOkNMSUVOVF9TRUNSRVQ=
```

Parâmetros que devem ser colocados no Body da requisição Post https://acessocidadao.es.gov.br/is/connect/token

|**Variavél**   |   **Descrição**   |
|---------------|-----------------|
|**grant_type**|Especifica para o provedor o tipo de autorização. Neste caso será **authorization_code**|
|**code**|Código retornado pela requisição anterior|
|**redirect_uri**|URI de retorno cadastrada para a aplicação cliente no formato *URL Encode*. Este parâmetro não pode conter caracteres especiais conforme consta na especificação 'auth 2.0 Redirection Endpoint'. Deve ser igual ao usado na URL da requisição inicial|

``` code-block:: http
    :linenos:
    :emphasize-lines: 3,5
    :caption: Exemplo da chamada HTTP:

    POST /is/connect/token HTTP/1.1
    Host: acessocidadao.es.gov.br
    Authorization: Basic Q0xJRU5UX0lEOkNMSUVOVF9TRUNSRVQ=
    Content-type: application/x-www-form-urlencoded
    grant-type=authorization_code&code=[CODE_RECEBIDO]&redirect_uri=htts://appcliente.com.br/loginacessocidadao
```

Exemplo da chamada cURL:

```
curl --location --request POST 'https://acessocidadao.es.gov.br/is/connect/token' \
--header 'Authorization: Basic Q0xJRU5UX0lEOkNMSUVOVF9TRUNSRVQ='
--header 'Content-type: application/x-www-form-urlencoded'
--data-urlencode 'grant-type=authorization_code'
--data-urlencode 'code=[CODE_RECEBIDO]'
--data-urlencode 'redirect_uri=htts://appcliente.com.br/loginacessocidadao'
```

``` important:: O parametro redirect_uri é **case sensitive** e deve ser exatamente igual o usado na requisição inicial.
```

O serviço retornará, em caso de sucesso, no formato JSON, as informações conforme exemplo:
```json
{
        "access_token": "(Token de acesso a recursos protegidos do autenticador.)",
        "id_token": "(Token de autenticação com informações básicas do usuário.)",
        "token_type": "(O tipo do token gerado. Padrão: Bearer)",
        "expires_in": "(Tempo de vida do token em segundos.)"
}
```

6\. De posse das informações do json anterior, a aplicação consumidora está habilitada para fazer chamada aos endpoints que solicitam Access Token de usuário do Acesso Cidadão.