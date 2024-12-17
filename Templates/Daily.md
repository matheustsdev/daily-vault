<%* 
moment.locale("pt-br")

const newName = moment(tp.file.title, "YYYY-MM-DD").format("DD [de] MMM");
tp.system.prompt(newName)
tp.file.rename(newName)
%>

- [ ] 