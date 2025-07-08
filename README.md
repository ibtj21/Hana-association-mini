# Hana-association-mini
This project generates simulated transactional data and applies association rule mining to identify common shopping patterns.
# Association Rule Mining with Simulated Market Basket Data

This mini-project demonstrates **Association Rule Mining** using simulated transaction data. We use the **Apriori algorithm** to uncover frequent itemsets and generate meaningful association rules based on customer shopping behavior.

---

## 1. Simulate Transaction Data

- **Goal:** Create at least 10 fake transactions.
- Each transaction contains **2–5 items**.
- Items are selected from a pool of at least **10 unique grocery items**.

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

## 2. Analyze with Apriori Algorithm

- **Data Processing:**
  - One-hot encode the transaction data using `pandas`.
  - Each transaction is converted to a row with `True/False` values for each item.
- **Apriori Algorithm:**
  - Minimum support threshold: **0.3** (itemset must appear in at least 3 out of 10 transactions).
  - We use `mlxtend.frequent_patterns.apriori()` to extract frequent itemsets.

**Example Output:**
```
Frequent Itemsets:
    support         itemsets
0      0.6          (Bread)
1      0.4         (Butter)
2      0.5         (Cheese)
3      0.5           (Eggs)
4      0.6           (Milk)
5      0.3  (Bread, Butter)
6      0.3    (Bread, Eggs)
7      0.3    (Bread, Milk)
8      0.3   (Eggs, Cheese)
9      0.3   (Milk, Cheese)
```

---

## 3. Generate Rules & Interpret

- Generated **association rules** using:
  - Metric: `confidence`
  - Minimum confidence: **0.7**
- We display the rules with their **support**, **confidence**, and **lift**.

**Example Rule Output:**
```
Association Rules:
   antecedents consequents  support  confidence  lift
0    (Butter)     (Bread)      0.3        0.75  1.25
```

---

##  Explained Rule

**Rule Chosen:**
> If a customer buys **Butter**, they are likely to also buy **Bread** with **75% confidence**.

### What this means in real life:
Out of all the times customers bought **Butter**, **75% of those times**, they also bought **Bread**. This insight can be used by grocery stores to:
- Place Bread and Butter close together.
- Offer a "Buy Butter, get Bread at 20% off" deal.
- Predict future purchases.

---

##  Libraries Used

- `pandas`
- `mlxtend.frequent_patterns` (for `apriori` and `association_rules`)

---

##  Learning Outcome

This small project helped demonstrate:
- How to simulate and encode transaction data
- How Apriori identifies frequent itemsets
- How to interpret confidence and lift in association rules
