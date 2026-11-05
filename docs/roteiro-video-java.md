# Roteiro do vídeo: Java Advanced, Sprint 4 (entrega final)

Duração-alvo: **14 minutos** (limite do enunciado: 15). As falas estão em itálico; leia com suas palavras, não decorado. Tudo entre `[COLCHETES]` você precisa preencher ou confirmar antes de gravar.

Critérios que o vídeo precisa cobrir (100 pontos): demonstração técnica com app online (40), narrativa e decisões (20), integração multidisciplinar com evidências (20), apresentação oral (10), organização (10).

> Atenção: o enunciado pede que **todos os integrantes participem do vídeo** (10 pontos). Como só você vai gravar, deixe isso claro na abertura (apresente a equipe e cite quem fez o quê) e, se der tempo, peça a cada colega um trecho curto de 30 a 60 segundos nos blocos 2, 3 ou 5, editando depois.

---

## Antes de gravar (checklist)

- [ ] Aplicação **online** aberta numa aba (abra com antecedência: o Web App pode levar 1 a 2 minutos para "acordar"): `https://app-vitalpet-rm562259.azurewebsites.net/web/login`. Não grave em localhost.
- [ ] Banco com a massa de dados original (reinicie o container se já foi alterado).
- [ ] Abas prontas: as 5 abas do GitHub listadas no bloco 3, a aplicação logada como ADMIN e o Swagger (`/swagger-ui/index.html`). O repositório precisa estar **público** (confira numa janela anônima).
- [ ] Segunda janela anônima para logar como VETERINARIO sem deslogar o ADMIN.
- [ ] Resolução mínima 1080p, microfone testado, notificações desligadas, zoom do navegador em 110%.
- [ ] Credenciais: ADMIN `admin@vitalpet.com.br` / VETERINARIO `ana.souza@vitalpet.com.br`, senha `VitalPet@123`.

---

## 1. Abertura (0:00 – 0:45)

**Tela:** logo/tela de login da aplicação online.

*"Olá, pessoal. Este é o vídeo de entrega final de Java Advanced da nossa equipe, do Challenge FIAP com a Clyvo Vet. Eu sou o Eduardo Martins, RM562259, e o projeto foi desenvolvido por mim, João Victor Alcantara, Phillipo Barbosa e Leonardo Aragaki Rodrigues. [CITE QUEM FEZ O QUÊ: ex. o João desenvolveu o app mobile, o Phillipo ficou com o banco de dados, etc.]*

*"Neste vídeo eu vou mostrar o problema que escolhemos resolver, as decisões técnicas por trás da solução, a aplicação funcionando online com os principais fluxos e, no final, como as outras disciplinas do semestre se conectam a este projeto."*

---

## 2. Narrativa da solução (0:45 – 2:30)

**Tela:** README do GitHub, seção "Narrativa da solução".

*"O problema que a Clyvo Vet nos trouxe é que a clínica veterinária cuida bem do pet **durante** a consulta, mas perde o fio depois dela. O tutor sai do consultório, esquece o retorno, esquece o tratamento, e a clínica não tem como acompanhar. Isso gera perda de continuidade do cuidado e perda de receita com retornos que não acontecem."*

*"A nossa solução se chama VitalPet e acompanha o pet **depois** da consulta. A ideia central é uma regra de negócio simples e forte: quando o veterinário finaliza um atendimento, o sistema cria automaticamente um acompanhamento clínico e um alerta de retorno para sete dias depois. E quando esse alerta é resolvido, o acompanhamento é concluído junto. Ou seja, o ciclo do cuidado fecha sozinho, sem depender de memória ou de planilha."*

*"O que a gente considera original aqui é justamente isso: o foco não é só cadastrar consulta, como a maioria dos sistemas de clínica faz, e sim garantir a continuidade. Toda a operação, clínicas, veterinários, tutores, pets, consultas, acompanhamentos e alertas, fica numa aplicação web com login e permissões por perfil."*

---

## 3. Decisões de design e escolhas tecnológicas (2:30 – 5:00)

**Tudo neste bloco é mostrado no GitHub**, no navegador. Deixe estas abas abertas antes de gravar, nesta ordem (repositório: `https://github.com/Tidlle/Sprint4-Java`):

| Aba | Endereço | Usada em |
|---|---|---|
| 1 | README, ancorado na tabela **Decisões de design e tecnologia** | 3.1 e 3.6 |
| 2 | `.../tree/main/src/main/java/com/clyvo/vitalpet` | 3.2 |
| 3 | `.../tree/main/src/main/resources/db` | 3.3 |
| 4 | `.../blob/main/src/main/java/com/clyvo/vitalpet/config/SecurityConfig.java` | 3.4 |
| 5 | `.../tree/main/src/main/resources` (pastas `templates` e `static`) | 3.5 |

Dica: aumente o zoom do navegador (Ctrl +) nos arquivos de código para ficarem legíveis no vídeo.

### 3.1 Visão geral e stack (aba 1)

