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

## Metodologia e Escolhas Técnicas
Para a etapa de simulação generativa, optamos pela utilização do modelo `Qwen/Qwen2.5-0.5B-Instruct`. A escolha deste modelo se justifica por ser uma tecnologia de pesos abertos (*open-weights*), altamente otimizada e capaz de ser executada localmente utilizando os recursos de GPU gratuitos do Google Colab (T4 GPU).

O processo de inferência foi realizado na abordagem *Zero-Shot*, na qual o modelo recebe um *prompt* estruturado contendo o perfil sociodemográfico detalhado do respondente (idade, sexo, escolaridade, classe, região, etc.) e a pergunta exata da pesquisa, devendo retornar apenas a alternativa escolhida.

Para estabelecer uma base de comparação sólida (*Baseline*), treinamos um modelo clássico de aprendizado de máquina: o `Random Forest Classifier`.

As métricas de avaliação seguiram as recomendações da literatura recente sobre o tema, dividindo a análise em dois níveis:
- Nível Individual: Acurácia e F1-Score, para verificar o acerto exato da resposta de cada indivíduo.
- Nível Distribucional: *Total Variation Distance* (TVD) e Distância de Jensen-Shannon (JS), para avaliar o quão bem a inteligência artificial consegue mimetizar a distribuição geral de opiniões da sociedade (o cenário macro), comparando os histogramas das respostas reais contra as simuladas.

## Análise dos Resultados: Desempenho e Distribuição

A análise das métricas revela o grande desafio que é simular a opinião pública e o comportamento humano utilizando modelos de linguagem (LLMs) compactos sem a aplicação de *Fine-Tuning*.

Em um nível individual, tanto o LLM quanto o Random Forest apresentaram acurácias relativamente baixas (variando entre 10% e 18%), o que é esperado, visto que a opinião humana carrega uma enorme subjetividade que não é totalmente explicada apenas por atributos sociodemográficos.

No entanto, a grande diferença aparece na análise distribucional (TVD e JS Distance). O objetivo ideal é que essas métricas sejam o mais próximas de zero possível:

- Na Pergunta 1 (Tarefas Domésticas): O LLM apresentou um TVD alto de 0.646, o que demonstra que a distribuição de suas respostas divergiu fortemente da realidade. Em contrapartida, o modelo Random Forest capturou de forma excelente a tendência populacional, atingindo um TVD de apenas 0.095.
- Na Pergunta 4 (Assédio, Mulheres): O LLM teve uma leve melhora em relação à primeira pergunta, reduzindo seu TVD para 0.301. Contudo, novamente o Random Forest foi superior na mimetização do comportamento coletivo, com um TVD de 0.117.

Estes resultados sugerem que, enquanto o Random Forest consegue mapear de forma puramente matemática a correlação dos dados na amostra, o LLM de 0.5B de parâmetros, na abordagem *Zero-Shot*, tende a convergir para os vieses inseridos durante o seu treinamento prévio, apresentando dificuldades para encarnar perfeitamente personas sociológicas específicas do contexto da cidade de São Paulo.

## Análise de Explicabilidade (*Permutation Importance*)

Com o intuito de compreender quais fatores sociodemográficos mais impactam a formulação da opinião dos paulistanos, aplicamos a técnica de *Permutation Importance* sobre o modelo Random Forest.

- Para a P1 (Divisão das Tarefas Domésticas): O modelo identificou de forma coesa com a literatura sociológica que as variáveis `SEXO` (0.0395), `FX_ID` (Faixa etária) e `RELIGIAO` são os preditores mais fortes. A percepção sobre quem executa as tarefas do lar é fortemente dividida pela questão de gênero e influenciada por perspectivas geracionais e religiosas.
- Para a P4 (Risco de Assédio): Como esta pergunta foi direcionada exclusivamente às mulheres da amostra (anulando a variável genérica de gênero), o modelo identificou novos vetores de importância. As variáveis como `FX_ID` (Faixa de idade, 0.036), `RACA` (0.033) e `IDADE` destacaram-se significativamente. Faz sentido prático que fatores interseccionais, como ser uma mulher jovem e/ou negra, alterem sensivelmente a percepção sobre em quais locais (como ruas, trabalho ou transporte público) o risco de assédio é sentido com maior gravidade. A variável `AMOSTRA` também obteve destaque, indicando possíveis vieses ligados ao formato da coleta do dado (Papel, Web, Tablet).


## Conclusão

O projeto demonstrou um pipeline completo para a avaliação de LLMs na tarefa de simulação social baseada na pesquisa `CESOP 04833`. Concluímos que simular a opinião pública com alta precisão requer mais do que apenas a injeção de perfis sociodemográficos no *prompt*. Modelos LLM compactos (como o de 0.5B utilizado) operando em *Zero-Shot* não conseguiram superar algoritmos tradicionais supervisionados (Random Forest) no mapeamento das distribuições populacionais. Para trabalhos futuros, sugere-se a aplicação da técnica de *Fine-Tuning* (como LoRA) no LLM com base em pesquisas anteriores, ou a utilização de modelos com mais parâmetros (ex: 8B ou superiores) para avaliar se o aumento da capacidade cognitiva se traduz em melhor adoção de personas sociais.


## Referências Bibliográficas

ARGENTO, C. *et al*. *Simulating Public Opinion: Comparing Distributional and Individual-Level Predictions from LLMs and Random Forests*. 2024.

CESOP – Centro de Estudos de Opinião Pública. *Percepção dos paulistanos acerca de questões relacionadas às mulheres (Pesquisa 04833)*. Unicamp.

HUGGING FACE. *Transformers Documentation*. Disponível em: https://huggingface.co/docs/transformers/index.

PEDREGOSA, F. *et al*. *Scikit-learn: Machine Learning in Python. Journal of Machine Learning Research*, 12, p. 2825-2830, 2011.
