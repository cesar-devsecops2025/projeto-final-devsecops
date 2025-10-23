# Política de Segurança no Desenvolvimento de Software da SecureTasks

## 1. Nosso Princípio: Segurança é Tarefa de Todos

Olá, time! Este documento não é para ser um livro de regras chatas, mas sim um guia para nos ajudar a construir produtos melhores e mais seguros. A partir de hoje, a segurança não é mais uma etapa final ou responsabilidade de uma única pessoa. Ela é parte do nosso trabalho diário, assim como escrever testes ou documentar uma API.

Nossa metodologia principal é o **"Shift Left"**: vamos encontrar e esmagar os bugs de segurança o mais cedo possível. Para isso, vamos contar com a ajuda de ferramentas automatizadas direto na nossa pipeline de CI/CD.

## 2. Nossas Ferramentas, Nossos Guardiões

Implementamos uma esteira de verificação automática que roda a cada `push` no repositório. Ela é nossa primeira linha de defesa e é composta por três tipos de análise:

#### a) Análise de Código Estático (SAST) com SonarCloud

* **O que faz?** O SonarCloud lê o nosso código-fonte procurando por "maus hábitos" que podem levar a vulnerabilidades, como SQL Injection, senhas no código, lógica complexa, etc.
* **Nossa regra:** Todo Pull Request (PR) deve ser aprovado pela análise do SonarCloud. Não faremos merge de código que introduza novas falhas críticas ou bugs. O objetivo é manter nosso código sempre "limpo".

#### b) Análise de Dependências (SCA) com Anchore/Syft

* **O que faz?** Nossos projetos usam dezenas de bibliotecas de terceiros (open source). Essa ferramenta funciona como um "verificador de antecedentes" para essas bibliotecas, nos avisando se alguma delas tem uma vulnerabilidade de segurança conhecida.
* **Nossa regra:** A pipeline irá falhar se uma dependência com vulnerabilidade de nível `CRITICAL` for detectada na imagem da nossa aplicação. Essas falhas devem ser resolvidas antes do deploy, seja atualizando a biblioteca ou buscando uma alternativa.

#### c) Análise de Imagens de Container com Trivy

* **O que faz?** O Trivy funciona como um "raio-x" nas nossas imagens Docker. Ele verifica o sistema operacional e os pacotes instalados dentro do container, procurando por falhas de segurança conhecidas (CVEs).
* **Nossa regra:** Nenhuma imagem com vulnerabilidades `CRITICAL` conhecidas e que possuam correção disponível será enviada para o ambiente de produção.

## 3. Lidando com uma Vulnerabilidade

Encontrou um alerta em uma das ferramentas? Sem pânico.

1.  **Analise:** A primeira etapa é entender o alerta. A ferramenta te dará detalhes sobre a falha e, na maioria das vezes, sugestões de como corrigi-la.
2.  **Corrija:** Aplique a correção. Pode ser algo simples como atualizar a versão de uma biblioteca ou refatorar um trecho de código.
3.  **Valide:** Suba a correção. A pipeline irá rodar novamente e, se tudo estiver certo, o alerta desaparecerá.

Se tiver dúvidas, abra uma discussão com o time. Aprender juntos faz parte do processo. Vamos construir coisas incríveis, e vamos fazer isso do jeito certo.