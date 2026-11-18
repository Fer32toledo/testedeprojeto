# CINE Brasil e o nome do curso no Censo da Educação Superior

**Pergunta investigada (ticket #3 do wayfinder):** para um ranking de cursos do ensino superior brasileiro, existe uma unidade estável "nome do curso + grau"? Como a CINE Brasil se estrutura, como o Censo usa esses códigos, qual é a variabilidade real dos nomes e existe de-para oficial reutilizável?

**Escopo deste documento:** fatos e riscos, com links. Não propõe esquema de dados.
**Data da pesquisa:** 18/11/2026. Números de microdados calculados nesta pesquisa a partir dos arquivos oficiais (ver "Como os números foram obtidos").

---

## 1. Resumo executivo (o que a evidência mostra)

1. A **CINE Brasil tem quatro níveis** (área geral, específica, detalhada, rótulo) e o **rótulo é explicitamente uma unidade de agrupamento, não o nome do curso**. O INEP repete isso em três fontes independentes (Portaria Inep 622/2025, FAQ, página da classificação). Ou seja, o código CINE **não substitui** "nome do curso"; ele é uma classificação temática atribuída ao curso.
2. O Censo **publica o nome do curso** (`NO_CURSO`) **junto com o código CINE** (`CO_CINE_ROTULO` e as três áreas), na tabela de Cursos dos microdados, **de 2009 a 2024**. Antes de 2009 o layout é outro e a classificação é a CINE Brasil 2000.
3. O nome varia **muito dentro de uma mesma edição** (em 2024, 290 de 453 pares rótulo+grau têm mais de uma grafia; média de 3,88 nomes por par) e **pouco para o mesmo curso entre edições** (0,9% dos cursos mudaram de nome de 2019 para 2023). A variação concentrada é de cauda: 89,9% dos registros usam o nome modal do seu par (2023).
4. Há **mudança de convenção de caixa não documentada**: nomes 100% maiúsculos até 2020, Title Case a partir de 2021, sem nota no dicionário.
5. A **estrutura CINE mudou em 2025** e a mudança foi aplicada retroativamente **só a partir de 2024**: 2.689 cursos trocaram de rótulo entre 2023 e 2024 (ex.: 1.415 de `0615S02` para `0613S01`). A série histórica quebra sem um mapa de-para por edição.
6. Existe **de-para oficial**, mas ele é uma **sugestão baseada no "rótulo mais recorrente"** por *denominação do curso no e-MEC* + grau, não uma norma, não é por edição e **não tem chave de junção direta** com os microdados.

---

## 2. Estrutura e códigos da CINE Brasil

### 2.1 Níveis e dimensão atual

- Quatro níveis hierárquicos: 1º área geral, 2º área específica, 3º área detalhada, 4º rótulo (Portaria MEC 1.715/2019, art. 3º; Manual CINE Brasil, seção 3).
- Dimensão vigente, divulgada na página oficial: **11 áreas gerais, 45 áreas específicas, 96 áreas detalhadas e 379 rótulos**.
  Confirmado por contagem direta no arquivo `estrutura_cine_brasil_2026.xlsx`: 11 / 45 / 96 / 379.
- O arquivo `estrutura_cine_brasil_com_registro_de_alteracoes_2026.xlsx` tem **397 rótulos** e 11 / 47 / 102 — a diferença são níveis suprimidos/inativados que permanecem no histórico.

### 2.2 Codificação

Regra descrita no Manual (seção 3), com o exemplo `0211A01 Animação`:

| Nível | Formato | Exemplo |
|---|---|---|
| Área geral | 2 dígitos | `02` Artes e humanidades |
| Área específica | 3 dígitos | `021` Artes |
| Área detalhada | 4 dígitos | `0211` Produção audiovisual, de mídia e cultural |
| Rótulo | 4 dígitos + letra inicial + 2 dígitos | `0211A01` Animação |

Consequências práticas documentadas:

- O código do rótulo é **alfanumérico de 7 caracteres**, com a **letra inicial do rótulo**. Por isso códigos com "E" viraram notação científica no Excel — problema tratado pela Nota Informativa de 01/11/2022, que passou a publicar `CO_CINE_ROTULO` **entre aspas duplas**.
- **Programas interdisciplinares** usam o algarismo **8** na terceira posição (ex.: `0388P01`, `0688P01`), e a área específica/detalhada tem nome padrão "Programas interdisciplinares abrangendo [área geral]" (Manual, seção 3).
- **ABI (Área Básica de Ingresso)** tem rótulos próprios na área geral 00 (`0011A01`…`0011A09`), com grau acadêmico não aplicável — nos microdados, `TP_GRAU_ACADEMICO` vem vazio nesses registros (2.046 registros em 2024).

### 2.3 Gerações da classificação

| Geração | Base | Evidência |
|---|---|---|
| **Cine Brasil 2000** | Isced 1997, adotada a partir do Censo de 2000 | Manual, introdução; arquivos de microdados até 2008 usam esse esquema (`AREACURSO` = `345A01`, `CODAREAGERAL` = `3` "Ciências sociais, negócios e direito") |
| **Cine Brasil 2018** | Isced-F 2013; 362 rótulos com data de inclusão 30/05/2019 | `estrutura_cine_brasil_com_registro_de_alteracoes_2026.xlsx` (coluna "Data de inclusão"); Manual |
| **Revisão 2025** | Remanejamentos e renumeração (ex.: `0617C01` → `0681C01`), inativação de 3 rótulos e exclusão de áreas específicas 019 e 069 | Manual (seção sobre alterações), planilha de alterações (datas 24/03/2025 e 21/11/2024) |

Não localizei, nos materiais publicados na página da Cine Brasil, uma **tabela oficial de correspondência entre a Cine Brasil 2000 e a Cine Brasil 2018/2025**.

---

## 3. Como o Censo da Educação Superior usa os códigos

### 3.1 Variáveis na tabela de Cursos

Arquivo `MICRODADOS_CADASTRO_CURSOS_AAAA.CSV` (mesmo conteúdo em 2009 e em 2024):

`NO_CURSO`, `CO_CURSO`, `NO_CINE_ROTULO`, `CO_CINE_ROTULO`, `CO_CINE_AREA_GERAL`, `NO_CINE_AREA_GERAL`, `CO_CINE_AREA_ESPECIFICA`, `NO_CINE_AREA_ESPECIFICA`, `CO_CINE_AREA_DETALHADA`, `NO_CINE_AREA_DETALHADA`, `TP_GRAU_ACADEMICO`, `TP_NIVEL_ACADEMICO`, `TP_MODALIDADE_ENSINO`, além das estatísticas (vagas, inscritos, ingressantes, matrículas, concluintes).

O dicionário de dados documenta a coleta dessas variáveis **de 2009 em diante**: as colunas "Coleta por ano" dos dicionários de 2019 e 2020 começam em `09` e vão até a edição corrente.

**Armadilha de nomenclatura:** nos dicionários de 2019, 2021, 2023 e 2024, a descrição de `NO_CINE_ROTULO` é literalmente *"Nome do curso, conforme adaptação da Classificação Internacional Normalizada da Educação Cine/Unesco"*, enquanto `NO_CURSO` é *"Nome do Curso"*. Quem lê só a descrição pode confundir o rótulo CINE com o nome do curso — exatamente o que a Portaria 622/2025, art. 13, proíbe conceitualmente.

**Descompasso dicionário × arquivo:** o dicionário empacotado no ZIP de 2019 **não lista** `NO_CURSO`, `CO_CURSO`, `IN_GRATUITO` nem `TP_DIMENSAO`, mas o `MICRODADOS_CADASTRO_CURSOS_2019.CSV` **contém essas quatro colunas** — foram inseridas depois, conforme a Nota Informativa de 01/11/2022, sem atualização do dicionário daquele ano. O dicionário só volta a documentar `NO_CURSO` em 2020. Confiar apenas no dicionário da edição pode levar a concluir erradamente que o nome do curso não existe antes de 2020.

### 3.2 De onde vêm o nome e o código do curso

- `NO_CURSO` = **denominação dada pela IES**, registrada no Cadastro e-MEC (Manual de preenchimento do Censo 2024, Módulo Curso: *"Nome do curso – nome do curso usado pela IES registrado no Cadastro e-MEC"*).
- `CO_CURSO` = *"código único que representa o curso oriundo do cadastro e-MEC"* (mesmo manual). Na prática é um identificador **por curso/oferta** (valores de 6 a 8 dígitos, distintos por IES), **não** o código de denominação do curso do e-MEC.
- Verificação empírica: `CO_CURSO` é 1:1 com nome e com rótulo dentro da mesma edição (0 casos de `CO_CURSO` com mais de um nome ou mais de um rótulo em 2019 e 2023).
- Quem classifica: cursos cadastrados no e-MEC **a partir de 01/06/2020** são classificados pela própria IES no cadastro; os anteriores foram classificados pelo Inep, com etapas de manifestação das IES (FAQ Cine Brasil). A classificação **não pode ser alterada pela IES após o protocolo** (Portaria 1.715/2019, art. 9º); correção só por pedido de "erro material" à CTCC (FAQ; Portaria 299/2023, art. 21, §3º).

### 3.3 O que o Censo NÃO publica

- **Não há variável de situação do curso** (`TP_SITUACAO_CURSO` ou equivalente) na tabela de Cursos publicada: os dicionários de 2019, 2021, 2023 e 2024 não a listam. O formulário de coleta (Anexo II – Módulo Curso 2024) tem o campo *"Situação do Curso no e-MEC: Em atividade / Em extinção / Extinto"*, mas ele não chega aos microdados.
  → Curso extinto ou fundido **desaparece** da tabela; não há como distingui-lo de curso que nunca existiu.
- Não há variável com o **código de denominação do curso do e-MEC** (aquele usado na tabela de correspondência do INEP).
- Cursos sequenciais de formação específica aparecem na mesma tabela (`TP_NIVEL_ACADEMICO = 2`), com grau não aplicável.

---

## 4. Quanto o nome do curso varia

### 4.1 Entre edições

Contagem de valores distintos de `NO_CURSO` por edição (normalização: NFD, remoção de acentos, maiúsculas, espaços colapsados):

| Edição | Nomes distintos | Nomes normalizados | Linhas na tabela de Cursos | % de nomes 100% maiúsculos |
|---|---|---|---|---|
| 2019 | 1.247 | 1.244 | 253.139 | 100,0% |
| 2020 | 1.308 | 1.304 | 344.691 | 100,0% |
| 2021 | 1.346 | 1.343 | 444.786 | 0,0% |
| 2022 | 1.380 | 1.377 | 573.019 | 0,0% |
| 2023 | 1.425 | 1.422 | 671.610 | 0,0% |
| 2024 | 1.497 | — | 720.349 | 0,0% |

Rotatividade de nomes entre edições consecutivas (após normalização):

| Transição | Nomes que saíram | Nomes que entraram |
|---|---|---|
| 2019→2020 | 49 | 109 |
| 2020→2021 | 64 | 103 |
| 2021→2022 | 36 | 70 |
| 2022→2023 | 26 | 71 |
| 2023→2024 | 44 | 116 |

Dois fatos distintos aparecem aqui:

1. **Mudança de convenção tipográfica em 2021.** Até 2020 todos os nomes são maiúsculos (`"ENGENHARIA DE ALIMENTOS"`); de 2021 em diante vêm em Title Case com capitalização interna irregular (`"Ciências Da Natureza"`, `"Abi - Educação Física"`). **O dicionário de 2021 não registra essa mudança** — `NO_CURSO` continua descrito apenas como "Nome do Curso", `Char(200)`. Qualquer série histórica que compare strings cruas quebra 100% entre 2020 e 2021.
2. **Rotatividade real de 2% a 8% ao ano**, compatível com criação de cursos e com renomeações.

Para o **mesmo curso** (`CO_CURSO`), a mudança de nome é pequena: 319 de 34.720 cursos presentes em 2019 e 2023 (0,9%) mudaram de nome; 75% dos cursos de 2023 já existiam em 2019.

Exemplos de renomeação no mesmo `CO_CURSO` (2019 → 2023, já normalizados):

| CO_CURSO | 2019 | 2023 |
|---|---|---|
| 1074156 | LETRAS - PORTUGUES E ESPANHOL | LETRAS - LINGUA PORTUGUESA |
| 23123 | COMUNICACAO SOCIAL | PUBLICIDADE E PROPAGANDA |
| 14332 | QUIMICA | QUIMICA - ATRIBUICOES TECNOLOGICAS |
| 1135269 | GESTAO DA TECNOLOGIA DA INFORMACAO | TECNOLOGIA DA INFORMACAO |

### 4.2 Dentro de uma mesma edição

Pares (rótulo CINE, grau acadêmico) e quantidade de nomes distintos:

| Edição | Pares (rótulo, grau) | Pares com >1 nome | Média de nomes por par |
|---|---|---|---|
| 2019 | 431 | 254 (58,9%) | 3,45 |
| 2023 | 445 | 283 (63,6%) | 3,81 |
| 2024 | 453 | 290 (64,0%) | 3,88 |

- **89,9% dos registros de 2023** usam o nome modal do seu par — a variação é de cauda longa, mas atinge quase dois terços dos pares.
- Há pares em que **nenhum nome domina**: 36 dos 453 pares de 2024 têm o nome modal cobrindo menos de 50% dos registros (ex.: `0588P01` interdisciplinar, `0114C02` Ciências licenciatura).
- Casos extremos de polissemia de nome dentro de um par (2023): `0215M01` Música, 71 nomes distintos ("Música", "Música - Canto", "Música - Violão"…); `0011A02` ABI Artes e humanidades, 34 nomes ABI distintos; `0588P01` interdisciplinar, 24 nomes.
- O inverso também ocorre: **152 dos 1.497 nomes de 2024 (10,2%) aparecem sob mais de um rótulo CINE**. Exemplos: "Artes Visuais" sob `0114A01`, `0114A02`, `0213A02` e `0213A03`; "Administração" sob `0413A01` e `0413G09`; "Artes Cênicas" sob `0114T01` e `0215A01`.
- **Grau é necessário para desambiguar**: 88 dos 354 rótulos usados em 2023 aparecem com mais de um grau acadêmico (o mesmo rótulo serve a bacharelado e licenciatura, por exemplo). Isso é coerente com a própria definição oficial do rótulo, que "respeita a diferença do grau acadêmico".
- `TP_GRAU_ACADEMICO` em 2024: Tecnológico 338.574, Bacharelado 243.210, Licenciatura 136.519, vazio 2.046 registros. **O valor "4 – Bacharelado e Licenciatura" previsto no dicionário não ocorre** em 2019 nem em 2024.

### 4.3 O que o INEP documenta — e o que não documenta

Documentado:

- **Dicionário de dados por edição**, com coluna "Coleta por ano" (desde quando a variável existe) e coluna "Notas sobre diferenças entre os anos". É o canal onde descontinuidades de variáveis são registradas (ex.: `QT_VG_NOVA` nula de 2009 a 2013).
- **`estrutura_cine_brasil_com_registro_de_alteracoes_2026.xlsx`**: para cada rótulo, "Data de inclusão", "Alteração" (texto descritivo) e "Data de alteração". Distribuição: inclusões 362 (2019), 2 (2020), 6 (2021), 11 (2023), 2 (2026), 14 sem data; alterações 1 (2019), 17 (2020), 1 (2023), 6 (2024), 28 (2025); 4 registros marcados como "Inativado".
- **Manual CINE Brasil**: descreve princípios, codificação, fluxogramas e a revisão de 2025 (renumerações `0617C01`→`0681C01`, `0613J01`→`0681J01`, `0616S01`→`0714S03`; inativação de `0724E03`, `0725P02`, `0811A03`).
- **Nota Informativa de 01/11/2022** (dentro dos ZIPs, pasta `leia-me`): justifica as aspas em `CO_CINE_ROTULO` e informa a inclusão de `CO_CURSO`, `NO_CURSO`, `IN_GRATUITO` e `TP_DIMENSAO` na tabela de Cursos.
- **Política de aplicação (Portaria Inep 622/2025)**: áreas gerais e específicas não mudam, salvo necessidade da ISCED-F (art. 8º); novas áreas detalhadas só por iniciativa do Inep; novos rótulos exigem 5 anos de dados, ≥5 cursos ativos e ≥1 ato de renovação publicado (art. 9º); rótulos podem ser inativados (art. 12); classificação considerada a partir do CES 2000 (art. 16).

Não documentado:

- A mudança de caixa de `NO_CURSO` entre 2020 e 2021 (nenhuma nota no dicionário de 2021).
- Um mapa de-para por edição para as renumerações de rótulos (a planilha de alterações é **uma só, vigente**, não uma série por ano).
- Qualquer nota metodológica específica sobre "fusão de cursos" ou sobre critérios de padronização de nomes pela DEED — o nome é tratado como dado declarado pela IES.

---

## 5. Existe arquivo/tabela oficial de-para (curso → área CINE)?

**Sim, um, e apenas na direção "denominação → rótulo sugerido".**

`tabela_de_correspondencia_entre_as_denominacoes_dos_cursos_e_as_sugestoes_de_rotulos_2026.pdf` (44 páginas), descrito na página oficial como:

> "A tabela de correspondência entre a denominação de curso e a classificação inicial sugerida para um novo curso no Cadastro e-MEC pode ser consultada durante o processo de classificação. Essa tabela associa quatro variáveis: o código e o nome da denominação do curso, conforme consta no Cadastro e-MEC; o grau acadêmico e o rótulo mais recorrente de classificação para aquela denominação."

Conteúdo medido:

- **1.441 linhas** de mapeamento; **1.433 pares únicos** (código de denominação, grau); **349 rótulos distintos** citados.
- **8 linhas duplicadas** (mesmo código + grau repetido, ex.: `ARTES PLÁSTICAS` código 91, Licenciatura, duas vezes).
- Só três graus: Bacharelado (674), Tecnológico (469), Licenciatura (298). Nenhuma linha para "Bacharelado e Licenciatura".
- Nenhum par (código, grau) mapeia para mais de um rótulo — a tabela é **função** de (código de denominação, grau).

Limitações relevantes para reuso:

1. É **"sugestão"** e usa o **"rótulo mais recorrente"** — estatística descritiva da base, não ato normativo. Não há garantia de que o curso esteja de fato classificado naquele rótulo.
2. A chave é o **código de denominação do e-MEC** (1, 3, 63, 4967…), que **não existe como variável nos microdados do Censo**. `CO_CURSO` do Censo é outro código (identificador de curso/oferta). Logo, o de-para **não é juntável por código** aos microdados; a junção só seria possível por string de nome + grau — justamente a parte instável.
3. Os universos não coincidem: 1.433 pares (código, grau) na tabela versus **1.497 nomes declarados por IES em 2024** e 453 pares (rótulo, grau). "Nome da IES" e "denominação e-MEC" são conceitos distintos por definição.
4. **Existe uma única versão (2026)**. Não há versões por edição do Censo nem histórico publicado da tabela.
5. O arquivo é PDF; não há CSV/XLSX equivalente publicado.

Não localizei de-para oficial **rótulo → área** além da própria estrutura (`estrutura_cine_brasil_2026.xlsx`, que já traz a hierarquia completa por linha) — ou seja, a hierarquia é reconstruível a partir da estrutura, sem necessidade de tabela separada.

---

## 6. Riscos concretos para um ranking por "curso"

1. **"Nome + grau" não é chave estável nem livre de ambiguidade.**
   - 64% dos pares (rótulo, grau) de 2024 têm mais de uma grafia; média de 3,88.
   - 10,2% dos nomes de 2024 aparecem sob mais de um rótulo CINE.
   - O mesmo nome muda de caixa (2020→2021) e de redação ao longo dos anos.
2. **A série histórica quebra na virada 2023→2024.** 17 rótulos usados nos dados de 2023 não existem na estrutura vigente (`0614C01`, `0615S01/S02/S03`, `0616E01/S01`, `0617A01/C01/C02/C03`, `0613J01`, `0199P01`, `0724E03`, `0725P02`, `0811A03`, entre outros). Nos dados de 2024 **não há nenhum registro** com os códigos suprimidos. 2.689 cursos mudaram de rótulo entre 2023 e 2024 — 2.445 deles dentro da área 06, quase todos remanejamento mecânico (ex.: `0615S02`→`0613S01`, 1.415 cursos).
3. **O de-para não resolve a junção.** Chave dele (código de denominação e-MEC) ≠ chave dos microdados (`CO_CURSO`); e ele reflete o "rótulo mais recorrente", não a classificação efetiva de cada curso.
4. **Cursos extintos e fundidos não são identificáveis.** Não há variável de situação do curso nos microdados; o curso some da tabela. Ranking sobre "cursos existentes em cada ano" sofre viés de sobrevivência; ranking sobre "todos os cursos que já existiram" exige reconstrução por nome.
5. **O significado de um rótulo pode mudar sem o código mudar** (remanejamentos, mudanças de nome de área — ex.: "Jornalismo" substituído por "Comunicação" nos nomes das áreas 03/032/0321 em 05/05/2020) **e o código pode mudar sem o significado mudar** (`0617C01`→`0681C01`). Um ranking agregado por rótulo é sensível às duas coisas.
6. **Reclassificação é possível, embora rara.** 126 de 34.720 cursos (0,4%) mudaram de rótulo entre 2019 e 2023, além dos 2.689 da virada 2023→2024. A Portaria 622/2025 permite inativar rótulos e o FAQ prevê correção por "erro material" via CTCC — não há garantia de imutabilidade.
7. **Armadilhas de formato entre edições** (todas verificadas nos arquivos):
   - ≤2008: separador `|`, layout totalmente distinto (`NOME_CURSO`, `AREACURSO`, `CODAREAGERAL`…), CINE 2000.
   - 2009 em diante: separador `;`, layout atual. Os arquivos **não são UTF-8 válidos** (falham em bytes `0xe7` em 2023 e 2024, por exemplo): `latin-1` é a leitura segura, apesar de o cabeçalho ser ASCII.
   - 2009: `CO_CINE_ROTULO` com **aspas triplas** (`"""0721E01"""`).
   - 2020: a coluna no arquivo chama-se **`CO_CINE_ROTULO2`** (única edição), mas o dicionário da mesma edição documenta `CO_CINE_ROTULO` — dicionário e arquivo divergem.
   - 2021 em diante: `CO_CINE_ROTULO` entre aspas duplas (Nota Informativa 2022).
8. **`NO_CINE_ROTULO` engana quem lê o dicionário**: a descrição diz "Nome do curso…", mas é o nome do **rótulo**. Usar essa variável como "nome do curso" produz um ranking por área temática, não por curso.
9. **Grau ausente em ABI e sequenciais** (2.046 registros em 2024, 296 em 2019) e inexistência do valor "4 – Bacharelado e Licenciatura" na prática: o par "nome + grau" tem casos com grau vazio, que precisam de tratamento explícito.
10. **A própria diversificação de nomenclaturas é a justificativa oficial da revisão da CINE** (Manual, introdução): o INEP reconhece que os nomes dos cursos se diversificam mais rápido do que a classificação — a instabilidade não é acidente, é característica do dado.

---

## 7. Como os números foram obtidos

- Download dos ZIPs oficiais de microdados (2008, 2009, 2019, 2020, 2021, 2022, 2023, 2024) e leitura de `MICRODADOS_CADASTRO_CURSOS_AAAA.CSV` com `csv` (delimitador `;`; latin-1 na leitura, com validação de que o arquivo é misto), descartando aspas em `CO_CINE_ROTULO`.
- Normalização de nomes: NFD, remoção de diacríticos, `upper()`, colapso de espaços.
- Contagens por edição: valores distintos de `NO_CURSO`, pares (rótulo, grau) e nomes por par; interseções entre edições; aderência de `CO_CURSO` a nome e rótulo.
- Estrutura CINE: leitura de `estrutura_cine_brasil_2026.xlsx` e `estrutura_cine_brasil_com_registro_de_alteracoes_2026.xlsx` (contagens de níveis, datas de inclusão/alteração).
- Tabela de correspondência: extração de texto do PDF e contagem de linhas/pares/rótulos por expressão regular sobre o padrão `código grau rótulo`.
- Limitação conhecida: 2020 tem a coluna `CO_CINE_ROTULO2`; a contagem de 2020 usa essa coluna. 2024 não foi submetido à checagem "1 nome por CO_CURSO" (feita só em 2019 e 2023).

---

## 8. Lacunas (o que não foi encontrado)

- Nenhuma nota metodológica do INEP sobre padronização/alteração de grafia de `NO_CURSO` (a mudança de 2021 não está documentada).
- Nenhum de-para publicado entre Cine Brasil 2000 e Cine Brasil 2018/2025.
- Nenhuma versão histórica da tabela de correspondência (só a de 2026).
- Nenhuma variável de situação do curso, de data de extinção ou de curso de origem (fusão) nos microdados.
- A página "Outros documentos" da Cine Brasil não expõe arquivos adicionais (apenas link para o arquivo histórico do Inep).

---

## Fontes

**Páginas oficiais do INEP (Cine Brasil)**
- Cine Brasil — visão geral: https://www.gov.br/inep/pt-br/areas-de-atuacao/pesquisas-estatisticas-e-indicadores/cine-brasil
- Cine Brasil — Classificação (contagens 11/45/96/379; descrição da tabela de correspondência): https://www.gov.br/inep/pt-br/areas-de-atuacao/pesquisas-estatisticas-e-indicadores/cine-brasil/classificacao
- Cine Brasil — Histórico: https://www.gov.br/inep/pt-br/areas-de-atuacao/pesquisas-estatisticas-e-indicadores/cine-brasil/historico
- Cine Brasil — Perguntas frequentes (momento da classificação; imutabilidade; erro material): https://www.gov.br/inep/pt-br/acesso-a-informacao/perguntas-frequentes/cine-brasil

**Planilhas e documentos oficiais da Cine Brasil**
- Estrutura Cine Brasil vigente (xlsx): https://download.inep.gov.br/pesquisas_estatisticas_indicadores_educacionais/cinebrasil/documentos/estrutura_cine_brasil_2026.xlsx
- Estrutura Cine Brasil com registro de alterações (xlsx): https://download.inep.gov.br/pesquisas_estatisticas_indicadores_educacionais/cinebrasil/documentos/estrutura_cine_brasil_com_registro_de_alteracoes_2026.xlsx
- Tabela de correspondência entre denominações de cursos e sugestões de rótulos (pdf, 2026): https://download.inep.gov.br/pesquisas_estatisticas_indicadores_educacionais/cinebrasil/documentos/tabela_de_correspondencia_entre_as_denominacoes_dos_cursos_e_as_sugestoes_de_rotulos_2026.pdf
- Manual para classificação dos cursos de graduação — Cine Brasil (pdf): https://download.inep.gov.br/publicacoes/institucionais/avaliacoes_e_exames_da_educacao_superior/manual_para_classificacao_dos_cursos_de_graducacao_e_sequenciais_cine_brasil.pdf

**Legislação**
- Portaria MEC nº 1.715, de 02/10/2019 (institui a Cine Brasil e a CTCC; quatro níveis; arts. 3º, 5º, 6º, 9º): https://abmes.org.br/arquivos/legislacoes/Portaria-mec-1715-2019-10-02.pdf
- Portaria MEC nº 299, de 27/02/2023 (Regimento interno da CTCC; art. 21, §3º): https://mecnormas.mec.gov.br/pesquisa/detalhar/687/portaria-mec-n-299-de-27-de-fevereiro-de-2023
- Portaria MEC nº 739, de 29/10/2025 (altera a 1.715/2019 e o Anexo de áreas gerais/específicas): https://mecnormas.mec.gov.br/pesquisa/detalhar/7225/portaria-mec-n-739-de-29-de-outubro-de-2025
- Portaria Inep nº 622, de 08/09/2025 (Política de aplicação da Cine Brasil; arts. 8º, 9º, 12, 13, 16): https://www.abmes.org.br/arquivos/legislacoes/Portaria-Inep-622-2025-09-08-Republicada.pdf

**Microdados do Censo da Educação Superior**
- Página oficial dos microdados: https://www.gov.br/inep/pt-br/acesso-a-informacao/dados-abertos/microdados/censo-da-educacao-superior
- Microdados 2008 (layout antigo, Cine 2000, separador `|`): https://download.inep.gov.br/microdados/microdados_censo_da_educacao_superior_2008.zip
- Microdados 2009 (primeiro ano com o layout atual e CINE 2018): https://download.inep.gov.br/microdados/microdados_censo_da_educacao_superior_2009.zip
- Microdados 2019: https://download.inep.gov.br/microdados/microdados_censo_da_educacao_superior_2019.zip
- Microdados 2020: https://download.inep.gov.br/microdados/microdados_censo_da_educacao_superior_2020.zip
- Microdados 2021: https://download.inep.gov.br/microdados/microdados_censo_da_educacao_superior_2021.zip
- Microdados 2022: https://download.inep.gov.br/microdados/microdados_censo_da_educacao_superior_2022.zip
- Microdados 2023: https://download.inep.gov.br/microdados/microdados_censo_da_educacao_superior_2023.zip
- Microdados 2024: https://download.inep.gov.br/microdados/microdados_censo_da_educacao_superior_2024.zip
- Dicionários de dados: `Anexos/ANEXO I - Dicionário de Dados/dicionário_dados_educação_superior.xlsx` dentro de cada ZIP (aba `cadastro_cursos`)
- Nota Informativa de 01/11/2022 (aspas em `CO_CINE_ROTULO`; inclusão de `CO_CURSO`, `NO_CURSO`, `IN_GRATUITO`, `TP_DIMENSAO`): `leia-me/Nota informativa.pdf` dentro dos ZIPs de 2019 e 2023
- Manual de preenchimento do Censo da Educação Superior 2024 — Módulo Curso (definição de "Nome do curso" e "Código do curso"): https://download.inep.gov.br/pesquisas_estatisticas_indicadores_educacionais/censo_da_educacao_superior/orientacoes/manuais/manual_curso_2024.pdf
