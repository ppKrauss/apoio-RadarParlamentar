# apoio-RadarParlamentar
dados de apoio


Dados copiados [desta Planilha GDoc](https://docs.google.com/spreadsheets/d/1N7dz8-d545y6ybuECacq3yWf0N97u-2jyntRn_QiWoE/).

Ver issue 273 e sua [wiki](https://github.com/radar-parlamentar/radar/wiki/Convers%C3%A3o-de-contexto-em-URN-LEX) 

-----

# SQL

## Parte1

Alterações e updates na tabela da casa legislativa, deixando consistente com a presente [modelagem_casalegislativa.csv](modelagem_casalegislativa.csv):
```sql
ALTER TABLE modelagem_casalegislativa ADD COLUMN local_lex text CHECK(char_length(local_lex)<200);
ALTER TABLE modelagem_casalegislativa ADD COLUMN autoridade_lex text CHECK(char_length(autoridade_lex)<200);

UPDATE modelagem_casalegislativa SET local_lex='br;sao.paulo;sao.paulo', autoridade_lex='camara.municipal' WHERE id=4;
UPDATE modelagem_casalegislativa SET local_lex='br', autoridade_lex='senado.federal' WHERE id=5;
UPDATE modelagem_casalegislativa SET local_lex='fr', autoridade_lex='federal' WHERE id=7;
UPDATE modelagem_casalegislativa SET local_lex='br', autoridade_lex='camara.deputados' WHERE id=23;
	
-- SELECT id, nome, nome_curto, esfera, local, local_lex, autoridade_lex FROM public.modelagem_casalegislativa;
-- mesmo que https://github.com/ppKrauss/apoio-RadarParlamentar/blob/master/modelagem_casalegislativa.csv

```
## Parte2
Gerando referencial de prefixos relevantes para os dados arquivados no Radar, 
```sql
SELECT DISTINCT  c.local_lex ||':'||autoridade_lex as local_autoridade, 'proposicao' AS escopo, sigla
FROM   modelagem_casalegislativa c INNER JOIN  modelagem_proposicao p
       ON p.casa_legislativa_id = c.id
ORDER BY 1,2,3
```
a partir destes, manualmente, via GDoc, foram compostos os relacionamentos completos com a coluna `urn_lex` de [urnlex_prefixos.csv](urnlex_prefixos.csv).


-----

# Abreviações para a tabela de prefixos
Caracterização de uma proposição na autoridade, e conjunto de abreviações para os tipos de proposição fixados pela autoridade.

## Na Câmara dos Depudados
Conforme [Manual de Redação da  Câmara dos deputados](http://bd.camara.gov.br/bd/bitstream/handle/bdcamara/5684/manual_redacao.pdf),

PROPOSIÇÃO: toda matéria sujeita à deliberação da Casa. Considera-se proposição a proposta de emenda à Constituição, os projetos, a emenda, a indicação, o requerimento, o recurso, o parecer, e a proposta de fiscalização e controle.

São as seguintes as siglas das proposições legislativas na Câmara dos Deputados:

* COM - Consulta solicitada por parlamentar a comissão técnica
* DCR - Denúncia por crime de responsabilidade
* DEN - Denúncia
* DTQ - Destaque
* DVS - Destaque para Votação em Separado
* DVT - Declaração de Voto
* EMC - Emenda Apresentada na Comissão
* EMD - Emenda
* EML - Emenda à LDO
* EMO - Emenda ao Orçamento
* EMP - Emenda de Plenário
* EMR - Emenda de Relator
* Sem - Emenda/Substitutivo do Senado
* ERD - Emenda de Redação
* ESB - Emenda ao Substitutivo
* EXP - Exposição
* INA - Indicação de Autoridade
* INC - Indicação
* MPV - Medida Provisória
* MSC - Mensagem
* PAR - Parecer de Comissão
* PDC - Projeto de Decreto Legislativo
* PEC - Proposta de Emenda à Constituição
* PET - Petição
* PFC - Proposta de Fiscalização e Controle
* PL - Projeto de Lei
* PLP - Projeto de Lei Complementar
* PLV - Projeto de Lei de Conversão
* PRC - Projeto de Resolução (CD)
* PRF - Projeto de Resolução do Senado Federal
* PRN - Projeto de Resolução (CN)
* PRO - Proposta
* RCP - Requerimento de Instituição de Comissão Parlamentar de Inquérito
* REC - Recurso
* REL - Relatório
* REM - Reclamação
* REP - Representação
* REQ - Requerimento
* RIC - Requerimento de Informação
* RPR - Representação
* SBE - Subemenda
* SBT - Substitutivo
* SDL - Sugestão de Emenda à LDO
* SIT - Solicitação de Informação ao TCU
* SOA - Sugestão de Emenda ao Orçamento
* STF - Ofício dirigido à Câmara dos Deputados, em que o Supremo Tribunal Federal solicita da Casa alguma ação legislativa
* SUG - Sugestão de entidade da sociedade civil à Câmara, para que adote alguma ação legislativa
* SUM - Súmula de jurisprudência emitida pela CCJC
* TER - Termo de Implementação
* TVR - Ato do Poder Executivo que submete à Câmara a concessão de serviços de radiodifusão, sonora e de imagens
* VTS - Voto em Separado

