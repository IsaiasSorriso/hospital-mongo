# **Sistema de Gestão Hospitalar com MongoDB**  

Este projeto implementa um **banco de dados para um hospital** utilizando **MongoDB**, substituindo sistemas baseados em planilhas e arquivos desatualizados. O banco de dados foi projetado para gerenciar informações de médicos, pacientes, consultas, planos de saúde, prescrições e exames, garantindo **eficiência, escalabilidade e integridade dos dados**.  

---

## **📌 Estrutura do Banco de Dados**  

O sistema é composto pelas seguintes **coleções** (equivalentes a tabelas em bancos relacionais):  

1. **`medicos`**  
   - Armazena informações dos médicos (CRM, nome, especialidade, contato, horários de atendimento).  

2. **`pacientes`**  
   - Contém dados pessoais, histórico médico e informações de contato dos pacientes.  

3. **`consultas`**  
   - Registra consultas médicas, vinculando pacientes e médicos.  

4. **`planos_saude`**  
   - Armazena informações sobre os planos de saúde disponíveis.  

5. **`prescricoes`**  
   - Registra medicamentos prescritos em consultas.  

6. **`exames`**  
   - Armazena resultados de exames solicitados em consultas.  

7. **`internacoes`**  
   - Gerencia internações hospitalares, relacionando pacientes, médicos e enfermeiros.  

8. **`enfermeiros`**  
   - Cadastro de profissionais de enfermagem.  

9. **`tipos_quarto`**  
   - Define os tipos de quartos disponíveis no hospital.  

10. **`especialidades`**  
    - Lista as especialidades médicas disponíveis.  

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

📌 **Repositório GitHub:** [github.com/IsaiasSorriso/hospital-mongodb](https://github.com/IsaiasSorriso/hospital-mongodb)  

--- 

**🎯 Objetivo do Projeto:**  
Modernizar a gestão hospitalar com um banco de dados eficiente e flexível.
