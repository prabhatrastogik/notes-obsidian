

## Teaser

### Adam optimization

**Input:**  
Training set of data points indexed by \( n \in \{1, \ldots, N\} \)  
Batch size \( B \)  
Error function per mini-batch \( E_{n:n+B-1}(w) \)  
Learning rate parameter \( \eta \)  
Decay parameters \( eta_1 \) and \( eta_2 \)  
Stabilization parameter \( \delta \)

**Output:**  
Final weight vector \( w \)

```
n ← 1  
s ← 0  
r ← 0  
repeat  
    Choose a mini-batch at random from D  
    g ← -∇Eₙ:ₙ₊ᴮ₋₁(w)      // evaluate gradient vector  
    s ← β₁s + (1 - β₁)g  
    r ← β₂r + (1 - β₂)g ⊙ g   // element-wise multiply  
    ŝ ← s / (1 - β₁ⁿ)         // bias correction  
    ṙ ← r / (1 - β₂ⁿ)         // bias correction  
    Δw ← -η * ŝ / (√ṙ + δ)    // element-wise operations  
    w ← w + Δw               // weight vector update  
    n ← n + B  
    if n + B > N then  
        shuffle data  
        n ← 1  
    end if  
until convergence  

return w
```


## Chapter: Optimizing the Descent: Learning Rate Adjustment Methods

### 1. Introduction: The Quest for the Minimum

At the heart of training many machine learning models, particularly deep neural networks, lies the process of **optimization**. We define a **loss function** (also called a cost function or objective function) that quantifies how poorly our model is performing on the training data. The goal of training is to find the set of model **parameters** (weights and biases) that minimize this loss function.

Imagine the loss function as a complex, high-dimensional landscape with hills, valleys, and plateaus. Our goal is to find the lowest point in this landscape – the **global minimum**. The most fundamental algorithm for navigating this landscape is **Gradient Descent**.

### 2. Gradient Descent: The Basic Compass

Gradient Descent is an iterative optimization algorithm that aims to find a local minimum of a differentiable function (our loss function, `J`).

**The Core Idea:** Start at a random point (initial set of parameters) in the loss landscape. Calculate the direction of the steepest descent from that point. Take a small step in that direction. Repeat.

**Key Components:**

1.  **Parameters (`θ`):** The variables of the model we want to learn (e.g., weights `W` and biases `b` in a neural network).
2.  **Loss Function (`J(θ)`):** A function that measures the error or "loss" of the model given the current parameters `θ`.
3.  **Gradient (`∇J(θ)`):** A vector containing the partial derivatives of the loss function with respect to each parameter. It points in the direction of the *steepest ascent* from the current point. `∇J(θ) = [∂J/∂θ₁, ∂J/∂θ₂, ..., ∂J/∂θn]`.
4.  **Learning Rate (`α`):** A scalar hyperparameter that controls the size of the step taken in the opposite direction of the gradient during each iteration.

**The Update Rule (Vanilla Gradient Descent):**

In each iteration `t`, we update the parameters `θ` as follows:

```
θ_(t+1) = θ_t - α * ∇J(θ_t)
```

Here:
*   `θ_t` are the parameters at iteration `t`.
*   `θ_(t+1)` are the updated parameters for the next iteration.
*   `α` is the learning rate.
*   `∇J(θ_t)` is the gradient of the loss function evaluated at `θ_t`.

We subtract the gradient because it points uphill, and we want to go downhill (minimize the loss).

**Variants of Gradient Descent:**

*   **Batch Gradient Descent:** Calculates the gradient using the *entire* training dataset in each iteration. Accurate gradient estimate, but computationally very expensive and slow for large datasets. Cannot be used for online learning.
*   **Stochastic Gradient Descent (SGD):** Calculates the gradient using *only one* randomly selected training example in each iteration. Very fast iterations, noisy gradient estimate, can help escape shallow local minima due to noise, suitable for online learning. The update rule is often written as: `θ_(t+1) = θ_t - α * ∇J(θ_t; x^(i), y^(i))`, where `(x^(i), y^(i))` is a single training sample.
*   **Mini-Batch Gradient Descent:** Calculates the gradient using a small, randomly selected *subset* (mini-batch) of the training data (e.g., 32, 64, 128 samples). Strikes a balance between the accuracy of Batch GD and the efficiency of SGD. It's the most commonly used variant in deep learning. The update rule is: `θ_(t+1) = θ_t - α * ∇J(θ_t; X^(batch), Y^(batch))`.

