---
Tarefa: TDP-1798
Desenvolvedor: Eu
Método de code review:
  - Misto
Responsabilidade:
  - Desenvolver
Data de início: 2025-06-03
Data de fim: 2025-07-02
---
# Quebra técnica (49h/36h)

- [x] Adicionar inserção de PumpingType usando o Maintenance (6h)
- [x] Adicionar inserção de PieceToConcrete usando o Maintenance (6h)
- [x] Adicionar inserção de duração de descarga por tipo de bomba e peça a concretar (10h)
- [x] Adicionar validação dos horários de descarga de acordo com o cadastrado (4h)
- [x] Cálculo do valor da bomba (1h)
----
- [x] Adicionar boolean para identificar que a bomba é do cliente na Construction (1h)
- [x] Exibir valor de bomba do cliente por padrão de acordo o registrado na obra na programação (pode ser alterado) (1h)
- [x] Cadastro de veículos (6h)
- [x] Cadastro de motorista e bombistas, auxiliares (4h)
- [x] Adicionar compartilhamento de recurso entre centrais na tela de centrais e ajustar alocações (6h)
- [x] Adicionar tipo de bomba nos dados da programação (1h)
- [x] Adicionar campo para informar betoneira própria ou não na programação (1h)
- [ ] Adicionar validações em campos abertos dos formulários para o tamanho no banco (2h)
- [ ] Alterar parâmetro geral para outra tela (1h)
## Duvidas

- Na opção de "Usa própria bomba", deve ser nulo quando não houver bombeamento? Usar como padrão o NÃO e adicionar um info de "Bomba sdjhfsdfjkghdf"
- Caso a opção seja próprio do cliente, pode ser ocultado o botão de selecionar bomba?
- Quais são os recursos compartilhados entre as centrais além de alocação?
---
Correções
- Remover veiculo de apoio dos tipos
- Colocar capacidade (m³) no titulo
- Remover obrigatoriedade da carteira de motorista/data

```sql
UPDATE "__EFMigrationsHistory" eh 
SET eh."MigrationId" = '20250605115935_CreateEntity-DefaultPumpingTypeDuration'
WHERE eh."MigrationId" = '20250605115935_CreateEntity-DefaultUnloadingDuration';

ALTER TABLE reg_default_unloading_duration
RENAME TO reg_default_pumping_type_durations;

ALTER TABLE reg_default_pumping_type_durations
RENAME COLUMN id_default_unloading_duration TO id_default_pumping_type_duration;
```
# Casos de teste

- [ ] 
- [ ] 
- [ ] 



