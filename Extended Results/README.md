# Extended Results
## The Detailed Setting
The detailed setting of different baselines is as follows:

- Vector-on-function Gaussian process (VFGP): it considers vector input and functional output, where a linear B-spline regression with $n_s$ basis is used to simulate ***x*** and the corresponding coefficients ***b***$=(b_1,\cdots,b_{n_s})$ are considered as inputs of the VFGP. The distance between inputs is measured by the vector norm. Its kernel setting is similar to FFGP except for the input kernels. The detachable structure is also used, which is $K(\cdot,\cdot)=k_x(\cdot,\cdot)\otimes T_\mathcal{Y}$, here the setting of $T_\mathcal{Y}$ is the same as that of FFGP.

- Function-on-vector Gaussian process (FVGP): it considers functional input and vector output. The distance between inputs is described in the same way as that of FFGP. The covariance function is defined through the Kronecker product of the corresponding kernel functions of ***x*** and ***y***$^V$. Here ***y***$^V$ represents the discretization version of the original functional output at ***T***$_0=\lbrace t_j\in\Omega_y,j=1,\cdots,M\rbrace$. For comparison, we take FVGP and VFGP as the ablation studies.

- Vector-on-vector Gaussian process (VVGP) [[1]]([1]): it considers vector input and vector output, where the covariance function is a Kronecker product of the corresponding kernel functions of **x**$^V$ and ***y***$^V$. Here **x**$^V$ is an $n_s\times p$ matrix, representing the discretization version of the original functional inputs at $S_0=\lbrace s_i\in\Omega_x,i=1\cdots,n_s\rbrace$.

- Spectral-Distance Gaussian process (SpeD-GP) [[2]]([2]): it considers both functional inputs and outputs. It measures the functional input covariance function using their spectral distance directly in the functional space. It also discretizes the functional output into vector **y** and describes the correlation matrix of the vectorized output. The final covariance functions of both inputs and outputs are co-kridged by the Kronecker product.

- Nonlinear function-on-function regression based on the signal compression (SCR) [[3]]([3]): it considers functional inputs and outputs and constructs a nonlinear regression estimated by sequentially selecting the optimal decompositions. However, it cannot qualify the prediction uncertainty.

[1]: Feng Z, Wang J, Zhou X, Zhai C, Ma Y (2023) Robust optimization for functional multiresponse in 3d printing process. Simulation Modelling Practice and Theory 126:102774.

[2]: Chen J, Mak S, Joseph VR, Zhang C (2021) Function-on-function kriging, with applications to three-dimensional printing of aortic tissues. Technometrics 63(3):384–395.

[3]: Qi X, Luo R (2019) Nonlinear function-on-function additive model with multiple predictor curves. Statistica Sinica 29(2):719–739.