From here on, when we refer to "SGD", we often implicitly mean Mini-Batch Gradient Descent unless specified otherwise.

### 3. The Challenge: Why a Fixed Learning Rate is Problematic

The learning rate `α` is arguably the most critical hyperparameter in Gradient Descent. Choosing the right *fixed* value is difficult:

*   **If `α` is too small:**
    *   Convergence will be very slow, requiring many iterations.
    *   The optimizer might get stuck in shallow local minima or saddle points, as the steps are too tiny to escape.
*   **If `α` is too large:**
    *   The optimizer might overshoot the minimum, causing the loss to oscillate wildly or even diverge (increase indefinitely).
    *   It might jump across narrow valleys containing the minimum without ever settling in.

Furthermore, the ideal learning rate might change during training. Initially, when far from the minimum, larger steps might be beneficial. As we approach a minimum, smaller steps are needed for fine-tuning and to avoid overshooting. Also, different parameters might require different learning rates depending on the geometry of the loss landscape concerning that specific parameter.

These challenges motivate the development of methods that **adjust the learning rate automatically** during training. These methods fall broadly into two categories: Momentum-based methods and Adaptive Gradient methods, along with Learning Rate Schedules.

### 4. Momentum-Based Methods: Adding Inertia

These methods introduce the concept of "momentum," analogous to a ball rolling down a hill. Instead of just considering the current gradient, they incorporate information from past updates.

#### 4.1. Momentum

**Intuition:** Imagine a ball rolling down the loss landscape. If it keeps rolling in the same direction (consistent gradients), it picks up speed (momentum). If the gradient changes direction frequently, the momentum helps smooth out the oscillations. This helps accelerate convergence, especially along dimensions where the gradient is consistent, and dampens oscillations in dimensions where the gradient changes sign often (e.g., in narrow ravines).

**Mechanism:** It introduces a "velocity" vector `v` which is an exponentially decaying moving average of past gradients.

**Update Rules:**

1.  Calculate the velocity update:
    ```
    v_t = β * v_(t-1) + α * ∇J(θ_t)
    ```
    *(Note: Sometimes the `α` is applied here, sometimes in the parameter update step. Another common formulation is `v_t = β * v_(t-1) + (1-β) * ∇J(θ_t)`, followed by `θ_(t+1) = θ_t - α * v_t`. The version above is more common in deep learning libraries like PyTorch/TensorFlow).*

2.  Update the parameters using the velocity:
    ```
    θ_(t+1) = θ_t - v_t
    ```

*   `β` (beta) is the momentum coefficient, typically set to values like 0.9, 0.99, etc. It controls how much influence past gradients have on the current update. A higher `β` means past gradients have more influence.
*   `v_t` represents the "velocity" or the accumulated momentum at iteration `t`. `v_0` is initialized to 0.

**Benefits:**
*   Faster convergence, especially in high-curvature directions or noisy gradient situations.
*   Helps escape shallow local minima and navigate plateaus.
*   Reduces oscillations.

**Drawbacks:**
*   Introduces another hyperparameter (`β`).
*   Can still overshoot if momentum becomes too high near the minimum.

#### 4.2. Nesterov Accelerated Gradient (NAG)

**Intuition:** NAG is a refinement of standard Momentum. Standard Momentum calculates the gradient at the *current* position `θ_t` and then takes a big jump in the direction of the accumulated gradient `v_t`. NAG is slightly smarter: it first makes a "lookahead" jump based on the current momentum (`θ_t - β * v_(t-1)`), calculates the gradient at this *future approximate position*, and then makes the final corrected update. It's like correcting your course *before* making the full step, based on where your momentum is taking you.

