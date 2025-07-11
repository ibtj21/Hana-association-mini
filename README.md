# Hana-association-mini

This mini-project demonstrates **Association Rule Mining** using simulated transaction data. The **Apriori algorithm** was used to uncover frequent itemsets and generate meaningful association rules based on customer shopping behavior.

---

## Simulate Transaction Data

We created synthetic transaction data consisting of 10 transactions, each containing 2–5 items selected from a list of common grocery items.

### 🔢 Python Code:

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

### ✅ Sample Output:

```
Transaction 1: ['Bread', 'Milk']
Transaction 2: ['Bread', 'Eggs', 'Cheese']
Transaction 3: ['Milk', 'Eggs', 'Butter']
Transaction 4: ['Milk', 'Cheese', 'Apples']
Transaction 5: ['Bread', 'Butter', 'Eggs']
Transaction 6: ['Milk', 'Cheese', 'Juice']
Transaction 7: ['Bread', 'Milk', 'Butter']
Transaction 8: ['Cheese', 'Eggs', 'Tomatoes']
Transaction 9: ['Bread', 'Butter', 'Apples']
Transaction 10: ['Milk', 'Bread', 'Cheese', 'Eggs']
```

---

## Analyze with Apriori Algorithm

We transformed the transaction data into a format suitable for analysis using `mlxtend` and then applied the Apriori algorithm.

### 📊 Data Processing (One-hot encoding)

```python
import pandas as pd

# Convert transactions to one-hot encoded dataframe
from mlxtend.preprocessing import TransactionEncoder

te = TransactionEncoder()
te_array = te.fit(transactions).transform(transactions)
df = pd.DataFrame(te_array, columns=te.columns_)
df.head()
```

### ✅ Output (First 5 rows):

```
   Apples  Bananas  Bread  Butter  Cereal  Cheese  Eggs  Juice  Milk  Tomatoes
0   False    False   True   False   False   False  True  False  True     False
1   False    False   True   False   False   True   True  False  False    False
2   False    False  False   True    False   False  True  False  True     False
3   True     False  False   False   False   True   False False  True     False
4   False    False   True   True    False   False  True  False  False    False
```

---

### 🧠 Apply Apriori Algorithm

```python
from mlxtend.frequent_patterns import apriori

frequent_itemsets = apriori(df, min_support=0.3, use_colnames=True)
frequent_itemsets.sort_values(by='support', ascending=False)
```

### ✅ Frequent Itemsets Output:

```
   support        itemsets
0      0.6           (Milk)
1      0.6          (Bread)
2      0.5         (Cheese)
3      0.5           (Eggs)
4      0.4         (Butter)
5      0.3        (Apples)
6      0.3  (Bread, Butter)
7      0.3    (Bread, Eggs)
8      0.3    (Bread, Milk)
9      0.3  (Milk, Cheese)
10     0.3   (Cheese, Eggs)
```

---

## Generate Rules & Interpret

We generated association rules using the `association_rules()` function with `confidence` as the evaluation metric and a minimum threshold of 0.7.

### 🔁 Rule Generation Code:

```python
from mlxtend.frequent_patterns import association_rules

rules = association_rules(frequent_itemsets, metric='confidence', min_threshold=0.7)
rules[['antecedents', 'consequents', 'support', 'confidence', 'lift']]
```

### ✅ Association Rules Output:

```
  antecedents consequents  support  confidence  lift
0    (Butter)     (Bread)      0.3        0.75  1.25
```

---

### 📌 Explained Rule

**Selected Rule:**

> If a customer buys **Butter**, they are likely to also buy **Bread** with **75% confidence**.

### Interpretation:

- Out of 4 transactions that include **Butter**, 3 also include **Bread**.
- Therefore, **Confidence = 3/4 = 0.75 or 75%**.
- **Lift = 1.25**, indicating that buying Butter increases the likelihood of buying Bread by 25% over random chance.

---

### 🤓 Understanding Confidence

**Definition:**

> Confidence measures the likelihood that a rule’s consequent is also bought when its antecedent is bought.

**Formula:**

```
Confidence(A ⇒ B) = (Transactions with A and B) / (Transactions with A)
```

**Applied to Our Rule:**

- Antecedent: `Butter`
- Consequent: `Bread`
- Transactions with Butter: 4
- Transactions with both Butter and Bread: 3

**Confidence = 3 / 4 = 0.75 or 75%**

---

## Conclusion

This mini-project successfully demonstrated:

- Creating and analyzing transaction data.
- Preprocessing with one-hot encoding.
- Using **Apriori** to identify frequent itemsets.
- Generating meaningful association rules with **support**, **confidence**, and **lift**.
- Interpreting results for business relevance in product placement and marketing.

---

## Reflection

This project helped strengthen my understanding of market basket analysis. I learned:

- How to simulate transaction datasets.
- The role of support, confidence, and lift in Association Rule Mining.
- How the Apriori algorithm can reveal hidden patterns in customer behavior.

Such techniques are crucial in building effective recommender systems and business strategies in real-world retail and e-commerce platforms.

---

## Libraries Used

- `pandas` – for data manipulation
- `mlxtend` – for `apriori()` and `association_rules()` functions
- `random` – for simulating transactions

---

