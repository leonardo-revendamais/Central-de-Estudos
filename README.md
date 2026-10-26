# Central de Estudos

Painel para acompanhar o planejamento de estudos do concurso da **Prefeitura de Curitiba**. A Central transforma a planilha de acompanhamento semanal num lugar só para seguir o cronograma, marcar o que foi estudado, controlar as horas líquidas, medir o desempenho em questões, guardar materiais e organizar revisões a partir dos erros.

Todo o projeto está num único arquivo, o `index.html`, com HTML, CSS e JavaScript, sem etapa de build.

---

## O plano de estudos

O conteúdo vem da planilha `Planejamento_de_estudos.xlsx` e está embutido no `index.html` (constante `PLANO`).

| Item | Valor |
|---|---|
| Período | 22/10/2026 a 11/12/2026 |
| Semanas | 8 (a Semana 1 tem 2 dias; as demais, 5 dias úteis) |
| Assuntos | 148 |
| Carga diária | 8 horas líquidas, de segunda a sexta-feira |
| Meta semanal | 8h por dia de estudo (16h na Semana 1 e 40h nas demais) |

**Distribuição de cada dia**

| Bloco | Tempo previsto |
|---|---|
| Conhecimentos Específicos | 4h |
| Legislação | 2h |
| Matéria básica (Português, Raciocínio Lógico, Informática) | 1h |
| Questões / revisão | 1h |

Cada célula da planilha vira um conteúdo no painel, com o texto original preservado, a data prevista e o tempo planejado do bloco.

---

## Áreas do painel

| Área | Para que serve |
|---|---|
| **Visão geral** | Conclusão do plano, conteúdos concluídos, em andamento e pendentes, horas líquidas, questões, acertos, erros e revisões pendentes. Inclui "Próximos passos" e gráficos de evolução. |
| **Meu cronograma** | Todas as semanas e assuntos na ordem do plano, com filtros por disciplina, status e busca. |
| **Semanas de estudo** | Página de cada semana com os conteúdos agrupados por dia ou por disciplina, horas, questões, materiais, principais erros e fechamento. |
| **Caderno de erros** | Registro livre de erros, com motivo, explicação, correção, próxima revisão, status e histórico. |
| **Revisões** | Revisões atrasadas, de hoje e futuras, com intervalos configuráveis e sugestões de reforço. |
| **Desempenho por disciplina** | Questões resolvidas, corretas e percentual de acertos por disciplina e assunto, com filtros. |
| **Horas líquidas** | Blocos de estudo com início, fim, pausas e desconto de almoço, meta diária e saldo do dia. |
| **Biblioteca de materiais** | Links e arquivos por disciplina, por semana e os links úteis gerais. |
| **Fechamentos semanais** | Histórico e comparação entre semanas: horas, meta, questões, acertos e conteúdos concluídos. |

---

## Regras de cálculo

- **Conclusão do plano** = conteúdos concluídos ÷ conteúdos cadastrados × 100.
- **Acertos** = questões corretas ÷ questões resolvidas × 100, com uma casa decimal. Sem questões resolvidas, o painel mostra "—".
- Conclusão do plano e percentual de acertos são métricas separadas.
- **Horas líquidas de um bloco** = término − início − pausas − almoço (quando marcado). Por padrão o almoço desconta 1h, igual à fórmula da planilha (`MAX(0, fim − início − 1h)`). O desconto é configurável e o formulário avisa quando uma pausa de 60 min ou mais já foi lançada, para não descontar duas vezes.
- **Sem lançamento duplicado**: as horas vêm só dos blocos de estudo e as questões vêm só dos conteúdos. O fechamento semanal soma a partir dessas duas fontes.
- **Sugestões de reforço**: assuntos com acerto abaixo de 70% (a partir de 5 questões) ou com 2 erros ou mais. As sugestões não mudam o cronograma.

---

## Check animado de conclusão

Cada conteúdo tem um círculo de conclusão. Ao marcar, o círculo se preenche, o check aparece, a data de conclusão é registrada e as barras da semana e do plano são atualizadas. Clicar de novo reabre o conteúdo. A animação só acontece por ação do usuário e respeita a preferência de movimento reduzido do sistema.

