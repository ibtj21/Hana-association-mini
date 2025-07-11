
# Hana-association-mini
This mini-project demonstrates *Association Rule Mining* using simulated transaction data.The *Apriori algorithm* was used to uncover frequent itemsets and generate meaningful association rules based on customer shopping behavior.

---

## 1. Simulate Transaction Data

- *Goal:* Create at least 10 fake transactions.
- Each transaction contains *2–5 items*.
- Items are selected from a pool of at least *10 unique grocery items*.

  ### Python Code:

```python
import random

items = ['Bread', 'Milk', 'Eggs', 'Butter', 'Cheese', 'Juice', 'Tomatoes', 'Apples', 'Bananas', 'Cereal']
transactions = []

for _ in range(10):
    transaction = random.sample(items, k=random.randint(2, 5))
    transactions.append(transaction)

for i, t in enumerate(transactions):
    print(f"Transaction {i+1}:", t)
```

*Sample Transaction Data output (10 total):*
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

The transaction data was transformed into a format suitable for analysis using `mlxtend`, and the Apriori algorithm was then applied.

### Data Processing (One-hot encoding)

- *Data Processing:*
  - One-hot encode the transaction data using pandas.
  - Each transaction is converted to a row with True/False values for each item.

```python
import pandas as pd

# Convert transactions to one-hot encoded dataframe
from mlxtend.preprocessing import TransactionEncoder

te = TransactionEncoder()
te_array = te.fit(transactions).transform(transactions)
df = pd.DataFrame(te_array, columns=te.columns_)
df.head()
```

### Output (First 5 rows):

```
   Apples  Bananas  Bread  Butter  Cereal  Cheese  Eggs  Juice  Milk  Tomatoes
0   False    False   True   False   False   False  True  False  True     False
1   False    False   True   False   False   True   True  False  False    False
2   False    False  False   True    False   False  True  False  True     False
3   True     False  False   False   False   True   False False  True     False
4   False    False   True   True    False   False  True  False  False    False
```

- *Apriori Algorithm:*
  - Minimum support threshold: *0.3* (itemset must appear in at least 3 out of 10 transactions).
  - We use mlxtend.frequent_patterns.apriori() to extract frequent itemsets.

```python
from mlxtend.frequent_patterns import apriori

frequent_itemsets = apriori(df, min_support=0.3, use_colnames=True)
frequent_itemsets.sort_values(by='support', ascending=False)
```

* Frequent Itemsets Output:*

Frequent Itemsets:
```
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

- Generated *association rules* using:
  - Metric: confidence
  - Minimum confidence: *0.7*
- The rules with their *support, **confidence, and **lift* were displayed.

```python
from mlxtend.frequent_patterns import association_rules

rules = association_rules(frequent_itemsets, metric='confidence', min_threshold=0.7)
rules[['antecedents', 'consequents', 'support', 'confidence', 'lift']]
```

### Association Rules Output:

```
  antecedents consequents  support  confidence  lift
0    (Butter)     (Bread)      0.3        0.75  1.25
```

### Key Metrics Summary

| *Metric*         | *What It Means*                                                                 |
|--------------------|-----------------------------------------------------------------------------------|
| *Antecedent*     | (Butter) — The rule is triggered when a customer buys Butter                    |
| *Consequent*     | (Bread) — Then the rule suggests they are also likely to buy Bread              |
| *Support = 0.3*  | 30% of all transactions included both Butter and Bread                            |
| *Confidence = 0.75* | 75% of the time when customers buy Butter, they also buy Bread                  |
| *Lift = 1.25*    | Customers who buy Butter are 1.25 times more likely to buy Bread than by chance   |

---

## Explained Rule

*Rule Chosen:*
> If a customer buys *Butter, they are likely to also buy **Bread* with *75% confidence*.

---

### What this means in real life:
Out of all the times customers bought *Butter, **75% of those times, they also bought **Bread*. This insight can be used by grocery stores to:

- Place Bread and Butter close together.
- Offer a "Buy Butter, get Bread at 20% off" deal.
- Predict future purchases and bundle promotions effectively.

---

### Confidence:

*Definition:*
> Of the transactions that contain the *antecedent* (A), how many also contain the *consequent* (B)?

*Formula:*

Confidence(A ⇒ B) = (Transactions with A and B) / (Transactions with A)

*Applied to Our Rule:*

Antecedent:   Butter  
Consequent:   Bread

Transactions with Butter: 4  
Transactions with both Butter and Bread: 3  

Confidence = 3 / 4 = 0.75 or 75%

---

### Why it matters:
High confidence suggests a strong association between the items. Retailers can leverage this information to:

- Recommend products
- Increase cross-selling
- Strategically place items

## Conclusion

This mini-project successfully demonstrated:

- Creating and analyzing transaction data.
- Preprocessing with one-hot encoding.
- Using **Apriori** to identify frequent itemsets.
- Generating meaningful association rules with **support**, **confidence**, and **lift**.
- Interpreting results for business relevance in product placement and marketing.

##  Libraries Used

- `pandas` – for data manipulation
- `mlxtend` – for `apriori()` and `association_rules()` functions
- `random` – for simulating transactions

---

##  Learning Outcome / Reflection

This small project helped demonstrate:
- How to simulate and encode transaction data
- How Apriori identifies frequent itemsets
- How to interpret confidence and lift in association rules
