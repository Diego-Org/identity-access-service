# identity-access-service

Serviço responsável pelo domínio de *Identidade e Acesso* do Marketplace para Serviços de Beleza.

Centraliza o cadastro de contas, a autenticação, a recuperação de senha e o controle de permissões dos usuários da plataforma.

## Objetivo

Garantir que clientes, profissionais e administradores acessem as funcionalidades da plataforma de acordo com sua identidade, suas permissões e a situação da conta.

## Responsabilidades

- Cadastrar contas de clientes e profissionais.
- Gerenciar a identidade e as credenciais dos usuários.
- Autenticar usuários cadastrados.
- Verificar a situação da conta durante a autenticação.
- Permitir a recuperação segura de senha.
- Controlar papéis e permissões de acesso.
- Impedir acessos não autorizados.
- Aplicar restrições de acesso para contas suspensas ou bloqueadas.
- Integrar provedores externos de autenticação, quando habilitados.

## Escopo funcional

| Caso de uso | Funcionalidade | Escopo |
| --- | --- | --- |
| UC-AUT-01 | Cadastrar conta | MVP |
| UC-AUT-02 | Autenticar usuário | MVP |
| UC-AUT-03 | Recuperar senha | MVP |
| UC-AUT-04 | Autenticar com provedor externo | Opcional |
| UC-AUT-05 | Controlar permissões de acesso | MVP |

## Atores

| Ator | Descrição |
| --- | --- |
| Visitante | Pessoa sem autenticação que pode iniciar o cadastro. |
| Cliente | Usuário que pesquisa e contrata serviços. |
| Profissional | Usuário que oferece serviços e gerencia atendimentos. |
| Administrador | Usuário autorizado a executar operações administrativas. |
| Provedor de autenticação | Serviço externo utilizado na autenticação social, quando disponível. |

## Regras de negócio

### Cadastro

- Cada conta deve possuir um identificador único.
- Não deve existir mais de uma conta associada ao mesmo identificador.
- Os dados obrigatórios devem ser validados conforme o tipo de conta.
- O cadastro público deve permitir apenas os papéis autorizados pela plataforma.
- O acesso administrativo deve ser concedido por um processo restrito.
- A criação da conta profissional não implica a habilitação automática do perfil para contratação.

### Autenticação

- O acesso deve ser concedido somente após a validação das credenciais.
- A situação da conta deve ser verificada antes de permitir o acesso.
- Contas suspensas ou bloqueadas devem ter a autenticação impedida conforme as regras da plataforma.
- As respostas de autenticação não devem expor informações sensíveis.

### Recuperação de senha

- A recuperação deve utilizar um mecanismo seguro de comprovação de autorização sobre a conta.
- Comprovações inválidas ou expiradas devem impedir a alteração da senha.
- A nova senha deve atender à política de segurança definida.
- As respostas não devem revelar indevidamente a existência de uma conta.

### Autorização

- Operações protegidas devem exigir autenticação.
- O usuário deve possuir permissão para executar a operação solicitada.
- O acesso deve considerar o papel do usuário e as restrições do recurso.
- Recursos de outros usuários não devem ser acessados ou alterados sem autorização.
- As validações de acesso devem ocorrer no backend.

## Limites do domínio

Este serviço mantém os dados necessários para identificar e autenticar usuários e controlar seu acesso.

| Responsabilidade | Domínio responsável |
| --- | --- |
| Contas, credenciais e permissões | Identidade e Acesso |
| Perfil pessoal e apresentação profissional | Perfis e Profissionais |
| Categorias e serviços oferecidos | Catálogo de Serviços |
| Horários e bloqueios de atendimento | Agenda e Disponibilidade |
| Reservas e acompanhamento de atendimentos | Agendamentos |
| Cobranças e recebimentos | Pagamentos |
| Moderação e supervisão da plataforma | Administração e Moderação |

Os demais domínios devem utilizar o identificador da conta como referência ao usuário e validar as permissões sobre os recursos que controlam.

## Fluxos principais

### Cadastro de conta

1. O visitante seleciona o tipo de conta.
2. Informa os dados obrigatórios.
3. O sistema valida os dados.
4. Verifica a unicidade do identificador.
5. Cria a conta.
6. Informa o resultado e as próximas etapas de acesso ou ativação.

### Autenticação

1. O usuário informa suas credenciais.
2. O sistema valida as credenciais.
3. Verifica a situação da conta.
4. Estabelece a sessão autenticada.
5. Permite o acesso às funcionalidades autorizadas.

### Recuperação de senha

1. O usuário solicita a recuperação da conta.
2. O sistema inicia o procedimento seguro.
3. O usuário comprova sua autorização sobre a conta.
4. Define uma nova senha.
5. O sistema valida e atualiza a credencial.
6. Confirma a alteração.

## Segurança

- Armazenar senhas utilizando um mecanismo seguro de hash.
- Não registrar senhas ou outras credenciais sensíveis em logs.
- Não retornar credenciais nas respostas da API.
- Validar os dados recebidos no backend.
- Proteger operações administrativas.
- Verificar permissões e propriedade dos recursos.
- Proteger o fluxo de recuperação de senha contra uso indevido.
- Definir o efeito da suspensão de contas sobre sessões existentes.

## Arquitetura

A especificação de casos de uso estabelece um *monólito modular orientado a domínios* como arquitetura de referência.

O componente identity-access-service representa o domínio de Identidade e Acesso. Sua implantação como microserviço independente depende de uma decisão arquitetural da equipe.

A organização interna deve preservar a separação entre regras de domínio, casos de uso e integrações técnicas.

## Documentação técnica

As seguintes informações deverão ser documentadas conforme a implementação:

- Tecnologias e versões utilizadas.
- Pré-requisitos de execução.
- Variáveis de ambiente.
- Configuração do banco de dados.
- Comandos de inicialização e testes.
- Endpoints e contratos da API.
- Mecanismo de autenticação e gerenciamento de sessões.
- Formato das respostas de erro.
- Configuração de provedores externos, quando aplicável.

## Critérios de aceite

- [ ] Cadastrar clientes e profissionais com dados válidos.
- [ ] Rejeitar dados inválidos e identificadores duplicados.
- [ ] Autenticar contas aptas com credenciais válidas.
- [ ] Rejeitar credenciais inválidas.
- [ ] Impedir o acesso de contas suspensas ou bloqueadas.
- [ ] Recuperar senha mediante comprovação válida.
- [ ] Rejeitar comprovações inválidas ou expiradas.
- [ ] Impedir operações sem autenticação ou permissão.
- [ ] Impedir acesso indevido a recursos de outros usuários.
- [ ] Impedir a concessão de acesso administrativo pelo cadastro público.
- [ ] Evitar a exposição de credenciais em respostas e logs.

## Decisões pendentes

- Identificador único utilizado pela conta.
- Possibilidade de uma conta possuir mais de um papel.
- Processo de ativação da conta.
- Política de senha.
- Mecanismo e duração das sessões.
- Canal e prazo de validade da recuperação de senha.
- Processo de concessão de privilégios administrativos.
- Provedores de autenticação social.
- Implantação como módulo ou microserviço independente.

## Referências

- Documento de Requisitos de Software — Marketplace para Serviços de Beleza.
- Especificação de Casos de Uso — Marketplace para Serviços de Beleza, versão 1.0.

> Este README descreve o escopo previsto para o domínio. Os critérios de aceite não representam funcionalidades já implementadas ou testes já executados.