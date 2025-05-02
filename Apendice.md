
# Prompts da Indexação

## Prompt para Extração de Entidades do Tipo Documento

**Objetivo**

Dado um documento de texto, descubra qual o nome do documento e resuma-o em no máximo 100 palavras. Caso o documento cite outro documento, inclua essa informação no resumo.

No nome do documento, inclua somente o tipo e o identificador. Exemplo: `PORTARIA Nº 13/1998 - SEC-CEXTERNO` terá o nome `PORTARIA 13/1998`.

A saída deve ter o formato:
```
{{initial_delimiter}}nome|||resumo{{completion_delimiter}}
```
Retorne em português. Use `{{initial_delimiter}}` como token inicial e `{{completion_delimiter}}` como token final.

### Exemplo

**Entrada:**
```
RESOLUÇÃO Nº 68/1965

-Vide Resolução nº 99/1965, de 18-03-1965.

Instruções sobre Fundos Rotativos, mandadas observar no Tribunal de Contas.

O TRIBUNAL DE CONTAS DO ESTADO DE GOIÁS...
```

**Saída:**
```
{{initial_delimiter}}RESOLUÇÃO 68/1965|||A Resolução nº 68/1965, do Tribunal de Contas do Estado de Goiás, estabelece normas regimentais para a gestão e fiscalização de Fundos Rotativos, incluindo instruções para pagamentos, movimentação e julgamento das prestações de contas trimestrais, conforme o artigo 31 da Constituição Estadual.{{completion_delimiter}}
```

## Prompt para o Pré-Processamento de Texto

**Objetivo**

Dado um texto jurídico, identifique as seções e altere o nome dos artigos especificando o documento ao qual pertencem.

**Etapas**

1. Identificar as seções: Introdução, Artigo 1, Artigo 2, ..., Artigo N e Conclusão.  
2. Nas seções de artigos, renomear cada artigo para: `Art. N do DOCUMENTO: <texto>`.  
3. Retornar o texto em português, usando `{{initial_delimiter}}` no início e `{{completion_delimiter}}` no final.

### Exemplo

**Entrada:**
```
TRIBUNAL DE CONTAS DO ESTADO DE GOIÁS  

PORTARIA Nº 1/2024 - SEC-CEXTERNO  

Designa equipe de fiscalização para realização de Acompanhamento da concessão do Parque Estadual da Serra de Caldas Novas – PESCaN.  

Art.1º Designar os servidores Letícia da Silva Manchini e Valdo de Sousa Filho, sob a coordenação de Vânia Mara de Souza e Silva.  
Art.2º Estabelecer a data de 30/04/2024 para entrega do Relatório final de fiscalização.  
CUMPRA-SE E PUBLIQUE-SE.  

SECRETARIA DE CONTROLE EXTERNO DO TRIBUNAL DE CONTAS DO ESTADO DE GOIÁS em Goiânia aos 15 de fevereiro de 2024.
```

**Saída:**
```
{{initial_delimiter}}
TRIBUNAL DE CONTAS DO ESTADO DE GOIÁS  

PORTARIA Nº 1/2024 - SEC-CEXTERNO  

Designa equipe de fiscalização para realização de Acompanhamento da concessão do Parque Estadual da Serra de Caldas Novas – PESCaN.  

Art.1º da Portaria Nº 1/2024: Designar os servidores Letícia da Silva Manchini e Valdo de Sousa Filho, sob a coordenação de Vânia Mara de Souza e Silva.  
Art.2º da Portaria Nº 1/2024: Estabelecer a data de 30/04/2024 para entrega do Relatório final de fiscalização.  
CUMPRA-SE E PUBLIQUE-SE.  

SECRETARIA DE CONTROLE EXTERNO DO TRIBUNAL DE CONTAS DO ESTADO DE GOIÁS em Goiânia aos 15 de fevereiro de 2024.
{{completion_delimiter}}
```

## Prompt para Extração de Entidades do Tipo Artigo

**Objetivo**

Dado um documento, identifique entidades do tipo artigo e as citações entre elas.

**Etapas**

1. Identificar todas as entidades do tipo artigo, sem inventar.  
2. Nomear cada entidade como `Artigo N do DOCUMENTO XX/YYYY`.  
3. Para cada entidade, extrair:
   - `entity_name`
   - `entity_type` (sempre `artigo`)
   - `entity_description`  
4. Formatar cada registro como:
```
entity|||<entity_name>|||<entity_type>|||<entity_description>{{record_delimiter}}
```
5. Retornar em português, usando `{{initial_delimiter}}`, `{{record_delimiter}}` e `{{completion_delimiter}}`.

## Prompt para Concatenação de Entidades

**Objetivo**

Dada uma lista de entidades estruturada como `classe|||id|||nome|||tipo|||descrição`, resuma cada descrição para no máximo N caracteres, sem duplicações.

**Etapas**

1. Resumir cada descrição para no máximo `{entity_extraction_char_limit}` caracteres.  
2. Estruturar como:
```
<classe>|||<id>|||<nome>|||<tipo>|||<nova_descrição>{{record_delimiter}}
```
3. Retornar em português, usando `{{initial_delimiter}}`, `{{record_delimiter}}` e `{{completion_delimiter}}`.

## Prompt para Extração de Triplas

**Objetivo**

Dado um texto e uma lista de entidades, identifique as relações claras entre pares de entidades.

**Etapas**

1. Para cada par relacionado, extrair:
   - `entidade_fonte`
   - `entidade_alvo`
   - `descrição_relacionamento`  
2. Formatar como:
```
relação|||<id_fonte>|||<entidade_fonte>|||<id_alvo>|||<entidade_alvo>|||<descrição>{{record_delimiter}}
```
3. Retornar em português, usando `{{initial_delimiter}}`, `{{record_delimiter}}` e `{{completion_delimiter}}`.

## Prompt para Filtragem e Classificação de Triplas

**Objetivo**

Filtrar e classificar relações jurídicas em: contém, altera, revoga ou retifica.

**Etapas**

1. Remover duplicatas.  
2. Descartar relações de documento com artigo de outro documento.  
3. Classificar cada relação:  
   - contém  
   - altera  
   - revoga  
   - retifica  
4. Formatar como:
```
relação|||<id_fonte>|||<entidade_fonte>|||<id_alvo>|||<entidade_alvo>|||<classificação>{{record_delimiter}}
```
5. Retornar em português, usando `{{initial_delimiter}}`, `{{record_delimiter}}` e `{{completion_delimiter}}`.
