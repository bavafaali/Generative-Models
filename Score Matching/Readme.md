### Score Matching

## Part I

- Trains a neural network with the score matching function below:

```math
\mathcal{L}(\theta) = \mathbb{E}_{\mathbf{x} \sim p(\mathbf{x})} \left[ \frac{1}{2} \left\lVert \mathbf{s}_\theta(\mathbf{x}) \right\rVert^2 + \mathrm{tr} \left( \nabla_\mathbf{x} \mathbf{s}_\theta(\mathbf{x}) \right) \right]
```


- Then generates samples using Langevin Dynamics:

  ![Equation](https://latex.codecogs.com/png.latex?\mathbf{x}_{i%20+%201}%20=%20\mathbf{x}_i%20+%20\varepsilon%20\nabla_\mathbf{x}\mathrm{log}%20p(\mathbf{x})%20+%20\sqrt{2\varepsilon}%20\mathbf{z}_i)

  ![Equation](https://latex.codecogs.com/png.latex?\mathbf{z}_i\sim\mathcal{N}(\mathbf{0},\mathbf{I}))

- Figure below shows the original samples and generated samples:

| Original Samples | Generated Samples |
|-----------------|-----------------|
| <img src="assets/swiss.png" width="300"> | <img src="assets/langev.png" width="300"> |

- It is evident that generated samples are not equally distributed and some parts are more dense. Furthermore, calculation of the Jacobian matrix is expensive at higher dimensions.

## Part II

- Trains a neural network with the denoising score matching function below:

  ![Equation](https://latex.codecogs.com/png.latex?\begin{align*}%20\mathcal{L}(\theta)%20&=%20\frac{1}{L}\sum_{i=1}^{L}{λ(σ_i)}\mathbb{E}_{\mathbf{x}\sim%20p(\mathbf{x}),%20\mathbf{\tilde{x}}\sim%20q_{σ_i}(\mathbf{\tilde{x}|x})}\left[\frac{1}{2}%20\left\lVert\mathbf{s_\theta}(\mathbf{\tilde{x},%20σ_i})%20-%20\nabla_\mathbf{\tilde{x}}%20\mathbf{log%20q_{σ_i}}(\mathbf{\tilde{x}|x})\right\rVert^2\right]%20\\%20&=\frac{1}{L}\sum_{i=1}^{L}\mathbb{E}_{\mathbf{x}\sim%20p(\mathbf{x}),%20\mathbf{z}\sim\mathcal{N}(\mathbf{0},\mathbf{I})}\left[\frac{1}{2}%20\left\lVert\mathbf{\sigma_i%20s_\theta}(\mathbf{x+σ_iz,%20σ_i})+%20z\right\rVert^2\right]%20+%20const.%20\end{align*})

- The neural network takes one additional input for the noise in \(t_i\).

- Then generates samples using Annealed Langevin Dynamics:

  ![Equation](https://latex.codecogs.com/png.latex?\mathbf{x}_{i%20+%201}%20=%20\mathbf{x}_i%20+%20\frac{α_i}{2}\mathbf{s_\theta}(\mathbf{x+σ_iz,%20σ_i})%20+%20\sqrt{\alpha_i}\mathbf{z}_i)

  ![Equation](https://latex.codecogs.com/png.latex?\mathbf{z}_i\sim\mathcal{N}(\mathbf{0},\mathbf{I}),\quad\mathbf{α_i=ϵ.\frac{σ_i}{σ_L}})

- Figure below shows the original samples and generated samples:

| Original Samples | Generated Samples |
|-----------------|-----------------|
| <img src="assets/swiss.png" width="300"> | <img src="assets/annealed_langevin.png" width="300"> |

- Figure below shows a comparison of the generated samples using Langevin Dynamics and Annealed Langevin Dynamics:

| Langevin Dynamics | Annealed Langevin Dynamics |
|-------------------|---------------------------|
| <img src="assets/langev.png" width="300"> | <img src="assets/annealed_langevin.png" width="300"> |

- It can be seen that generated samples using Annealed Langevin Dynamics have a more proportionate distribution.
