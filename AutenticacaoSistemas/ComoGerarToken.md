# Como gerar um access_token de sistema

Para que a autenticação aconteça, todo o canal de comunicação deve ser realizado com o protocolo HTTPS. Será feito uma requisição a uma URL de autorização do Acesso Cidadão e, após a autenticação ser concluída, retornará um token de acesso em nome do sistema para consumir os serviços protegidos.

A utilização da autenticação de sistemas no Acesso Cidadão depende dos seguintes passos:

1\. Para obter o token de acesso de sistema, o consumidor deve fazer uma requisição POST para o endereço https://acessocidadao.es.gov.br/is/connect/token passando as seguintes informações:

Parâmetros do Header para requisição POST:

|**Variável**|**Descrição**|
|------------|-----------|
|Content-Type|Tipo do conteúdo da requisição que está sendo enviada. Nesse caso estamos enviando como um formulário application/x-www-form-urlencoded.|
|Authorization|Informação codificada em Base64, no seguinte formato: **CLIENT_ID:CLIENT_SECRET** (utilizar [codificador para Base64](https://www.base64decode.org) para gerar codificação). A palavra Basic deve está antes da informação. Exemplo: O resultado da codificação em Base64 do texto CLIENT_ID:CLIENT_SECRET é Q0xJRU5UX0lEOkNMSUVOVF9TRUNSRVQ= [Referência](https://tools.ietf.org/html/rfc7617#page-4)|


```code-block::
    :caption: Exemplo de header

    Content-Type: application/x-www-form-urlencoded
    Authorization: Basic Q0xJRU5UX0lEOkNMSUVOVF9TRUNSRVQ=
```

Parâmetros que devem ser colocados no Body da requisição POST para **https://acessocidadao.es.gov.br/is/connect/token**

|**Variável**|**Descrição**|
|------------|-----------|
|grant_type|Especifica para o provedor o tipo de autorização. Neste caso será 'client_credentials'|
|scope|Especifica os recursos que o serviço consumidor quer obter. Um ou mais escopos inseridos para a aplicação cadastrada.|

```code-block:: http
    :caption: Exemplo da chamada HTTP
    
    POST /is/connect/token HTTP/1.1
    Host: acessicidadao.es.gov.br
    Authorization: Basic Q0xJRU5UX0lEOkNMSUVOVF9TRUNSRVQ=
    Content-type: application/x-www-form-urlencoded
    
    grant-type=client_credentials&scope=scopes-selecionados
```

```code-block::
    :caption: Exemplo da chamada cURL
    
    curl --location --request POST 'https://acessocidadao.es.gov.br/is/connect/token' \
    --header 'Authorization: Basic Q0xJRU5UX0lEOkNMSUVOVF9TRUNSRVQ='
    --header 'Content-type: application/x-www-form-urlencoded'
    --data-urlencode 'grant-type=client_credentials'
    --data-urlencode 'scope=scopes-selecionados'
```

```code-block:: json
    :caption: O serviço retornará, em caso de sucesso, as informações abaixo no formato JSON

    {
        "access_token": "(Token de acesso a recursos protegidos do autenticador.)",
        "token_type": "(O tipo do token gerado. Padrão: Bearer)",
        "expires_in": "(Tempo de vida do token em segundos.)"
    }
```

2\. De posse do token do json anterior, a aplicação consumidora está habilitada a fazer chamada aos endpoints protegidos que solicitam Access Token do Acesso Cidadão.
