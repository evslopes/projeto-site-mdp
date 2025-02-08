# Mãe da Prole - Site Dinâmico e Gerenciável

Este projeto visa criar um site dinâmico, gerenciável e com custo otimizado para a "Mãe da Prole", doula e educadora perinatal.  O site permitirá a divulgação de seus cursos, artigos de blog, informações de contato e integração com suas plataformas existentes (Instagram e Hotmart).

## 1. Arquitetura e Tecnologias

O projeto foi desenvolvido com uma arquitetura moderna, separando o front-end (interface do usuário), o back-end (API e lógica de negócios) e o gerenciamento de conteúdo (CMS). Isso proporciona flexibilidade, desempenho e escalabilidade.

### Diagrama de Arquitetura e Infraestrutura

```+-----------------+     +-----------------+     +-----------------+    +---------------+
|    Navegador    |---->|    Frontend     |---->|     Backend     |--->|  PostgreSQL   |
|     (Usuário)   |     |     (Vue.js)    |     |  (Spring Boot)  |    |    (Heroku)   |
+-----------------+     +--------+--------+     +--------+--------+    +-------+-------+
        ^                      |  (Netlify)           | (Heroku)             |       |
        |                      |        ^                 |                  |       |
        |  Requisições        |        | API REST        | Webhooks         |       |
        |  e Respostas       |        v                 v                  |       |
+-----------------+     +--------+--------+     +-----------------+    |       |
|     Internet    |     |      Strapi     |     |     Hotmart     |----+       |
+-----------------+     |   (Headless CMS) |<----|   (Plataforma)  |            |
        ^                      |  (Heroku)       |                 |            |
        |                      +-----------------+     +-----------------+            |
        |                                                                             |
        | Integração com Instagram                                                    |
        +-----------------------------------------------------------------------------+
        |
        | (Local - Desenvolvimento)
        v
+---------------------------------------------+
|               Docker Compose                |
|  +----------+   +----------+   +----------+  |
|  | Frontend |   | Backend  |   | PostgreSQL |  |
|  +----------+   +----------+   +----------+  |
+---------------------------------------------+
```

