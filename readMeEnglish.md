# 📝 MongoDB Exam: Query Logic & Operations

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

2️⃣ Filtro Geográfico e Biológico
Cenário:

Identificar doadores do estado do Ceará (CE) com tipos sanguíneos A ou O.

Destaque técnico:

$in → múltiplos valores

$elemMatch → subdocumentos
