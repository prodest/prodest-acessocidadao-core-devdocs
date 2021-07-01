# Exemplo de implementação

Os exemplos apresentados são maneiras básicas de se realizar o login com o Acesso Cidadão. Cada desenvolvedor deve escrever seu código da maneira que lhe convir mas respeitando
a segurança e integridade de suas aplicações, bem como a do Acesso Cidadão.  

O exemplo disponibilizado é escrito utilizando DotNet Framework. Conforme outras linguagens sejam aplicadas serão inclusas nesta página.  

## C# e DOTNET

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