# Scopes de identidade (usuários)

Scopes de identidade protegem [recursos de identidade](/Recursos).

## Scopes do Acesso Cidadão

Os scopes públicos estão disponíveis para qualquer sistema cadastrado no Acesso Cidadão. Os scopes não públicos só serão disponibilizados após solicitação com justificativa para a equipe responsável seguindo o modelo de solicitação apresentado abaixo.

### Padrão do Protocolo

* openid
  * avatarUrl
  * apelido (campo "Como gostaria de ser chamado" do perfil de usuário)
  * sub (id do usuário, DEPRECADO, sempre que possível utilizar o campo subNovo que vai substituir o sub na nova versão)
* profile
  * subNovo (id do usuário, vai substituir o sub na nova versão)

### Públicos

* agentepublico (se tem papel institucional cadastrado no AC)
  * agentePublico (true ou false)
* email
  * email (email principal cadastrado)
* permissoes (disponível caso o sistema esteja configurado como corporativo e habilitado para controlar [autorização de usuários](/AutorizacaoUsuarios/AutorizacaoUsuarios))
  * permissao 

``` important:: O scope de permissões será deprecado na nova versão. Toda as informações de autorização devem ser buscadas via API do Acesso Cidadão.
```

### Necessário solicitação
* nome
  * nome
  * nomeValidado (true ou false)
* cpf
  * cpf
* dataNascimento
  * dataNascimento
  * dataNascimentoValidada (true ou false)
* filiacao
  * nomePaiValidado
  * nomePai
  * nomeMaeValidado
  * nomeMae
* roles
  * role (apenas os perfis do usuários no sistema solicitante)
* offline_access
  * reservado pelo protocolo

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
