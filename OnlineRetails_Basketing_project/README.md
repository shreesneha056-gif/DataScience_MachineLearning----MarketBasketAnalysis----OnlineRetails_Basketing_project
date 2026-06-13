# Online Retail Market Basket Analysis (Association Rule Mining)

This repository contains a data-driven **Market Basket Analysis (MBA)** case study conducted on transactional e-commerce data from an international online retailer. By implementing the classical **Apriori Algorithm** for Association Rule Mining, this project identifies strong, hidden relationships between products frequently purchased together in single transactions. These insights enable data-backed cross-selling strategies, shelf placement adjustments, and targeted promotional campaigns.

---

## 📂 Dataset Repository & Architecture

The analytical pipeline processes historical sales logs capturing multi-country business-to-business (B2B) transactions.

### Dataset Profile (`Online Retail.csv`)
* **Core Attributes:**
  - `InvoiceNo`: Unique 6-digit transactional identifier. Invoices starting with 'C' indicate cancellations.
  - `StockCode`: Distinct product/item token code.
  - `Description`: Nominal product name text.
  - `Quantity`: The number of item units generated per transaction.
  - `InvoiceDate`: Timestamp marking transaction occurrence.
  - `UnitPrice`: Product price per unit.
  - `CustomerID`: Unique customer identifier.
  - `Country`: The country where the consumer session originated (e.g., France, Germany, UK).

---

## 🛠️ Transactional Data Engineering Pipeline

Market Basket Analysis requires structured boolean matrices rather than continuous transaction ledger logs. The data processing inside `OnlineRetails_Basketing.ipynb` executes the following transformation matrix:

1. **Data Sanitization:** Stripping trailing whitespaces from item descriptions and filtering out structural noise (such as transaction cancellations or negative quantities).
2. **One-Hot Basket Encoding:** Grouping data records by `InvoiceNo` and `Description`, aggregating the quantities, and pivoting the frame to build a binary user-basket matrix:
   - `0`: Item was **not** present in the transaction.
   - `1`: Item **was** included in the transaction basket.
3. **Regional Segmentation:** Filtering baskets by specific geographic regions (e.g., isolating French or German transactions) to minimize cultural buying cross-contamination and surface localized rules.

---

## 🧮 Theoretical Framework & Evaluation Metrics

The project uses the **Apriori Algorithm** to discover frequent itemsets ($A \rightarrow B$, where $A$ is the *Antecedent* and $B$ is the *Consequent*) using three core statistical indicators:

### 1. Support
Measures the baseline frequency or popularity of an itemset within the entire transactional space.
$$\text{Support}(A \rightarrow B) = \frac{\text{Transactions containing both } A \text{ and } B}{\text{Total Transactions}}$$

### 2. Confidence
Measures the reliability or operational certainty of the rule—signifying how frequently item $B$ appears given that item $A$ has already been purchased.
$$\text{Confidence}(A \rightarrow B) = \frac{\text{Support}(A \cup B)}{\text{Support}(A)}$$

### 3. Lift
Measures the structural strength of the rule by comparing the actual co-occurrence frequency of the items against their theoretical independence.
$$\text{Lift}(A \rightarrow B) = \frac{\text{Support}(A \cup B)}{\text{Support}(A) \times \text{Support}(B)}$$
* **$\text{Lift}