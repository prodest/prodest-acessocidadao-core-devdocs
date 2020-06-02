# Introdução 

A ideia inicial do Acesso Cidadão (AC) era criar um sistema que centralizasse as informações do cidadão e do servidor público em uma base de dados única, facilitando assim a validação da consistência dos dados e provendo autenticação e autorização de uma forma simples e segura. Com isso promovendo uma experiência de acesso e login único aos diversos serviços prestados pelo governo, com foco em iniciativas de governo digital.

Ao fornecer uma plataforma abrangente para comprovação de informações e documentos pessoais, o sistema Acesso Cidadão disponibiliza a qualquer cidadão um local único e centralizado com as informações necessárias para o consumo dos serviços online prestados pelo Estado. A medida que mais informações são agregadas no perfil de cada pessoa o grau de confiabilidade da conta do cidadão aumenta, o que permite que sistemas e serviços do governo que requerem maior nível de segurança sejam acessados.

Um dos principais focos do sistema é promover o auto-atendimento sempre que possível - registro de conta, simplificação na administração do perfil pessoal e recuperação de acesso pelo próprio usuário. Juntando a isso um sistema aprimorado de auditoria, conseguimos um custo menor para o gerenciamento das identidades digitais. Outro objetivo importante da ferramenta é garantir, através de tecnologias modernas de autenticação e autorização, maneiras simples e confiáveis de acessar informações públicas independente da mídia. 

Resumindo, o Acesso Cidadão implementa uma solução abrangente que proporciona autenticação e autorização de usuários interligados a um processo de login único. O sistema está sendo utilizado para autorizar o acesso a uma grande gama de serviços online oferecidos pelo Estado do Espiríto Santo: sites, aplicações web e mobile e até mesmo web services e “APIs” de governo digital.

## O que é o Acesso Cidadão

Atualmente a solução chamada de Acesso Cidadão(AC) pode ser dividida em várias partes sendo a principal um servidor IdentityServer [https://identityserver.io/](https://identityserver.io/) integrado às bases do estado, fornecendo, portanto, OpenID Connect e Oauth2. Toda integração é feita usando esses protocolos e padrões amplamente utilizados em todo mundo o que permite a disponibilização das seguintes funcionalidades:

**Autenticação como serviço** 
Lógica de login centralizada e workflow para todo tipo de aplicação (web, nativo, mobile e serviços). Isso inclui os serviços de Single sign-on (e sign-out). Leia mais [aqui](./AutenticacaoUsuarios/AutenticacaoUsuarios.md);

    O Single sign-out está sendo reformulado e é esperado que esteja funcionando normalmente na nova versão

**Controle de acesso para APIs** 
Geração de token de acesso para APIs em uma variedade de tipos de cliente. Leia mais [aqui](./AutenticacaoSistemas/AutenticacaoSistemas.md);

**Gateway de Federação**
Suporta federação de identidade a partir uma lista de provedores. Isso permite que sua aplicação seja integrada com outros provedores sem precisar se preocupar com os detalhes de integração de cada serviço.

## Outros serviços

**Api do Acesso Cidadão**
Api do AC que fornce informações sobre os objetos gerenciados pelo próprio AC: pessoas, agentes públicos, papeis institucionais, sistemas e suas permissões, grupos, etc... Leia mais [aqui](./Api/ApiAcessoCidadao.md);

**Autorização como serviço**
Lógica de autorização centralizada e sistema para conceder e remover permissões. Leia mais [aqui](./Autorizacao/AutorizacaoUsuarios.md);

