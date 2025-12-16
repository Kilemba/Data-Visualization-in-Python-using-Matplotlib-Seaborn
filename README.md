# Data-Visualization-in-Python-using-Matplotlib-Seaborn
A simple beginner's guide to creating charts and graphs in Python. All examples use a fraud detection dataset and are designed to run in **Google Colab**.

## Table of Contents

- [Introduction](#introduction)
- [Dataset Overview](#dataset-overview)
- [Getting Started in Google Colab](#getting-started-in-google-colab)
- [Visualization Types](#visualization-types)
- [Quick Reference](#quick-reference)
- [Tips & Best Practices](#tips--best-practices)

---

## Introduction

### Why Make Charts?

Looking at numbers in a spreadsheet is hard. Your brain can't spot patterns quickly. But when you turn those numbers into pictures (charts), patterns jump out at you instantly!

**What charts help you do:**

- **See patterns fast**: A spike in a chart is obvious. The same spike in numbers? You might miss it.
- **Compare things easily**: Which is bigger? Looking at two bars is faster than comparing two numbers.
- **Find weird values**: Outliers (strange values) stand out visually.
- **Explain to others**: A good chart explains your findings without needing a statistics degree.

### Simple Rules for Good Charts

1. **Pick the right chart**
   - Comparing categories? Use a bar chart
   - Showing trends over time? Use a line chart
   - Showing relationships? Use a scatter plot

2. **Always label everything**
   - Add a title (what is this chart showing?)
   - Label your axes (what do the numbers mean?)
   - Add a legend if you have multiple colors

3. **Keep it simple**
   - One main idea per chart
   - Don't use too many colors
   - If someone needs 5 minutes to understand your chart, it's too complicated

---

## Dataset Overview

All examples use a **Fraud Transactions Dataset**. Think of it like a bank's transaction records where we're trying to catch fraudsters.

### The Columns (What Each Column Means)

| Column | What It Is | Example |
|--------|-----------|---------|
| `amount` | How much money in the transaction | $45.99, $1250.00 |
| `transaction_type` | How they paid | ATM, Card, Online, QR code |
| `merchant_category` | What store type | Grocery, Restaurant, Gas station |
| `country` | Where it happened | US, UK, India |
| `hour` | What time of day | 0 (midnight) to 23 (11pm) |
| `device_risk_score` | Is the device suspicious? (0=safe, 1=risky) | 0.05, 0.73 |
| `ip_risk_score` | Is the internet address suspicious? | 0.12, 0.88 |
| `is_fraud` | Was it fraud? | 0 = No, 1 = Yes |

---

## Getting Started in Google Colab

### Step 1: Open Google Colab

1. Go to [colab.research.google.com](https://colab.research.google.com)
2. Click "New Notebook"
3. You're ready to code!

### Step 2: Import Libraries

Copy and paste this into your first cell:

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# This line makes charts look better
sns.set_style("whitegrid")
```

Then press `Shift + Enter` to run it.

**What these libraries do:**
- `pandas` - reads and organizes your data
- `matplotlib` - creates basic charts
- `seaborn` - creates prettier charts more easily

### Step 3: Load Your Data

If your CSV file is on your computer:

```python
from google.colab import files
uploaded = files.upload()  # Click "Choose Files" and select your CSV

# Then load it
df = pd.read_csv('synthetic_fraud_dataset.csv')
```

Or if it's online somewhere:

```python
df = pd.read_csv('url-to-your-file.csv')
```

### Step 4: Check Your Data

```python
# See the first few rows
print(df.head())

# Check what columns you have
print(df.columns)
```

Now you're ready to make charts!

---

## Visualization Types

## 1. Box Plot (See the Spread and Find Outliers)

### What Is It?

A box plot shows you 5 important things about your numbers:
- The smallest value (bottom line)
- Where 25% of values sit (bottom of box)
- The middle value/median (line in the box)
- Where 75% of values sit (top of box)
- The biggest value (top line)
- Any weird outliers (dots outside the lines)

**Think of it like this**: The box shows where most of your data lives. The lines (whiskers) show the normal range. Dots are the weird ones.

### When to Use It

Use it when you want to:
- Find outliers (unusually high or low values)
- Compare two groups (like fraud vs not fraud)
- See if your data is spread out or bunched together

### Example: Transaction Amount by Fraud Status

```python
plt.figure(figsize=(6,4))
sns.boxplot(x='is_fraud', y='amount', data=df)
plt.title('Transaction Amount by Fraud Status')
plt.xlabel('Is Fraud (0 = No, 1 = Yes)')
plt.ylabel('Amount')
plt.show()
```

**Breaking down the code:**
- Line 1: Make a canvas (6 inches wide, 4 inches tall)
- Line 2: Create the box plot
  - `x='is_fraud'` → put fraud status on the bottom
  - `y='amount'` → put money amounts on the side
  - `data=df` → use our dataset
- Lines 3-5: Add labels so people know what they're looking at
- Line 6: Show the chart

**What to look for**: If the fraud box is higher than the non-fraud box, it means fraudsters try to steal bigger amounts!

---

## 2. Scatter Plot (See if Two Things Are Related)

### What Is It?

A scatter plot puts dots on a graph. Each dot is one transaction. The position shows two things about that transaction.

**Think of it like this**: Imagine plotting people's height (x-axis) vs weight (y-axis). Each dot is one person. You'd probably see taller people tend to weigh more (dots going up-right).

### When to Use It

 Use it when you want to:
- See if two things move together
- Find relationships between numbers
- Spot unusual combinations

**What patterns mean:**
- Dots going up-right = when one goes up, the other goes up too
- Dots going down-right = when one goes up, the other goes down
- Dots all over = no relationship

### Example: Device Risk Score vs Transaction Amount

```python
plt.figure(figsize=(6,4))
plt.scatter(df['device_risk_score'], df['amount'], alpha=0.5)
plt.title('Device Risk Score vs Transaction Amount')
plt.xlabel('Device Risk Score')
plt.ylabel('Amount')
plt.show()
```

**Breaking down the code:**
- `plt.scatter()` → make a scatter plot
- `alpha=0.5` → make dots 50% see-through (so you can see overlapping dots)

**What to look for**: If you see lots of dots in the top-right corner (high risk + high amount), fraudsters might be using risky devices for big purchases.

---

## 3. Histogram (See How Numbers Are Spread Out)

### What Is It?

A histogram groups your numbers into buckets and counts how many are in each bucket. Then it draws bars to show the counts.

**Think of it like this**: Imagine sorting candy by weight ranges: 0-10g, 10-20g, 20-30g, etc. Then counting each pile. That's a histogram!

### When to Use It

Use it when you want to:
- See the typical range of values
- Find if most values are high, low, or in the middle
- Check if your data looks normal (bell-shaped curve)

**The shape tells a story:**
- Bell shape = most values in the middle (normal)
- Skewed right = mostly small values, few big ones
- Flat = values spread evenly

### Example: Distribution of Transaction Amounts

```python
plt.figure(figsize=(6,4))
plt.hist(df['amount'], bins=20)
plt.title('Distribution of Transaction Amounts')
plt.xlabel('Amount')
plt.ylabel('Frequency')
plt.show()
```

**Breaking down the code:**
- `plt.hist()` → make a histogram
- `bins=20` → split the data into 20 groups
  - More bins = see more detail (but can be messy)
  - Fewer bins = see overall shape (but lose detail)

**What to look for**: You'll probably see tall bars on the left (many small transactions) and short bars spreading right (few big transactions). This is normal—most people buy small stuff daily!

---

## 4. Count Plot (Count How Many in Each Category)

### What Is It?

A count plot is a simple bar chart that counts things in categories.

**Think of it like this**: Counting how many apples, oranges, and bananas you have in a fruit basket, then drawing bars to show the counts.

### When to Use It

 Use it when you want to:
- Count how many times each category appears
- See which category is most common
- Check if your data is balanced (important for fraud detection!)

### Example: Fraud vs Non-Fraud Transactions

```python
plt.figure(figsize=(5,4))
sns.countplot(x='is_fraud', data=df)
plt.title('Fraud vs Non-Fraud Transactions')
plt.xlabel('Is Fraud')
plt.ylabel('Count')
plt.show()
```

### Example: Transaction Type Frequency

```python
plt.figure(figsize=(7,4))
sns.countplot(x='transaction_type', data=df)
plt.title('Transaction Type Frequency')
plt.show()
```

**What to look for**: The non-fraud bar will be MUCH taller. That's realistic—fraud is rare! In real life, maybe only 1-2% of transactions are fraud.

---

## 5. Correlation Heatmap (See Which Numbers Move Together)

### What Is It?

Correlation measures if two numbers move together. The heatmap shows all correlations at once using colors.

**Correlation values:**
- **+1** = perfect together (when one goes up, the other always goes up)
- **0** = no relationship (they do their own thing)
- **-1** = perfect opposite (when one goes up, the other goes down)

**Think of it like this**: Height and weight have positive correlation. As people get taller, they usually weigh more. Temperature and ice cream sales have positive correlation too!

### When to Use It

 Use it when you want to:
- See which variables are connected
- Find the strongest relationships in your data
- Understand what predicts fraud

**Reading the colors:**
- Bright red = strong positive relationship
- White/light = weak or no relationship
- Bright blue = strong negative relationship

### Example: Correlation Heatmap

```python
plt.figure(figsize=(10,6))
correlation = df.select_dtypes(include='number').corr()
sns.heatmap(correlation, annot=True, cmap='coolwarm')
plt.title('Correlation Heatmap')
plt.show()
```

**Breaking down the code:**
- Line 2: Pick only number columns (correlation only works with numbers)
- `.corr()` → calculate how each number relates to every other number
- `annot=True` → show the actual numbers in each box
- `cmap='coolwarm'` → use red/blue colors (red=positive, blue=negative)

**What to look for**: If `device_risk_score` has a number like 0.7 with `is_fraud`, that's a strong sign! The device risk score is good at predicting fraud.

---

## 6. Pie Chart (Show Percentages)

### What Is It?

A pie chart cuts a circle into slices. Each slice shows what percentage of the total that category is.

**Think of it like this**: Cutting a real pie! If you ate 3 out of 8 slices, your slice is 3/8 = 37.5% of the pie.

### When to Use It

 Good for:
- Showing parts of a whole
- When you have 2-5 categories
- Making percentages obvious

 Avoid when:
- You have many categories (gets messy)
- You need to compare exact amounts (bar charts are better)

### Example: Fraud Percentage

```python
fraud_counts = df['is_fraud'].value_counts()
plt.figure(figsize=(5,5))
plt.pie(fraud_counts, labels=['Not Fraud', 'Fraud'], autopct='%1.1f%%')
plt.title('Fraud Distribution')
plt.show()
```

**Breaking down the code:**
- Line 1: Count how many frauds and non-frauds
- `labels=['Not Fraud', 'Fraud']` → name the slices
- `autopct='%1.1f%%'` → show percentages on the slices

**What to look for**: The fraud slice will be tiny (maybe 1-3%). This shows why fraud detection is hard—you're looking for a needle in a haystack!

---

## 7. Error Bars (Show the Range/Spread)

### What Is It?

Error bars add lines to your chart showing how spread out the data is. They show you can't trust a single number—there's a range.

**Think of it like this**: If someone says "average height is 5'8"", error bars show how much people actually vary around that average.

### When to Use It

Use it when you want to:
- Show averages AND how much variation there is
- Compare groups while showing uncertainty
- Be honest about data spread

**What it tells you:**
- Long error bars = data is all over the place
- Short error bars = data is consistent
- If error bars don't overlap = groups are probably different

### Example: Average Amount by Fraud Status with Variability

```python
means = df.groupby('is_fraud')['amount'].mean()
stds = df.groupby('is_fraud')['amount'].std()

plt.figure(figsize=(5,4))
plt.errorbar(means.index, means, yerr=stds, fmt='o')
plt.title('Average Transaction Amount with Error Bars')
plt.xlabel('Is Fraud')
plt.ylabel('Average Amount')
plt.show()
```

**Breaking down the code:**
- Line 1: Calculate average amount for each group (fraud/not fraud)
- Line 2: Calculate the spread (standard deviation)
- `yerr=stds` → use the spread for the error bar length
- `fmt='o'` → show the average as a circle

**What to look for**: If fraud has a higher average AND longer error bars, it means fraudsters attempt bigger amounts but with lots of variation (some test with small amounts, some go big).

---

## 8. Figure Size (Make Charts Bigger or Smaller)

### What Is It?

`figsize` controls how big your chart appears. Measured in inches.

```python
plt.figure(figsize=(width, height))
```

### Why It Matters

- Too small = labels are unreadable
- Too big = wastes space
- Right size = perfect for sharing

**Common sizes:**
- `(6, 4)` → small, good for simple charts
- `(8, 6)` → medium, good default
- `(10, 8)` → large, good for presentations
- `(10, 10)` → square, good for heatmaps

### Tips

1. **Start with (8, 6)** and adjust if needed
2. **Make it bigger** if text is hard to read
3. **Use square (10, 10)** for heatmaps and pie charts
4. **Use wide (12, 6)** if you have many categories on the x-axis

In Google Colab, bigger charts are usually better because screens vary!

---

## Quick Reference

### Which Chart Should I Use?

| I Want To... | Use This Chart |
|--------------|----------------|
| Count categories | Count plot |
| Show percentages | Pie chart |
| Compare groups | Box plot |
| See if two things relate | Scatter plot |
| See how one number spreads | Histogram |
| See many relationships at once | Correlation heatmap |
| Show averages with variation | Error bars |

### Code Templates

```python
# Count Plot (categories)
sns.countplot(x='category_column', data=df)

# Box Plot (compare groups)
sns.boxplot(x='category_column', y='number_column', data=df)

# Scatter Plot (relationship)
plt.scatter(df['number1'], df['number2'], alpha=0.5)

# Histogram (distribution)
plt.hist(df['number_column'], bins=20)

# Heatmap (correlations)
sns.heatmap(df.corr(), annot=True, cmap='coolwarm')

# Pie Chart (percentages)
plt.pie(counts, labels=names, autopct='%1.1f%%')
```

---

## Tips & Best Practices

### Do These Things 

1. **Always add titles and labels** - Future you will forget what the chart shows!
2. **Use `figsize=(8,6)` as default** - Makes charts readable
3. **Try different bin numbers** - For histograms, test bins=10, 20, 30
4. **Use `alpha=0.5` for scatter plots** - See overlapping points
5. **Save your charts**:
```python
plt.savefig('my_chart.png', dpi=300, bbox_inches='tight')
```
### Don't Do These Things 

1. **Don't forget labels** - Nobody knows what axis X87 means
2. **Don't use pie charts for 10+ categories** - Use bar chart instead
3. **Don't use too many colors** - Keep it simple
4. **Don't make 3D charts** - They look cool but are hard to read
5. **Don't ignore outliers** - They might be errors or interesting findings

### Common Problems in Google Colab

**Problem: Chart doesn't show**
```python
# Add this at the end
plt.show()
```

**Problem: Chart is tiny**
```python
# Make it bigger
plt.figure(figsize=(10, 6))
```

**Problem: Multiple charts overlap**
```python
# Add this between charts
plt.figure()  # Starts a new chart
```

**Problem: Want to see all columns**
```python
# Show all columns
print(df.columns.tolist())
```

---

## Running Your First Chart in Colab

Here's a complete example you can copy-paste:

```python
# 1. Import libraries
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# 2. Upload your file
from google.colab import files
uploaded = files.upload()

# 3. Load the data
df = pd.read_csv('synthetic_fraud_dataset.csv')

# 4. Check it loaded
print(df.head())

# 5. Make your first chart!
plt.figure(figsize=(8, 5))
sns.countplot(x='is_fraud', data=df)
plt.title('Fraud vs Non-Fraud Transactions')
plt.xlabel('Is Fraud (0=No, 1=Yes)')
plt.ylabel('Count')
plt.show()
```
That's it! You've made your first chart! 
---
## Next Steps

Once you're comfortable with these basics:

1. Try changing colors: `sns.countplot(x='is_fraud', data=df, palette='viridis')`
2. Try saving charts: `plt.savefig('fraud_chart.png')`
3. Try combining charts: Look up "matplotlib subplots"
4. Explore Seaborn styles: `sns.set_style('darkgrid')`

---

✅ **Master these 7 chart types and you can analyze any dataset!** These are the foundation of data visualization and are used every day by data scientists, analysts, and researchers worldwide.

**Remember**: The goal isn't to make pretty charts. The goal is to understand your data and communicate insights clearly. Start simple, and add complexity only when needed!
