# Additive Results
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
**Scenario EC1:** Assume $x$ is a GP with mean zero and covariance function $\mathrm{cov}(x(s),x(s'))=\mathrm{exp}\lbrace -100(s-s')^2\rbrace$. Let $\mu_0(t)=\mathrm{log}(1+t)$ and $\beta(t,s)=3\sqrt{s}\mathrm{cos}(2\pi t)$. We generate different trajectories of the GP as $x_i$, and the corresponding $y_i(t)=\mu_0(t)+\int_0^1  \beta(t,s)x_i(s)ds+\varepsilon_i(t)$, here $\varepsilon_i(t)\sim \mathrm{N}(0,0.1^2\mathrm{I}_y)$, $i=1,\cdots,n$.
We set $x^\star(s)=\mathrm{cos}\left(1+5\mathrm e^s+3\mathrm{sin}^2(2\pi s)\right)$, and the desired output $y^\star=f(x^\star)$.

### EC1-Table results

| File Name        | Description                             |
| ---------------- | --------------------------------------- |
| Figure_EC1.pdf   | The prediction using the FFGP, VFGP, FVGP, VVGP, SpeD-GP, and SCR estimator, respectively, together with their corresponding point-wise 95\%-credible intervals (in the shaded area) when $n = 50$.  |
| Figure_EC2.pdf   | The absolute error of the FFGP is the smallest for inputs and outputs, indicating that the optimized $\hat{x}$ by FFGP is almost the same as the optimal $x^{\star}$ with a tiny difference. Its corresponding estimated $\hat{y}$ is also the closest to the optimal $y^{\star}$ among all the baselines. |








**Table EC1:** We employ the proposed MSE criterion to select the optimal kernel as the correlation function for $x$, as shown in Table EC1. It can be seen that the optimal combination of FFGP is the Mat\'ern kernel with $\nu=5/2$ for $k_x$ and the exponential kernel for $k_y$. Additionally, FFGP has the smallest MSE compared to baselines with their corresponding optimal kernel combinations.

**Table EC2:** From the kernel selection results in Table EC1, we show the predicted MMSE for different models with different sample sizes and noise errors in Table EC2, respectively. FFGP consistently performs the best, with the smallest MMSE.

**Table EC3:** Table EC3 shows the optimization results of different methods based on the fitted models with $n=10$ and $n=50$, respectively. The solution found by FFGP approximates to the true $x^{*}$ most with the smallest IAE, $L_b$ and $L_v$.

**Table EC4:** Table EC3 shows the average training time of 50 replicates and the runtime for robust optimization with different sample sizes when $\lambda=0.01$. The FFGP enjoys the shortest time compared to other baselines. The discretization of the inputs for the VFGP, VVGP, SpeD-GP, and SCR causes it to take a longer runtime.

<div style="text-align: center;">
<table class="tg"><thead>
  <tr>
    <th class="tg-c3ow" colspan="7">Table EC1: The MSE ($\times 10^{-1}$) for different kernels with $n=50$</th>
  </tr></thead>
<tbody>
  <tr>
    <td class="tg-c3ow">$k_x$</td>
    <td class="tg-c3ow">$k_y$</td>
    <td class="tg-c3ow">FFGP</td>
    <td class="tg-c3ow">VFGP</td>
    <td class="tg-c3ow">FVGP</td>
    <td class="tg-baqh">VVGP</td>
    <td class="tg-baqh">SpeD-GP</td>
  </tr>
  <tr>
    <td class="tg-c3ow">``Gaussian"</td>
    <td class="tg-c3ow">``Exponential"</td>
    <td class="tg-7btt">0.0787</td>
    <td class="tg-c3ow">2.2111</td>
    <td class="tg-7btt"><b>0.1657</td>
    <td class="tg-baqh">0.2371</td>
    <td class="tg-amwm"><b>3.4309</td>
  </tr>
  <tr>
    <td class="tg-baqh">``Gaussian"</td>
    <td class="tg-baqh">``Wiener"</td>
    <td class="tg-baqh">0.3433</td>
    <td class="tg-baqh">0.6864</td>
    <td class="tg-baqh">0.1724</td>
    <td class="tg-baqh">0.2371</td>
    <td class="tg-baqh">3.7243</td>
  </tr>
  <tr>
    <td class="tg-baqh">``Mat&eacutern"</td>
    <td class="tg-baqh">``Exponential"</td>
    <td class="tg-baqh"><b>0.0680</td>
    <td class="tg-amwm">5.8097</td>
    <td class="tg-baqh">0.1658</td>
    <td class="tg-amwm"><b>0.1717</td>
    <td class="tg-baqh">3.5775</td>
  </tr>
  <tr>
    <td class="tg-baqh">``Mat&eacutern"</td>
    <td class="tg-baqh">``Wiener"</td>
    <td class="tg-baqh">0.3503</td>
    <td class="tg-baqh"><b>0.6037</td>
    <td class="tg-baqh">0.1681</td>
    <td class="tg-baqh">0.1718</td>
    <td class="tg-baqh">3.7687</td>
  </tr>
</tbody></table>


<table class="tg"><thead>
  <tr>
    <th class="tg-c3ow" colspan="8">Table EC2: The MMSE (standard deviation) of the testing set of 50 replicates</th>
  </tr></thead>
<tbody>
  <tr>
    <td class="tg-c3ow">$n$</td>
    <td class="tg-c3ow">$\lambda$</td>
    <td class="tg-c3ow">FFGP</td>
    <td class="tg-c3ow">VFGP</td>
    <td class="tg-c3ow">FVGP</td>
    <td class="tg-c3ow">VVGP</td>
    <td class="tg-c3ow">SpeD-GP</td>
    <td class="tg-c3ow">SCR</td>
  </tr>
  <tr>
    <td class="tg-c3ow" rowspan="2">10</td>
    <td class="tg-c3ow">0.01</td>
    <td class="tg-7btt"><b>0.0158 (0.0012)</td>
    <td class="tg-c3ow">0.6697 (0.0075)</td>
    <td class="tg-c3ow">0.0173 (0.0034)</td>
    <td class="tg-c3ow">0.0282 (0.0038)</td>
    <td class="tg-c3ow">0.5720 (0.0128)</td>
    <td class="tg-c3ow">0.0168 (0.0033)</td>
  </tr>
  <tr>
    <td class="tg-c3ow">1</td>
    <td class="tg-7btt"><b>0.1472 (0.0637)</td>
    <td class="tg-c3ow">1.1256 (0.1377)</td>
    <td class="tg-c3ow">1.0300 (0.3260)</td>
    <td class="tg-c3ow">0.6539 (0.3158)</td>
    <td class="tg-c3ow">1.6832 (0.3336)</td>
    <td class="tg-c3ow">1.0405 (0.3285)</td>
  </tr>
  <tr>
    <td class="tg-c3ow" rowspan="2">20</td>
    <td class="tg-c3ow">0.01</td>
    <td class="tg-7btt"><b>0.0047 (0.0010)</td>
    <td class="tg-c3ow">0.4392 (0.0079)</td>
    <td class="tg-c3ow">0.0104 (0.0033)</td>
    <td class="tg-c3ow">0.0122 (0.0034)</td>
    <td class="tg-c3ow">0.5989 (0.0099)</td>
    <td class="tg-c3ow">0.0133 (0.0035)</td>
  </tr>
  <tr>
    <td class="tg-c3ow">1</td>
    <td class="tg-7btt"><b>0.1435 (0.0644)</td>
    <td class="tg-c3ow">0.7615 (0.1010)</td>
    <td class="tg-c3ow">1.0351 (0.3287)</td>
    <td class="tg-c3ow">0.9869 (0.3109)</td>
    <td class="tg-c3ow">1.8046 (0.3395)</td>
    <td class="tg-c3ow">1.0394 (0.3278)</td>
  </tr>
  <tr>
    <td class="tg-c3ow" rowspan="2">30</td>
    <td class="tg-c3ow">0.01</td>
    <td class="tg-7btt"><b>0.0038 (0.0010)</td>
    <td class="tg-c3ow">0.2880 (0.0069)</td>
    <td class="tg-c3ow">0.0104 (0.0033)</td>
    <td class="tg-c3ow">0.0111 (0.0033)</td>
    <td class="tg-c3ow">0.5498 (0.0061)</td>
    <td class="tg-c3ow">0.0142 (0.0035)</td>
  </tr>
  <tr>
    <td class="tg-c3ow">1</td>
    <td class="tg-7btt"><b>0.1402 (0.0589)</td>
    <td class="tg-c3ow">0.2848 (0.0873)</td>
    <td class="tg-c3ow">1.0132 (0.3192)</td>
    <td class="tg-c3ow">0.9511 (0.2984)</td>
    <td class="tg-c3ow">1.7414 (0.3677)</td>
    <td class="tg-c3ow">1.0213 (0.3183)</td>
  </tr>
  <tr>
    <td class="tg-c3ow" rowspan="2">40</td>
    <td class="tg-c3ow">0.01</td>
    <td class="tg-7btt"><b>0.0035 (0.0010)</td>
    <td class="tg-c3ow">0.1018 (0.0030)</td>
    <td class="tg-c3ow">0.0104 (0.0033)</td>
    <td class="tg-c3ow">0.0112 (0.0033)</td>
    <td class="tg-c3ow">0.5243 (0.0079)</td>
    <td class="tg-c3ow">0.0131 (0.0035)</td>
  </tr>
  <tr>
    <td class="tg-c3ow">1</td>
    <td class="tg-7btt"><b>0.1443 (0.0647)</td>
    <td class="tg-c3ow">0.1790 (0.0692)</td>
    <td class="tg-c3ow">1.0342 (0.3285)</td>
    <td class="tg-c3ow">1.0146 (0.3201)</td>
    <td class="tg-c3ow">1.5919 (0.3503)</td>
    <td class="tg-c3ow">1.0392 (0.3287)</td>
  </tr>
  <tr>
    <td class="tg-c3ow" rowspan="2">50</td>
    <td class="tg-c3ow">0.01</td>
    <td class="tg-7btt"><b>0.0031 (0.0009)</td>
    <td class="tg-c3ow">0.0733 (0.0027)</td>
    <td class="tg-c3ow">0.0104 (0.0033)</td>
    <td class="tg-c3ow">0.0111 (0.0033)</td>
    <td class="tg-c3ow">0.3291 (0.0045)</td>
    <td class="tg-c3ow">0.0133 (0.0036)</td>
  </tr>
  <tr>
    <td class="tg-c3ow">1</td>
    <td class="tg-7btt"><b>0.1410 (0.0597)</td>
    <td class="tg-c3ow">0.1848 (0.0896)</td>
    <td class="tg-c3ow">1.0336 (0.3378)</td>
    <td class="tg-c3ow">0.9623 (0.3078)</td>
    <td class="tg-c3ow">1.4821 (0.3543)</td>
    <td class="tg-c3ow">1.0438 (0.3391)</td>
  </tr>
</tbody></table>


<table class="tg"><thead>
  <tr>
    <th class="tg-c3ow"></th>
    <th class="tg-c3ow" colspan="12">Table EC3: The optimization results with $n=10$ and $n=50$</th>
  </tr></thead>
<tbody>
  <tr>
    <td class="tg-0pky"></td>
    <td class="tg-0pky" colspan="6">$n=10$</td>
    <td class="tg-0lax" colspan="6">$n=50$</td>
  </tr>
  <tr>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow">FFGP</td>
    <td class="tg-c3ow">VFGP</td>
    <td class="tg-c3ow">FVGP</td>
    <td class="tg-c3ow">VVGP</td>
    <td class="tg-c3ow">SpeD-GP</td>
    <td class="tg-c3ow">SCR</td>
    <td class="tg-baqh">FFGP</td>
    <td class="tg-baqh">VFGP</td>
    <td class="tg-baqh">FVGP</td>
    <td class="tg-baqh">VVGP</td>
    <td class="tg-baqh">SpeD-GP</td>
    <td class="tg-baqh">SCR</td>
  </tr>
  <tr>
    <td class="tg-c3ow">IAE</td>
    <td class="tg-7btt">0.0101</td>
    <td class="tg-c3ow">0.3774</td>
    <td class="tg-c3ow">0.0505 </td>
    <td class="tg-c3ow">0.9178</td>
    <td class="tg-c3ow">0.4995</td>
    <td class="tg-c3ow">0.2307</td>
    <td class="tg-1wig">0.0003</td>
    <td class="tg-0lax">0.0099</td>
    <td class="tg-0lax">0.0009</td>
    <td class="tg-0lax">0.0179</td>
    <td class="tg-0lax">0.4871</td>
    <td class="tg-0lax">0.0686</td>
  </tr>
  <tr>
    <td class="tg-c3ow">$L_b$</td>
    <td class="tg-7btt">0.0042</td>
    <td class="tg-c3ow">0.2146</td>
    <td class="tg-c3ow">0.0106</td>
    <td class="tg-c3ow">0.0102</td>
    <td class="tg-c3ow">0.5116</td>
    <td class="tg-c3ow">0.0375</td>
    <td class="tg-1wig">0.0033</td>
    <td class="tg-0lax">0.0177</td>
    <td class="tg-0lax">0.0107</td>
    <td class="tg-0lax">0.0101</td>
    <td class="tg-0lax">0.4315</td>
    <td class="tg-0lax">0.0106</td>
  </tr>
  <tr>
    <td class="tg-c3ow">$L_v$</td>
    <td class="tg-c3ow">0.0038</td>
    <td class="tg-c3ow">0.2268</td>
    <td class="tg-c3ow">0.0198 </td>
    <td class="tg-7btt">0.0029</td>
    <td class="tg-c3ow">0.6311</td>
    <td class="tg-c3ow">NA</td>
    <td class="tg-0lax">0.0008</td>
    <td class="tg-0lax">0.0878</td>
    <td class="tg-0lax">0.0034</td>
    <td class="tg-1wig">0.0001</td>
    <td class="tg-0lax">0.5743</td>
    <td class="tg-0lax">NA</td>
  </tr>
</tbody></table>


<table class="tg"><thead>
  <tr>
    <th class="tg-c3ow"></th>
    <th class="tg-c3ow" colspan="12">Table EC4: The Mean Runtime (s) of 50 replicates for different methods for training and the runtime (s) for robust optimization</th>
  </tr></thead>
<tbody>
  <tr>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow" colspan="6">FFGP training</td>
    <td class="tg-c3ow" colspan="6">Robust optimization</td>
  </tr>
  <tr>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow">FFGP</td>
    <td class="tg-c3ow">VFGP</td>
    <td class="tg-c3ow">FVGP</td>
    <td class="tg-c3ow">VVGP</td>
    <td class="tg-c3ow">SpeD-GP</td>
    <td class="tg-c3ow">SCR</td>
    <td class="tg-baqh">FFGP</td>
    <td class="tg-baqh">VFGP</td>
    <td class="tg-baqh">FVGP</td>
    <td class="tg-baqh">VVGP</td>
    <td class="tg-baqh">SpeD-GP</td>
    <td class="tg-baqh">SCR</td>
  </tr>
  <tr>
    <td class="tg-c3ow">$n=10$</td>
    <td class="tg-7btt">4.12</td>
    <td class="tg-c3ow">11.34</td>
    <td class="tg-c3ow">12.50</td>
    <td class="tg-c3ow">12.79</td>
    <td class="tg-c3ow">6.76</td>
    <td class="tg-c3ow">317.57</td>
    <td class="tg-amwm">18.67</td>
    <td class="tg-baqh">21.86</td>
    <td class="tg-baqh">25.19</td>
    <td class="tg-baqh">58.37</td>
    <td class="tg-baqh">79.71</td>
    <td class="tg-baqh">56.83</td>
  </tr>
  <tr>
    <td class="tg-c3ow">$n=20$</td>
    <td class="tg-7btt">16.56</td>
    <td class="tg-c3ow">74.86</td>
    <td class="tg-c3ow">54.69</td>
    <td class="tg-c3ow">47.47</td>
    <td class="tg-c3ow">63.79</td>
    <td class="tg-c3ow">318.44</td>
    <td class="tg-amwm">-</td>
    <td class="tg-baqh">-</td>
    <td class="tg-baqh">-</td>
    <td class="tg-baqh">-</td>
    <td class="tg-baqh">-</td>
    <td class="tg-baqh">-</td>
  </tr>
  <tr>
    <td class="tg-c3ow">$n=30$</td>
    <td class="tg-7btt">38.41</td>
    <td class="tg-c3ow">89.04</td>
    <td class="tg-c3ow">182.28</td>
    <td class="tg-c3ow">132.69</td>
    <td class="tg-c3ow">222.94</td>
    <td class="tg-c3ow">328.84</td>
    <td class="tg-baqh">-</td>
    <td class="tg-baqh">-</td>
    <td class="tg-baqh">-</td>
    <td class="tg-amwm">-</td>
    <td class="tg-baqh">-</td>
    <td class="tg-baqh">-</td>
  </tr>
  <tr>
    <td class="tg-baqh">$n=40$</td>
    <td class="tg-amwm">67.02</td>
    <td class="tg-baqh">119.28</td>
    <td class="tg-baqh">342.22</td>
    <td class="tg-baqh">326.50</td>
    <td class="tg-baqh">416.06</td>
    <td class="tg-baqh">326.72</td>
    <td class="tg-baqh">-</td>
    <td class="tg-baqh">-</td>
    <td class="tg-baqh">-</td>
    <td class="tg-baqh">-</td>
    <td class="tg-baqh">-</td>
    <td class="tg-baqh">-</td>
  </tr>
  <tr>
    <td class="tg-baqh">$n=50$</td>
    <td class="tg-amwm">101.42</td>
    <td class="tg-baqh">134.50</td>
    <td class="tg-baqh">1663.86</td>
    <td class="tg-baqh">698.00</td>
    <td class="tg-baqh">1449.34</td>
    <td class="tg-baqh">521.89</td>
    <td class="tg-amwm">26.17</td>
    <td class="tg-baqh">33.61</td>
    <td class="tg-baqh">391.90</td>
    <td class="tg-baqh">1622.36</td>
    <td class="tg-baqh">410.69</td>
    <td class="tg-baqh">86.41</td>
  </tr>
</tbody></table>



### The figure results in Scenario EC1

| File Name        | Description                             |
| ---------------- | --------------------------------------- |
| Figure_EC1.pdf   | The prediction using the FFGP, VFGP, FVGP, VVGP, SpeD-GP, and SCR estimator, respectively, together with their corresponding point-wise 95\%-credible intervals (in the shaded area) when $n = 50$.  |
| Figure_EC2.pdf   | The absolute error of the FFGP is the smallest for inputs and outputs, indicating that the optimized $\hat{x}$ by FFGP is almost the same as the optimal $x^{\star}$ with a tiny difference. Its corresponding estimated $\hat{y}$ is also the closest to the optimal $y^{\star}$ among all the baselines. |




## 3-D Nonlinear Model
**Scenario EC2:** Consider $x(s)=(x_1,x_2,x_3)^T$ as a three-dimensional functional vector in $\Omega_x=[0,1]$. Assume $x_i(s)$ is identical and independent GP with mean zero and covariance function $\mathrm{cov}(x(s),x(s'))=\mathrm{exp}\lbrace-100(s-s')^2\rbrace$, where $s\in\Omega_x$ and $i=1,2,3$.
Let $\boldsymbol{\beta}(t,s)=s[\mathrm{sin}(6\pi t)+3\mathrm{cos}(\pi t^2)]$, $F_1(s,t,x_1)=(s+t)x_1$, $F_2(s,t,x_2)=\sqrt(st)x_2$, and $F_3(s,t,x_3)=\mathrm e^{-(s+t)}x_3$. The true system $f(\boldsymbol{x},t)=
\int\beta(s,t)[F_1(s,t,x_1)+F_2(s,t,x_2)+F_3(s,t,x_3)]ds$. We generate $y_i(t)=f(\boldsymbol{x}_i,t)+\varepsilon_i(t)$, where $\varepsilon_i(t)\sim\mathrm{N}(0,0.1^2\mathrm{I}_y)$, $i=1,\cdots,n$. We set $\boldsymbol{x}^\star=(x_1^\star,x_2^\star,x_3^\star)$  and the desired output $y^\star=f(\boldsymbol{x}^\star)$, where $x_1^\star=\mathrm{cos}(\pi/(1+\mathrm{sin}(\mathrm e^s+\pi s)^2))$, $x_2^\star=\mathrm{sin}(\pi(1+\mathrm{sin}(s+\pi s)^2))$, and $x_3^\star=\mathrm{cos}(2\pi(1+\mathrm{sin}(\mathrm e^s+\pi s)^2))$.

### The table results in Scenario EC1
**Table EC5:** Table EC5 shows the MSE for baselines with different kernel selections. We can see that the optimal kernel combination of FFGP is the same as in Scenario 5.1 and Scenario EC1.

**Table EC6:** Based on the kernel selection results in Figure \ref{kerop3}, Table \ref{s3-mmse} shows the predicted MMSE with different sample sizes and noise errors. FFGP performs much better than the other baselines and converges gradually as the sample size increases. SpeD-GP has an unsatisfactory MMSE. This may be due to inappropriate input transforms.

**Table EC7:** Table \ref{ro3} displays the optimization results for various optimization algorithms based on their corresponding estimated models with $n = 30$ and $150$. The performance of FFGP is the best.

**Table EC8:** The average training time of 50 replicates and the runtime for robust optimization with different sample sizes when $\lambda=0.01$ in Scenario \ref{exam3} are provided in Table \ref{time3}. It can be seen that the runtime of training the FFGP and finding robust solutions based on the FRGD algorithm is the shortest. The proposed method has a better performance compared to other baselines.


   <table class="tg"><thead>
  <tr>
    <th class="tg-c3ow" colspan="7">Table EC7: The MSE for different kernels with $n=150$ in Scenario EC2</th>
  </tr></thead>
<tbody>
  <tr>
    <td class="tg-c3ow">$k_x$</td>
    <td class="tg-c3ow">$k_y$</td>
    <td class="tg-c3ow">FFGP</td>
    <td class="tg-c3ow">VFGP</td>
    <td class="tg-c3ow">FVGP</td>
    <td class="tg-c3ow">VVGP</td>
    <td class="tg-c3ow">SpeD-GP</td>
  </tr>
  <tr>
    <td class="tg-c3ow">``Gaussian"</td>
    <td class="tg-c3ow">``Exponential"</td>
    <td class="tg-c3ow">0.0040</td>
    <td class="tg-c3ow">0.5959</td>
    <td class="tg-c3ow">0.2537</td>
    <td class="tg-7btt">0.1867</td>
    <td class="tg-c3ow">0.6887</td>
  </tr>
  <tr>
    <td class="tg-c3ow">``Gaussian"</td>
    <td class="tg-c3ow">``Wiener"</td>
    <td class="tg-c3ow">0.0240</td>
    <td class="tg-7btt">0.5958</td>
    <td class="tg-c3ow">0.2275</td>
    <td class="tg-c3ow">0.2061</td>
    <td class="tg-7btt">0.6236</td>
  </tr>
  <tr>
    <td class="tg-c3ow">``Mat&eacutern"</td>
    <td class="tg-c3ow">``Exponential"</td>
    <td class="tg-7btt">0.0017</td>
    <td class="tg-c3ow">0.6605</td>
    <td class="tg-c3ow">0.2278</td>
    <td class="tg-c3ow">0.1986</td>
    <td class="tg-c3ow">0.6312</td>
  </tr>
  <tr>
    <td class="tg-c3ow">``Mat&eacutern"</td>
    <td class="tg-c3ow">``Wiener"</td>
    <td class="tg-c3ow">0.0221</td>
    <td class="tg-c3ow">0.6491</td>
    <td class="tg-7btt">0.2177</td>
    <td class="tg-c3ow">0.2046</td>
    <td class="tg-c3ow">0.6267</td>
  </tr>
</tbody></table>


<table class="tg"><thead>
  <tr>
    <th class="tg-c3ow" colspan="8">Table EC6: The MMSE (standard deviation) of the testing set of 50 replicates in Scenario EC2</th>
  </tr></thead>
<tbody>
  <tr>
    <td class="tg-c3ow">$n$</td>
    <td class="tg-c3ow">$\lambda$</td>
    <td class="tg-c3ow">FFGP</td>
    <td class="tg-c3ow">VFGP</td>
    <td class="tg-c3ow">FVGP</td>
    <td class="tg-c3ow">VVGP</td>
    <td class="tg-c3ow">SpeD-GP</td>
    <td class="tg-baqh">SCR</td>
  </tr>
  <tr>
    <td class="tg-baqh" rowspan="2">30</td>
    <td class="tg-baqh">0.01</td>
    <td class="tg-amwm">0.0075 (0.0015)</td>
    <td class="tg-baqh">0.5957 (0.0036)</td>
    <td class="tg-baqh">0.2430 (0.0032)</td>
    <td class="tg-baqh">0.2555 (0.0033)</td>
    <td class="tg-baqh">0.6717 (0.0029)</td>
    <td class="tg-baqh">0.0124 (0.0015)</td>
  </tr>
  <tr>
    <td class="tg-baqh">1</td>
    <td class="tg-amwm">0.1670 (0.0275)</td>
    <td class="tg-baqh">0.5957 (0.0342)</td>
    <td class="tg-baqh">0.4628 (0.0482)</td>
    <td class="tg-baqh">0.4711 (0.0476)</td>
    <td class="tg-baqh">0.7217 (0.0287)</td>
    <td class="tg-baqh">0.2975 (0.0298)</td>
  </tr>
  <tr>
    <td class="tg-c3ow" rowspan="2">60</td>
    <td class="tg-c3ow">0.01</td>
    <td class="tg-7btt">0.0043 (0.0005)</td>
    <td class="tg-c3ow">0.5958 (0.0034)</td>
    <td class="tg-c3ow">0.2466 (0.0032)</td>
    <td class="tg-c3ow">0.2585 (0.0030)</td>
    <td class="tg-c3ow">0.7370 (0.0017)</td>
    <td class="tg-baqh">0.0113 (0.0012)</td>
  </tr>
  <tr>
    <td class="tg-baqh">1</td>
    <td class="tg-amwm">0.1492 (0.0252)</td>
    <td class="tg-baqh">0.5958 (0.0224)</td>
    <td class="tg-baqh">0.3088 (0.0350)</td>
    <td class="tg-baqh">0.4054 (0.0364)</td>
    <td class="tg-baqh">0.7656 (0.0277)</td>
    <td class="tg-baqh">0.1641 (0.0264)</td>
  </tr>
  <tr>
    <td class="tg-baqh" rowspan="2">90</td>
    <td class="tg-baqh">0.01</td>
    <td class="tg-amwm">0.0021 (0.0002)</td>
    <td class="tg-baqh">0.5958 (0.0028)</td>
    <td class="tg-baqh">0.2161 (0.0023)</td>
    <td class="tg-baqh">0.1962 (0.0026)</td>
    <td class="tg-baqh">0.7022 (0.0037)</td>
    <td class="tg-baqh">0.0073 (0.0007)</td>
  </tr>
  <tr>
    <td class="tg-baqh">1</td>
    <td class="tg-amwm">0.0836 (0.0113)</td>
    <td class="tg-baqh">0.5958 (0.0201)</td>
    <td class="tg-baqh">0.2771 (0.0255)</td>
    <td class="tg-baqh">0.3929 (0.0298)</td>
    <td class="tg-baqh">0.7339 (0.0321)</td>
    <td class="tg-baqh">0.1459 (0.0187)</td>
  </tr>
  <tr>
    <td class="tg-baqh" rowspan="2">120</td>
    <td class="tg-baqh">0.01</td>
    <td class="tg-amwm">0.0019 (0.0002)</td>
    <td class="tg-baqh">0.5958 (0.0028)</td>
    <td class="tg-baqh">0.2309 (0.0027)</td>
    <td class="tg-baqh">0.2058 (0.0030)</td>
    <td class="tg-baqh">0.7201 (0.0018)</td>
    <td class="tg-baqh">0.0063 (0.0007)</td>
  </tr>
  <tr>
    <td class="tg-c3ow">1</td>
    <td class="tg-7btt">0.0664 (0.0093)</td>
    <td class="tg-c3ow">0.5958 (0.0264)</td>
    <td class="tg-c3ow">0.2962 (0.0303)</td>
    <td class="tg-c3ow">0.4305 (0.0362)</td>
    <td class="tg-c3ow">0.7383 (0.0187)</td>
    <td class="tg-baqh">0.1397 (0.0177)</td>
  </tr>
  <tr>
    <td class="tg-c3ow" rowspan="2">150</td>
    <td class="tg-c3ow">0.01</td>
    <td class="tg-7btt">0.0017 (0.0002)</td>
    <td class="tg-c3ow">0.5958 (0.0027)</td>
    <td class="tg-c3ow">0.2181 (0.0021)</td>
    <td class="tg-c3ow">0.1869 (0.0020)</td>
    <td class="tg-c3ow">0.6220 (0.0020)</td>
    <td class="tg-baqh">0.0068 (0.0006)</td>
  </tr>
  <tr>
    <td class="tg-c3ow">1</td>
    <td class="tg-7btt">0.0411 (0.0084)</td>
    <td class="tg-c3ow">0.5958 (0.0193)</td>
    <td class="tg-c3ow">0.2331 (0.0126)</td>
    <td class="tg-c3ow">0.2450 (0.0146)</td>
    <td class="tg-c3ow">0.6265 (0.0098)</td>
    <td class="tg-baqh">0.1150 (0.0119)</td>
  </tr>
</tbody></table>

    <table class="tg"><thead>
  <tr>
    <th class="tg-c3ow"></th>
    <th class="tg-c3ow" colspan="12">Table EC7: The optimization results with $n=30$ and $n=150$</th>
  </tr></thead>
<tbody>
  <tr>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow" colspan="6">$n=30$</td>
    <td class="tg-c3ow" colspan="6">$n=50$</td>
  </tr>
  <tr>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow">FFGP</td>
    <td class="tg-c3ow">VFGP</td>
    <td class="tg-c3ow">FVGP</td>
    <td class="tg-c3ow">VVGP</td>
    <td class="tg-c3ow">SpeD-GP</td>
    <td class="tg-c3ow">SCR</td>
    <td class="tg-c3ow">FFGP</td>
    <td class="tg-c3ow">VFGP</td>
    <td class="tg-c3ow">FVGP</td>
    <td class="tg-c3ow">VVGP</td>
    <td class="tg-c3ow">SpeD-GP</td>
    <td class="tg-c3ow">SCR</td>
  </tr>
  <tr>
    <td class="tg-c3ow">IAE</td>
    <td class="tg-7btt">0.1158</td>
    <td class="tg-c3ow">0.2050</td>
    <td class="tg-c3ow">0.3934</td>
    <td class="tg-c3ow">0.4969</td>
    <td class="tg-c3ow">0.8884</td>
    <td class="tg-c3ow">0.3436</td>
    <td class="tg-7btt">0.0043</td>
    <td class="tg-c3ow">0.0115</td>
    <td class="tg-c3ow">0.0199</td>
    <td class="tg-c3ow">0.4590</td>
    <td class="tg-c3ow">0.8598</td>
    <td class="tg-c3ow">0.0095</td>
  </tr>
  <tr>
    <td class="tg-c3ow">$L_b$</td>
    <td class="tg-7btt">0.0127</td>
    <td class="tg-c3ow">0.2922</td>
    <td class="tg-c3ow">0.0612</td>
    <td class="tg-c3ow">0.0216</td>
    <td class="tg-c3ow">0.0297</td>
    <td class="tg-c3ow">0.0214</td>
    <td class="tg-7btt">0.0055</td>
    <td class="tg-c3ow">0.0201</td>
    <td class="tg-c3ow">0.0327</td>
    <td class="tg-c3ow">0.3540</td>
    <td class="tg-c3ow">0.0964</td>
    <td class="tg-c3ow">0.0065</td>
  </tr>
  <tr>
    <td class="tg-c3ow">$L_v$</td>
    <td class="tg-7btt">0.0073</td>
    <td class="tg-c3ow">0.0514</td>
    <td class="tg-c3ow">0.0131</td>
    <td class="tg-c3ow">0.0752</td>
    <td class="tg-c3ow">0.9101</td>
    <td class="tg-c3ow">NA</td>
    <td class="tg-7btt">0.0044</td>
    <td class="tg-c3ow">0.0050</td>
    <td class="tg-c3ow">0.0187</td>
    <td class="tg-c3ow">20.5023</td>
    <td class="tg-c3ow">0.0331</td>
    <td class="tg-c3ow">NA</td>
  </tr>
</tbody></table>




   <table class="tg"><thead>
  <tr>
    <th class="tg-c3ow"></th>
    <th class="tg-c3ow" colspan="12">Table EC8: The Mean Runtime (s) of 50 replicates for different methods for training and the runtime (s) for robust optimization</th>
  </tr></thead>
<tbody>
  <tr>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow" colspan="6">FFGP training</td>
    <td class="tg-c3ow" colspan="6">Robust optimization</td>
  </tr>
  <tr>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow">FFGP</td>
    <td class="tg-c3ow">VFGP</td>
    <td class="tg-c3ow">FVGP</td>
    <td class="tg-c3ow">VVGP</td>
    <td class="tg-c3ow">SpeD-GP</td>
    <td class="tg-c3ow">SCR</td>
    <td class="tg-c3ow">FFGP</td>
    <td class="tg-c3ow">VFGP</td>
    <td class="tg-c3ow">FVGP</td>
    <td class="tg-c3ow">VVGP</td>
    <td class="tg-c3ow">SpeD-GP</td>
    <td class="tg-c3ow">SCR</td>
  </tr>
  <tr>
    <td class="tg-c3ow">$n=30$</td>
    <td class="tg-7btt">53.01</td>
    <td class="tg-c3ow">63.22</td>
    <td class="tg-c3ow">344.07</td>
    <td class="tg-c3ow">175.64</td>
    <td class="tg-c3ow">319.54</td>
    <td class="tg-c3ow">732.01</td>
    <td class="tg-7btt">214.32</td>
    <td class="tg-c3ow">1191.84</td>
    <td class="tg-c3ow">1601.37</td>
    <td class="tg-c3ow">896.14</td>
    <td class="tg-c3ow">817.80</td>
    <td class="tg-c3ow">56.83</td>
  </tr>
  <tr>
    <td class="tg-c3ow">$n=60$</td>
    <td class="tg-7btt">171.81</td>
    <td class="tg-c3ow">183.50</td>
    <td class="tg-c3ow">1831.63</td>
    <td class="tg-c3ow">896.56</td>
    <td class="tg-c3ow">718.56</td>
    <td class="tg-c3ow">1503.74</td>
    <td class="tg-7btt">-</td>
    <td class="tg-c3ow">-</td>
    <td class="tg-c3ow">-</td>
    <td class="tg-c3ow">-</td>
    <td class="tg-c3ow">-</td>
    <td class="tg-c3ow">-</td>
  </tr>
  <tr>
    <td class="tg-c3ow">$n=90$</td>
    <td class="tg-7btt">401.41</td>
    <td class="tg-c3ow">412.13</td>
    <td class="tg-c3ow">6027.72</td>
    <td class="tg-c3ow">5294.21</td>
    <td class="tg-c3ow">3163.05</td>
    <td class="tg-c3ow">1767.33</td>
    <td class="tg-c3ow">-</td>
    <td class="tg-c3ow">-</td>
    <td class="tg-c3ow">-</td>
    <td class="tg-7btt">-</td>
    <td class="tg-c3ow">-</td>
    <td class="tg-c3ow">-</td>
  </tr>
  <tr>
    <td class="tg-c3ow">$n=120$</td>
    <td class="tg-7btt">617.24</td>
    <td class="tg-c3ow">669.82</td>
    <td class="tg-c3ow">27380.98</td>
    <td class="tg-c3ow">22825.31</td>
    <td class="tg-c3ow">19741.98</td>
    <td class="tg-c3ow">1769.15</td>
    <td class="tg-c3ow">-</td>
    <td class="tg-c3ow">-</td>
    <td class="tg-c3ow">-</td>
    <td class="tg-c3ow">-</td>
    <td class="tg-c3ow">-</td>
    <td class="tg-c3ow">-</td>
  </tr>
  <tr>
    <td class="tg-c3ow">$n=150$</td>
    <td class="tg-7btt">955.16</td>
    <td class="tg-c3ow">958.70</td>
    <td class="tg-c3ow">35598.08</td>
    <td class="tg-c3ow">33250.98</td>
    <td class="tg-c3ow">31445.86</td>
    <td class="tg-c3ow">3104.00</td>
    <td class="tg-7btt">274.69</td>
    <td class="tg-c3ow">1429.53</td>
    <td class="tg-c3ow">4482.31</td>
    <td class="tg-c3ow">1424.01</td>
    <td class="tg-c3ow">1298.12</td>
    <td class="tg-c3ow">3601.18</td>
  </tr>
</tbody></table>











### The figure results in Scenario EC1
