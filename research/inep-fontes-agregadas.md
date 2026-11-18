# Tabelas agregadas do Censo da Educação Superior (INEP): matrículas, ingressantes e concluintes por curso, grau e categoria administrativa

**Pergunta investigada (ticket #2 do wayfinder):** quais tabelas **agregadas** publicadas pelo INEP trazem, por edição, matrículas, ingressantes e concluintes discriminados por **curso, grau e categoria administrativa** — e em que formato?

- **Data da verificação:** 18/11/2026.
- **Método:** requisições `HEAD`/`GET` às páginas oficiais em `gov.br` e a `download.inep.gov.br`, download integral dos ZIPs das edições 2020–2024, abertura das planilhas com `openpyxl`/`xlrd` e leitura dos PDFs de apoio. Todos os números de linhas, colunas, abas e tamanhos abaixo foram medidos nos arquivos baixados, não estimados.
- **Escopo desta nota:** apenas fatos e links. Nenhuma decisão de esquema de dados ou de arquitetura é proposta aqui.

---

## 1. Resposta curta

Sim. O INEP publica **quatro famílias** de tabelas agregadas do Censo da Educação Superior, e apenas **uma** delas desce ao nível de **curso**:

| Família | Onde | Nível de curso? | Métricas |
|---|---|---|---|
| **Sinopse Estatística da Educação Superior** (arquivo principal, tabelas 1.x a 7.x) | ZIP anual em `download.inep.gov.br` | **Não** | cursos, vagas, inscritos, ingressantes, matrículas, concluintes |
| **Anexos I, II e III — "Dados Gerais por Curso" (tabelas 8.1, 8.2, 8.3)** | mesmo ZIP anual | **Sim** (Curso – Cine Rótulo) | cursos, vagas, inscritos, ingressantes, matrículas, concluintes |
| **Tabelas de divulgação** (`.xls`, séries nacionais) | `download.inep.gov.br` | Não | cursos, matrículas, ingressantes, concluintes |
| **Microdados** (CSV, uma linha por oferta de curso em IES) | `download.inep.gov.br` | Sim (curso + IES) | idem + dezenas de recortes |

**Última edição publicada: Censo 2024** (resultados divulgados em 22/09/2025; página das sinopses atualizada em 14/07/2026). A edição 2025 **ainda não está publicada** — o cronograma oficial vigente prevê divulgação "até 30/11/2026". Portanto, "últimos 5 anos" = **2020, 2021, 2022, 2023 e 2024**.

---

## 2. As quatro famílias, em detalhe

### 2.1 Sinopse Estatística (arquivo principal, por município)

Um único XLSX (e um ODS equivalente) por edição, com **71 tabelas** assim distribuídas — números declarados pelo próprio INEP na aba oculta de apresentação da planilha ("Sinopse"):

> "Tabela de Instituições: 2 · Tabelas de Docentes: 10 · Tabelas de Cursos: 3 · Tabelas de Processos Seletivos: 17 · Tabelas de Matrículas: 16 · Tabelas de Concluintes: 16 · Tabelas de Dados Gerais: 4 · Tabelas de Dados Gerais por Curso (Anexo): 3"

A mesma planilha informa que é "elaborada nos formatos adotados e recomendados pelos Padrões de Interoperabilidade de Governo Eletrônico – ePING (Versão 2015)" e que os dados são desagregados por **Brasil, Regiões Geográficas, Unidades da Federação e Municípios**.

### 2.2 Anexos 8.1 / 8.2 / 8.3 — "Dados Gerais por Curso"

Publicados como **arquivos separados dentro do mesmo ZIP**, em XLSX e ODS. A aba `8.1 a 8.3` do arquivo principal não contém dados: contém apenas a explicação de que, "em virtude da dimensão das bases", os dados por curso foram disponibilizados nos Anexos I, II e III.

- **8.1 (Anexo I)** — Graduação, presenciais **e** a distância.
- **8.2 (Anexo II)** — Graduação **presenciais**.
- **8.3 (Anexo III)** — Graduação **a distância (EaD)**.

Títulos oficiais (aba `Sumário`, edição 2024):

> "8.1 - Número de Cursos, Vagas Oferecidas, Candidatos Inscritos, Ingressantes, Matrículas e Concluintes nos Cursos de Graduação - Presenciais e a Distância, por Organização Acadêmica, Grau Acadêmico do Curso e Dependência Administrativa, segundo a Região Geográfica, a Unidade da Federação, Município e Curso - Cine Rótulo - 2024"
>
> "8.2 - … Cursos de Graduação - Presenciais … - 2024"
>
> "8.3 - … Cursos de Graduação a Distância (EaD) … - 2024"

### 2.3 Tabelas de divulgação (`.xls`)

Arquivo nacional, sem recorte geográfico, usado no material de imprensa: `tabelas_de_divulgacao_censo_da_educacao_superior_2024.xls` (718.336 bytes, **29 abas**). Traz séries de 2014 a 2024 (ex.: `Tab A` = cursos, matrículas, ingressantes e concluintes por ano; `Tab B` = matrículas por modalidade e organização acadêmica; `Tab1.01` em diante = tabelas temáticas). É a família mais simples para **série histórica nacional**, mas não tem curso nem UF.

### 2.4 Microdados (CSV)

`microdados_censo_da_educacao_superior_2024.zip` (**456.807.449 bytes**). Conteúdo verificado:

- `dados/MICRODADOS_CADASTRO_CURSOS_2024.CSV` — 452.844.981 bytes, **720.350 linhas** (720.349 registros + cabeçalho), **223 colunas**, separador `;`, codificação Windows-1252.
- `dados/MICRODADOS_ED_SUP_IES_2024.CSV` — 1.000.086 bytes.
- `Anexos/ANEXO I - Dicionário de Dados/dicionário_dados_educação_superior.xlsx` (abas `cadastro_ies` e `cadastro_cursos`).
- `Anexos/ANEXO II - Questionários…` (4 PDFs: Módulo Aluno, Curso, Docente, IES).
- `leia-me/Leia_me.pdf` (Manual do Usuário, setembro de 2025) e `leia-me/Nota informativa.pdf`.

O manual explica a mudança estrutural: "em atendimento à Lei nº 12.527/2011 (LAI) e à Lei nº 13.709/2018 (LGPD), os Microdados do Censo da Educação Superior **passam a ser estruturados a nível de Instituições de Ensino Superior (IES) e cursos**". Isso é visível no ZIP de 2024: **não há mais arquivo de aluno**, apenas cursos e IES.

---

## 3. Downloads das últimas 5 edições

### 3.1 Página oficial (índice)

- `https://www.gov.br/inep/pt-br/acesso-a-informacao/dados-abertos/sinopses-estatisticas/educacao-superior` — HTTP 200; lista as edições de 1995 a 2024. Aviso textual na página: as edições 2010 a 2022 têm "Arquivo atualizado em 18/10/2024, incorporando dados por município".

### 3.2 ZIPs da Sinopse (arquivo principal + 3 anexos)

| Edição | URL | Bytes (verificado por HEAD) |
|---|---|---|
| 2024 | `https://download.inep.gov.br/informacoes_estatisticas/sinopses_estatisticas/sinopses_educacao_superior/sinopse_educacao_superior_2024.zip` | 397.378.396 |
| 2023 | `…/sinopse_educacao_superior_2023.zip` | 381.866.951 |
| 2022 | `…/sinopse_educacao_superior_2022.zip` | 360.893.549 |
| 2021 | `…/sinopse_educacao_superior_2021.zip` | 302.144.294 |
| 2020 | `…/sinopse_educacao_superior_2020.zip` | 284.364.534 |

O caminho completo é sempre `https://download.inep.gov.br/informacoes_estatisticas/sinopses_estatisticas/sinopses_educacao_superior/`.

**Não existem** (HTTP 404 verificado): `sinopse_educacao_superior_2025.zip` e `…_2026.zip`; também 404 para páginas por ano como `/sinopses-estatisticas/educacao-superior-2024`.

### 3.3 Inventário interno de cada ZIP (bytes medidos)

| Edição | Arquivo principal `.xlsx` | `.ods` equivalente | Anexo I (8.1) | Anexo II (8.2) | Anexo III (8.3) |
|---|---|---|---|---|---|
| 2024 | 78.768.018 | 49.623.963 | 336.582.031 | 31.262.069 | 327.352.352 |
| 2023 | 72.536.352 | 47.280.976 | 319.007.967 | 31.748.376 | 309.724.982 |
| 2022 | 67.683.981 | 43.351.743 | 283.361.881 | 31.892.064 | 273.616.910 |
| 2021 | 62.342.174 | 17.067.146 | 241.403.885 | 31.771.851 | 225.169.597 |
| 2020 | 62.410.413 | 42.601.254 | 192.505.811 | 31.885.661 | 180.422.848 |

Nomes internos exatos: `Sinopse_Educação_Superior_Municipio_AAAA.xlsx|.ods` e `anexo_I_tab_8_1_dados_gerais_por_curso_AAAA.xlsx|.ods`, `anexo_II_tab_8_2_…`, `anexo_III_tab_8_3_…`. Os ZIPs também trazem `MD5_Sinopse_Educação_Superior_Municipio_AAAA.txt`. A edição 2023 inclui ainda `Nota Informativa 02.pdf`.

### 3.4 Documentos de apoio (verificados)

| Documento | URL | Bytes |
|---|---|---|
| Notas Estatísticas 2024 | `https://download.inep.gov.br/publicacoes/institucionais/estatisticas_e_indicadores/notas_estatisticas_censo_escolar_da_educacao_superior_2024.pdf` | 638.380 |
| Resumo Técnico 2024 | `https://download.inep.gov.br/publicacoes/institucionais/avaliacoes_e_exames_da_educacao_superior/resumo_tecnico_do_censo_da_educacao_superior_2024.pdf` | 1.632.890 |
| Apresentação dos resultados 2024 | `https://download.inep.gov.br/educacao_superior/censo_superior/documentos/2024/apresentacao_censo_da_educacao_superior_2024.pdf` | 6.455.810 |
| Boletim nº 45 (divulgação 2024) | `https://download.inep.gov.br/educacao_superior/censo_superior/boletins/boletim_n_45_censo_superior.pdf` | 1.304.591 |
| Tabelas de divulgação 2024 | `https://download.inep.gov.br/educacao_superior/censo_superior/documentos/2024/tabelas_de_divulgacao_censo_da_educacao_superior_2024.xls` | 718.336 |
| Nota sobre alterações nos dados de vagas/inscritos de 2023 | dentro do ZIP 2023: `Nota Informativa 02.pdf` | 87.173 |

> Atenção ao nome do arquivo das Notas Estatísticas: contém `censo_escolar` no caminho (`notas_estatisticas_censo_escolar_da_educacao_superior_2024.pdf`), embora o conteúdo seja da educação superior. O padrão "sem `escolar`" retorna 404.

---

## 4. Estrutura interna verificada

### 4.1 Abas

Número de abas por edição (arquivo principal): **2020 = 79**, **2021 = 79**, **2023 = 82**, **2024 = 82**.

Diferenças entre edições, verificadas nos nomes das abas:

- **2020** não tem as abas `1.2`, `2.10` e `7.4` (presentes a partir de 2021/2023).
- A aba `3.1` de 2020 tem **220 colunas** (1.181 linhas), contra **76 colunas** em 2021–2024 — ou seja, o layout de `3.1` mudou depois de 2020.
- `5.1`, `5.3` e `6.3` mantêm o mesmo número de colunas em todas as cinco edições (38, 97 e 75).

Abas da edição 2024 (lista exata, 82 itens):

```
Sinopse_Estatística | Resumo | Equipe Técnica | Como Citar | Sumário | 1 Instituições  | 1.1 | 1.2 |
2 Docentes  | 2.1 | 2.2 | 2.3 Características do Docente | 2.4 | 2.5 | 2.6 | 2.7 | 2.8 | 2.9 | 2.10 |
3 Cursos | 3.1 | 3.2 | 3.3 | 4 Processos Seletivos  | 4.1 Vagas e Inscritos | 4.2 Ingressantes | 4.3 | 4.4 |
4.5 | 4.6 Características do Aluno | 4.7 | 4.8 | 4.9 | 4.10 | 4.11 | 4.12 Financiamento | 4.13 | 4.14 |
4.15 Reserva de Vagas | 4.16 | 4.17 | 5 Matrículas  | 5.1 | 5.2 | 5.3 | 5.4 | 5.5 Características do Aluno |
5.6 | 5.7 | 5.8 | 5.9 | 5.10 | 5.11 Financiamento | 5.12 | 5.13 | 5.14 Reserva de Vagas | 5.15 | 5.16 |
6 Concluintes  | 6.1 | 6.2 | 6.3 | 6.4 | 6.5 Características do Aluno | 6.6 | 6.7 | 6.8 | 6.9 | 6.10 |
6.11 Financiamento | 6.12 | 6.13 | 6.14 Reserva de Vagas | 6.15 | 6.16 | Dados Gerais | 7.1 | 7.2 | 7.3 | 7.4 |
Dados Gerais Por Curso | 8.1 a 8.3
```

Observação: `Sinopse_Estatística`, `Resumo`, `Equipe Técnica` e `Como Citar` aparecem vazias ao leitor de células — o conteúdo dessas abas está em **imagens e caixas de texto** embutidas (`xl/media/image1.png`, `xl/drawings/drawingN.xml`), não em células. Foi lendo o `drawing4.xml` que se obteve o texto oficial de citação da seção 7.

### 4.2 Tabelas-chave da Sinopse (edições 2021–2024)

| Aba | Título (2024) | Dimensão medida (2024) |
|---|---|---|
| `3.1` | Número de Cursos de Graduação e Sequenciais de Formação Específica – Presenciais e a Distância, por Organização Acadêmica, **Modalidade de Ensino** e Dependência Administrativa, segundo Região, UF e Município | 1.178 × 76 |
| `4.4` | Número de **Ingressantes** nos Cursos de Graduação – Presenciais e a Distância, por Organização Acadêmica, **Grau Acadêmico** e Dependência Administrativa, segundo Região, UF e Município | 3.369 × 97 |
| `5.3` | Número de **Matrículas** nos Cursos de Graduação – Presenciais e a Distância, por Organização Acadêmica, **Grau Acadêmico** e Dependência Administrativa, segundo Região, UF e Município | 3.466 × 97 |
| `6.3` | Número de **Concluintes** nos Cursos de Graduação – Presenciais e a Distância, por Organização Acadêmica, **Grau Acadêmico** e Dependência Administrativa, segundo Região, UF e Município | 2.961 × 75 |
| `5.1` | Matrículas por Organização Acadêmica e Dependência Administrativa, segundo Região, UF, Município e **Nível Acadêmico** (Graduação / Sequencial) | 3.473 × 38 |
| `7.1` | Ingressantes, Matrículas, Concluintes, **Matrículas Trancadas, Desvinculadas, Transferidos, Falecidos**, por Organização Acadêmica e Dependência Administrativa | 3.610 × 166 |
| `7.2` | Idem + cursos, vagas e inscritos — **presenciais** | 1.169 × 234 |
| `7.3` | Idem + **polos** — **EaD** | 3.574 × 257 |
| `7.4` | Idem — instituições **comunitárias/confessionais** | 531 × 154 |

**Colunas das tabelas 5.3 / 4.4 / 6.3** (árvore de cabeçalho com 5 linhas, mesclada; verificada coluna a coluna em 5.3):

- Colunas 1–4: `Região Geográfica`, `Unidade da Federação`, `Município`, `Código do Município` (linhas = Total Brasil, regiões, UFs e municípios).
- Coluna 5: `Total` geral.
- Depois, o bloco se repete para **Organização Acadêmica** × **Grau Acadêmico** × **Dependência Administrativa**:
  - Organização Acadêmica: `Universidade`, `Centro Universitário`, `Faculdade`, `Instituto Federal de Educação, Ciência e Tecnologia (IF) e Centro Federal de Educação Tecnológica (Cefet)`.
  - Grau: `Bacharelado`, `Licenciatura`, `Tecnológico`, `Não Aplicável` (nota de rodapé: cursos sequenciais/área básica de ingresso).
  - Dependência: `Total`, `Federal`, `Estadual`, `Municipal`, `Privada > Total`, `Privada > Com Fins Lucrativos`, `Privada > Sem Fins Lucrativos`.
  - O bloco IF/Cefet é a exceção: traz `Total` + os 4 graus, sem quebra por dependência (é sempre federal) — colunas 93–97.
- Em 5.3 a modalidade **não** é dimensão: a tabela soma presenciais + EaD. Presenciais aparecem isoladas em `5.2`/`4.2`/`6.2` ("Cursos de Graduação – Presenciais"); EaD só aparece isolada nas tabelas 7.3 e nos anexos 8.3.

**Metas de leitura:** cada célula é uma **contagem**, não uma taxa. Não há colunas de percentual, média ou indicador nessas tabelas.

### 4.3 Anexos 8.x: árvore de colunas e variáveis

Estrutura do Anexo I (8.1) de 2024 — aba única `8.1`, **213.544 linhas × 536 colunas**, das quais **521 com nome de variável** na linha 12:

Linhas 1–4: título e nota ("8.1 - Número de Cursos, Vagas Oferecidas, Candidatos Inscritos, Ingressantes, Matrículas e Concluintes … por Organização Acadêmica, Grau Acadêmico do Curso e Dependência Administrativa, segundo a Região Geográfica, a Unidade da Federação, Município e Curso - Cine Rótulo - 2024").

Linhas 6–11: cabeçalho hierárquico (mesclado), com esta lógica:

- **6 métricas** em sequência: `Número de Cursos` (col. 8), `Vagas Oferecidas` (79), `Candidatos Inscritos` (172), `Ingressantes` (265), `Matrículas` (358), `Concluintes` (451).
- Dentro de cada métrica, **4 organizações acadêmicas**: `Total`, `Universidade`, `Centro Universitário`, `Faculdade`, `IF/Cefet`.
- Dentro de cada organização, **graus**: `Total`, `Bacharelado`, `Licenciatura`, `Tecnológico`, `Não Aplicável`.
- Dentro de cada grau, **dependência**: `Total`, `Federal`, `Estadual`, `Municipal`, `Privada` → `Total`, `Com Fins Lucrativos`, `Sem Fins Lucrativos`.

Linha 12 (nomes de variável) — padrão verificado:

- Prefixos de métrica: `NCURSO`, `VAG`, `INSCR`, `INGR`, `MAT`, `CONCL`.
- Sufixos: `B` = bacharelado, `L` = licenciatura, `T` = tecnológico, `NA` = não aplicável; `1` = Universidade, `2` = Centro Universitário, `4` = Faculdade, `6` = IF/Cefet; e um segundo dígito para dependência (`1` Federal, `2` Estadual, `3` Municipal, `4` Privada, `5` Com fins lucrativos, `6` Sem fins lucrativos).
- Exemplos reais: `NCURSO` (total), `NCURSO1` (universidades), `NCURSOB1` (bacharelado em universidades), `NCURSOB11` (bacharelado federal em universidades), `MATT` (matrículas totais), `INGRB4` (ingressantes de bacharelado em faculdades), `CONCLL46` (concluintes de licenciatura, privada sem fins lucrativos, em faculdades).

Colunas 1–7 (dimensões): `NO_REGIAO`, `CO_UF`, `NO_UF`, `NO_MUNICIPIO`, `CO_MUNICIPIO`, `NO_CINE_ROTULO`, `CO_CINE_ROTULO`.

**Não existe coluna de IES** (nem `CO_IES`, nem nome de instituição) nos anexos 8.x. A agregação para no **município × curso**.

Dimensões medidas nos anexos:

| Arquivo | Aba | Linhas × colunas (2024) |
|---|---|---|
| Anexo I (8.1, presencial + EaD) | `8.1` | 213.544 × 536 |
| Anexo II (8.2, presencial) | `8.2` | 19.096 × 527 |
| Anexo III (8.3, EaD) | `8.3` | não medido em 2024; em 2022: 174.566 × 521 |

Referência cruzada: o Anexo I de **2020** também tem **536 colunas** (121.031 linhas), o que indica layout de colunas estável entre 2020 e 2024; o número de linhas cresce com a expansão da oferta e com a abertura por município.

### 4.4 O que cada métrica significa (dicionário de dados oficial, 2024)

Do `dicionário_dados_educação_superior.xlsx`, aba `cadastro_cursos`:

- `QT_CURSO` — "Número de cursos".
- `QT_VG_TOTAL` — "Quantidade total de vagas oferecidas".
- `QT_INSCRITO_TOTAL` — "Quantidade total de inscritos".
- `QT_ING` — "Quantidade de ingressantes"; categoria: "Cálculo de ingressantes: soma do número de alunos com data de ingresso de 01 de janeiro e 01 de julho do ano de referência do censo".
- `QT_MAT` — "Quantidade de matrículas"; categoria: "Cálculo de matrículas: soma do número de alunos com situação de vínculo ao curso igual a: Cursando e/ou Formado".
- `QT_CONC` — "Quantidade de concluintes".
- `QT_SIT_TRANCADA` — "Quantidade de alunos com situação de vínculo no curso - matrícula trancada".

Códigos das dimensões (mesmo dicionário):

- `TP_CATEGORIA_ADMINISTRATIVA`: 1 Pública Federal · 2 Pública Estadual · 3 Pública Municipal · 4 Privada com fins lucrativos · 5 Privada sem fins lucrativos · 6 Privada - Particular em sentido estrito · 7 Especial · 8 Privada comunitária · 9 Privada confessional.
- `TP_ORGANIZACAO_ACADEMICA`: 1 Universidade · 2 Centro Universitário · 3 Faculdade · 4 Instituto Federal de Educação, Ciência e Tecnologia.
- `TP_GRAU_ACADEMICO`: 1 Bacharelado · 2 Licenciatura · 3 Tecnológico · 4 Bacharelado e Licenciatura · (.) Não aplicável.
- `TP_MODALIDADE_ENSINO`: 1 Presencial · 2 Curso a distância.
- `TP_NIVEL_ACADEMICO`: 1 Graduação · 2 Sequencial de Formação Específica.
- `TP_REDE`: 1 Pública · 2 Privada.

> Nota de consistência: o dicionário lista **9** categorias administrativas, mas a Sinopse publica apenas cinco colunas (Total, Federal, Estadual, Municipal, Privada) e abre a Privada em "com fins lucrativos" / "sem fins lucrativos". A correspondência exata entre os 9 códigos e as 2 colunas da Sinopse **não está documentada no dicionário** — é uma lacuna a confirmar com o INEP antes de qualquer uso analítico.

---

## 5. Recortes disponíveis (matriz)

| Recorte | Sinopse 3.x | Sinopse 4.4 / 5.3 / 6.3 | Anexos 8.1/8.2/8.3 | Tabelas de divulgação |
|---|---|---|---|---|
| Brasil / Região / **UF** / Município | Sim (linhas) | Sim (linhas) | Sim (linhas + `CO_UF`) | Só Brasil |
| **Modalidade** presencial/EaD | Sim (colunas em 3.1) | **Não** (soma as duas) | **Sim, por arquivo** (8.2 presencial, 8.3 EaD) | Sim (Tab B) |
| **Grau** (bacharelado/licenciatura/tecnológico) | Não em 3.1 | **Sim** | **Sim** | Parcial |
| **Categoria administrativa** (federal/estadual/municipal/privada c/ e s/ fins) | Sim | **Sim** | **Sim** | Sim |
| Organização acadêmica (universidade/CU/faculdade/IF) | Sim | **Sim** | **Sim** | Sim |
| **Curso** (CINE Rótulo) | Não | Não | **Sim** | Não |
| Turno, sexo, cor/raça, financiamento, reserva de vagas | Não | Não nessas abas (existem em 4.5–4.17, 5.4–5.16, 6.4–6.16) | Não | Não |
| IES individual | Não | Não | **Não** | Não |

---

## 6. Qual família serve melhor a um ranking de cursos

**Recomendação: os Anexos I, II e III (tabelas 8.1, 8.2 e 8.3) — "Dados Gerais por Curso".**

Por quê, com base no que foi verificado:

1. É a **única publicação agregada** que tem a dimensão **curso** (`NO_CINE_ROTULO` / `CO_CINE_ROTULO`). Nenhuma tabela 1.x–7.x da Sinopse desce a curso; nenhuma tabela de divulgação tem curso.
2. Contém **as três métricas pedidas** (`INGR*`, `MAT*`, `CONCL*`) mais vagas, inscritos e número de cursos, no mesmo registro.
3. Traz **grau** e **categoria administrativa** como colunas, e **UF/município** como linhas — o que permite ranking por curso dentro de um recorte geográfico e administrativo sem processar microdados.
4. Resolve **modalidade por arquivo** (8.2 presencial, 8.3 EaD, 8.1 ambas), evitando a ambiguidade das tabelas 4.4/5.3/6.3, que somam presencial e EaD.
5. O layout de colunas é **estável** (536 colunas no Anexo I em 2020 e em 2024), o que favorece comparação entre edições.
6. É um XLSX/ODS oficial, publicado anualmente, com hash MD5 no próprio ZIP — dispensa tratar o CSV de 452 MB.

Limitações que precisam constar de qualquer ranking construído sobre os anexos:

- **A agregação geográfica é o município do local de oferta, não a IES.** Em um município com várias IES (ou vários campi), a linha "Direito" soma todas elas. Logo, o anexo permite ranking **curso × município × categoria**, mas **não ranking por instituição**.
- **"Curso" = CINE Rótulo**, um rótulo agregado da classificação CINE/Unesco, e não o projeto pedagógico específico ofertado por uma IES.
- Os anexos cobrem **somente graduação** (não sequenciais de formação específica).
- Não há, nos anexos, turno, sexo, cor/raça, financiamento ou reserva de vagas.
- Contagens, não indicadores de qualidade — para qualidade existem outras publicações (Enade/CPC/IDD), fora do escopo desta nota.

**Quando os anexos não bastam:** se o ranking precisar ser **por IES**, a única fonte oficial é o CSV de microdados (`CO_IES`, `CO_CURSO`, `NO_CURSO`, `QT_CURSO`, `QT_VG_TOTAL`, `QT_INSCRITO_TOTAL`, `QT_ING`, `QT_MAT`, `QT_CONC` — 223 colunas, 720.349 registros em 2024). Isso já não é "tabela agregada publicada", é microdado a agregar.

**Alternativa para série histórica longa e simples:** as `tabelas_de_divulgacao_…_2024.xls` (aba `Tab A`), com 2014–2024 por categoria administrativa — mas só Brasil e sem curso.

---

## 7. Licença, termos de uso e como citar

### 7.1 Licença declarada

Rodapé de todas as páginas do portal do INEP:

> "Todo o conteúdo deste site está publicado sob a licença **Creative Commons Atribuição-SemDerivações 3.0 Não Adaptada**." — com link para `https://creativecommons.org/licenses/by-nd/3.0/deed.pt_BR`.

Implicações práticas (leitura da licença, não aconselhamento jurídico): reuso permitido **com atribuição**, vedada a distribuição de **versão modificada** do conteúdo. Dados factuais e contagens não são, em regra, objeto de direito autoral; ainda assim, a licença do site é BY-ND e deve ser respeitada na reprodução de tabelas e textos.

O portal `gov.br` tem, além disso, o **Termo de Uso** geral (`https://www.gov.br/pt-br/termos-de-uso`), que trata de uso do serviço, responsabilidades e privacidade — **não** concede licença sobre conteúdo e não menciona Creative Commons.

As planilhas da Sinopse **não trazem aba ou cláusula de licença própria**; a única indicação de uso é a aba "Como Citar" (texto em imagem) e o rodapé do site.

### 7.2 Como citar (texto oficial, extraído da aba "Como Citar" da própria planilha 2024)

**ISO:**

> Instituto Nacional de Estudos e Pesquisas Educacionais Anísio Teixeira. *Sinopse Estatística da Educação Superior 2024*. [online]. Brasília: Inep, 2025. [citado aaaa-mm-dd]. Disponível em: <https://www.gov.br/inep/pt-br/acesso-a-informacao/dados-abertos/sinopses-estatisticas/educacao-superior>.

**ABNT:**

> INSTITUTO NACIONAL DE ESTUDOS E PESQUISAS EDUCACIONAIS ANÍSIO TEIXEIRA. *Sinopse Estatística da Educação Superior 2024*. Brasília: Inep, 2025. Disponível em: <https://www.gov.br/inep/pt-br/acesso-a-informacao/dados-abertos/sinopses-estatisticas/educacao-superior>. Acesso em: dd mm. aaaa.

A planilha instrui: "*yyyy-mm-dd e dd mm. yyyy devem ser substituídos pela data da consulta".

### 7.3 Instrumentos legais e datas

- **Portaria nº 493, de 21 de novembro de 2024** — cronograma do Censo da Educação Superior 2024, alterada pela **Portaria nº 43, de 28 de janeiro de 2025** (números citados no índice da página de legislação do INEP; o conteúdo dessa página é carregado dinamicamente e não pôde ser lido em HTML bruto). A publicação da portaria de 2024 no DOU é confirmada pela notícia oficial de 26/11/2024.
- **Portaria nº 771, de 5 de dezembro de 2025** — cronograma do Censo 2025, citada na página "Cronograma" (esta, sim, verificada em texto): coleta de 02/03/2026 a 10/07/2026; "Divulgação do Censo da Educação Superior: até 30/11/2026".
- **Divulgação dos resultados:** Censo 2024 em **22/09/2025** (notícia oficial); Censo 2023 em **03/10/2024** (Nota Informativa 02/2023).
- Base legal da pesquisa: **Lei nº 9.448/1997**, citada na Cartilha de Orientação do Censo.

---

## 8. Lacunas, armadilhas e links quebrados

1. **Link quebrado em documento oficial do próprio INEP.** O Manual do Usuário dos microdados 2024 indica a Sinopse em `https://www.gov.br/inep/pt-br/acesso-a-informacao/dados-abertos/sinopses-estatisticas/educacao-superior-graduacao` — essa URL responde **HTTP 404**. O caminho válido é `…/sinopses-estatisticas/educacao-superior`.
2. **Edição 2025 inexistente até 18/11/2026.** `sinopse_educacao_superior_2025.zip` → 404; páginas `/educacao-superior-2025` → 404. O cronograma prevê divulgação até 30/11/2026, ou seja, pode aparecer nas próximas semanas.
3. **Anexo III de 2024 não foi medido em linhas/colunas** (só o de 2022). O Anexo I de 2024 e os Anexos I/II/III de 2022 foram medidos.
4. **2020 é o ano fora do padrão:** menos abas (79, sem `1.2`, `2.10`, `7.4`) e tabela `3.1` com 220 colunas em vez de 76. Para séries comparáveis, 2021–2024 são homogêneos.
5. **Mapeamento 9 → 2 categorias privadas** não documentado (ver nota na seção 4.4).
6. **Correção oficial de dados:** a `Nota Informativa 02.pdf` (dentro do ZIP 2023) informa que, após a divulgação de 03/10/2024, o INEP identificou variação expressiva em vagas/inscritos dos Institutos Federais (IFMT, código 3164 — de 1.185 para 24.428 vagas em programas especiais) e republicou os dados. Quem usar a edição 2023 deve conferir se baixou a versão corrigida.
7. **Microdados 2024 não têm mais nível de aluno** — só `CURSOS` e `IES`. Comparações com anos anteriores que dependiam do arquivo de aluno exigem tratamento à parte.
8. As abas `Resumo`, `Equipe Técnica`, `Como Citar` e `Dados Gerais` do arquivo principal são **imagens**, não células — automações que leem células as verão vazias.

---

## 9. Fontes

Páginas e arquivos efetivamente consultados (todas em domínios oficiais):

**Índices e páginas institucionais**

- Sinopses Estatísticas — Educação Superior: https://www.gov.br/inep/pt-br/acesso-a-informacao/dados-abertos/sinopses-estatisticas/educacao-superior
- Índice de Sinopses Estatísticas: https://www.gov.br/inep/pt-br/acesso-a-informacao/dados-abertos/sinopses-estatisticas
- Microdados do Censo da Educação Superior: https://www.gov.br/inep/pt-br/acesso-a-informacao/dados-abertos/microdados/censo-da-educacao-superior
- Dados Abertos (INEP): https://www.gov.br/inep/pt-br/acesso-a-informacao/dados-abertos
- Censo da Educação Superior (área de atuação): https://www.gov.br/inep/pt-br/areas-de-atuacao/pesquisas-estatisticas-e-indicadores/censo-da-educacao-superior
- Censo da Educação Superior — Cronograma: https://www.gov.br/inep/pt-br/areas-de-atuacao/pesquisas-estatisticas-e-indicadores/censo-da-educacao-superior/orientacoes/cronograma
- Legislação do Censo da Educação Superior: https://www.gov.br/inep/pt-br/centrais-de-conteudo/legislacao/censo-da-educacao-superior
- Notícia "Inep divulga resultado do Censo Superior 2024" (22/09/2025): https://www.gov.br/inep/pt-br/centrais-de-conteudo/noticias/censo-da-educacao-superior/inep-divulga-resultado-do-censo-superior-2024
- Notícia "Inep publica o cronograma do Censo Superior 2024" (26/11/2024): https://www.gov.br/inep/pt-br/centrais-de-conteudo/noticias/censo-da-educacao-superior/inep-publica-o-cronograma-do-censo-superior-2024
- Cartilha de Orientação do Censo da Educação Superior: https://www.gov.br/inep/pt-br/centrais-de-conteudo/acervo-linha-editorial/publicacoes-institucionais/estatisticas-e-indicadores-educacionais/censo-da-educacao-superior-cartilha-de-orientacao-2026

**Arquivos de dados (ZIPs)**

- Sinopse 2024: https://download.inep.gov.br/informacoes_estatisticas/sinopses_estatisticas/sinopses_educacao_superior/sinopse_educacao_superior_2024.zip
- Sinopse 2023: https://download.inep.gov.br/informacoes_estatisticas/sinopses_estatisticas/sinopses_educacao_superior/sinopse_educacao_superior_2023.zip
- Sinopse 2022: https://download.inep.gov.br/informacoes_estatisticas/sinopses_estatisticas/sinopses_educacao_superior/sinopse_educacao_superior_2022.zip
- Sinopse 2021: https://download.inep.gov.br/informacoes_estatisticas/sinopses_estatisticas/sinopses_educacao_superior/sinopse_educacao_superior_2021.zip
- Sinopse 2020: https://download.inep.gov.br/informacoes_estatisticas/sinopses_estatisticas/sinopses_educacao_superior/sinopse_educacao_superior_2020.zip
- Microdados 2024: https://download.inep.gov.br/microdados/microdados_censo_da_educacao_superior_2024.zip

**Documentos de apoio**

- Notas Estatísticas 2024: https://download.inep.gov.br/publicacoes/institucionais/estatisticas_e_indicadores/notas_estatisticas_censo_escolar_da_educacao_superior_2024.pdf
- Resumo Técnico 2024: https://download.inep.gov.br/publicacoes/institucionais/avaliacoes_e_exames_da_educacao_superior/resumo_tecnico_do_censo_da_educacao_superior_2024.pdf
- Apresentação dos resultados 2024: https://download.inep.gov.br/educacao_superior/censo_superior/documentos/2024/apresentacao_censo_da_educacao_superior_2024.pdf
- Boletim nº 45: https://download.inep.gov.br/educacao_superior/censo_superior/boletins/boletim_n_45_censo_superior.pdf
- Tabelas de divulgação 2024: https://download.inep.gov.br/educacao_superior/censo_superior/documentos/2024/tabelas_de_divulgacao_censo_da_educacao_superior_2024.xls

**Licença e termos**

- Termo de Uso do portal gov.br: https://www.gov.br/pt-br/termos-de-uso
- Licença Creative Commons Atribuição-SemDerivações 3.0 Não Adaptada: https://creativecommons.org/licenses/by-nd/3.0/deed.pt_BR
