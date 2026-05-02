# 📝 MongoDB Exam: Lógica de Consultas e Operações

Este repositório contém a resolução de um desafio técnico focado em um sistema de gestão de doadores de sangue.

O objetivo é demonstrar domínio de:
- Filtros complexos  
- Projeções de dados  
- Operações de atualização em larga escala  

---
🏆 Avaliação Técnica

Nota: ⭐ 38/40

Avaliador: Gustavo Nunes Rocha

Feedback:

"Seu desempenho foi excelente! Você demonstrou forte domínio da filtragem, projeção e das operações de atualização."

## 🔍 Consultas e Filtros Estruturados

As consultas abaixo foram projetadas para extrair informações específicas seguindo regras de negócio rígidas.

---

### 1️⃣ Auditoria de Contatos (Limitação e Projeção)

**Objetivo:**  
Listar apenas dados essenciais (ID, Nome e Email) dos 10 primeiros registros.

```javascript
db.doador.find(
  {},
  {
    _id: 0,
    idDoador: 1,
    nomDoador: 1,
    dscEmailDoador: 1
  }
).limit(10)
```

---

### 2️⃣ Filtro Geográfico e Biológico

**Cenário:**  
Identificar doadores do estado do Ceará (CE) com tipos sanguíneos A ou O.

**Destaque técnico:**
- `$in` → múltiplos valores  
- `$elemMatch` → subdocumentos  

```javascript
db.doador.find(
  {
    $and: [
      { indTipoSangDoador: { $in: ["A", "O"] } },
      {
        enderecoDoador: {
          $elemMatch: { dscUFDoador: "CE" }
        }
      }
    ]
  },
  {
    _id: 0,
    codDoador: 1,
    nomDoador: 1,
    "enderecoDoador.dscCidadeDoador": 1
  }
)
```

---

### 3️⃣ Relatório de Doações por Período e Volume

**Cenário:**  
Filtrar doações feitas em 2021 com volume entre 400ml e 600ml.

**Destaque técnico:**
- Uso de `ISODate` para precisão temporal  

```javascript
db.doacao.find(
  {
    $and: [
      { qtdSangueDoada: { $gte: 400, $lt: 600 } },
      {
        datDoacao: {
          $gte: ISODate("2021-01-01T00:00:00Z"),
          $lt: ISODate("2022-01-01T00:00:00Z")
        }
      }
    ]
  },
  {
    _id: 0,
    idDoacao: 1,
    qtdSangueDoada: 1
  }
)
```

---

## ⚡ Atualizações e Inserções de Dados

Operações para manter a base de dados atualizada.

---

### 4️⃣ Gestão de Arrays (Update com `$each`)

**Objetivo:**  
Adicionar novos itens sem duplicar valores existentes.

```javascript
db.doador.updateOne(
  { idDoador: 5 },
  {
    $addToSet: {
      dscLancheDoador: {
        $each: [
          "suco de amendoas",
          "salmao flambado a grega italiana"
        ]
      }
    }
  }
)
```

---

### 5️⃣ Registro de Nova Doação

**Objetivo:**  
Inserir uma nova doação vinculada ao doador.

```javascript
db.doacao.insertOne({
  idDoacao: 98779,
  idDoador: 50,
  datDoacao: ISODate("2021-01-23T09:00:00Z"),
  qtdSangueDoada: 500
})
```

---

## 💡 Aprendizados Técnicos

- 📌 Subdocumentos → `enderecoDoador.dscCidadeDoador`  
- 📌 Datas corretas → uso de `ISODate`  
- 📌 Performance → projeções (`_id: 0`)  

---

## 👨‍💻 Autor

**Brayan Costa Santos**  
Graduando em Sistemas para Internet - IFES  
