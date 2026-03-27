# CAC 40 Portfolio Construction and Stochastic Dominance Analysis

## Project Overview
This project studies whether randomly generated portfolios built from **CAC 40 stocks** can outperform the **CAC 40 index** not only in a traditional risk/return framework, but also through a more robust **stochastic dominance** approach.

The notebook combines **financial data collection**, **portfolio simulation**, **descriptive statistics**, **interactive visualizations**, and **stochastic dominance testing** to answer a practical investment question:

> Can portfolios that stochastically dominate the CAC 40 in year *t* remain interesting investment candidates in year *t+1*?

---

## Important Note
To access the charts properly, **download the notebook file (`projetRUIZ.ipynb`) and open it locally** in **Jupyter Notebook**, **JupyterLab**, or **VS Code**.

Some figures in this project are **interactive Plotly charts**, so they are best viewed directly from the notebook rather than through a simple file preview.

---

## Main Objective
The goal of the project is to compare **100 randomly generated portfolios** with the **CAC 40 index** over the period **2014–2024**, and to determine:

1. how these portfolios behave in terms of **return** and **risk**,
2. whether some portfolios **stochastically dominate** the benchmark,
3. whether this dominance is **stable over time**,
4. and whether dominance in year *t* can be used as a useful signal for **performance in year *t+1***.

This makes the project both a **quantitative finance exercise** and an **economic interpretation exercise**, because stochastic dominance is directly connected to investor preferences and risk aversion.

---

## Data Used
The project uses daily market data for:
- the **CAC 40 index** (`^FCHI`),
- the stocks composing the **CAC 40**,
- over the period **January 1, 2014 to December 31, 2024**.

The data is downloaded with **`yfinance`** and only **closing prices** are retained in order to compute daily returns.

### Data cleaning choice
The ticker **`URW.PA`** was removed from the study because its price history is incomplete over most of the analysis period. Keeping it would have introduced too many missing values and would have made the portfolio comparisons less consistent.

---

## Project Workflow

### 1. Data collection and preparation
The notebook starts by downloading CAC 40 stock prices and the index itself. A first descriptive step checks:
- the available date range,
- the number of observations,
- the proportion of missing values,
- and the overall consistency of the dataset.

This creates the price matrix used throughout the project.

### 2. Random portfolio generation
The project then generates **100 random portfolios** labeled **P1 to P100**.

Each portfolio:
- contains at least **two stocks**,
- is built **without short selling**,
- has **strictly positive weights**,
- and the weights always **sum to 1**.

The weights are generated with a **Dirichlet distribution**, which is a convenient way to obtain valid portfolio weights.

### 3. Daily return matrix construction
Once prices are available, the notebook computes:
- stock daily returns,
- portfolio daily returns,
- and CAC 40 daily returns.

Portfolio returns are obtained through matrix multiplication between:
- the matrix of stock returns,
- and the matrix of portfolio weights.

The final dataset contains the daily return series for:
- **100 portfolios**,
- plus the **CAC 40 index**.

### 4. Descriptive statistics for 2023
A specific focus is placed on **2023** in order to compute:
- annualized mean return,
- daily variance,
- daily standard deviation.

This section makes it possible to compare all portfolios and the CAC 40 in a simple and readable way.

### 5. Risk/return analysis and visualizations
The notebook produces a **risk/return scatter plot** for 2023, using:
- daily standard deviation on the x-axis,
- annualized mean return on the y-axis.

Several bonus visualizations are also included:
- number of assets held in each portfolio,
- **iso-Sharpe lines**,
- identification of the **maximum Sharpe portfolio**,
- KPI comparison between the CAC 40 and the **top 3 Sharpe portfolios**.

These charts help identify which portfolios offer the most attractive return/risk trade-off.

---

## Stochastic Dominance Analysis

### Why stochastic dominance?
A traditional mean/variance comparison can miss important information. Two portfolios may have similar average return and volatility but very different downside behavior.

Stochastic dominance improves the analysis by comparing the **entire return distribution**, not just a few summary statistics.

### Orders considered
The project studies two levels of stochastic dominance:

- **First-order stochastic dominance (FSD / order 1)**  
  A portfolio is preferred by all investors with an increasing utility function.