---

## Onde os dados ficam salvos

O comportamento depende de onde a página é aberta.

| Onde a página abre | Como salva | Anexos de arquivo |
|---|---|---|
| **Artifact publicado no Claude** | Banco de dados do próprio artifact, na área privada de quem está usando. Funciona em qualquer dispositivo. | Sim (PDF, imagens, textos; até 20 MB) |
| **Fora do Claude** (GitHub Pages, arquivo local) | Modo local: só no navegador daquele dispositivo (`localStorage`). | Não |

No modo local, a página mostra um aviso amarelo no topo. Os dados não passam de um navegador para outro.

### Estrutura dos registros

Todos os registros ficam numa coleção por usuário (`data/users/<id>`), separados pelo campo `kind`:

| `kind` | Conteúdo |
|---|---|
| `week` | Semana: nome, início, fim, meta de horas, ordem |
| `content` | Assunto: disciplina, semana, status, data prevista, data de conclusão, questões resolvidas e corretas, observações, indicação de revisão |
| `session` | Bloco de estudo: data, início, fim, pausas, almoço, conteúdo associado |
| `error` | Erro: data, disciplina, assunto, questão, respostas, motivo, explicação, correção, próxima revisão, status, histórico |
| `review` | Revisão: assunto, disciplina, data prevista, motivo, origem, status, resultado |
| `material` | Link ou arquivo: nome, URL, tipo, conteúdo ou semana associada |
| `closing` | Fechamento da semana: dificuldades, assuntos para reforçar, observações, prioridades |
| `settings` | Preferências: meta diária, almoço, intervalos de revisão, dados do plano |

Ao excluir um conteúdo, os materiais, blocos de tempo e erros ligados a ele são mantidos e apenas desvinculados.

---

## Importação da planilha

Na primeira abertura, o painel mostra o aviso **"Sua planilha está pronta para entrar"**. O botão **Conferir e importar** mostra tudo o que foi identificado antes de gravar. Também existe uma importação genérica de `.xlsx` ou `.csv` (no Cronograma), em que cada aba vira uma semana.

O que foi trazido da planilha original:

- As 8 semanas com período e meta.
- Os 148 assuntos, com os textos sem alteração.
- A sessão de estudo de 22/10 (12:53 às 17:00), que dá 3h07 líquidas.
- O link "DOC" de apoio do dia 22/10 e o caderno de apoio da Semana 1 (Google Docs).
- Os 5 links úteis da aba Apoio: Qconcursos, TEC Concursos, Planalto, legislação municipal de Curitiba e portal da Prefeitura.
- Período, carga diária, distribuição e a observação sobre conferir o edital e as retificações.

Limitações conhecidas da importação:

- O link de "Anotações/Resumo" do dia 22/10 aponta para um arquivo no computador (`file:///…/Revisão de Administração Pública.html`). Ele é guardado, mas não abre pela web.
- A Semana 1 não tem coluna Status na planilha; todos os assuntos entram como Pendente.
- As seções "Principais erros" e "Fechamento" estavam vazias; o caderno de erros e os fechamentos começam em branco.
- Fórmulas não são importadas; as horas são recalculadas pelo painel.

---

## Tecnologia

- HTML, CSS e JavaScript puros, num único arquivo.
- [SheetJS](https://sheetjs.com/) 0.18.5 (via cdnjs) para ler planilhas na importação.
- Fontes Bricolage Grotesque, Figtree e JetBrains Mono (Google Fonts).
- Gráficos desenhados em SVG, sem biblioteca externa.
- Paleta: hotpink `#FF69B4`, lavender `#E6E6FA`, branco `#FFFFFF` e grafite arroxeado `#2D2438`, com tema escuro automático.
- Layout responsivo: menu lateral no computador e barra inferior no celular.

---

## Observação sobre privacidade

Este repositório é **público**. O `index.html` contém o plano de estudos completo e os links dos documentos de apoio no Google Docs. Os documentos continuam protegidos pelas permissões do próprio Google, mas os endereços ficam visíveis para qualquer pessoa.
