---
Ticket: 466065
Status: Em análise
Data de início: 2025-01-15
Data de fim: ""
---

# Ordem
LEGADO
1. GerGeral
2. ConEquipamento
3. ConFuncionario
4. ConFuncComplem
5. GerCondPag
6. GerMunicipio

TECH
1. Gravel, ProductGroup, Supply, Supplier, ConcreteRecipeCharacteristicsType
2. SlumpClass, SlumpUnitType,
3. Slump
4. ConcreteBatchingPlantGroup
5. ConcreteBatchingPlantRegional
6. ConcreteBatchingPlant
7. ConcreteRecipeFamilyVersion
8. ConcreteRecipe


9. Contract (ULTIMO)

OCP KEY TECH: c6e465a0a75241d283cb3eb9341f669f

TmhueVVKQUJmX3JDclBKZ0d1OVU6YkxDVXJGWU1UaEtWZ0FyRkJpSEM0UQ==

# Ações

![[Pasted image 20250115180513.png]]
Geovane falou que é legal limpar a ger_db_events pra evitar lixos

## Force update
- UPDATE `topsys`.`ger_feriado` SET `municipio`=`municipio` where municipio = 0 AND data > '2000-01-01';
- 

Tive que add na id_integrations:
- ger_interv;

Rotas do TECH:
- 