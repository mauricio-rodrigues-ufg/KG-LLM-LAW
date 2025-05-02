
# Indexing Prompts

## Prompt for Document-Type Entity Extraction

**Objective**

Given a text document, determine the document's name and summarize it in up to 100 words. If the document cites another document, include that information in the summary.

In the document name, include only the type and the identifier. Example: `PORTARIA Nº 13/1998 - SEC-CEXTERNO` should have the name `PORTARIA 13/1998`.

The output must follow the format:
```
{{initial_delimiter}}name|||summary{{completion_delimiter}}
```
Return in Portuguese. Use `{{initial_delimiter}}` as the start token and `{{completion_delimiter}}` as the end token.

### Example

**Input:**
```
RESOLUÇÃO Nº 68/1965

-Vide Resolução nº 99/1965, de 18-03-1965.

Instruções sobre Fundos Rotativos, mandadas observar no Tribunal de Contas.

O TRIBUNAL DE CONTAS DO ESTADO DE GOIÁS...
```

**Output:**
```
{{initial_delimiter}}RESOLUÇÃO 68/1965|||The Resolution No. 68/1965 of the Tribunal de Contas do Estado de Goiás establishes the regimental guidelines for the management and oversight of Revolving Funds, including instructions for payments, transactions, and the quarterly audit of financial statements, in accordance with Article 31 of the State Constitution.{{completion_delimiter}}
```

## Prompt for Text Preprocessing

**Objective**

Given a legal text, identify its sections and rename the articles by specifying the document to which they belong.

**Steps**

1. Identify the sections: Introduction, Article 1, Article 2, ..., Article N, and Conclusion.  
2. In the article sections, rename each article to: `Art. N of DOCUMENT: <text>`.  
3. Return the text in Portuguese, using `{{initial_delimiter}}` at the beginning and `{{completion_delimiter}}` at the end.

### Example

**Input:**
```
TRIBUNAL DE CONTAS DO ESTADO DE GOIÁS  

PORTARIA Nº 1/2024 - SEC-CEXTERNO  

Designa equipe de fiscalização para realização de Acompanhamento da concessão do Parque Estadual da Serra de Caldas Novas – PESCaN.  

Art.1º Designar os servidores Letícia da Silva Manchini e Valdo de Sousa Filho, sob a coordenação de Vânia Mara de Souza e Silva.  
Art.2º Estabelecer a data de 30/04/2024 para entrega do Relatório final de fiscalização.  
CUMPRA-SE E PUBLIQUE-SE.  

SECRETARIA DE CONTROLE EXTERNO DO TRIBUNAL DE CONTAS DO ESTADO DE GOIÁS em Goiânia aos 15 de fevereiro de 2024.
```

**Output:**
```
{{initial_delimiter}}
TRIBUNAL DE CONTAS DO ESTADO DE GOIÁS  

PORTARIA Nº 1/2024 - SEC-CEXTERNO  

Designates the inspection team responsible for monitoring the concession of the Serra de Caldas Novas State Park – PESCaN.  

Art.1º of Portaria 1/2024: Assign Letícia da Silva Manchini and Valdo de Sousa Filho, under the coordination of Vânia Mara de Souza e Silva.  
Art.2º of Portaria 1/2024: Set the date of 04/30/2024 for submission of the final inspection report.  
COMPLY AND PUBLISH.  

SECRETARIAT OF EXTERNAL CONTROL OF THE TRIBUNAL DE CONTAS DO ESTADO DE GOIÁS in Goiânia on February 15, 2024.
{{completion_delimiter}}
```

## Prompt for Article-Type Entity Extraction

**Objective**

Given a document, identify entities of type "article" and their inter-citations.

**Steps**

1. Identify all entities of type "article" without inventing any.  
2. Name each entity as `Article N of DOCUMENT XX/YYYY`.  
3. For each entity, extract:
   - `entity_name`
   - `entity_type` (always `article`)
   - `entity_description`  
4. Format each record as:
```
entity|||<entity_name>|||<entity_type>|||<entity_description>{{record_delimiter}}
```
5. Return in Portuguese, using `{{initial_delimiter}}`, `{{record_delimiter}}`, and `{{completion_delimiter}}`.

## Prompt for Entity Concatenation

**Objective**

Given a list of entities structured as `class|||id|||name|||type|||description`, summarize each description to a maximum of N characters without duplication.

**Steps**

1. Summarize each description to at most `{entity_extraction_char_limit}` characters.  
2. Structure as:
```
<class>|||<id>|||<name>|||<type>|||<new_description>{{record_delimiter}}
```
3. Return in Portuguese, using `{{initial_delimiter}}`, `{{record_delimiter}}`, and `{{completion_delimiter}}`.

## Prompt for Triple Extraction

**Objective**

Given a text and a list of entities, identify clear relationships between pairs of entities.

**Steps**

1. For each related pair, extract:
   - `source_entity`
   - `target_entity`
   - `relationship_description`  
2. Format as:
```
relation|||<source_id>|||<source_entity>|||<target_id>|||<target_entity>|||<description>{{record_delimiter}}
```
3. Return in Portuguese, using `{{initial_delimiter}}`, `{{record_delimiter}}`, and `{{completion_delimiter}}`.

## Prompt for Triple Filtering and Classification

**Objective**

Filter and classify legal relations into: contains, amends, repeals, or rectifies.

**Steps**

1. Remove duplicates.  
2. Discard relations between a document and an article of another document.  
3. Classify each relationship as:
   - contains  
   - amends  
   - repeals  
   - rectifies  
4. Format as:
```
relation|||<source_id>|||<source_entity>|||<target_id>|||<target_entity>|||<classification>{{record_delimiter}}
```
5. Return in Portuguese, using `{{initial_delimiter}}`, `{{record_delimiter}}`, and `{{completion_delimiter}}`.
