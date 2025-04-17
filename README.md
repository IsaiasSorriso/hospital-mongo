# **Sistema de Gestão Hospitalar com MongoDB**  

Este projeto implementa um **banco de dados para um hospital** utilizando **MongoDB**, substituindo sistemas baseados em planilhas e arquivos desatualizados. O banco de dados foi projetado para gerenciar informações de médicos, pacientes, consultas, planos de saúde, prescrições e exames, garantindo **eficiência, escalabilidade e integridade dos dados**.  

---

## **📌 Estrutura do Banco de Dados**  

O sistema é composto pelas seguintes **coleções** (equivalentes a tabelas em bancos relacionais):  

1. **`medicos`**  
   - Armazena informações dos médicos (CRM, nome, especialidade, contato, horários de atendimento).  
   - Exemplo:  
     ```javascript
     {
       _id: ObjectId("..."),
       crm: "123456/SP",
       nome: "Dr. João Silva",
       especialidades: ["Cardiologia", "Clínico Geral"],
       telefone: "(11) 99999-9999"
     }
     ```  

2. **`pacientes`**  
   - Contém dados pessoais, histórico médico e informações de contato dos pacientes.  
   - Exemplo:  
     ```javascript
     {
       _id: ObjectId("..."),
       cpf: "123.456.789-00",
       nome: "Maria Oliveira",
       historicoMedico: {
         alergias: ["Penicilina"],
         doencasCronicas: ["Hipertensão"]
       }
     }
     ```  

3. **`consultas`**  
   - Registra consultas médicas, vinculando pacientes e médicos.  
   - Exemplo:  
     ```javascript
     {
       _id: ObjectId("..."),
       paciente: ObjectId("..."),
       medico: ObjectId("..."),
       dataHora: ISODate("2024-03-20T14:30:00Z"),
       motivo: "Dor no peito",
       status: "Concluída"
     }
     ```  

4. **`planos_saude`**  
   - Armazena informações sobre os planos de saúde disponíveis.  
   - Exemplo:  
     ```javascript
     {
       _id: ObjectId("..."),
       nome: "Saúde Total",
       cobertura: ["Consultas", "Exames", "Internação"]
     }
     ```  

5. **`prescricoes`**  
   - Registra medicamentos prescritos em consultas.  
   - Exemplo:  
     ```javascript
     {
       _id: ObjectId("..."),
       consulta: ObjectId("..."),
       medicamentos: [
         { nome: "Paracetamol", dosagem: "500mg" }
       ]
     }
     ```  

6. **`exames`**  
   - Armazena resultados de exames solicitados em consultas.  
   - Exemplo:  
     ```javascript
     {
       _id: ObjectId("..."),
       paciente: ObjectId("..."),
       tipo: "Hemograma",
       resultado: { hemoglobina: "14 g/dL" }
     }
     ```  

---

## **🔍 Queries e Análises**  

O sistema permite **consultas avançadas** para extrair informações úteis, como:  

### **1. Pacientes com Consultas em um Período**  
```javascript
db.consultas.aggregate([
  { $match: { dataHora: { $gte: ISODate("2024-01-01") } } },
  { $lookup: { from: "pacientes", localField: "paciente", foreignField: "_id", as: "paciente" } }
])
```  
**Saída:** Lista de pacientes com consultas em 2024.  

### **2. Médicos com Mais Atendimentos**  
```javascript
db.consultas.aggregate([
  { $group: { _id: "$medico", total: { $sum: 1 } } },
  { $sort: { total: -1 } }
])
```  
**Saída:** Ranking de médicos por número de consultas.  

### **3. Prescrições Mais Comuns**  
```javascript
db.prescricoes.aggregate([
  { $unwind: "$medicamentos" },
  { $group: { _id: "$medicamentos.nome", total: { $sum: 1 } } }
])
```  
**Saída:** Medicamentos mais prescritos.  

---

## **⚙️ Como Usar**  

### **1. Inserindo Dados**  
```javascript
// Inserir um médico
db.medicos.insertOne({
  crm: "654321/RJ",
  nome: "Dra. Ana Souza",
  especialidades: ["Pediatria"]
})
```  

### **2. Consultando Dados**  
```javascript
// Buscar pacientes com hipertensão
db.pacientes.find({
  "historicoMedico.doencasCronicas": "Hipertensão"
})
```  

### **3. Atualizando Registros**  
```javascript
// Atualizar telefone de um paciente
db.pacientes.updateOne(
  { cpf: "123.456.789-00" },
  { $set: { telefone: "(11) 98888-7777" } }
)
```  

### **4. Deletando Dados**  
```javascript
// Remover uma consulta cancelada
db.consultas.deleteOne({ _id: ObjectId("...") })
```  

---

## **📊 Benefícios do Sistema**  

✅ **Dados centralizados** (evita planilhas desorganizadas)  
✅ **Consultas rápidas** (filtros por paciente, médico, data)  
✅ **Escalável** (suporta milhares de registros)  
✅ **Seguro** (controle de acesso por usuário)  

---

## **📌 Conclusão**  
Este banco de dados em **MongoDB** resolve os principais problemas de hospitais que usam planilhas, oferecendo **organização, eficiência e análises avançadas**.  

**Dúvidas?** Consulte a documentação do MongoDB ou abra uma *issue* no repositório.  

📌 **Repositório GitHub:** [github.com/IsaiasSorriso/hospital-mongo](https://github.com/IsaiasSorriso/hospital-mongo)  

--- 

**🎯 Objetivo do Projeto:**  
Modernizar a gestão hospitalar com um banco de dados eficiente e flexível.
