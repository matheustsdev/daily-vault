---
Tipo: Vila
Estado:
---

<% await tp.file.move("/RPG/Soleia - World/Locais/Vilas/" + tp.file.title) %>
<%*
const title = await tp.system.prompt("Título");
await tp.file.rename(title);
%>


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
