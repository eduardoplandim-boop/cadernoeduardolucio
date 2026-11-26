# Caderno de Erros | Eduardo Landim

Aplicação web progressiva (**PWA**) e **Single Page Application (SPA)** desenvolvida para o gerenciamento estratégico de erros em questões de concursos públicos, revisão ativa em ciclos e memorização por repetição espaçada.

O sistema transforma questões erradas em diagnósticos acionáveis: cada erro registrado é classificado por **matéria**, **tópico**, **nível de prioridade** e **causa raiz (origem do erro)**, integrando-se automaticamente a um fluxo de revisão ativa (**Segundo Ciclo**) e a um baralho de **Flashcards 3D**.

---

## Sumário

1. [Visão Geral e Metodologia](#1-visão-geral-e-metodologia)
2. [Módulos da Aplicação](#2-módulos-da-aplicação)
3. [Regras de Negócio e Algoritmos de Estudo](#3-regras-de-negócio-e-algoritmos-de-estudo)
4. [Arquitetura Técnica e Persistência de Dados](#4-arquitetura-técnica-e-persistência-de-dados)
5. [Tecnologias e Estrutura de Arquivos](#5-tecnologias-e-estrutura-de-arquivos)

---

## 1. Visão Geral e Metodologia

O projeto foi desenhado especificamente para a rotina de preparação de alto rendimento em concursos públicos, estruturando o estudo sobre erros em quatro pilares:

- **Catalogação Estruturada:** registro não apenas do que foi errado, mas da regra correta (*Correção*) e de um gatilho mental rápido (*Como evitar novamente / Macete*).
- **Diagnóstico de Causa Raiz:** identificação obrigatória do motivo pelo qual o erro ocorreu (desconhecimento teórico, confusão entre institutos, esquecimento ou falha de interpretação do enunciado).
- **Evocação Ativa (Segundo Ciclo):** revisão focada nos itens pendentes com ocultação da resposta correta, forçando o cérebro a recuperar a informação antes da conferência.
- **Repetição Espaçada (Flashcards 3D):** conversão direta dos erros em cartões de pergunta/resposta com agendamento dinâmico baseado na facilidade de recordação.

---

## 2. Módulos da Aplicação

A interface é dividida em **8 áreas principais**, acessíveis pela barra lateral responsiva:

### 2.1. Dashboard Analítico
Painel central de acompanhamento diário contendo:
- **Banner Personalizado:** caricatura vetorial exclusiva em SVG de Eduardo Landim e sistema de sorteio inteligente entre **32 frases motivacionais** voltadas para concursos (com controle para nunca repetir a frase imediatamente anterior).
- **Indicadores-Chave (KPIs):** cards de resumo para *Total de Erros*, *Matérias Monitoradas*, *Prioridade Alta*, *Prioridade Média*, *Erros Superados* e *Taxa Percentual de Domínio*.
- **Barra de Superação:** indicador visual proporcional do progresso geral entre erros críticos, em consolidação e superados.
- **Visualização Gráfica:** gráfico de barras (*Erros por Matéria*) e gráfico de rosca (*Distribuição por Prioridade*).
- **Acesso Rápido:** listagem dos últimos 5 erros cadastrados e ranking dos 5 tópicos com maior incidência de erros.

### 2.2. Registrar / Editar Erro
Formulário estruturado para catalogação completa do erro:
- **Matéria e Tópico:** seleção de matéria (com criação rápida inline de novas disciplinas) e autocompletar dinâmico (`datalist`) de tópicos vinculados à matéria escolhida.
- **Tríade Pedagógica:** *Descrição do Erro*, *Correção / Entendimento Correto* e *Como evitar esse erro novamente (Macete ou Alerta)*.
- **Classificação e Metadados:** seleção visual de *Prioridade*, *Origem do Erro* (obrigatória), *Questão de Referência* (código, banca/ano ou link externo) e *Data do Registro*.

### 2.3. Meus Erros
Central de consulta e gerenciamento do acervo catalogado:
- **Busca Textual em Tempo Real:** pesquisa instantânea em descrições, correções, macetes, matérias, tópicos e referências.
- **Filtros Combinados:** filtragem simultânea por Matéria, Tópico (contextual), Prioridade, Origem do Erro e intervalo de datas (inicial e final).
- **Agrupamento Hierárquico:** organização em árvore expansível (`Matéria → Tópico → Registros`) com contadores de prioridade por grupo.
- **Ações Rápidas Inline:** alteração imediata de Prioridade e Origem diretamente no card, modal de detalhes completos e conversão direta do erro em Flashcard.

### 2.4. Meu Segundo Ciclo (Revisão Ativa)
Ambiente dedicado exclusivamente aos erros que ainda **não foram superados** (Prioridades Alta e Média):
- **Mapa de Pontos Críticos:** destaque automático das matérias e dos tópicos com maior concentração de pendências.
- **Modal de Revisão Ativa:** exibe apenas a descrição do erro e a referência da questão, mantendo a correção e o macete ocultos até o acionamento do botão **Revelar Correção**.
- **Reclassificação com Histórico:** ao concluir a revisão, contabiliza o número de revisões (`reviewCount`) e atualiza o status do erro, preservando a data exata de superação (`overcomeAt`).

### 2.5. Flashcards Interativos (3D)
Sistema de memorização integrado ao Caderno de Erros, dividido em duas abas:
- **Modo Estudo (3D):** cartão interativo com animação CSS 3D de giro (`perspective` + `rotateY`), exibição opcional de dica/macete sem virar o cartão, barra de progresso da fila, embaralhamento e suporte completo a **atalhos de teclado** (`Espaço` para virar; teclas `1`, `2`, `3` e `4` para avaliar).
- **Gerenciar Baralho:** grade de visualização, busca, filtragem (para revisar hoje, todos, pendentes, dominados ou vinculados a erros), criação avulsa e **importação automática em lote** de erros ainda não convertidos em flashcards.

### 2.6. Minha Evolução
Módulo de inteligência e diagnóstico de desempenho:
- **Diagnóstico por Origem do Erro:** identifica automaticamente a causa mais frequente de erro nos estudos e apresenta uma **recomendação prática de ajuste de método** correspondente àquela falha.
- **Gráfico de Causas:** distribuição comparativa em barras horizontais e barras de progresso por categoria de origem.
- **Gráfico Temporal Acumulado:** curva de evolução histórica comparando o *Total de Erros Catalogados* versus *Erros Superados* ao longo do tempo.
- **Taxa de Superação por Matéria:** barras segmentadas de domínio individual por disciplina.

### 2.7. Matérias e Tópicos
Gerenciador da estrutura programática do edital:
- Vem pré-configurado com 6 disciplinas base (*Direito Constitucional*, *Direito Administrativo*, *Direito Penal*, *Direito Processual Penal*, *Língua Portuguesa* e *Raciocínio Lógico*) e seus principais tópicos.
- Permite adicionar/remover matérias e tópicos manualmente, além de sincronizar e incorporar automaticamente novos tópicos digitados durante o cadastro de erros ou flashcards.
- Possui trava de integridade que impede a exclusão de matérias que possuam erros vinculados.

### 2.8. Backup e Configurações
Central de sincronização em nuvem, portabilidade e auditoria:
- **Painel de Nuvem (Supabase):** status de conexão em tempo real, configuração de credenciais e controles manuais para baixar ou enviar dados à nuvem.
- **Portabilidade JSON:** exportação completa do banco em arquivo `.json` datado e importação com normalização automática de registros legados.
- **Dados de Exemplo e Manutenção:** carregamento rápido de 8 erros e 6 flashcards reais de concursos para demonstração, além de opção de limpeza segura com confirmação.
- **Log de Auditoria:** histórico cronológico das últimas ações realizadas no sistema (criações, edições, revisões e importações).

---

## 3. Regras de Negócio e Algoritmos de Estudo

### 3.1. Níveis de Prioridade
| Nível | Indicador | Papel no Sistema |
| :--- | :---: | :--- |
| **Prioridade Alta** | 🔴 | Erros recorrentes ou conceitos críticos ainda não dominados. Permanece no topo do *Segundo Ciclo*. |
| **Prioridade Média** | 🟡 | Conceitos já compreendidos, mas em fase de consolidação. Permanece no *Segundo Ciclo* em seção secundária. |
| **Superado** | 🟢 | Conteúdo dominado e validado em revisão. Sai da fila de pendências do *Segundo Ciclo* e alimenta as métricas de evolução. |

### 3.2. Categorias de Origem do Erro
Todo registro exige a classificação em uma das quatro causas cognitivas (registros antigos importados sem essa informação recebem o selo neutro *Não classificado*):
1. **Desconhecimento:** conteúdo, termo ou dispositivo nunca estudado anteriormente.
2. **Confusão entre conceitos:** troca de regras ou características entre institutos parecidos (ex.: *anulação × revogação*, *concussão × corrupção passiva*).
3. **Esquecimento:** assunto já estudado, mas cujo detalhe, prazo ou quórum não foi recordado na hora da questão.
4. **Erro de interpretação:** domínio do conteúdo teórico, porém com falha de leitura do comando da questão (ex.: não notar *"EXCETO"* ou *"INCORRETA"*).

### 3.3. Algoritmo de Repetição Espaçada (Flashcards)
Após virar um flashcard, a autoavaliação calcula o próximo agendamento (`nextReview`) e o intervalo em dias (`intervalDays`):
- **`1` — Errei (`again`):** zera o intervalo (`0 dias`), mantém a revisão para o próprio dia, altera o status para *Em revisão* e **reinsere o cartão no final da fila da sessão atual** até que seja acertado.
- **`2` — Difícil (`hard`):** agenda a próxima revisão para **+1 dia** (`status: revisar`).
- **`3` — Bom (`good`):** dobra o intervalo anterior com piso mínimo de **3 dias** (`max(3, intervalDays * 2)`), incrementando o contador de acertos.
- **`4` — Dominado (`easy`):** dobra o intervalo anterior com piso mínimo de **7 dias** (`max(7, intervalDays * 2)`) e promove o cartão para o status **Dominado**.

---

## 4. Arquitetura Técnica e Persistência de Dados

O sistema opera sob uma arquitetura **Offline-First / Persistência Híbrida**, combinando velocidade local com sincronização multi-dispositivo na nuvem:

1. **Camada Local (`localStorage`):**
   - Todas as leituras e gravações ocorrem instantaneamente no armazenamento local do navegador (`caderno_erros_eduardo_landim_v1`), garantindo resposta imediata na interface e funcionamento mesmo durante oscilações de rede.
2. **Camada de Nuvem (Supabase PostgreSQL / PostgREST):**
   - O estado consolidado da aplicação (`errors`, `subjects`, `flashcards`, `history` e `lastSavedAt`) é sincronizado com a tabela `caderno_sync` (formato `JSONB`) no **Supabase** via API REST oficial.
   - **Sincronização Inteligente:** ao abrir o aplicativo ou retomar o foco da aba (`visibilitychange`), o sistema compara os carimbos de tempo (`updated_at` vs. `lastSavedAt`). Dispositivos novos ou desatualizados puxam automaticamente os dados da nuvem, enquanto alterações locais são enviadas com *debounce* automático de `600ms`.
3. **Progressive Web App (PWA):**
   - Conta com `manifest.json` e Service Worker (`sw.js`) utilizando estratégia *Network-First com Fallback para Cache*, permitindo a instalação do aplicativo na tela inicial do celular (Android/iOS) ou no desktop como aplicativo independente.

---

## 5. Tecnologias e Estrutura de Arquivos

### Stack Tecnológica
- **HTML5 & JavaScript (ES6+ Vanilla):** gerenciamento de estado reativo, roteamento SPA client-side, manipulação segura de DOM (com sanitização anti-XSS via `escapeHTML`) e consumo REST via `fetch`.
- **Tailwind CSS (via CDN) + CSS3 Customizado:** estilização responsiva, gradientes personalizados, animações de transição (`fadeIn`) e perspectiva 3D (`preserve-3d` / `backface-visibility`) para os flashcards.
- **Chart.js (v4.4.1):** renderização dinâmica e responsiva dos gráficos de barras, rosca e linha temporal.
- **Lucide Icons:** conjunto de ícones vetoriais leves.
- **Supabase REST API (PostgREST):** banco de dados PostgreSQL em nuvem para sincronização entre dispositivos.

### Estrutura do Repositório
```text
├── index.html      # Aplicação SPA completa (interface, estilos, estado, gráficos e sincronização Supabase)
├── manifest.json   # Manifesto de instalação PWA (ícones, cores de tema e modo standalone)
├── sw.js           # Service Worker para gerenciamento de cache e suporte PWA
├── icon.svg        # Ícone vetorial principal da aplicação e atalho mobile
└── README.md       # Documentação descritiva e técnica do projeto
```