**Mechanism:**

1.  Calculate the approximate future position (lookahead):
    ```
    θ_lookahead = θ_t - β * v_(t-1)
    ```
2.  Calculate the gradient at the lookahead position:
    ```
    ∇J(θ_lookahead)
    ```
3.  Update the velocity using the lookahead gradient:
    ```
    v_t = β * v_(t-1) + α * ∇J(θ_lookahead)
    ```
4.  Update the parameters:
    ```
    θ_(t+1) = θ_t - v_t
    ```

*(Note: Implementations often use a slightly rearranged but mathematically equivalent formulation for efficiency).*

**Benefits:**
*   Often provides slightly faster convergence than standard Momentum.
*   Acts as a better correction mechanism, preventing overshooting more effectively.

**Drawbacks:**
*   Slightly more complex conceptually.
*   Still requires tuning `α` and `β`.

### 5. Adaptive Gradient Methods: Per-Parameter Learning Rates

These methods go a step further by adapting the learning rate *individually for each parameter*. The core idea is to give smaller updates (lower effective learning rate) to parameters associated with frequently occurring features (large or consistent gradients) and larger updates (higher effective learning rate) to parameters associated with infrequent features (small or sparse gradients). This is achieved by scaling the learning rate based on the history of gradients for each parameter.

#### 5.1. AdaGrad (Adaptive Gradient Algorithm)

**Intuition:** Scale down the learning rate for parameters that have seen large gradients in the past.

**Mechanism:** It accumulates the *sum of squares* of past gradients for each parameter. This accumulated sum is then used to normalize the learning rate for that specific parameter in the update rule.

**Update Rules:**

Let `g_t = ∇J(θ_t)` be the gradient at iteration `t`.
Let `g_{t, i}` be the gradient component for parameter `θ_i`.

1.  Accumulate squared gradients component-wise:
    ```
    G_t = G_(t-1) + g_t ⊙ g_t
    ```
    Where `G_t` is a vector storing the sum of squares of past gradients up to iteration `t` for each parameter, `G_0` is initialized to 0, and `⊙` denotes element-wise multiplication.

2.  Update parameters element-wise:
    ```
    θ_(t+1), i = θ_{t, i} - (α / (sqrt(G_{t, i}) + ε)) * g_{t, i}
    ```

*   `α` is the global learning rate.
*   `G_{t, i}` is the accumulated sum of squares of gradients for parameter `θ_i` up to time `t`.
*   `ε` (epsilon) is a small smoothing constant (e.g., 1e-8) to prevent division by zero.

**Benefits:**
*   Eliminates the need to manually tune the learning rate as much (though the global `α` still matters).
*   Works well for problems with sparse gradients (e.g., Natural Language Processing, where some words/features occur infrequently).

**Drawbacks:**
*   The main issue: The denominator `sqrt(G_{t, i})` grows monotonically throughout training because squared gradients are always positive. This causes the effective learning rate to shrink continuously and eventually become infinitesimally small, potentially stopping learning prematurely before reaching the minimum.

#### 5.2. RMSProp (Root Mean Square Propagation)

**Intuition:** Address AdaGrad's aggressively decaying learning rate by using an *exponentially decaying average* of squared gradients instead of accumulating all past squared gradients. This means recent gradients have more influence on the scaling factor than very old gradients.

**Mechanism:** It maintains a moving average of the squared gradients.

**Update Rules:**

1.  Calculate the decaying average of squared gradients component-wise:
    ```
    E[g^2]_t = β₂ * E[g^2]_(t-1) + (1 - β₂) * (g_t ⊙ g_t)
    ```
    Where `E[g^2]_t` is the moving average of squared gradients at time `t`. `β₂` is the decay rate (a hyperparameter, often around 0.999). `E[g^2]_0` is initialized to 0.

2.  Update parameters element-wise:
    ```
    θ_(t+1), i = θ_{t, i} - (α / (sqrt(E[g^2]_{t, i}) + ε)) * g_{t, i}
    ```

