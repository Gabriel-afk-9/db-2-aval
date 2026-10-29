# Análise do Repositório — Trabalho de Banco de Dados (TABD)

## 1. Visão geral

O repositório enviado foi analisado com base nos requisitos da prova de **Tópicos em Banco de Dados (TABD)**, considerando o problema **Sistema de Transportadora**, correspondente ao Grupo 6.

O trabalho está bem desenvolvido nos aspectos documental e conceitual. Entretanto, foram identificados alguns pontos que merecem revisão, principalmente na precisão do modelo conceitual e na apresentação do mapeamento NoSQL.

A análise considera o PDF final, os arquivos LaTeX e os diagramas presentes no projeto.

## 2. Requisitos da prova

A prova divide o trabalho em duas tarefas:

| Tarefa | Descrição | Valor |
|---|---|---:|
| Tarefa 1 — MER/DER | Modelar o banco de dados usando o Modelo Entidade-Relacionamento, incluindo os requisitos adicionais e explicando como cada requisito foi modelado. | 4,0 pontos |
| Tarefa 2 — NoSQL | Mapear o MER para um modelo NoSQL e comentar as escolhas feitas para entidades, relacionamentos, hierarquias e agregações. | 6,0 pontos |

Além disso, o trabalho exige:

- Dois requisitos adicionais, descritos e modelados.
- Cada requisito adicional deve gerar uma nova entidade, um novo relacionamento, uma agregação ou uma hierarquia. Não basta acrescentar atributos a entidades existentes.
- Descrição das suposições adotadas quando os requisitos originais não especificarem informações suficientes para a modelagem, como cardinalidades e restrições de participação.
- Entrega de todas as respostas em um único arquivo PDF.
- Inclusão dos nomes dos integrantes do grupo.

A data de entrega indicada na prova é **30/10/2026, até às 23h59**.

---

## 3. Verificação dos requisitos

| Critério | Resultado | Análise |
|---|---|---|
| Identificação do grupo e dos integrantes | Atendido | A capa apresenta os quatro integrantes indicados para o Grupo 6. |
| Tarefa 1 — MER/DER | Atendido | Há diagrama geral, diagramas complementares, entidades, atributos e cardinalidades documentados. |
| Explicação dos requisitos originais | Atendido | Uma tabela relaciona os requisitos do enunciado às decisões de modelagem. |
| Dois requisitos adicionais | Atendido | RA1 cria uma hierarquia de encomendas; RA2 cria uma agregação de embarques com ocorrências. |
| Premissas de modelagem | Atendido | O documento registra 11 premissas, incluindo cardinalidades e regras de negócio. |
| Tarefa 2 — Mapeamento NoSQL | Atendido | O trabalho define oito coleções MongoDB e explica as estratégias de incorporação e referência. |
| Justificativa das escolhas NoSQL | Atendido | Há tabelas, exemplos de documentos, índices e justificativas para as principais decisões. |
| Entrega em um único PDF | Atendido | Existe um PDF final completo no repositório. Esse é o arquivo que deve ser enviado ao SIGAA. |

Esses resultados indicam que o documento cobre formalmente os principais itens exigidos pela prova. Entretanto, a presença de diagramas e tabelas não garante que todas as relações estejam semanticamente corretas. Os pontos a seguir precisam ser considerados.

---

## 4. Problemas encontrados e correções recomendadas

### 4.1. Inconsistência nas cardinalidades do relacionamento ATENDE

**Prioridade: alta**

A premissa P6 afirma que um caminhão atende uma ou mais cidades. Entretanto, a Tabela 2 descreve a participação como parcial nos dois lados, permitindo que um caminhão não atenda a nenhuma cidade.

O diagrama geral indica `(1,N)` para Caminhão e `(0,N)` para Cidade, enquanto a tabela informa participação parcial nos dois lados.

**Como corrigir:**

