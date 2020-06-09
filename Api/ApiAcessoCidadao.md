# API do Acesso Cidadão

[Documentação](https://sistemas.es.gov.br/prodest/acessocidadao.webapi/swagger/)

## Como requisitar scopes

Os scopes devem ser solicitados via Solicitação de Atendimento que deve ser enviada para o email atendimento@prodest.es.gov.br por pessoa cadastrada como responsavél por abertura de chamados do orgão solicitante.

``` important:: Na V1 da API os scopes acessocidadao-base e AcessoCidadao são obrigatórios para todos os endpoints e devem ser requisitados juntos com qualquer outro scope específico
```

### Modelo de solicitação

**Titulo da solicitação:** Inclusão de scopes da API do AC em sistema do Acesso Cidadão

**Informações no corpo da solicitação:**

- **Nome do sistema**;
- **Sigla do sistema**;
- **Scopes solicitados**;
- **Justificativa detalhada** de como cada um dos endpoints solicitados serão utilizadas e porque são necessários para as funcionalidades sendo construidas;

(Caso o responsável pelo sistema não tenha permissão para abrir chamados na central de atendimento)

- **Nome do responsável pelo sistema**
- **Email do responsável pelo sistema**