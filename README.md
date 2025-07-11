# Hana-association-mini

This mini-project demonstrates **Association Rule Mining** using simulated transaction data. The **Apriori algorithm** was used to uncover frequent itemsets and generate meaningful association rules based on customer shopping behavior.

---

## Simulate Transaction Data

- Created at least 10 fake transactions.
- Each transaction contains **2–5 items** selected from a pool of at least **10 unique grocery items**.

**Sample Transaction Data (10 total):**

```python
[
    ['Bread', 'Milk'],
    ['Bread', 'Eggs', 'Cheese'],
    ['Milk', 'Eggs', 'Butter'],
    ['Milk', 'Cheese', 'Apples'],
    ['Bread', 'Butter', 'Eggs'],
    ['Milk', 'Cheese', 'Juice'],
    ['Bread', 'Milk', 'Butter'],
    ['Cheese', 'Eggs', 'Tomatoes'],
    ['Bread', 'Butter', 'Apples'],
    ['Milk', 'Bread', 'Cheese', 'Eggs']
]
```

---

## Analyze with Apriori Algorithm

### Data Processing

* One-hot encoded the transaction data using **pandas**.
* Each transaction was transformed into a row with boolean values (True/False) for each item.

### Apriori Analysis

* **Minimum support threshold:** `0.3` (itemset must appear in at least 3 out of 10 transactions).
* Used `mlxtend.frequent_patterns.apriori()` to extract frequent itemsets.

**Example Frequent Itemsets Output:**

| Support | Itemsets        |
| ------- | --------------- |
| 0.6     | (Bread)         |
| 0.4     | (Butter)        |
| 0.5     | (Cheese)        |
| 0.5     | (Eggs)          |
| 0.6     | (Milk)          |
| 0.3     | (Bread, Butter) |
| 0.3     | (Bread, Eggs)   |
| 0.3     | (Bread, Milk)   |
| 0.3     | (Eggs, Cheese)  |
| 0.3     | (Milk, Cheese)  |

---

## Generate Rules & Interpret

### Association Rules

* Used `mlxtend.frequent_patterns.association_rules()`
* **Metric used:** `confidence`
* **Minimum confidence threshold:** `0.7`

**Example Rule Output:**

| Antecedents | Consequents | Support | Confidence | Lift |
| ----------- | ----------- | ------- | ---------- | ---- |
| (Butter)    | (Bread)     | 0.3     | 0.75       | 1.25 |

---

### Explained Rule

**Rule Chosen:**

> If a customer buys **Butter**, they are likely to also buy **Bread** with **75% confidence**.

### Real-world Meaning:

Out of all the times customers bought **Butter**, **75% of those times, they also bought Bread**.  
This kind of insight is valuable for grocery stores to:

* Place **Bread and Butter** close together.
* Offer bundled promotions (e.g., *Buy Butter, get Bread at 20% off*).
* Improve product recommendations and layout optimization.

---

### Understanding Confidence

**Definition:**

> Of the transactions that contain the *antecedent* (A), how many also contain the *consequent* (B)?

**Formula:**

```
Confidence(A ⇒ B) = (Transactions with A and B) / (Transactions with A)
```

**Example:**

- Antecedent: `Butter`
- Consequent: `Bread`
- Transactions with `Butter`: 4  
- Transactions with both `Butter` and `Bread`: 3  
- **Confidence = 3 / 4 = 0.75 (75%)**

---

## Conclusion

This mini-project applied **Association Rule Mining** to simulated grocery data. Using the **Apriori algorithm**, we:

- Discovered frequent itemsets using support.
- Generated rules using confidence and lift.
- Interpreted rules to find actionable insights for product pairing and recommendations.

---

## Reflection

Working on this project helped reinforce key data mining techniques, including:

* Data simulation and one-hot encoding
* Pattern discovery using **Apriori**
* Metric interpretation (support, confidence, lift)
* Extracting business insights from simple customer behavior

These skills are transferable to larger datasets and real-world applications, especially in **retail analytics** and **recommendation systems**.

---

## Libraries Used

* `pandas`
* `mlxtend.frequent_patterns`
  * `apriori()`
  * `association_rules()`

---
