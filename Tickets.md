# Hoje
```dataview
TABLE Ticket, Status, date(today) FROM "Tickets" SORT Ticket WHERE row["Data de início"] = date(today) OR row["Data de fim"] = date(today)
```

# Ontem
```dataview
TABLE Ticket, Status FROM "Tickets" SORT Ticket WHERE 
((row["Data de início"] = date(yesterday) OR row["Data de fim"] = date(yesterday)) AND date(yesterday).weekday != 7) 
OR ((row["Data de início"] = (date(yesterday) - dur(2 days)) OR row["Data de fim"] = (date(yesterday) - dur(2 days))) AND date(yesterday).weekday = 7)
```
# Em análise
```dataview
TABLE Ticket, Status FROM "Tickets" SORT Ticket WHERE Status != "Concluído"
```

# Concluídos
```dataview
TABLE Ticket, Status FROM "Tickets" SORT "Data de início" DESC WHERE Status = "Concluído"
```
