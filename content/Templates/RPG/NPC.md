---
Local: 
Descrição: 
Facção:
---
<% await tp.file.move("/RPG/Soleia - World/NPCs/" + tp.file.title) %>
<%*
const title = await tp.system.prompt("Título");
await tp.file.rename(title);
%>

# Descrição


# Objetivo


# Motivação


# Vínculos



# Ficha

```statblock  
name: <% tp.file.title %>  
size: string  
type: string  
subtype: string  
alignment: string  
ac: number  
hp: number  
hit_dice: string  
speed: string  
stats: [number, number, number, number, number, number]    
saves:  
- <ability-score>: number  
skillsaves:  
- <skill-name>: number  
damage_vulnerabilities: string  
damage_resistances: string  
damage_immunities: string  
condition_immunities: string  
senses: string  
languages: string  
cr: number  
spells:  
- <description>  
- <spell level>: <spell-list>  
traits:  
- [<trait-name>, <trait-description>]  
- ...  
actions:  
- [<trait-name>, <trait-description>]  
- ...  
```