<svg xmlns="http://www.w3.org/2000/svg" width="800" height="400" viewBox="0 0 800 500">
  <style>
    rect {
      fill: #f0f0f0; /* Cor de fundo dos retângulos */
      stroke: #000;   /* Cor da borda */
      stroke-width: 2;
    }
    text {
      font-family: sans-serif;
      font-size: 14px;
      text-anchor: middle; /* Centraliza o texto horizontalmente */
      dominant-baseline: middle; /* Centraliza o texto verticalmente (aproximado) */
    }
    .arrow {
        stroke: #000;
        stroke-width: 2;
        marker-end: url(#arrowhead);
    }
  </style>
    <defs>
        <marker id="arrowhead" markerWidth="10" markerHeight="7"
            refX="0" refY="3.5" orient="auto">
            <polygon points="0 0, 10 3.5, 0 7" />
        </marker>
    </defs>

  <rect x="20" y="150" width="120" height="60" rx="10" ry="10"/>
  <text x="80" y="180">Navegador</text>

  <rect x="180" y="50" width="160" height="80" rx="10" ry="10"/>
  <text x="260" y="90">Frontend (Vue.js)</text>
  <text x="260" y="110">Netlify</text>

  <rect x="400" y="50" width="160" height="80" rx="10" ry="10"/>
  <text x="480" y="90">Backend (Spring Boot)</text>
    <text x="480" y="110">Heroku</text>

  <rect x="400" y="250" width="160" height="80" rx="10" ry="10"/>
  <text x="480" y="290">Strapi (CMS)</text>
     <text x="480" y="310">Heroku</text>

  <rect x="620" y="150" width="160" height="80" rx="10" ry="10"/>
  <text x="700" y="190">PostgreSQL</text>
    <text x="700" y="210">(Heroku Postgres)</text>


  <rect x="400" y="400" width="160" height="60" rx="10" ry="10" />
    <text x="480" y="430">Hotmart</text>

  <line x1="140" y1="180" x2="180" y2="90" class="arrow"/>
    <line x1="340" y1="90" x2="400" y2="90" class="arrow"/>
    <line x1="560" y1="90" x2="620" y2="190" class="arrow"/>
    <line x1="340" y1="90" x2="400" y2="290" class="arrow" />
     <line x1="560" y1="290" x2="620" y2="190" class="arrow"/>
    <line x1="560" y1="430" x2="400" y2="90"  class="arrow" />

  </svg>

**Descrição do Diagrama:**

*   **Navegador (Usuário):** O ponto de partida.  O usuário acessa o site através de um navegador web (Chrome, Firefox, Safari, etc.).
*   **Internet:** A rede que conecta o usuário ao site.
*   **Frontend (Vue.js):**  A interface do usuário, construída com Vue.js.  É hospedada no *Netlify*.  O Netlify serve os arquivos estáticos (HTML, CSS, JavaScript) do front-end.
*   **Backend (Spring Boot):** A API REST, construída com Java e Spring Boot.  É hospedada no *Heroku*.  Responsável pela lógica de negócios que não é coberta pelo Strapi (ex: integração com webhooks da Hotmart).
*   **Strapi (Headless CMS):**  O sistema de gerenciamento de conteúdo.  Também hospedado no *Heroku*.  Fornece uma API para que o front-end (e o back-end, se necessário) possa buscar e gerenciar o conteúdo (artigos do blog, informações dos cursos, etc.).
*   **Banco de Dados (PostgreSQL):**  Banco de dados relacional.  Hospedado no *Heroku* como um serviço (Heroku Postgres).  Armazena os dados do Strapi e, potencialmente, dados adicionais do back-end Spring Boot.
*   **Hotmart:**  Plataforma externa de cursos.  O site se integra com a Hotmart para exibir informações dos cursos e (opcionalmente) receber notificações de vendas (webhooks).
* **Integração com Instagram:** O site irá puxar informações, como posts, para apresentar na página.
* **Infraestrutura Docker(Local):** Para facilitar o desenvolvimento, o backend e o banco de dados podem ser executados localmente, de forma conteinerizada.

**Comunicação:**

*   O navegador faz requisições HTTP(S) para o front-end (Netlify).
*   O front-end faz requisições à API REST do back-end (Spring Boot) e à API do Strapi para obter e exibir os dados.
*   O back-end (Spring Boot) se comunica com o banco de dados PostgreSQL para persistir dados (se necessário, além do que o Strapi já gerencia).
*   O Strapi também se comunica com o PostgreSQL para armazenar o conteúdo.
*   O back-end pode se comunicar com a Hotmart via API e receber notificações via Webhooks.
*   O Docker compose é utilizado para subir todos os serviços em desenvolvimento

**Hospedagem (Ambiente de Produção):**

*   **Front-end:** Netlify.
*   **Back-end:** Heroku.
*   **Strapi:** Heroku.
*   **PostgreSQL:** Heroku Postgres (add-on).

**Hospedagem (Ambiente de Desenvolvimento - Local):**

*   **Docker Compose:** Orquestra os containers do front-end (em modo de desenvolvimento, com hot-reload), back-end (Spring Boot), Strapi e PostgreSQL, facilitando a configuração e execução local.

### Back-end (Java/Spring Boot)

*   **Spring Boot:** Framework Java robusto para criação de APIs RESTful. Simplifica a configuração, desenvolvimento e deploy.
*   **Spring Data JPA:** Facilita a interação com o banco de dados PostgreSQL.
*   **Spring Security (Opcional):** Para autenticação e autorização mais complexas (ex: área de membros, *se necessário*). Implementado apenas se houver necessidade *real* de uma área de membros customizada, para manter a complexidade sob controle.
*   **API REST:** O back-end expõe uma API para consumo pelo front-end e, potencialmente, por outros aplicativos no futuro.

### Front-end (HTML5/CSS3 + Vue.js)

*   **Vue.js:** Framework JavaScript progressivo, fácil de aprender e ideal para interfaces dinâmicas e reativas.
*   **HTML5/CSS3:**  Base para estrutura e estilização do site, com design responsivo (adaptável a diferentes tamanhos de tela).
*   **Bibliotecas Auxiliares:**
    *   **Axios:** Para fazer requisições HTTP à API do back-end.
    *   **Vue Router:** Para gerenciar a navegação entre as páginas do site.
    *   **Vuetify (Opcional):**  UI framework com componentes pré-estilizados para acelerar o desenvolvimento (botões, menus, etc.).  Usado se houver tempo e necessidade.

### Banco de Dados

*   **PostgreSQL:** Banco de dados relacional de código aberto, robusto, confiável e com bom desempenho.

### Gerenciamento de Conteúdo (CMS)

*   **Strapi (Headless CMS):**  CMS de código aberto (Node.js) que fornece apenas a API para gerenciar o conteúdo (textos, imagens, vídeos, etc.).  O front-end consome essa API e renderiza o conteúdo.
    *   **Vantagens:**
        *   **Flexibilidade:** Liberdade total para criar o front-end.
        *   **Desempenho:** Mais rápido do que CMSs tradicionais.
        *   **Reutilização:** O conteúdo pode ser usado em outros canais (ex: aplicativos).

### Hospedagem

*   **Back-end e Banco de Dados:** Heroku (plano gratuito para começar).
*   **Front-end:** Netlify (plano gratuito).

### Integração com Hotmart

O site se integra com a Hotmart através de suas APIs e Webhooks para:

*   Exibir informações dos cursos (nome, descrição, preço, link de compra).
*   Receber notificações de vendas (opcional, para funcionalidades avançadas).

*Área de Membros:*  A criação de uma área de membros *customizada* é considerada *opcional e de baixa prioridade* no escopo inicial do projeto, devido à complexidade. A prioridade é usar a área de membros nativa da Hotmart (Hotmart Club).

## 2. Conteúdo do Site

O site incluirá as seguintes seções:

*   **Página Inicial:**
    *   Apresentação da "Mãe da Prole" (foto, biografia resumida).
    *   Chamada para ação principal (ex: conhecer os cursos).
    *   Depoimentos de clientes.
    *   Links para as principais seções do site (blog, cursos, sobre).
    *   Integração com o Instagram.

*   **Sobre:**
    *   História completa da "Mãe da Prole", jornada, valores, missão.
    *   Qualificações e experiência.
    *   Fotos.

*   **Cursos:**
    *   Lista de cursos (integrada com a Hotmart).
    *   Página individual para cada curso (descrição detalhada, público-alvo, benefícios, módulos, preço, link para compra na Hotmart).

*   **Blog:**
    *   Artigos sobre temas relevantes para o público (gestação, parto, amamentação, etc.).
    *   Categorias e tags para organização.
    *   Comentários (com moderação).
    *   Compartilhamento em redes sociais.

*   **Contato:**
    *   Formulário de contato.
    *   Informações de contato (e-mail, WhatsApp, redes sociais).
    *   FAQ (Perguntas Frequentes).

*   **Recursos (Opcional):**
    *   E-books, guias, checklists para download (como *lead magnets*).
    *   Vídeos.

*   **Página de Captura (Landing Page - Opcional):**
    *   Página focada em uma única ação (ex: inscrição na lista de e-mails).

## 3. Fluxo de Trabalho e Desenvolvimento

O projeto segue uma abordagem de desenvolvimento iterativo e incremental, utilizando o framework Scrum, com as seguintes fases:

1.  **Planejamento (Sprint Planning):** Definição do escopo, funcionalidades, design (wireframes/mockups) e conteúdo.  Priorização das tarefas no Backlog.
2.  **Configuração do Ambiente:**  Instalação das ferramentas (Java, Node.js, Docker, editores de código, etc.).  Configuração dos projetos (Spring Boot, Vue.js, Strapi). Configuração dos repositórios (Git/Github).
3.  **Desenvolvimento (Sprints):**
    *   **Back-end:** Criação das entidades JPA, repositórios, serviços e controllers (API REST).
    *   **Front-end:** Criação dos componentes Vue.js, configuração das rotas, estilização com CSS.
    *   **CMS (Strapi):** Configuração dos tipos de conteúdo.
    *   **Integração:** Conexão do front-end com as APIs do back-end e do Strapi.
4.  **Testes:** Testes unitários (JUnit, Mockito), testes de integração e testes de aceitação.
5.  **Revisão (Sprint Review):** Demonstração das funcionalidades implementadas para a "Mãe da Prole" e coleta de feedback.
6.  **Retrospectiva (Sprint Retrospective):**  Discussão da equipe sobre o que funcionou bem, o que pode ser melhorado e definição de ações para o próximo ciclo.
7.  **Deploy:** Implantação no Heroku (back-end/Strapi) e Netlify (front-end).
8.  **Manutenção e Evolução:** Monitoramento, correção de bugs, adição de novas funcionalidades e atualização de conteúdo.

**Ferramentas de Gerenciamento:**

*   **Kanban:** Trello (ou ferramenta similar, como Jira) para gerenciamento visual das tarefas e acompanhamento do progresso.

Este README fornece uma visão geral do projeto. Detalhes mais específicos sobre a implementação (código, configurações, etc.) podem ser encontrados na documentação inline do código e em arquivos de configuração individuais (ex: `pom.xml`, `docker-compose.yml`, arquivos de configuração do Strapi).
