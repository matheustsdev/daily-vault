---
Ticket: 466065
Status: Em análise
Data de início: 2025-01-15
Data de fim: ""
---

# Ordem
TECH
1. Gravel, ProductGroup, Supply, Supplier, ConcreteRecipeCharacteristicsType
2. SlumpClass, SlumpUnitType,
3. Slump
4. ConcreteBatchingPlantGroup
5. ConcreteBatchingPlantRegional
6. ConcreteBatchingPlant
7. ConcreteRecipeFamilyVersion
8. ConcreteRecipe

LEGADO
1. GerGeral
2. ConEquipamento
3. ConFuncionario
4. ConFuncComplem
5. GerCondPag
6. GerMunicipio
9. FisFilial
9. Contract (ULTIMO)
10. ConPropostaItem
11. ConReajusteItemreg

# Processos

1. Criar tratativa no apim para conversão de Subscription key para o tenant relacionado;
2. 

![[Pasted image 20250115180513.png]]
Geovane falou que é legal limpar a ger_db_events pra evitar lixos

## Comandos para forçar integração no legado

```sql
UPDATE `topsys`.`ger_geral` SET `id_atual`=`id_atual`;  
UPDATE `topsys`.`ger_interv` SET `id_atual`=`id_atual`;  
UPDATE `topsys`.`ger_feriado` SET `id_atual`=`id_atual`;  
UPDATE `topsys`.`ger_municipio` SET `id_atual`=`id_atual`;  
UPDATE `topsys`.`ger_cond_pag` SET `id_atual`=`id_atual`;  
UPDATE `topsys`.`ger_tribcontrib` SET `IdAtual`=`IdAtual`;  
UPDATE `topsys`.`con_funcionario` SET `id_atual`=`id_atual`;  
UPDATE `topsys`.`con_vendedor` SET `id_atual`=`id_atual`;  
UPDATE `topsys`.`con_equipamento` SET `id_atual`=`id_atual`;  
UPDATE `topsys`.`con_peca_concretar` SET `id_atual`=`id_atual`;  
UPDATE `topsys`.`fis_mercadoria` SET `id_atual`=`id_atual`;  
UPDATE `topsys`.`fis_cfop` SET `id_atual`=`id_atual`;  
UPDATE `topsys`.`fis_obs_fiscal` SET `id_atual`=`id_atual`;  
UPDATE `topsys`.`fis_oper_fiscal` SET `id_atual`=`id_atual`;  
UPDATE `topsys`.`fis_filial` SET `id_atual`=`id_atual`;  
UPDATE `topsys`.`con_usina` SET `id_atual`=`id_atual`;  
UPDATE `topsys`.`con_proposta_item` SET `id_atual`=`id_atual`;  
UPDATE `topsys`.`con_contrato` SET `id_atual`=`id_atual`;  
UPDATE `topsys`.`con_reajuste_item` SET `id_atual`=`id_atual`;
```



# Observações

