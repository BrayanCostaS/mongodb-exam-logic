# 📝 MongoDB Exam: Query Logic & Operations

This repository contains the solution for a technical challenge focused on a blood donor management system.

The main goal is to demonstrate proficiency in:
- Complex Filtering
- Data Projection
- Large-Scale Update Operations

---
🏆 Technical Evaluation

Score: ⭐ 38/40

Evaluator: Gustavo Nunes Rocha

Feedback:

"Your performance was excellent! You demonstrated a strong command of filtering, projection, and update operations."

## 🔍 Structured Queries & Filters

The queries below were designed to extract specific information in strict adherence to business logic rules.

---

### 1️⃣ Contact Audit (Limitation & Projection)

**Objective:**  
List essential data only (ID, Name, and Email) for the top 10 records.

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

2️⃣ Geographic & Biological Filter
Scenario:

Identify blood donors from the state of Ceará (CE) with blood types A or O.

Technical Highlights:

$in → multiple values

$elemMatch → subdocuments

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
3️⃣ Donation Report by Date Range & Volume
Scenario:

Filter donations made in 2021 with volume between 400ml and 600ml.

Technical Highlights:

Use of ISODate for temporal accuracy

db.doacao.find(
  {
    $and: [
      { qtdSangueDoada: { $gte: 400,$lt: 600 } },
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

⚡ Data Updates & Insertions
Operations executed to keep the database updated.

4️⃣ Array Management (Update with $each)
Objective:

Add new items without duplicating existing values.

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

5️⃣ New Donation Record
Objective:

Insert a new donation document linked to the donor.

db.doacao.insertOne({
  idDoacao: 98779,
  idDoador: 50,
  datDoacao: ISODate("2021-01-23T09:00:00Z"),
  qtdSangueDoada: 500
})

💡 Key Learnings
📌 Subdocuments → enderecoDoador.dscCidadeDoador

📌 Proper dates → use of ISODate

📌 Performance → projections (_id: 0)

👨‍💻 Author
Brayan Costa Santos

Undergraduate Student in Internet Technology & Systems - IFES
