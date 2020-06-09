# Definindo Recursos

O principal papel de um serviço de tokens OpenID Connect/OAuth é controlar o acesso a recursos.

Os dois principais tipos de recurso no Acesso Cidadão são:

- **Recursos de Identidade:** representam claims sobre os cidadãos como Id, apelido ou endereço de email.
- **Recursos de API:** representam funcionalidades que o cliente quer acessar. Em geral, são endpoints HTTP (aka APIs).

``` important:: Scopes de identidade e scopes de API significam duas coisas diferentes apesar do mesmo nome. Como explicado abaixo o protocolo define
    o conceito de scope, mas apenas como recursos protegidos e nada mais.
```

## Recursos de identidade

Um recurso de identidade é um grupo de *"claims"* nomeadas e que pode ser requisitado a partir de um parametro *scope*:

- Uma *claim* é um declaração sobre um cidadão, por exemplo o nome, endereço de e-mail ou data de nascimento. Cada um desses dados individualmente é chamado de *claim*.
- Quando agrupamos algumas claims e damos um nome temos um recurso de identidade.
- Os recursos de identidade podem ser requisitados por um *scope* que nesse caso vai ser o nome do recurso de identidade.

Por exemplo, temos o scope **nome** do acesso cidadão que contém as claims **nome** e **nomeValidado**. Ou o scope **openid** que é obrigatório pela [especificação OpenId](https://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims) e contém a claim ***sub*** com o id do cidadão (*subject id*).

``` important:: No momento do login o usuário deve autorizar que a aplicação cliente acesse o seus recursos de identidade. A recomendação é que nunca
    se solicite mais recursos do que o estritamente necessário para o funcionamento da sua aplicação.
```

## Recursos de APIs

``` important:: Os recursos de API são definidos por sistema no AC Admin. Um sistema precisa ser definido como Corporativo para exibir a área de Autorização onde isso é criado/configurado. Caso só esteja interessado em consumir os scopes de outro sistema, esses devem ser requisitados aos gestores de cada sistema.
```

A especificação OAuth 2.0 Original possui o conceito de *scope*, que é definido como um escopo de acesso (*the scope of access*) que um cliente requisita. Falando tecnicamente
o parametro scope é uma lista de valores delimitidos por espaço sem qualquer estrutura ou semântica, que precisam ser definidas por quem está criando o serviço sendo consumido. 
Ou simplificando, quem cria um serviço protegido precisa definir o que esses escopos significam.

Em sistemas mais complexos, é normal termos a noção de *recursos*. Por exemplo, um dado ou endpoint/ação específico.
Também podemos ter o conceito de escopes. Alguns escopes são exclusivos de um recurso, outros podem ser compartilhados entre vários recursos.

Abaixo vamos explicar como podemos usar recursos para estruturar os scopes.

### Scopes de API

Vamos modelar um cenário simples - um sistema tem três operações lógicas: *obter*, *alterar* e *excluir*.

Você pdoe definir que sua api tem três escopos diferentes e um cliente requisitando todos as três permissões pediria esses
escopos no formato "obter alterar excluir" via protocolo.

### Autorização baseada em scopes

Quando um cliente pede um scope e aquele scope é permitido, os valores permitidos serão incluidos no token de acesso
numa claim do tipo *scope*

```code-block:: 
    :caption: Token de acesso de usuário (tem campo sub)

    {
        "typ": "at+jwt"
    }.
    {
        "client_id": "teste_app",
        "sub": "123",
        "scope": "obter alterar excluir"
    }
```

```code-block:: 
    :caption: Token de acesso de sistema (não tem campo sub)

    {
        "typ": "at+jwt"
    }.
    {
        "client_id": "teste_app",
        "scope": "obter alterar excluir"
    }
```

A aplicação que vai receber o token de acesso pode usar essa informação para saber se o cliente pode realmente consumir determinada funcionalidade.

``` important:: Scopes de API só servem para autorizar clientes e não usuários ou sistemas. Por exemplo, o escopo **alterar** permite que o cliente
    acesse aquela funcionalidade. No entanto, na maioria das vezes esse cliente só deveria alterar dados que pertencem àquele sistema/usuário. Essa
    lógica de autorização adicional é lógica de aplicação e não coberta pelo protocolo OAuth.
```

### Parametrizando Scopes

Conforme a superficie da API vai aumentando, uma lista simples de escopos como usado acime acaba não sendo prático.

Nesse momento você precisa a introduzir algum tipo de regra de nomenclatura para organizar os scopes, e talvez agrupar vários recursos em um único escopo.
Num exemplo em que temos dois recursos, por exemplo, pagamento e clientes. E várias ações exclusivas como pagar e criar, podemos organizar os scopes como exemplificado abaixo:

```code-block:: 

    pagamento:obter
    pagamento:pagar
    cliente:obter
    cliente:criar
    cliente:alterar
```

Também podemos agrupar esses recursos e ter um escopo ***pagamento*** e outro ***cliente*** que permitem todas as ações com um unico escopo. 

No final das contas, você pode definir os ***scopes*** como quiser desde que sua aplicação consiga entender essas *strings* e dar algum contexto na hora de autorizar o acesso aos recursos protegidos.
