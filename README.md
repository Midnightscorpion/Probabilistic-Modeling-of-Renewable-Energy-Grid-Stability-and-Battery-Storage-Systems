# Renewable Energy Grid Stability & Battery Storage

## Overview

Solar and wind power are intermittent, while electricity demand varies over time. This uncertainty can create periods where renewable generation is insufficient to meet demand, resulting in a **loss of load**.

This project develops a probabilistic model of a renewable-energy grid with battery storage to investigate whether storage can reduce the probability of such events.

The central reliability measure is the **Loss-of-Load Probability (LOLP)**:

$$P_t=P(G_t+B_t\leq D_t)$$

where:

* $G_t$ = renewable generation at time $t$
* $B_t$ = power supplied by the battery
* $D_t$ = electricity demand

## Main Research Question

> **To what extent can battery storage reduce the probability of loss of load in a grid with stochastic solar, wind, and consumer demand?**

### Supporting Questions

1. How should solar generation, wind generation, and demand be probabilistically modelled?
2. How does battery capacity and state-of-charge affect the Loss-of-Load Probability?
3. Does load-shifting reduce peak demand and improve grid reliability?

## Methodology

The model will be developed in the following stages:

1. **Define the probability space**
   
   Model hourly realizations of renewable generation and consumer demand.

3. **Model renewable uncertainty**

   Wind speed:

   $$W_t\sim Weibull(k,\lambda)$$

   Solar irradiance:

   $$S_t\sim Beta(\alpha,\beta)$$

4. **Model renewable generation**

   Convert wind speed and solar irradiance into hourly wind and solar power output:

   $$G_t=W_t + S_t$$

5. **Model battery storage**

   Represent battery state-of-charge as a stochastic process / discrete Markov chain:

   $$B_{t+1}=f(B_t,G_t,D_t)$$

6. **Evaluate reliability**

   Determine whether each hour experiences a loss of load:

$$
I_t=
\begin{cases}
1,&E_t<0\\
0,&E_t\geq0
\end{cases}
$$


   and estimate:

   $$\bar{L_T}=\frac{1}{T}\sum_{t=1}^{T}I_t$$

8. **Apply probability and statistical methods**

   * Central Limit Theorem (CLT)
   * Chebyshev's Inequality
   * Hypothesis testing
   * Markov chains

9. **Compare scenarios**

   Compare LOLP for different battery capacities and demand-management strategies.

## Expected Outcome

The project aims to quantify the relationship between **battery capacity and grid reliability**, and determine whether battery storage and load-shifting can significantly reduce the probability of unmet electricity demand.

## Scope

This project focuses on **probabilistic supply-demand reliability**. It does not attempt to model the full physical stability of a power grid, such as voltage stability, frequency dynamics, transmission constraints, or generator faults.

## Tools

* Python
* NumPy / SciPy
* Pandas
* Matplotlib
* Statistical and probabilistic modelling