- Alinhar a tabela e o diagrama com a premissa P6.
- Para Caminhão, o mínimo deve ser 1, caso a premissa P6 seja obrigatória.
- Para Cidade, a participação pode ser parcial, caso existam cidades sem atendimento cadastrado.
- Garantir que a cardinalidade e a restrição de participação sejam apresentadas de forma consistente em todo o documento.

### 4.2. Cardinalidade de EMBARCA incompatível com a premissa P7

**Prioridade: alta**

A premissa P7 afirma que uma nota fiscal pode ter um ou mais embarques. Porém, a Tabela 2 e o diagrama geral apresentam participação parcial nos dois lados do relacionamento EMBARCA, permitindo uma nota fiscal sem embarques.

**Como corrigir:**

- Se a premissa P7 for obrigatória, a participação de NotaFiscal deve exigir pelo menos um embarque.
- A cardinalidade deve ser expressa de maneira consistente na tabela e nos diagramas.
- Caso o grupo queira permitir notas fiscais sem embarques, a premissa P7 deverá ser revista para refletir essa possibilidade.

### 4.3. A visão geral do DER não deixa clara a agregação EMBARCA

**Prioridade: alta**

No diagrama geral, EMBARCA aparece visualmente ligado a Caminhão e Embarque. Entretanto, a descrição conceitual e a Figura 3 apresentam EMBARCA como relacionamento entre NotaFiscal e Caminhão, materializado pela entidade Embarque.

A Figura 3 detalha essa ideia, mas a visão geral pode permitir uma interpretação diferente.

**Como corrigir:**

- Ajustar o DER geral para representar inequivocamente a agregação descrita na Figura 3.
- Garantir que os dois diagramas comuniquem a mesma estrutura conceitual.
- Revisar a ligação entre NotaFiscal, Caminhão, EMBARCA e Embarque, preservando a intenção da modelagem adotada pelo grupo.

### 4.4. Garantias de integridade do NoSQL precisam ser explicadas com mais precisão

**Prioridade: média**

O trabalho explica que um índice único em `notas_fiscais.encomenda_id` garante o relacionamento 1:1 entre nota fiscal e encomenda.

Esse índice impede que várias notas fiscais tenham o mesmo identificador de encomenda. Entretanto, sozinho, ele não garante que toda encomenda tenha uma nota fiscal.

Além disso, o documento mantém referências entre notas fiscais e faturas e um resumo das notas dentro da fatura. Essas informações duplicadas precisam permanecer sincronizadas.

**Como corrigir:**

- Explicar quais regras são asseguradas por índices.
- Informar quais regras dependem de validação da aplicação.
- Explicar como as atualizações entre notas fiscais e faturas serão mantidas consistentes.
- Se necessário, mencionar transações para operações que precisem ser atômicas entre documentos.

---

## 5. Avaliação da modelagem MER/DER

No geral, a modelagem está adequada e apresenta boa cobertura do problema da transportadora.

### 5.1. Entidades e relacionamentos

Foram modelados:

- Cliente;
- Coleta;
- Encomenda;
- NotaFiscal;
- Fatura;
- Caminhão;
- Cidade;
- Região;
- Depósito.

Além dessas entidades, o trabalho contempla estruturas adicionais. Os papéis de remetente, destinatário e pagador também são diferenciados.

### 5.2. Requisito adicional RA1 — Hierarquia de encomendas

A especialização em encomendas comuns, frágeis, perecíveis e perigosas atende ao requisito adicional.

A hierarquia está descrita como total e disjunta, com atributos próprios para cada subtipo.

Esse requisito atende à exigência de criar uma hierarquia, em vez de simplesmente adicionar atributos a uma entidade existente.

### 5.3. Requisito adicional RA2 — Embarques e ocorrências

O requisito cria a entidade Embarque e a entidade Ocorrência, permitindo registrar eventos associados a um trecho específico.

A proposta é coerente com o problema e está explicada na documentação.

