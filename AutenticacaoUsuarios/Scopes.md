# Scopes de usuário

## Definindo Recursos

O principal papel de um serviço de tokens OpenID Connect/OAuth é controlar o acesso a recursos.

Os dois principais tipos de recurso no Acesso Cidadão são:

- **Recursos de Identidade:** representam claims sobre os cidadãos como Id, apelido ou endereço de email. 
- **Recursos de API:** representam funcionalidades que o cliente quer acessar. Em geral, são endpoints HTTP (aka APIs). Melhor explicado [aqui](/AutorizacaoSistemas/Scopes)

## Recursos de identidade

Um recurso de identidade é um grupo de *"claims"* nomeadas e que pode ser requisitado a partir de um parametro *scope*:

- Uma *claim* é um declaração sobre um cidadão, por exemplo o nome, endereço de e-mail ou data de nascimento. Cada um desses dados individualmente é chamado de *claim*.
- Quando agrupamos algumas claims e damos um nome temos um recurso de identidade.
- Os recursos de identidade podem ser requisitados por um *scope* que nesse caso vai ser o nome do recurso de identidade.

Por exemplo, temos o scope nome do acesso cidadão que contém as claims nome e nomeValidado. Ou o scope openid que é obrigatório pela [especificação OpenId](https://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims) e contém a claim *sub* com o id do cidadão (*subject id*).

### Scopes do Acesso Cidadão

Os scopes públicos estão disponíveis para qualquer sistema cadastrado no Acesso Cidadão. Os scopes não públicos só serão disponibilizados após solicitação com justificativa para a equipe responsável seguindo o modelo de solicitação apresentado abaixo.

#### Padrão do Protocolo

* openid
  * avatarUrl
  * apelido
  * sub
* profile (já contém a claim apelido com o campo "Como gostaria de ser chamado")
  * subNovo

#### Públicos

* agentepublico (se tem papel institucional cadastrado no AC)
  * agentePublico (true ou false)
* email
  * email
* permissoes (disponível caso o sistema esteja configurado como corporativo e habilitado para controlar [autorização de usuários](/AutorizacaoUsuarios/AutorizacaoUsuarios))
  * permissao

#### Necessário solicitação
* nome
  * nome
  * nomeValidado (true ou false)
* cpf
  * cpf
  * cpfValidado (true ou false)
* dataNascimento
  * dataNascimento
  * dataNascimentoValidada (true ou false)
* documentos
  * cnhNumero
  * cnhCedula
* filiacao
  * nomePaiValidado
  * nomePai
  * nomeMaeValidado
  * nomeMae
* roles
  * role
* offline_access
  * especial do protocolo

## Como requisitar scopes não públicos

Os scopes não públicos não são disponibilizados diretamente porque contém informações pessoais dos cidadãos e exigem um maior grau de controle tanto de como esses dados serão usados como 
de como serão armazenados e tratados. Toda requisição por esses scopes deve levar em consideração principalmente a [Lei Geral de Proteção de Dados (LGPD) – Lei nº 13.709/18](http://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/L13709.htm)

A Solicitação de Atendimento deve ser enviada para o email atendimento@prodest.es.gov.br por pessoa cadastrada como responsavél por abertura de chamados do orgão solicitante.

### Modelo de solicitação

**Titulo da solicitação:** Inclusão de scopes de usuário em sistema do Acesso Cidadão

**Informações no corpo da solicitação:**

- **Nome do sistema**;
- **Sigla do sistema**;
- **Scopes solicitados**;
- **Justificativa detalhada** de como cada uma das claims solicitadas serão utilizadas, armazenadas e porque são necessárias para as funcionalidades sendo construidas;

(Caso o responsável pelo sistema não tenha permissão para abrir chamados na central de atendimento)

- **Nome do responsável pelo sistema**
- **Email do responsável pelo sistema**