**Tela:** README, role até a tabela **Stack** e depois até **Decisões de design e tecnologia**.

*"Vou passar pelas principais decisões técnicas, todas documentadas aqui no README do repositório. Usamos **Java 17 com Spring Boot 3.3**, porque é o framework que a disciplina trabalha, tem um ecossistema maduro para web, segurança e acesso a dados, e facilita o deploy em nuvem."*

### 3.2 Camadas (aba 2)

**Tela:** pasta `src/main/java/com/clyvo/vitalpet`. Passe o mouse sobre `controller`, `web`, `service`, `repository`, `model`, `dto`, `mapper`.

*"Organizamos o código em camadas. A pasta `controller` expõe a API REST, a pasta `web` tem os controllers das telas, o `service` concentra as regras de negócio, o `repository` acessa o banco com Spring Data JPA, e usamos `dto` e `mapper` para nunca expor a entidade direto. Separar o controller web do controller REST nos permitiu manter a API aberta para o app mobile e, ao mesmo tempo, proteger as telas com login."*

### 3.3 Flyway e bancos (aba 3)

**Tela:** pasta `src/main/resources/db`. Entre em `migration` e depois em `migration-sqlserver`; abra o `V1__create_schema.sql`.

*"Para o banco usamos **Flyway**. O schema e a massa de dados são versionados em dois arquivos: `V1__create_schema` e `V2__seed_data`. Isso garante que qualquer ambiente sobe com o mesmo banco, sem script manual. Temos duas versões: a pasta `migration`, para o H2 em memória, usado em desenvolvimento e testes, e a `migration-sqlserver`, em T-SQL, para o Azure SQL na nuvem."*

### 3.4 Spring Security (aba 4)

**Tela:** arquivo `SecurityConfig.java`. Aponte com o mouse as regras de rota por perfil (`hasRole`).

*"Para segurança usamos **Spring Security** com dois perfis: ADMIN e VETERINARIO. As rotas `/web` exigem login, e aqui estão as regras: o ADMIN acessa tudo, enquanto o veterinário não acessa Clínicas, Veterinários e Tutores. Além disso, o veterinário só enxerga as próprias consultas. Esse vínculo vem da tabela de usuários, que liga o usuário ao registro de veterinário."*

### 3.5 Frontend (aba 5)

**Tela:** pastas `templates` e `static` (abra `static/img` para mostrar a logo e os ícones). Se preferir, mostre os prints no README.

*"Para o frontend usamos **Thymeleaf**, renderizado no servidor. As telas ficam nesta pasta `templates`, e aqui em `static` estão o CSS, a logo e os ícones. Fizemos um tema escuro com os mesmos ícones e a mesma logo do nosso aplicativo mobile, para manter a identidade visual entre as plataformas. Como a interface é renderizada no servidor, o próprio Thymeleaf esconde itens do menu conforme o perfil do usuário."*

### 3.6 Fechamento do bloco (volte à aba 1)

**Tela:** README, tabela **Decisões de design e tecnologia**.

*"Por fim, mantivemos a API REST documentada com **Swagger** e testes automatizados cobrindo o contexto, a segurança e a regra do alerta. Todas essas decisões e as justificativas de cada uma estão resumidas nesta tabela do README."*

---

## 4. Demonstração técnica (5:00 – 12:00)

### 4.1 Login e dashboard (5:00 – 6:00)

**Tela:** aplicação online, login como ADMIN.

*"Vamos para a aplicação, que está rodando online no Azure, neste endereço, `app-vitalpet-rm562259.azurewebsites.net`. Não é localhost. Entro como ADMIN."*

**Ação:** logar. Passar o mouse sobre os cards do dashboard.

*"Aqui está o dashboard operacional: consultas agendadas em destaque, com as concluídas, os alertas pendentes e os de alta prioridade, o faturamento das consultas concluídas e os indicadores gerais de clínicas, veterinários, tutores e pets ativos. Todos esses números vêm do banco em tempo real."*

### 4.2 CRUD e validações (6:00 – 7:30)

**Ação:** menu Pets → "Novo pet". Clicar em Salvar com tudo vazio.

*"Cada cadastro tem validação. Se eu tentar salvar um pet vazio, o sistema aponta os campos obrigatórios, tanto no formulário quanto na API."*

**Ação:** preencher: Nome "Bob", Espécie "Cachorro", Raça "Beagle", data de nascimento, Sexo "Macho", Peso "12", Tutor qualquer. Salvar.

*"Preenchendo corretamente, o pet é criado e aparece na lista. Também dá para editar e desativar. O mesmo padrão vale para Clínicas, Veterinários e Tutores."*

**Ação:** editar o Bob mudando o peso para 13 e salvar; depois desativar.

### 4.3 Fluxo completo 1: finalizar consulta (7:30 – 9:30)

**Ação:** menu Consultas, filtro Status = Agendada. Clicar em **Finalizar** na consulta da Luna.

