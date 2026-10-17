# 🏥⛓️ Hospital Database on a Blockchain

What if patient records couldn't be quietly edited after the fact?

This project is a small, hands-on **blockchain written from scratch in Python** (no blockchain libraries), used to store **hospital patient records**. Every block is linked to the one before it using a **SHA-256 hash**, so changing any old record breaks the chain.

![Python](https://img.shields.io/badge/Python-3-blue)
![Hashing](https://img.shields.io/badge/Hashing-SHA--256-purple)
![Notebook](https://img.shields.io/badge/Made%20in-Google%20Colab-orange)

---

## 💡 The idea

Hospital data is sensitive and must be trustworthy. A blockchain gives you **tamper evidence**: each block stores the hash of the previous block, so altering one block changes its hash and invalidates everything after it.

---

## 🧩 What's inside

### 1. Patient record (nested dictionary)

A hospital record is modelled as a Python dictionary with nested sections:

| Section | Fields |
|---|---|
| Patient | ID, name, gender, address, phone number |
| Diagnosis | Diagnosis number, details, remarks |
| `patientAttendedBy` | Doctor name, gender, designation, address |
| `patientBillDetails` | Bill number, amount, payment method |

### 2. The block: `neuralCoinBlock`

```python
class neuralCoinBlock:
  def __init__(self, preBlockHash, tranList):
    self.preBlockHash = preBlockHash
    self.tranList = tranList
    self.blockData = "-".join(tranList) + "-" + preBlockHash
    self.blockHash = hashlib.sha256(self.blockData.encode()).hexdigest()
```

Each block holds:

- the **previous block's hash**
- a **list of transactions**
- its own **block data** (transactions joined with the previous hash)
- its own **SHA-256 block hash**

### 3. Chaining blocks

```python
InitialBlock = neuralCoinBlock("Encode Password", [t1, t2])
SecondBlock  = neuralCoinBlock(InitialBlock.blockHash, [t1, t3])
```

The second block takes the first block's hash as input, which is what links them into a chain.

### 4. Timestamped blocks

A `createBlock()` helper builds a block and logs its hash together with a timestamp in the **Asia/Kolkata (IST)** timezone into an `index` list, a simple ledger of when each block was created.

```python
createBlock("second block", ['askjddjsdh', 'askdhakjsdghaj'])
```

---

## 🚀 Run it

**In Google Colab (easiest):** upload `Hospitaldatabase.ipynb` to [Colab](https://colab.research.google.com) and run all cells.

**Locally:**

```bash
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>
pip install pytz jupyter
jupyter notebook Hospitaldatabase.ipynb
```

`hashlib` and `datetime` are part of the Python standard library.

---

## 🔎 Things to try

- Change one character in a transaction and watch the block hash change completely
- Change an early block and see that every later block no longer matches
- Add your own record fields and chain more blocks

---

## ⚠️ Known limitations

This is a learning project, not a production system.

- **No chain validation:** there is no function yet that re-checks the whole chain for tampering.
- **No consensus or proof of work:** it is a single local chain, not a network.
- **No persistence:** blocks live in memory and disappear when the notebook restarts.
- **`createBlock()` accepts any previous hash**, so it doesn't enforce linking to the real latest block.
- **Patient dictionaries are not fully hashed yet.** `"-".join()` on a dictionary only joins its *keys*, so the values (names, bills, diagnoses) don't affect the hash. To fix it, serialise the record first, for example `[json.dumps(hosp, sort_keys=True)]`.
- **Privacy:** real patient data should never go on a public chain. Use dummy data only, and in real systems store hashes on-chain and the records off-chain, encrypted.

---

## 🔮 Future improvements

- A `validateChain()` function
- Proof-of-work mining
- Saving the chain to JSON or a database
- Encrypting patient data
- A simple web or CLI interface for adding and viewing records
- Digital signatures so only authorised doctors can add blocks

---

## 🛠️ Tech

- **Python 3**
- **hashlib** for SHA-256
- **datetime + pytz** for timestamps
- **Google Colab / Jupyter**

---

⭐ If you found this fun, give the repo a star!