*   `β₂` controls the decay rate of the squared gradient average.
*   `ε` prevents division by zero.

**Benefits:**
*   Resolves AdaGrad's vanishing learning rate problem by using a moving average.
*   Generally performs well in practice and is a popular choice.

**Drawbacks:**
*   Introduces another hyperparameter (`β₂`).

#### 5.3. AdaDelta

AdaDelta is another method developed around the same time as RMSProp to address AdaGrad's shortcomings. It's conceptually similar to RMSProp but takes it a step further by also maintaining a decaying average of the *squared parameter updates*. A key characteristic is that it theoretically doesn't require a base learning rate `α`, although implementations often still include one. It's less commonly used now compared to RMSProp and Adam.

#### 5.4. Adam (Adaptive Moment Estimation)

**Intuition:** Combine the best of both worlds: the momentum concept (using a moving average of the gradient itself, like Momentum/NAG) and the adaptive per-parameter learning rates based on squared gradients (like RMSProp).

**Mechanism:** Adam computes adaptive learning rates for each parameter by storing *two* exponentially decaying moving averages:
1.  `m_t`: Estimate of the first moment (the mean) of the gradients (like momentum).
2.  `v_t`: Estimate of the second moment (the uncentered variance) of the gradients (like RMSProp).

**Update Rules:**

1.  Calculate gradient: `g_t = ∇J(θ_t)`
2.  Update biased first moment estimate:
    ```
    m_t = β₁ * m_(t-1) + (1 - β₁) * g_t
    ```
3.  Update biased second raw moment estimate:
    ```
    v_t = β₂ * v_(t-1) + (1 - β₂) * (g_t ⊙ g_t)
    ```
4.  Compute bias-corrected first moment estimate:
    ```
    m̂_t = m_t / (1 - β₁^t)
    ```
5.  Compute bias-corrected second raw moment estimate:
    ```
    v̂_t = v_t / (1 - β₂^t)
    ```
6.  Update parameters:
    ```
    θ_(t+1) = θ_t - (α / (sqrt(v̂_t) + ε)) * m̂_t
    ```

*   `α`: The learning rate (step size).
*   `β₁`: Exponential decay rate for the first moment estimates (e.g., 0.9).
*   `β₂`: Exponential decay rate for the second moment estimates (e.g., 0.999).
*   `ε`: Small constant for numerical stability (e.g., 1e-8).
*   `t`: Timestep (iteration number).

The bias correction steps (4 and 5) are crucial, especially during the initial iterations. Since `m_0` and `v_0` are initialized to 0, the estimates `m_t` and `v_t` are biased towards zero early in training. The correction terms counteract this bias.

**Benefits:**
*   Combines the advantages of Momentum and RMSProp.
*   Computationally efficient and requires little memory.
*   Generally works well across a wide range of problems with default hyperparameter values (`β₁=0.9`, `β₂=0.999`, `ε=1e-8`).
*   Often considered the default or go-to optimizer for deep learning.

**Drawbacks:**
*   Has several hyperparameters to tune (`α`, `β₁`, `β₂`, `ε`), although defaults often work well.
*   In some specific cases (rarely), it might converge to suboptimal solutions compared to SGD with carefully tuned momentum and learning rate schedules.

### 6. Learning Rate Schedules: Pre-Planned Adjustments

Instead of adapting the learning rate based on gradient statistics, Learning Rate Schedules modify the *global* learning rate `α` according to a predetermined plan, usually based on the training epoch or iteration number. They are often used *in combination* with optimizers like SGD+Momentum or even Adam.

**Intuition:** Start with a relatively large learning rate to make quick progress initially, then gradually decrease it as training progresses to allow for fine-tuning and stable convergence near the minimum.

**Common Schedules:**

1.  **Step Decay:** Reduce the learning rate by a certain factor (e.g., divide by 10) every fixed number of epochs (e.g., every 30 epochs).
    *   *Example:* Start with `α=0.1`. After 30 epochs, `α=0.01`. After 60 epochs, `α=0.001`.