*"Agora vou mostrar o primeiro fluxo que não é CRUD, que é o coração do projeto. Aqui estão as consultas agendadas. Vou finalizar a da Luna."*

**Ação:** preencher: Diagnóstico "Infecção leve de ouvido", Tratamento "Gotas otológicas por 7 dias", Acompanhamento "Observar coceira e secreção nos próximos dias". Clicar em **Finalizar consulta**.

*"Ao informar diagnóstico, tratamento e a descrição do acompanhamento e finalizar, três coisas acontecem numa única transação: a consulta vira CONCLUÍDA, é criado um acompanhamento vinculado a ela, e é criado um alerta de retorno agendado para daqui a sete dias."*

**Ação:** voltar à lista (consulta agora CONCLUÍDA) e abrir menu Alertas.

*"A consulta aparece como concluída, e aqui em Alertas está o alerta de retorno da Luna, com status pendente. Ninguém precisou cadastrar isso à mão."*

### 4.4 Fluxo completo 2: resolver alerta (9:30 – 10:30)

**Ação:** em Alertas, localizar o alerta da Luna e clicar em **Resolver**.

*"O segundo fluxo é o inverso. Quando o tutor volta e o alerta é resolvido, o sistema conclui automaticamente o acompanhamento vinculado."*

**Ação:** abrir o Swagger, `GET /api/acompanhamentos` com status CONCLUIDO, Execute.

*"Para comprovar, consulto a API de acompanhamentos aqui no Swagger: o acompanhamento da Luna está com status CONCLUÍDO e com data de fim preenchida. Fechamos o ciclo do cuidado."*

### 4.5 Perfis e proteção de rotas (10:30 – 12:00)

**Ação:** janela anônima, login como `ana.souza@vitalpet.com.br`.

*"Agora o controle de acesso. Na janela anônima entro como uma veterinária, a Ana Souza. Repare que o menu dela não mostra Clínicas, Veterinários nem Tutores."*

**Ação:** abrir Consultas.

*"Em Consultas ela vê somente os atendimentos dela. Se eu tentar abrir a consulta de outro veterinário digitando o link direto, o sistema bloqueia."*

**Ação:** digitar `/web/clinicas` na barra de endereço, mostrar a resposta 403. Depois tentar abrir `/web/consultas/{id da consulta de outro veterinário}/finalizar` e mostrar o bloqueio.

*"E se ela tentar acessar a tela de Clínicas pela URL, recebe acesso negado. A proteção está no servidor, não só no menu."*

---

## 5. Integração multidisciplinar (12:00 – 13:45)

**Tela:** README, tabela **Integração multidisciplinar**. Para cada disciplina, abra a evidência indicada.

*"Este projeto também conversa com as outras disciplinas do semestre. Vou mostrar uma evidência de cada."*

- **Mobile** *(confirmado)*
  **Mostrar:** repositório do app mobile, arquivo `src/services/petService.ts`.
  *"O aplicativo mobile consome a mesma API REST deste projeto, e a logo, os ícones e as cores do app foram reaproveitados aqui."*

- **DevOps** *(confirmado)*
  **Mostrar:** `azure-pipelines-ci.yml`, `azure-pipelines-cd.yml` e `docs/arquitetura-DevOps.png`.
  *"A cada push, as pipelines do Azure DevOps fazem o build, rodam os testes e publicam a aplicação no Azure, com o banco no Azure SQL. Foi assim que ela chegou online."*

- **Banco de dados** *(confirmado; [cite procedures ou MongoDB só se estiverem ligados a este banco])*
  **Mostrar:** pastas `db/migration` e `db/migration-sqlserver`.
  *"O modelo relacional tem 8 tabelas, versionadas em scripts SQL: uma versão para H2 e outra para o Azure SQL."*

- **Compliance e testes** *[CONFIRME]*
  **Mostrar:** Azure Boards e a pasta `src/test`.
  *"Planejamos o projeto no Azure Boards e temos testes automatizados, que rodam a cada push."*

- **IA** *[CONFIRME; se não houver, apague este item]*
  *"[Diga em uma frase onde a IA aparece na aplicação.]"*

*"Todas essas evidências estão na tabela do README."*

---

## 6. Encerramento (13:45 – 14:30)

**Tela:** README no GitHub, topo da página.

*"Resumindo: entregamos uma aplicação web em Java e Spring Boot, online, com frontend, banco versionado com Flyway, segurança por perfil, dois fluxos completos que fecham o ciclo de cuidado do pet, e integração com mobile, nuvem e banco de dados. O código está no GitHub público, com o README completo, e o link da aplicação está no repositório. Obrigado pela atenção."*

---

## Dicas rápidas

- Se algo falhar ao vivo (erro, lentidão do deploy), não regrave tudo: corte e refaça só o trecho.
- Fale devagar nos blocos 3 e 5; são os que valem pontos de narrativa e integração.
- Não leia o README; mostre a tela e explique com suas palavras.
- Termine o vídeo em até 15 minutos: se passar, encurte o bloco 4.2 (CRUD) primeiro.
