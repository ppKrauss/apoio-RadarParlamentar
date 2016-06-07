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