- **Second-order stochastic dominance (SSD / order 2)**  
  A portfolio is preferred by all investors with an increasing and concave utility function, meaning **risk-averse investors**.

### Testing method
The notebook uses the **`PySDTest`** package to test dominance between:
- a portfolio return series,
- and the CAC 40 return series,
- year by year.

For each year and each portfolio, the code tests dominance in **both directions**:
- portfolio → CAC 40,
- CAC 40 → portfolio.

The results are stored in two dominance matrices:
- **`SD1_matrix`** for order 1,
- **`SD2_matrix_fixed`** for order 2.

Each matrix value is coded as:
- **0** = no clear dominance,
- **1** = the portfolio dominates the CAC 40,
- **2** = the CAC 40 dominates the portfolio.

A consistency adjustment is then applied so that any first-order dominance is also reflected in the second-order matrix.

---

## Main Findings Highlighted in the Notebook
Here are the most visible results produced in the notebook:

### 2023 descriptive results
- The **CAC 40 ranked 70th out of 101 series** in annualized return in 2023.
- The top 2023 return portfolios included **P47**, **P85**, and **P82**.
- The least risky portfolios included **P71**, **P56**, **P6**, **P58**, and **P37**.

### Sharpe-style comparison in 2023
A KPI comparison is performed between the CAC 40 and the top 3 Sharpe portfolios. In the notebook output, the comparison includes:
- **CAC40**,
- **P82**,
- **P1**,
- **P90**.

### Dominance results for 2023
- **15 portfolios out of 100** dominate the CAC 40 at **order 1**.
- **21 portfolios out of 100** dominate the CAC 40 at **order 2**.
- Among them, **6 portfolios become dominant only at order 2**, which is consistent with the idea that second-order dominance is less restrictive and more relevant for risk-averse investors.

### Stability over time
The notebook also studies which portfolios dominate most often over time:
- the most stable **order 1** dominant portfolio is **P13**, dominating in **5 out of 10 years**,
- the most stable **order 2** dominant portfolio is **P83**, dominating in **7 out of 10 years**.

### Investment interpretation
The final sections suggest that dominance, especially **second-order dominance**, may provide a useful active investment filter. According to the project’s interpretation:
- **order 1 dominance** is stronger but rarer,
- **order 2 dominance** appears more realistic for actual investors,
- and portfolios dominant in year *t* often seem to perform better than the CAC 40 in year *t+1*, especially under **SD2**.

---

## What the Project Demonstrates
This project shows that:

- building random portfolios from a major equity index can generate very different risk/return profiles,
- some portfolios can dominate the benchmark in a distributional sense,
- stochastic dominance provides a deeper investment comparison than a simple return/volatility analysis,
- and second-order dominance may be especially meaningful for practical portfolio selection.

In other words, the notebook does not only ask **which portfolio earned more**, but also **which portfolio offered a more favorable return distribution for an investor**.

---

## Tools and Libraries
The project relies on the following Python libraries:
- **NumPy**
- **Pandas**
- **yfinance**
- **Plotly**
- **Matplotlib**
- **PySDTest**
- **SciPy**
- **joblib**
- **tqdm**

---

## Outputs Produced
During execution, the notebook creates several useful outputs, including:
- descriptive tables,
- interactive charts,
- dominance matrices,
- graphical comparisons of empirical CDFs and integrated CDFs,
- and an exported CSV file for the year *t+1* performance analysis:
  - `outputs/Q10_table_pretty_SD1_SD2.csv`

---

## How to Use the Project
1. **Download** the notebook file `projetRUIZ.ipynb`.
2. Open it locally in **Jupyter Notebook**, **JupyterLab**, or **VS Code**.
3. Install the required Python packages.
4. Run the notebook from top to bottom.
5. Explore the interactive charts and exported outputs.

---

## Conclusion
This project is a complete introduction to:
- **portfolio construction**,
- **financial return analysis**,
- **interactive data visualization**,
- and **stochastic dominance testing**.

Its main strength is that it connects **quantitative finance methods** with an **economic interpretation of investor preferences**. Rather than relying only on average return and volatility, it evaluates whether a portfolio is statistically preferable to the CAC 40 from the perspective of different types of investors.

Overall, the notebook suggests that **second-order stochastic dominance** may be the most convincing framework for identifying portfolios that are both attractive and economically meaningful for a risk-averse investor.
