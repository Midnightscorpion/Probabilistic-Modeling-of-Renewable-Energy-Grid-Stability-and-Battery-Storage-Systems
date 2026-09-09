# Renewable Energy Grid Stability & Battery Storage

## Overview

The increasing use of solar and wind energy introduces uncertainty into electricity generation. Wind speed varies continuously, solar generation depends on time of day and weather, and consumer demand follows daily, weekly, and seasonal patterns.

When renewable generation is insufficient to meet demand, the grid may experience a **loss of load**. Battery storage can reduce this risk by storing excess generation and supplying energy during periods of deficit.

This project develops a probabilistic reference model for a renewable-energy system with battery storage and investigates how these uncertainties combine to affect grid reliability.

---

## Main Research Question

> **To what extent can battery storage reduce the probability of loss of load in a grid with stochastic renewable generation and consumer demand?**

### Supporting Questions

1. How can wind, solar generation, and consumer demand be represented using appropriate probabilistic models?
2. How do battery capacity and state-of-charge affect the probability of a supply deficit?
3. Does load-shifting reduce peak demand and improve grid reliability?

---

## Reference Model

The system is represented through the hourly **energy balance**:

$$
E_t = G_t + B_t - D_t
$$

where:

* $E_t$ = net energy balance at time $t$
* $G_t$ = renewable generation
* $B_t$ = usable battery power supplied
* $D_t$ = consumer demand

A deficit occurs when:

$$
E_t < 0
$$

Therefore, the **Loss-of-Load Probability (LOLP)** is:

$$
LOLP = P(E_t < 0)
$$

---

## Probabilistic Model

### 1. Wind Generation

Wind speed is modelled as a continuous random variable:

$$
W_t \sim Weibull(k,\lambda)
$$

with probability density function:

$$
f_W(w)=
\frac{k}{\lambda}
\left(\frac{w}{\lambda}\right)^{k-1}
e^{-(w/\lambda)^k}
$$

Wind speed is then converted into electrical power through a wind-turbine power function:

$$
P_t^{wind}=f_W(W_t)
$$

The function accounts for the turbine's cut-in, rated, and cut-out wind speeds.

---

### 2. Solar Generation

Solar irradiance is modelled using a Beta distribution after normalization:

$$
S_t \sim Beta(\alpha,\beta)
$$

where:

$$
0\leq S_t\leq1
$$

Solar generation is then obtained through a photovoltaic conversion function:

$$
P_t^{solar}=f_S(S_t,t)
$$

During nighttime:

$$
P_t^{solar}=0
$$

The solar model may also account for differences in irradiance across hours and seasons.

---

### 3. Total Renewable Generation

Wind and solar power are combined to obtain total renewable generation:

$$
\boxed{
G_t=P_t^{wind}+P_t^{solar}
}
$$

This distinction is important because wind speed and solar irradiance are **resource variables**, not directly comparable measures of electrical generation.

---

### 4. Consumer Demand

Consumer demand is modelled as a random variable:

$$
D_t\sim F_D
$$

The choice of $F_D$ will be determined from existing research and observed characteristics of electricity consumption.

The model will consider temporal patterns such as:

* time of day
* weekday vs. weekend
* seasonal variation

A reference demand model will then be selected and justified.

---

### 5. Battery Storage

Battery storage is represented through its **state of charge (SOC)**.

The stored energy is:

$$
SOC_t\in[0, C]
$$

where $C$ is the battery capacity.

The battery evolves according to its previous state:

$$
\frac{P_t^{dis}\Delta t}{\eta_d}
$$

where:

* $P_t^{ch}$ = charging power
* $P_t^{dis}$ = discharging power
* $\eta_c$ = charging efficiency
* $\eta_d$ = discharging efficiency

The battery can be represented as a discrete **Markov chain**, where the probability of the next state depends on the current state:

$$
P(SOC_{t+1}=j\mid SOC_t=i)
$$

The stored energy $SOC_t$ is distinguished from the usable battery output $B_t$:

$$
B_t=P_t^{dis}
$$

subject to battery capacity and charge/discharge constraints.

---

## Energy Balance and Loss of Load

Combining the components gives:

$$D_t$$ 

or:

$$
\boxed{
E_t = D_t + B_t - D_t
}
$$

Define an indicator variable:

$$
I_t=
\begin{cases}
1,&E_t<0\\
0,&E_t\geq0
\end{cases}
$$

