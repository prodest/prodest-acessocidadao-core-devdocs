# Como gerar um token de acesso para sistema

## ClientCredentials

Client Credentials (Credenciais de Cliente) é um serviço que roda em um **<em>Servidor de Autenticação</em>** com o qual um sistema (<em>cliente</em>) consome uma API externa de maneira segura, 
através da troca de dados de login e senha (credenciais) por um <em>**token**</em>.  

Por meio deste serviço um sistema cliente recebe as credenciais de um cliente e devolve a ele um token (<em>access token</em>). Com esse token o sistema cliente faz a requisição para a
API. A API então verifica a autenticidade desse token com o serviço. Se as informações conferirem o sistem cliente consome a API normalmente.  

!["Esquema funcionamento ClientCredentials"](../_images/ClientCredentials.png)  

O Acesso Cidadão funciona como o serviço de autenticação: recebe as informações do cliente, devolve um token de acesso com o qual é possível consumir os recursos protegidos.  


## Parâmetros requeridos na solicitação de um token no Acesso Cidadão  

``` important:: Toda comunicação com Acesso Cidadão deve ser realizada com o protocolo HTTPS
```

A utilização da autorização de sistemas no Acesso Cidadão depende dos seguintes passos:

### 1\. Requisição POST

Para obter o token de acesso de sistema, o cliente deve fazer uma requisição POST para o endereço 
[https://acessocidadao.es.gov.br/is/connect/token](https://acessocidadao.es.gov.br/is/connect/token) passando as seguintes informações:

#### Parâmetros do Header para requisição POST:

|**Variável**|**Descrição**|
|------------|-----------|
|Content-Type|Tipo do conteúdo da requisição que está sendo enviada. Nesse caso estamos enviando como um formulário application/x-www-form-urlencoded.|
|Authorization|Informação codificada em Base64, no seguinte formato: **CLIENT_ID:CLIENT_SECRET** (utilizar [codificador para Base64](https://www.base64decode.org) para gerar codificação). A palavra Basic deve está antes da informação. Exemplo: O resultado da codificação em Base64 do texto CLIENT_ID:CLIENT_SECRET é Q0xJRU5UX0lEOkNMSUVOVF9TRUNSRVQ= [Referência](https://tools.ietf.org/html/rfc7617#page-4)|


```code-block:: csharp
    :caption: Exemplo em .NET de Classe que realiza Encode e Decode do Client ID e do Client Secret 

    public static class ExtensionMethods
    {
        public static string EncodeBase64(this string value)
        {
            var valueBytes = Encoding.UTF8.GetBytes(value);
            return Convert.ToBase64String(valueBytes);
        }

        public static string DecodeBase64(this string value)
        {
            var valueBytes = System.Convert.FromBase64String(value);
            return Encoding.UTF8.GetString(valueBytes);
        }
    }
```


```code-block::
    :caption: Exemplo de header

    Content-Type: application/x-www-form-urlencoded
    Authorization: Basic Q0xJRU5UX0lEOkNMSUVOVF9TRUNSRVQ=
```

#### Parâmetros que devem ser colocados no Body da requisição POST para **https://acessocidadao.es.gov.br/is/connect/token**

|**Variável**|**Descrição**|
|------------|-----------|
|grant_type|Especifica para o provedor o tipo de autorização. Neste caso será 'client_credentials'|
|scope|Especifica os recursos que o serviço consumidor quer obter. Um ou mais escopos inseridos para a aplicação cadastrada.

```code-block:: csharp
    :caption: Exemplo de código em .NET da requisição POST
    
    private static async Task Main()
    {
        var client = new HttpClient();
        
        // requisitar token via POST
        var base64EncodedString = "client:secret".EncodeBase64();
        
        var nvc = new List<KeyValuePair<string, string>>();
        nvc.Add(new KeyValuePair<string, string>("grant_type", "client_credentials"));
        nvc.Add(new KeyValuePair<string, string>("scope", "api1"));
        
        var data = new FormUrlEncodedContent(nvc);

        var url = "https://acessocidadao.es.gov.br/is/connect/token";
        
        var postClient = new HttpClient();
        postClient.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Basic", base64EncodedString);
        
        var postResponse = await postClient.PostAsync(url, data);

        string result = postResponse.Content.ReadAsStringAsync().Result;

        dynamic parseado = JObject.Parse(result);
    
        //Chamar a API
        var apiClient = new HttpClient();
        apiClient.SetBearerToken((string)parseado.access_token);
        
        var response = await apiClient.GetAsync("**URL DA API**");

        if (!response.IsSuccessStatusCode)
        {
            Console.WriteLine(response.StatusCode);
        }
        else
        {
            var content = await response.Content.ReadAsStringAsync();
            Console.WriteLine(JArray.Parse(content));
        }

        Console.ReadKey();
    }
```

```code-block:: http
    :caption: Exemplo da chamada HTTP
    
    POST /is/connect/token HTTP/1.1
    Host: acessicidadao.es.gov.br
    Authorization: Basic Q0xJRU5UX0lEOkNMSUVOVF9TRUNSRVQ=
    Content-type: application/x-www-form-urlencoded
    
    grant_type=client_credentials&scope=scopes-selecionados
```

```code-block::
    :caption: Exemplo da chamada cURL
    
    curl --location --request POST 'https://acessocidadao.es.gov.br/is/connect/token' \
    --header 'Authorization: Basic Q0xJRU5UX0lEOkNMSUVOVF9TRUNSRVQ='
    --header 'Content-type: application/x-www-form-urlencoded'
    --data-urlencode 'grant_type=client_credentials'
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

### 2\. Resposta = Access Token 

De posse do token do json anterior, a aplicação consumidora está habilitada a fazer chamada aos endpoints protegidos que solicitam Access Token do Acesso Cidadão.

``` important:: 
    Além da necessidade de um Token de Acesso a maioria dos serviços que podem ser consumidos tem regras específicas que 
    devem ser consultadas na documentação de cada serviço. Em geral, pelo menos alguns scopes de sistema são exigidos
    por serviço sendo consumido. 
```

Você pode consultar mais informações sobre scopes de sistema na seção de [Recursos](/Recursos).
