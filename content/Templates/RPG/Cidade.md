---
Tipo: Cidade
Estado:
---
<% await tp.file.move("/RPG/Soleia - World/Locais/Cidades/" + tp.file.title) %>
<%*
const title = await tp.system.prompt("Título");
await tp.file.rename(title);
%>

**Visão Geral:**

*   Uma breve descrição da cidade (1-2 parágrafos). Incluir:
    *   Localização geográfica (em qual reino/região está localizada).
    *   Importância (centro comercial, militar, religioso, etc.).
    *   Atmosfera geral (próspera, decadente, pacífica, caótica, etc.).

**Governo:**

*   Tipo de governo (monarquia, conselho, teocracia, etc.).
*   Quem governa (nomes, títulos, descrição breve).
*   Como o poder é exercido (leis, costumes, influência).

**Submundo:**

*   Atividades ilegais comuns (contrabando, jogos de azar, roubo, etc.).
*   Grupos criminosos atuantes (nomes, descrição breve, áreas de influência).
*   Corrupção (nível de influência na cidade, quem é corrupto).

**Religião e Templos:**

*   Deuses principais cultuados na cidade.
*   Templos importantes (nomes, descrição breve, importância).
*   Seitas ou cultos incomuns (descrição breve, atividades).

**Recursos:**

*   Principais fontes de renda da cidade (comércio, agricultura, mineração, etc.).
*   Recursos naturais importantes na região.
*   Produção artesanal (itens produzidos na cidade, guildas envolvidas).

**Defesas:**

*   Muralhas (estado de conservação, pontos fortes e fracos).
*   Guarda municipal (tamanho, treinamento, equipamento, moral).
*   Outras defesas (milícias, guardas de guildas, magia defensiva).

**Social:**

*   Composição da população (raças, classes sociais, culturas).
*   Costumes e tradições importantes.
*   Problemas sociais (pobreza, criminalidade, tensões raciais, etc.).
*   Eventos importantes (festivais, feiras, celebrações).

**Tavernas e Hospedarias:**

```dataview
TABLE descrição
FROM "Locais/Tavernas"
WHERE contains(localização, this.file.name)

# Governo


# Submundo


# Religião e templos


# Recursos


# Defesas


# Social
## Tavernas
```dataview
TABLE Tipo FROM "Soleia - World/Tavernas"
WHERE Local = [[]]
```

# NPCs
```dataview
TABLE Descrição FROM "Soleia - World/NPCs"
WHERE Local = [[]]
```