Then:

$$
P(I_t=1)=P(E_t<0)
$$

and the empirical Loss-of-Load Probability can be estimated as:

$$
\frac{1}{T}
\sum_{t=1}^{T}I_t
$$

The size of the energy deficit can also be measured as:

$$
Deficit_t=\max(0,-E_t)
$$

---

## Distribution of the Energy Balance

The final random variable is:

$$
E_t=G_t+B_t-D_t
$$

which depends on the underlying stochastic variables:

$$
W_t,\quad S_t,\quad D_t,\quad SOC_t
$$

Because these variables may follow different distributions and may not be independent, $E_t$ is not assumed to follow a standard named probability distribution.

The distribution of $E_t$ will instead be investigated through the chosen component models and simulation.

The main quantity of interest is:

$$
\boxed{
P(E_t<0)
}
$$

---

## Statistical Analysis

The model will use the following probability and statistical methods:

### Central Limit Theorem

Used to investigate the behaviour of aggregate renewable generation:

$$
\bar{G}_n
\overset{d}{\longrightarrow}
N\left(
\mu_G,
\frac{\sigma_G^2}{n}
\right)
$$

### Chebyshev's Inequality

Used to establish probability bounds:

$$
P(|X-\mu|\geq k\sigma)
\leq
\frac{1}{k^2}
$$

### Hypothesis Testing

Used to evaluate whether load-shifting significantly reduces peak demand:

$$
H_0:\mu_{peak,shifted}=\mu_{peak,original}
$$

$$
H_1:\mu_{peak,shifted}<\mu_{peak,original}
$$

### Markov Chains

Used to model transitions between battery state-of-charge levels:

$$
P(SOC_{t+1}\mid SOC_t)
$$

---

## Scenario Analysis

The reference model will be used to compare different battery and demand-management scenarios.

| Scenario                | Battery Capacity | Load Shifting | Expected Output |
| ----------------------- | ---------------: | ------------- | --------------- |
| Baseline                |             None | No            | Reference LOLP  |
| Small Storage           |              Low | No            | LOLP            |
| Medium Storage          |           Medium | No            | LOLP            |
| Large Storage           |             High | No            | LOLP            |
| Storage + Load Shifting |         Variable | Yes           | LOLP            |

The relationship between battery capacity and reliability will be examined through:

$$
C_{battery}\rightarrow P(E_t<0)
$$

---

## Expected Outcome

The project aims to determine how uncertainty in renewable generation and consumer demand propagates through the energy balance and how battery storage changes the resulting probability of supply deficits.

The expected relationship is:

$$
\text{Increasing battery capacity}
\quad\Rightarrow\quad
\text{decreasing LOLP}
$$

although the magnitude of this improvement is expected to depend on the size and duration of generation deficits.

---

## Assumptions

The initial reference model will assume:

* hourly time intervals;
* wind speed follows a Weibull distribution;
* normalized daylight solar irradiance follows a Beta distribution;
* solar generation is approximately zero during nighttime;
* consumer demand follows a selected probabilistic model;
* the battery has finite capacity and charge/discharge limits;
* battery state evolves according to its current state and system conditions;
* renewable generation and demand may contain temporal patterns;
* transmission constraints, voltage stability, frequency dynamics, and generator failures are outside the scope of the initial model.

---

## Methodology

The project will follow these stages:

1. **Review existing models** for wind, solar irradiance, and electricity consumption.

2. **Define the reference probability model** and its assumptions.

3. **Model wind speed** and convert it into wind power.

4. **Model solar irradiance** and convert it into solar power.

5. **Select and model consumer demand** using an appropriate probability distribution.

6. **Construct the battery model** using state-of-charge and Markov transitions.

7. **Define the energy balance**:

 $$E_t=G_t+B_t-D_t$$

8. **Determine the distribution of $E_t$** using simulation.

9. **Calculate LOLP and deficit size** under different battery capacities.

10. **Test load-shifting strategies** and compare peak demand.

11. **Apply CLT and Chebyshev's Inequality** to analyse the resulting distributions.

12. **Compare scenarios and evaluate the model's assumptions.**

---

## Tools

* Python
* NumPy/SciPy
* Pandas
* Matplotlib
* Probability & Statistical Modelling
* Monte Carlo Simulation
* Markov Chains