## 1-D Linear Model
Assume $x$ is a GP with mean zero and covariance function $\mathrm{cov}(x(s),x(s'))=\mathrm{exp}\lbrace -100(s-s')^2\rbrace$. Let $\mu_0(t)=\mathrm{log}(1+t)$ and $\beta(t,s)=3\sqrt{s}\mathrm{cos}(2\pi t)$. We generate different trajectories of the GP as $x_i$, and the corresponding $y_i(t)=\mu_0(t)+\int_0^1  \beta(t,s)x_i(s)ds+\varepsilon_i(t)$, here $\varepsilon_i(t)\sim \mathrm{N}(0,0.1^2\mathrm{I}_y)$, $i=1,\cdots,n$.
We set $x^\star(s)=\mathrm{cos}\left(1+5\mathrm e^s+3\mathrm{sin}^2(2\pi s)\right)$, and the desired output $y^\star=f(x^\star)$.

### `EC1-Table results`
The `EC1-Table results` file includes four tables for the 1-D linear model. The detailed description of different tables is as follows:

| Table Name        | Description                             |
| ---------------- | --------------------------------------- |
| Table EC1   | The MSE ($\times 10^{-1}$) for different kernels with $n=50$|
| Table EC2   | The MMSE (standard deviation) of the testing set of 50 replicates|
| Table EC3   | The optimization results with $n=10$ and $n=50$|
| Table EC4   | The Mean Runtime (s) of 50 replicates for different methods for training and the runtime (s) for robust optimization|
| Table EC5   | The MSE for different settings of $\tau_x$ and $\tau_y$ with $n=50$|


### The figure results for the 1-D linear model

| File Name        | Description                             |
| ---------------- | --------------------------------------- |
| `Figure EC1`   | The prediction using the FFGP, VFGP, FVGP, VVGP, SpeD-GP, and SCR estimator, respectively, together with their corresponding point-wise 95\%-credible intervals (in the shaded area) when $n = 50$.|
| `Figure EC2`   | The absolute error of optimal input and output estimated by different methods with $n=50$|




## 3-D Nonlinear Model
Consider $x(s)=(x_1,x_2,x_3)^T$ as a three-dimensional functional vector in $\Omega_x=[0,1]$. Assume $x_i(s)$ is identical and independent GP with mean zero and covariance function $\mathrm{cov}(x(s),x(s'))=\mathrm{exp}\lbrace-100(s-s')^2\rbrace$, where $s\in\Omega_x$ and $i=1,2,3$.
Let $\boldsymbol{\beta}(t,s)=s[\mathrm{sin}(6\pi t)+3\mathrm{cos}(\pi t^2)]$, $F_1(s,t,x_1)=(s+t)x_1$, $F_2(s,t,x_2)=\sqrt(st)x_2$, and $F_3(s,t,x_3)=\mathrm e^{-(s+t)}x_3$. The true system $f(\boldsymbol{x},t)=
\int\beta(s,t)[F_1(s,t,x_1)+F_2(s,t,x_2)+F_3(s,t,x_3)]ds$. We generate $y_i(t)=f(\boldsymbol{x}_i,t)+\varepsilon_i(t)$, where $\varepsilon_i(t)\sim\mathrm{N}(0,0.1^2\mathrm{I}_y)$, $i=1,\cdots,n$. We set $\boldsymbol{x}^\star=(x_1^\star,x_2^\star,x_3^\star)$  and the desired output $y^\star=f(\boldsymbol{x}^\star)$, where $x_1^\star=\mathrm{cos}(\pi/(1+\mathrm{sin}(\mathrm e^s+\pi s)^2))$, $x_2^\star=\mathrm{sin}(\pi(1+\mathrm{sin}(s+\pi s)^2))$, and $x_3^\star=\mathrm{cos}(2\pi(1+\mathrm{sin}(\mathrm e^s+\pi s)^2))$.

### `EC2-Table results`
The `EC2-Table results` file contains four tables. The detailed description of different tables is as follows:

| Table Name        | Description                             |
| ---------------- | --------------------------------------- |
| Table EC6   | The MSE for different kernels with $n=150$|
| Table EC7   | The MMSE (standard deviation) of the testing set of 50 replicates|
| Table EC8   | The optimization results with $n=30$ and $n=150$|
| Table EC9   | The Mean Runtime (s) of 50 replicates for different methods for training and the runtime (s) for robust optimization|


### The figure results in Scenario EC2

| File Name        | Description                             |
| ---------------- | --------------------------------------- |
| `Figure EC3`   | The prediction using the FFGP, VFGP, FVGP, VVGP, SpeD-GP, and SCR estimator, respectively, together with their corresponding point-wise 95\%-credible intervals (in the shaded area) when $n = 150$|
| `Figure EC4`   | The absolute error of optimal input and output estimated by different methods with $n=150$|



## Robust Optimization Analysis
For better illustration, we provide the optimal functional input parameter and the corresponding output compared to the target input and output, as seen in Figures EC5-EC8.

| File Name        | Description                             |
| -------------------- | --------------------------------------- |
|`Figure EC5`| The optimal input and output estimated by different methods with $n=50$ in Scenario 5.1|
|`Figure EC6`| The optimal input and output estimated by different methods with $n=50$ in Scenario EC1|
| `Figure EC7`   | The optimal input and output estimated by different methods with $n=50$ in Scenario EC2|
| `Figure EC8`   | The optimal structure of metamaterial and the corresponding stress-strain curve of different methods in the case study|


## `MSD Results`
The `MSD Results` file contains the table of the mean standard deviation (MSD) of 50 replicates for different methods in all Scenarios.