2.  **Exponential Decay:** Multiply the learning rate by a decay factor less than 1 after each epoch or iteration.
    *   *Formula:* `α_t = α_0 * decay_rate^(t / decay_steps)`
3.  **Time-Based Decay:** Decrease the learning rate based on the iteration number.
    *   *Formula:* `α_t = α_0 / (1 + decay_rate * t)`
4.  **Cosine Annealing:** Smoothly decrease the learning rate following a cosine curve from the initial value down to a minimum value (often 0) over a set number of epochs, then potentially restart.
    *   *Benefit:* Spends more time at lower learning rates near the end. Often used with "warm restarts" (SGDR - Stochastic Gradient Descent with Warm Restarts), where the learning rate is reset to its initial value periodically, helping escape local optima.
5.  **Learning Rate Warmup:** Start with a very small learning rate, linearly increase it to the target initial learning rate over the first few epochs, and *then* apply a decay schedule (like step, exponential, or cosine).
    *   *Benefit:* Helps stabilize training in the very beginning, especially for large models or batches, preventing early divergence caused by potentially large initial gradients.

**Benefits:**
*   Provides a structured way to decrease the learning rate, aiding convergence.
*   Can lead to better final model performance when tuned correctly.
*   Relatively simple to implement.

**Drawbacks:**
*   Requires careful tuning of the schedule parameters (initial rate, decay factor, decay steps, etc.).
*   The schedule is predefined and doesn't react to the actual training dynamics (loss plateaus, etc.) unless combined with other techniques like "ReduceLROnPlateau".

### 7. Choosing the Right Optimizer and Strategy

There is no single "best" optimizer for all tasks. The choice often depends on:

*   **Dataset:** Size, sparsity, noise level.
*   **Model Architecture:** Complexity, sensitivity to initialization.
*   **Computational Resources:** Memory and time constraints.
*   **Hyperparameter Sensitivity:** Some optimizers require more tuning than others.

**General Recommendations & Practice:**

1.  **Start with Adam:** Adam with its default parameters is often a great starting point and works well for a wide variety of deep learning tasks.
2.  **Consider SGD with Momentum + Schedule:** For some tasks, particularly in computer vision, well-tuned SGD with Momentum combined with a carefully designed learning rate schedule (like step decay or cosine annealing with warmup) can achieve state-of-the-art results, sometimes slightly outperforming Adam in terms of final generalization. However, this often requires more careful hyperparameter tuning.
3.  **RMSProp:** A solid alternative to Adam, especially if memory is slightly more constrained or if Adam seems to struggle on a particular task.
4.  **Experiment:** Always treat the choice of optimizer and its hyperparameters (especially the initial learning rate `α`) as something to experiment with.
5.  **Combine Methods:** Using an adaptive optimizer like Adam *together* with a learning rate schedule (e.g., starting Adam with `α=0.001` and applying cosine annealing) is a common and effective practice.
6.  **Monitor Training:** Always monitor the loss curve during training. If the loss diverges, the learning rate is likely too high. If it decreases extremely slowly or plateaus too early, the learning rate might be too low, or you might need a more sophisticated optimizer or schedule.

### 8. Conclusion

Moving beyond a fixed learning rate is crucial for efficient and effective training of modern machine learning models. Gradient descent, while fundamental, struggles with the challenges of complex loss landscapes. Momentum-based methods like Momentum and NAG introduce inertia to accelerate convergence and smooth oscillations. Adaptive gradient methods like AdaGrad, RMSProp, and Adam dynamically adjust the learning rate for each parameter based on past gradient information, often leading to faster convergence with less manual tuning. Adam, combining momentum and adaptive rates, has become a standard choice. Learning rate schedules provide a complementary approach by systematically decreasing the global learning rate over time. Understanding these different techniques and knowing when and how to apply them – often in combination – is a key skill for any machine learning practitioner aiming to effectively navigate the optimization landscape and find the best possible models. The journey from vanilla Gradient Descent to sophisticated adaptive methods highlights the continuous effort to make the training process faster, more robust, and ultimately more successful.