### 5.4. Rastreabilidade dos requisitos

Um ponto positivo é a Tabela 3, que relaciona os itens R01 a R12 do enunciado às decisões tomadas no MER.

Essa organização facilita a conferência pelo professor e demonstra como os requisitos originais foram transformados em elementos do modelo conceitual.

---

## 6. Avaliação do mapeamento NoSQL

O mapeamento atende ao que foi solicitado no aspecto conceitual e documental.

A prova não exige explicitamente uma aplicação funcional ou um banco MongoDB executável. Ela exige o mapeamento do MER para NoSQL e comentários sobre as escolhas realizadas.

O trabalho apresenta oito coleções MongoDB:

| Coleção MongoDB | Conteúdo principal |
|---|---|
| `clientes` | Cadastro dos clientes |
| `coletas` | Coletas e encomendas incorporadas |
| `notas_fiscais` | Notas fiscais, embarques e ocorrências |
| `faturas` | Faturas e resumo das notas fiscais |
| `caminhoes` | Caminhões e cidades atendidas |
| `cidades` | Cidades e referência à região |
| `regioes` | Regiões |
| `depositos` | Depósitos e cidade associada |

As escolhas de incorporação de documentos, referências, snapshots, índices e desnormalização são justificadas. Os exemplos JSON ajudam a demonstrar como os dados seriam organizados.

### 6.1. Limitação identificada

O ZIP contém documentação e arquivos-fonte LaTeX para gerar os documentos, mas não foram identificados scripts de criação de coleções, validadores JSON Schema ou código de aplicação que implemente o banco.

Isso **não constitui necessariamente uma falha**, pois a prova não exige explicitamente uma implementação prática. Entretanto, seria necessário complementar o projeto caso o professor também espere um banco executável.

---

## 7. Avaliação estimada

A tabela abaixo apresenta uma estimativa técnica, não uma previsão garantida da nota do professor.

| Tarefa | Valor máximo | Nota estimada |
|---|---:|---:|
| Tarefa 1 — MER/DER | 4,0 | 3,3 |
| Tarefa 2 — NoSQL | 6,0 | 5,0 |
| **Total** | **10,0** | **8,3** |

### Justificativa

A estimativa considera que:

- Os requisitos estão amplamente cobertos.
- Os dois requisitos adicionais estão presentes.
- As duas tarefas foram documentadas.
- Existem inconsistências entre premissas, tabelas e diagramas.
- Algumas garantias de integridade do NoSQL precisam de explicações mais precisas.

A nota pode variar conforme o rigor do professor, a legibilidade dos diagramas e os critérios de correção adotados.

---

## 8. Checklist de correções antes da entrega

- [ ] Corrigir a participação mínima de Caminhão no relacionamento ATENDE.
- [ ] Alinhar a cardinalidade de EMBARCA com a premissa P7.
- [ ] Revisar a visão geral do DER para representar claramente a agregação.
- [ ] Explicar a diferença entre índice único e garantia completa de integridade no MongoDB.
- [ ] Revisar a consistência das referências entre notas fiscais e faturas.
- [ ] Gerar novamente o PDF e conferir a legibilidade dos diagramas.

---

## 9. Veredito final

O trabalho está bem encaminhado e próximo de estar pronto para entrega. Os dois requisitos adicionais estão presentes, as duas tarefas foram documentadas e o PDF está organizado.

Antes de enviar, recomendo priorizar as três primeiras correções, pois elas afetam diretamente a coerência do MER/DER.

O mapeamento NoSQL também merece uma revisão das garantias de integridade, principalmente para deixar claro como o sistema manteria a consistência entre documentos relacionados.

**Conclusão:** o repositório atende à maior parte do que a prova exige, mas recomendo corrigir as inconsistências identificadas antes de enviar a versão final ao SIGAA.

**Nota estimada atual: 8,3/10,0.**
