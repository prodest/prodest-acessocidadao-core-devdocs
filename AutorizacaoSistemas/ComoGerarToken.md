# Como gerar um token de acesso para sistema

Nessa página é explicado todo o processo necessário para autorização de um sistema. Em resumo será feito uma requisição a URL específica do Acesso Cidadão de onde será retornado um token de acesso em nome do sistema permitindo assim consumir os recursos protegidos.

``` important:: Toda comunicação com Acesso Cidadão deve ser realizada com o protocolo HTTPS
```

A utilização da autorização de sistemas no Acesso Cidadão depende dos seguintes passos:

1\. Para obter o token de acesso de sistema, o cliente deve fazer uma requisição POST para o endereço https://acessocidadao.es.gov.br/is/connect/token passando as seguintes informações:

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

``` important:: 
    Além da necessidade de um Token de Acesso a maioria dos serviços que podem ser consumidos tem regras específicas que 
    devem ser consultadas na documentação de cada serviço. Em geral, pelo menos alguns scopes de sistema são exigidos
    por serviço sendo consumido. 
```

Você pode consultar mais informações sobre scopes de sistema na seção de [Recursos](/Recursos).
