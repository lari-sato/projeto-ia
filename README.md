# **Projeto: Simulação de Opinião Pública com Modelos de Linguagem (LLMs)**
## *“Percepção dos paulistanos acerca de questões relacionadas às mulheres.”*
### Grupo: Mulheres
- Beatriz Lima de Moura | 10416616
- Giovana Simões Franco | 10417646
- Julia Santos Oliveira | 10417672
- Larissa Yuri Sato     | 10418318

## Introdução
A opinião pública é um importante instrumento para compreender percepções, valores e comportamentos presentes em uma sociedade. Pesquisas de opinião permitem observar como diferentes grupos sociais se posicionam diante de temas relevantes, além de possibilitarem análises sobre desigualdades, percepções coletivas e padrões de resposta associados a características sociodemográficas.

Nos últimos anos, os Modelos de Linguagem de Grande Escala, conhecidos como LLMs, passaram a ser explorados em tarefas que vão além da geração de texto, incluindo simulações sociais, apoio à tomada de decisão e análise de comportamento. Nesse contexto, surge o interesse em investigar se esses modelos são capazes de simular respostas de questionários de opinião pública a partir de informações sobre o perfil dos respondentes.

Este trabalho tem como base a pesquisa CESOP 04833, intitulada **“Percepção dos paulistanos acerca de questões relacionadas às mulheres”**, que reúne dados de entrevistados da cidade de São Paulo sobre temas como divisão de tarefas domésticas, assédio, violência e desigualdades de gênero. A escolha dessa pesquisa se justifica pela relevância social do tema e pela possibilidade de analisar questões associadas às experiências e percepções de diferentes grupos da população.

## Pesquisa utilizada: CESOP 04833
A pesquisa contém respostas individuais de entrevistados, acompanhadas por variáveis sociodemográficas, como sexo, idade, escolaridade, renda, religião, raça/cor, ocupação, classe social e região da cidade. Essas informações são utilizadas para construir perfis de respondentes que serão fornecidos ao modelo de linguagem durante a etapa de simulação.

Para este trabalho, foram selecionadas duas questões: 

A **Questão 1**, representada pela variável `P1`, pergunta ao entrevistado como ele definiria a divisão dos afazeres domésticos em sua casa. Essa questão possui múltiplas alternativas e permite observar percepções sobre a responsabilidade de homens e mulheres nas tarefas domésticas.

A **Questão 4**, representada pela variável `P4`, pergunta em qual local a entrevistada acredita correr maior risco de sofrer algum tipo de assédio. Essa questão foi aplicada somente a mulheres, portanto, a análise dessa variável considera apenas respondentes do sexo feminino.

A seleção dessas perguntas permite avaliar o desempenho do modelo em dois tipos de situação: uma questão aplicada ao conjunto geral dos entrevistados e uma questão direcionada a um recorte específico da amostra.
