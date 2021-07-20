# Carga de sistemas externos

## Últimas Cargas de dados

   <iframe width="300" height="150" src="https://dashboard.acessocidadao.es.gov.br/historicocarga" frameborder="0"></iframe>

## Organograma

Os dados de organizações e suas unidades são controlados pelo Organograma, um sistema corporativo para manutenção de informações organizacionais. Seus dados são importados diariamente pelo acesso cidadão. O Acesso Cidadão mantém o mínimo possível dessas informações e sempre consulta o Organograma quando necessário. Qualquer informação de organização deve ser atualizada diretamente no Organograma.

[site](https://organograma.es.gov.br/)
  
## Siarhes

O Sistema Integrado de Administração de Recursos Humanos do Espírito Santo, Siarhes, é uma ferramenta de administração utilizada para gestão do pessoal ativo no âmbito do Poder Executivo - Administração Direta, Autarquias e Fundações - e para o pagamento de Benefícios Previdenciário dos poderes Executivo, Legislativo e Judiciário.

### Gestores

A rotina de carga de gestores é responsável por determinar de forma automática os gestores de cada uma das unidades organizacionais importadas do sistema Organograma. Para isso são usadas informações tanto do Organograma quanto do Siarhes.

Em resumo: O Siarhes permite que um determinando Cargo ou Função seja configurado como gestor de uma unidade. Logo, a pessoa que estiver em uma unidade e possuir o cargo que foi marcado como gestor daquela unidade é definido com gestor.

Como a rotina é feita de forma automática existe um comportamente padrão para o caso de um gestor não ser detectado que aponta o gestor da unidade organizacional imediamente acima como gestor da unidade orfã.

Abaixo vamos detalhar todos as interações possíveis de acontecer durante a carga de dados e como uma unidade pode ficar sem gestor:

###### Normal

Nesse caso existe um cargo ou função configurada no Siarhes e existe apenas uma pessoa nessa unidade com esse cargo. Ela é marcada como gestora.

###### SemGestorSiarhes

Nesse caso nenhum cargo ou função foi configurado como gestor no Siarhes.

###### SemGestorPapel

Nesse caso a unidade não possui ninguem lotado naquela área com o cargo ou função que foi configurado como gestor.

###### OcupacaoNaoEncontrada

Esse caso ocorre quando não foi encontrado nenhuma pessoa no estado com o cargo que foi configurado como gestor. Deve ser analisado com cuidado pois pode indicar que a configuração foi feita de forma errada no Siarhes.

###### SemOrgaoSiarhes
###### OrgaoNaoEncontrado

Esses dois casos acontecem quando uma unidade ou organização foi apagada do Siarhes e esse fato ainda não foi propagado para todos os sistemas. Normalmente desaparece nas próximas cargas.

##### Desempate

Em alguns casos um cargo ou função é definido como gestor no Siarhes só que mais de uma pessoa possui essas ocupações naquele setor. Nesse caso precisamos desempatar e definir quem realmente é o gestor.

###### DesempatePrioridade

O desempate por prioridade acontece quando uma das pessoas possui uma prioridade superior na sua ocupação do que a outra. Um exemplo comum de quando isso acontece é nas substituições. Quando uma pessoa sai de férias ela não deixa uma unidade, mas quem a está substituindo possui no momento da substituição a mesma ocupação e também está na mesma área, o que implica num caso que precisa de desempate. Nesse caso exemplo a ocupação em substituição tem sempre uma prioridade superior a ocupação original e por esse motivo a pessoa que está substituindo a pessoa de férias vai ser determinada como gestora.

###### ErroDesempate

Um erro de desempate acontece quando várias pessoas possuem o mesmo cargo na unidade, esse cargo está configurado como gestor e não existe diferença de prioridade entre essas ocupações. Normalmente é um erro de configuração no Siarhes.

#### Gestor Acima

Como explicado na introdução caso um gestor não possa ser detectado o gestor da unidade organizacional imediamente acima é apontado como gestor da unidade orfã.

###### GestorAcimaNormal

Nesse caso uma unidade acima possui gestor e esse é apontado para a unidade filha. Detalhe que esse comportamento é recursivo e caso vários gestores não estejam definidos o gestor acima é indicado para todos as unidades filhas. Isso pode fazer com que no caso de não existirem configurações no Siarhes um presidente ou secretário de orgão ser definido como gestor de todas as unidades da organização.

##### Organização

O Siarhes só permite a configuração de gestores para unidades o que faz com que precisemos de um regra diferente definir os gestores das organizações. Toda organização no Siarhes possui uma estrutura bem definida de modo que para determinar o gestor de uma organização pegamos o gestor da unidade que representa a organização e que está na raiz da estrutura de unidade.

###### OrganizacaoNormal

Nesse caso conseguimos determinar o gestor de uma organização de forma normal, conforme explicado acima.

###### OrganizacaoSemSetorGestor

Não foi possivel identificar a unidade que representa a organização e assim não conseguimos determinar o gestor da organização.

###### OrganizacaoSetorResponsavelSemGestor

Identificamos a unidade responsável por uma organização, só que essa unidade não tem o gestor definido.

###### GestorOrganizacaoVazio

Esse caso acontece quando não conseguimos determinar o gestor de uma unidade e mesmo subindo de forma recursiva até a raiz da organização essa também está sem gestor. Na prática esse é o único caso em que não conseguimos determinar nenhuma gestor para uma unidade.

###### PapelApagado

Essa situação acontece quando um papel e definido como gestor de uma unidade e posteriormente precisa ser apagado. Essa é um situação temporária que é corrigida na próxima carga de gestores.


### Resultado da última carga

Toda vez que a carga é executada um relatório com os dados da carga é gerado e salvo no arquivo do link abaixo. O arquivo disponibilizado abaixo sempre vai ter a última versão do que aconteceu na carga de gestores.

[link do resultado da ultima carga de gestores](http://acessocidadao.static.es.gov.br/reportcarga/gestores.csv)

