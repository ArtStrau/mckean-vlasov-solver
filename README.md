# McKean–Vlasov Solver

**Finite-difference solver** for the one-dimensional (1D) **deterministic McKean–Vlasov** (aggregation–diffusion / **nonlocal** Fokker–Planck) **partial differential equation (PDE)** with periodic boundary conditions (BCs).

This repository solves the 1D mean-field PDE with a **Morse-type interaction potential** in a parameter regime motivated by the reference below. In the **advection–diffusion interpretation**, the interaction generates a **nonlocal density-dependent drift (velocity) field**.

The code includes a conventional explicit baseline scheme and a **semi-implicit time-splitting** that treats diffusion implicitly while keeping the nonlocal interaction explicit. This relaxes the stability restriction on the timestep and **can significantly reduce elapsed runtime** for a fixed final time (**observed speedups** of about $7\times$–$15\times$ compared to the explicit scheme, depending on parameters and resolution). Alternatively, one can trade the relaxed time-step restriction for finer spatial resolution at comparable elapsed time.

**Reference:** N. Wehlitz, M. Sadeghi, A. Montefusco, C. Schütte, G. A. Pavliotis, S. Winkelmann,  
Approximating particle-based clustering dynamics by stochastic PDEs,  
*SIAM J. Appl. Dyn. Syst.* **24**(2), 1231–1250 (2025);  
doi: [10.1137/24M1676661](https://doi.org/10.1137/24M1676661), open-access reprint: [arXiv:2407.18952](https://doi.org/10.48550/arXiv.2407.18952)

## Model (1D, periodic)

We solve on the periodic interval (1D torus) $\mathbb{T} = [-L/2, L/2)$ the mean-field PDE for the concentration (density) field $c(x,t)$:

$$
\partial_t c(x,t) + \partial_x \big(c(x,t) v(x,t)\big) = \frac{\sigma^2}{2} \partial_{xx} c(x,t),
$$

where the nonlocal drift (velocity) field is given by the convolution

$$
v(x,t) = (F*c)(x,t), \qquad (F*c)(x,t) = \int_{\mathbb{T}} F(x-y) c(y,t) \mathrm{d}y.
$$

The interaction force is defined from a potential $V$ via $F(x) = -V'(x)$. We use the generalized Morse potential

$$
V(x) = -C_a \mathrm{e}^{-|x|/\ell_a} + C_r \mathrm{e}^{-|x|/\ell_r},
$$

understood in its periodic form on $\mathbb{T}$. Here $C_a, C_r > 0$ are the attraction/repulsion strengths and $\ell_a, \ell_r > 0$ are the corresponding interaction length scales.
For completeness, for $x\neq 0$ one may write the force explicitly as
$F(x) = -\tfrac{C_a}{\ell_a} \mathrm{e}^{-|x|/\ell_a}\mathrm{sign}(x) + \tfrac{C_r}{\ell_r} \mathrm{e}^{-|x|/\ell_r} \mathrm{sign}(x)$ and set $F(0)=0$ by symmetry.