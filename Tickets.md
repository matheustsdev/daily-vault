
```dataview
TABLE Ticket, Status FROM "Tickets" SORT Ticket WHERE Status = "Concluído" AND
```

# Em análise
```dataview
TABLE Ticket, Status FROM "Tickets" SORT Ticket WHERE Status != "Concluído"
```

# Concluídos
```dataview
TABLE Ticket, Status FROM "Tickets" SORT Ticket WHERE Status = "Concluído"
```
