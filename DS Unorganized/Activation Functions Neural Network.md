

## Activation Functions - Injecting Non-Linearity

### 3.1 Introduction: Why Do We Need Activation Functions?

At its core, a neural network learns a mapping from inputs to outputs by passing data through layers of interconnected nodes (neurons). Each connection has a weight, and each neuron typically computes a weighted sum of its inputs plus a bias term. If we were to simply pass this weighted sum directly to the next layer, the entire network, no matter how many layers deep, would behave like a single linear transformation.

**Mathematically:**
Consider two sequential layers without activation functions:
Layer 1 output: `h1 = W1*x + b1`
Layer 2 output: `y = W2*h1 + b2 = W2*(W1*x + b1) + b2 = (W2*W1)*x + (W2*b1 + b2)`
Let `W' = W2*W1` and `b' = W2*b1 + b2`. Then `y = W'*x + b'`.
This shows that stacking linear layers results in another linear layer. Such a network could only learn linear relationships between inputs and outputs, severely limiting its representational power. Most real-world data (images, text, sound) involves highly complex, non-linear patterns.

**Activation functions** are the crucial ingredient that introduces non-linearity into the network. They are applied to the output of each neuron (or sometimes, each layer), transforming the weighted sum before it's passed on. This non-linearity allows neural networks to approximate arbitrarily complex functions, enabling them to learn intricate patterns from data.

**Desirable Properties of Activation Functions:**

*   **Non-linearity:** This is the primary requirement.
*   **Differentiability:** Necessary for gradient-based optimization methods like backpropagation. The gradient indicates how to update weights.
*   **Computational Efficiency:** Activation functions are applied potentially millions of times during training; they should be fast to compute.
*   **Appropriate Output Range:** The range can affect gradient flow and network stability (e.g., avoiding excessively large values).
*   **Avoidance of Saturation (in relevant regions):** Saturation occurs when the function's output becomes flat for large positive or negative inputs. In saturated regions, the gradient is close to zero, hindering learning (the "vanishing gradient" problem).
*   **Zero-Centered Output (sometimes desirable):** Functions whose average output is close to zero can help gradients flow efficiently during training, often leading to faster convergence.

### 3.2 Early Activation Functions

These functions were foundational but are less commonly used in hidden layers today, though Sigmoid and Tanh still have specific applications.

#### 3.2.1 Sigmoid (Logistic) Function

*   **Formula:** `σ(x) = 1 / (1 + exp(-x))`
*   **Graph:** An "S"-shaped curve.
*   **Range:** (0, 1)
*   **Derivative:** `σ'(x) = σ(x) * (1 - σ(x))`
*   **Properties:** Non-linear, differentiable, outputs values between 0 and 1 (interpretable as probabilities).
*   **Pros:**
    *   Smooth gradient.
    *   Output range is bounded (0 to 1), useful for representing probabilities.
*   **Cons:**
    *   **Vanishing Gradients:** Saturates for large positive or negative inputs (derivative approaches 0). This significantly slows down or halts learning in deep networks, especially for neurons in earlier layers.
    *   **Not Zero-Centered:** Outputs are always positive. This can lead to zig-zagging dynamics during gradient descent if all inputs to a layer are positive.
    *   Computationally more expensive than ReLU due to the exponential function.
*   **When to Use:**
    *   **Output Layer for Binary Classification:** Its (0, 1) range is perfect for outputting the probability of the positive class.
    *   Historically used in hidden layers, but largely replaced by ReLU and its variants.
    *   Sometimes used in specific architectures like LSTMs/GRUs (as gating functions).

#### 3.2.2 Tanh (Hyperbolic Tangent) Function

*   **Formula:** `tanh(x) = (exp(x) - exp(-x)) / (exp(x) + exp(-x))` (which is equivalent to `2 * σ(2x) - 1`)
*   **Graph:** Also "S"-shaped, similar to Sigmoid but steeper.
*   **Range:** (-1, 1)
*   **Derivative:** `tanh'(x) = 1 - tanh²(x)`
*   **Properties:** Non-linear, differentiable, zero-centered output range.
*   **Pros:**
    *   **Zero-Centered:** Output range is (-1, 1), which often helps convergence compared to Sigmoid because the average output is closer to zero.
    *   Steeper gradient around zero than Sigmoid (can sometimes lead to faster learning initially).
*   **Cons:**
    *   **Vanishing Gradients:** Still suffers from saturation and vanishing gradients for large inputs, although its gradient is larger than Sigmoid's.
    *   Computationally more expensive than ReLU.
*   **When to Use:**
    *   **Hidden Layers:** Often preferred over Sigmoid for hidden layers due to its zero-centered property, although largely superseded by ReLU variants now.
    *   Can be used in output layers if the target values naturally fall within the (-1, 1) range (e.g., normalized regression tasks, though Linear is often simpler).
    *   Common in recurrent neural networks (RNNs) like LSTMs and GRUs.

### 3.3 The ReLU Revolution and its Variants

The introduction of the Rectified Linear Unit (ReLU) marked a significant improvement in training deep neural networks, primarily by addressing the vanishing gradient problem.

#### 3.3.1 ReLU (Rectified Linear Unit)

*   **Formula:** `ReLU(x) = max(0, x)`
*   **Graph:** A ramp function: zero for negative inputs, linear with a slope of 1 for positive inputs.
*   **Range:** [0, ∞)
*   **Derivative:** `ReLU'(x) = 1` if `x > 0`, `0` if `x < 0` (undefined at `x=0`, but typically assigned 0 or 1 in practice).
*   **Properties:** Non-linear (piecewise linear), computationally very efficient, non-saturating for positive inputs.
*   **Pros:**
    *   **Computational Efficiency:** Extremely fast to compute (just a simple threshold).
    *   **Avoids Vanishing Gradients (for positive inputs):** The gradient is constant (1) for positive inputs, allowing gradients to flow effectively through deep networks. This significantly accelerated training.
    *   **Sparsity:** Can lead to sparse activations (many outputs are zero), which can be computationally and representationally efficient.
*   **Cons:**
    *   **Dying ReLU Problem:** Neurons whose inputs consistently fall below zero will always output zero. Their gradients will also be zero, meaning their weights will never be updated. These neurons effectively "die" and cease to contribute to learning.
    *   **Not Zero-Centered:** Outputs are always non-negative.
    *   The unbounded positive range can potentially lead to exploding activations if not managed (e.g., with proper initialization or batch normalization).
*   **When to Use:**
    *   **The default choice for hidden layers in most feedforward networks (CNNs, MLPs).** Start with ReLU and only switch if you encounter issues like dying neurons.

#### 3.3.2 Leaky ReLU (LReLU)

Designed specifically to address the "Dying ReLU" problem.

*   **Formula:** `LeakyReLU(x) = max(α*x, x)` where `α` is a small positive constant (e.g., 0.01).
*   **Graph:** Similar to ReLU, but has a small, non-zero slope (`α`) for negative inputs instead of being flat zero.
*   **Range:** (-∞, ∞)
*   **Derivative:** `LeakyReLU'(x) = 1` if `x > 0`, `α` if `x < 0`.
*   **Properties:** Non-linear, computationally efficient (slightly more than ReLU), attempts to fix dying ReLU.
*   **Pros:**
    *   **Addresses Dying ReLU:** Allows a small, non-zero gradient when the unit is not active (input < 0), enabling updates even for neurons receiving negative inputs.
    *   Retains the computational efficiency and non-saturation benefits of ReLU for positive inputs.
*   **Cons:**
    *   Results are not always consistently better than ReLU.
    *   Introduces an extra hyperparameter (`α`) to tune, although the default (0.01) often works reasonably well.
    *   Still not zero-centered.
*   **When to Use:**
    *   **Hidden Layers:** A good alternative to ReLU, especially if you observe or suspect dying neurons. Often used in GANs.

#### 3.3.3 Parametric ReLU (PReLU)

Extends Leaky ReLU by making the slope `α` a learnable parameter.

*   **Formula:** `PReLU(x) = max(α*x, x)` where `α` is a *learnable parameter*, initialized typically to a small value (like 0.25). Note: `α` can be learned per-channel or even per-neuron.
*   **Graph:** Similar to Leaky ReLU, but the slope for negative inputs is learned during training.
*   **Range:** (-∞, ∞)
*   **Derivative:** Depends on the learned `α`.
*   **Properties:** Adapts the negative slope based on data.
*   **Pros:**
    *   Potentially better performance than Leaky ReLU as the network can learn the optimal slope.
    *   Addresses the dying ReLU problem.
*   **Cons:**
    *   Increases the number of parameters in the model.
    *   Risk of overfitting if not enough data is available.
    *   Can be slightly more computationally complex during training due to learning `α`.
*   **When to Use:**
    *   **Hidden Layers:** When seeking potentially better performance than ReLU/Leaky ReLU and having sufficient data. Can be particularly useful in image recognition tasks.

#### 3.3.4 ELU (Exponential Linear Unit)

Aims to combine the benefits of ReLU (non-saturation for positives) with properties closer to zero-mean outputs and robustness to noise.

*   **Formula:** `ELU(x) = x` if `x > 0`, `α * (exp(x) - 1)` if `x <= 0`. `α` is a hyperparameter, usually set to 1.0.
*   **Graph:** Like ReLU for positive inputs, but smoothly curves into negative saturation for negative inputs.
*   **Range:** (`-α`, ∞)
*   **Derivative:** `ELU'(x) = 1` if `x > 0`, `α * exp(x)` (or `ELU(x) + α`) if `x <= 0`.
*   **Properties:** Non-linear, produces negative outputs (pushes mean activation closer to zero), smooth transition around zero.
*   **Pros:**
    *   **Closer to Zero-Mean Outputs:** Compared to ReLU/Leaky ReLU, negative outputs can help speed up learning.
    *   **Addresses Dying ReLU:** Provides gradients for negative inputs.
    *   Smoothness around zero might offer some robustness benefits.
    *   Reported to achieve higher classification accuracy than ReLU in some cases.
*   **Cons:**
    *   **Computationally More Expensive:** Involves the `exp()` function for negative inputs, slower than ReLU/Leaky ReLU.
    *   Introduces the hyperparameter `α` (though often fixed at 1).
*   **When to Use:**
    *   **Hidden Layers:** An alternative to ReLU/Leaky ReLU, particularly when faster convergence or slightly better performance is desired and the computational overhead is acceptable. Often works well in CNNs.

#### 3.3.5 SELU (Scaled Exponential Linear Unit)

A special variant of ELU designed to induce self-normalizing properties in deep networks.

*   **Formula:** `SELU(x) = λ * ELU(x)` where `ELU(x)` uses specific values for `α` and `λ` derived mathematically:
    *   `α ≈ 1.6733`
    *   `λ ≈ 1.0507`
    *   So, `SELU(x) = λ * x` if `x > 0`, `λ * α * (exp(x) - 1)` if `x <= 0`.
*   **Graph:** Similar shape to ELU, but scaled.
*   **Range:** (`-λ*α`, ∞)
*   **Properties:** If used correctly (specific weight initialization - Lecun Normal, specific dropout variant - AlphaDropout), activations can converge towards zero mean and unit variance, promoting stable training in deep networks without explicit normalization layers like Batch Normalization.
*   **Pros:**
    *   **Self-Normalizing:** Can eliminate the need for Batch Normalization, potentially simplifying network architecture and improving performance in some scenarios (e.g., smaller batch sizes, recurrent structures).
*   **Cons:**
    *   **Strict Requirements:** Requires specific weight initialization (`lecun_normal`) and potentially AlphaDropout to work effectively. Sensitive to deviations from these conditions.
    *   Not as widely adopted or universally successful as ReLU variants or techniques like Batch Normalization + ReLU.
    *   Computationally more expensive than ReLU.
*   **When to Use:**
    *   **Deep Feedforward Networks:** Specifically when aiming for self-normalization and willing to adhere to the required initialization and architecture constraints. Less common than other ReLU variants.

### 3.4 Output Layer Activation Functions

The choice of activation function for the output layer is primarily determined by the type of problem the network is solving.

#### 3.4.1 Linear (Identity) Function

*   **Formula:** `Linear(x) = x`
*   **Graph:** A straight line with a slope of 1.
*   **Range:** (-∞, ∞)
*   **Properties:** Linear, differentiable, unbounded output.
*   **Pros:**
    *   Allows the network to output values across any range.
    *   Simple and computationally trivial.
*   **Cons:**
    *   Does not introduce non-linearity (which is fine for the *final* output layer in certain tasks).
*   **When to Use:**
    *   **Output Layer for Regression Problems:** When predicting continuous values (e.g., house prices, temperature) that are not constrained to a specific range or probability distribution.

#### 3.4.2 Sigmoid (Revisited)

*   **Formula:** `σ(x) = 1 / (1 + exp(-x))`
*   **Range:** (0, 1)
*   **When to Use:**
    *   **Output Layer for Binary Classification:** Outputs a probability between 0 and 1 for the positive class. Usually paired with a Binary Cross-Entropy loss function.
    *   **Output Layer for Multi-Label Classification:** If each output node represents an independent binary classification task (e.g., tagging an image with multiple possible objects, where each tag is present/absent independently), Sigmoid can be applied element-wise to the output vector.

#### 3.4.3 Softmax Function

*   **Formula:** `Softmax(x_i) = exp(x_i) / Σ_j(exp(x_j))` for `i = 1, ..., N` where `x` is the input vector of size `N`.
*   **Graph:** Not a simple 2D graph; it operates on a vector.
*   **Range:** Each element is (0, 1), and the sum of all elements is 1.
*   **Properties:** Non-linear, differentiable, converts a vector of arbitrary real values into a probability distribution over `N` classes.
*   **Pros:**
    *   Outputs a probability distribution, making it ideal for multi-class classification where classes are mutually exclusive.
    *   The output probabilities sum to 1, providing a clear interpretation.
*   **Cons:**
    *   Only suitable for mutually exclusive classes (an input belongs to exactly one class).
    *   Computationally more complex than element-wise functions.
    *   Can be sensitive to very large or very small input values due to the exponential function (though numerical stability tricks are often used in implementations).
*   **When to Use:**
    *   **Output Layer for Multi-Class Classification:** When predicting the probability distribution over `N` mutually exclusive classes (e.g., classifying an image as either 'cat', 'dog', or 'bird'). Usually paired with a Categorical Cross-Entropy loss function.

### 3.5 Other Notable Activation Functions

While the above cover the most common functions, research continues to propose new ones.

#### 3.5.1 Swish / SiLU (Sigmoid Linear Unit)

*   **Formula:** `Swish(x) = x * σ(βx) = x / (1 + exp(-βx))`. Often `β` is fixed at 1 (SiLU) or made a learnable parameter.
*   **Properties:** Smooth, non-monotonic (it can decrease even when the input increases), computationally more expensive than ReLU but often less than ELU/SELU if `β=1`. Outperforms ReLU on many benchmarks.
*   **When to Use:** As a potential replacement for ReLU in hidden layers, especially in deep networks. Its effectiveness can be task-dependent.

#### 3.5.2 GELU (Gaussian Error Linear Unit)

*   **Formula:** `GELU(x) = x * Φ(x)` where `Φ(x)` is the standard Gaussian cumulative distribution function (CDF). Often approximated as `0.5 * x * (1 + tanh(sqrt(2/π) * (x + 0.044715 * x³)))`.
*   **Properties:** Smooth approximation of ReLU, non-monotonic, currently state-of-the-art in many Natural Language Processing (NLP) models.
*   **When to Use:** Increasingly popular in **Transformer models** (like BERT, GPT) for NLP tasks. Can also be used in computer vision.

#### 3.5.3 Softplus

*   **Formula:** `Softplus(x) = log(1 + exp(x))`
*   **Properties:** Smooth approximation of ReLU (`Softplus'(x) = Sigmoid(x)`). Strictly positive output range (0, ∞).
*   **When to Use:** Less common now. Sometimes used where a smooth version of ReLU is desired or where strictly positive outputs are needed (e.g., modeling the variance of a distribution). Computationally more expensive than ReLU.

### 3.6 Choosing the Right Activation Function: Practical Guidelines

1.  **Output Layer:** The choice is strongly dictated by the task:
    *   **Regression:** Linear (Identity).
    *   **Binary Classification:** Sigmoid.
    *   **Multi-Class Classification (mutually exclusive):** Softmax.
    *   **Multi-Label Classification (non-exclusive):** Sigmoid (applied element-wise).

2.  **Hidden Layers:**
    *   **Start with ReLU:** It's computationally efficient and works well in a wide range of tasks, especially for CNNs and standard MLPs. It's the most common default.
    *   **If experiencing "Dying ReLU":** Switch to Leaky ReLU, PReLU, or ELU. Leaky ReLU is often a simple and effective fix. ELU might offer slightly better performance/convergence at a higher computational cost. PReLU offers learnable slopes but adds parameters.
    *   **Consider Swish/GELU:** If working with very deep networks or specific architectures like Transformers (especially GELU), these newer functions might offer performance benefits, though they come with increased computational cost.
    *   **Avoid Sigmoid and Tanh in deep hidden layers:** Due to the vanishing gradient problem, they are generally poor choices for deep networks compared to ReLU variants. They remain relevant within specific recurrent units (LSTMs, GRUs) for their gating mechanisms.
    *   **SELU:** Only consider if you specifically need self-normalization and are prepared to meet its strict initialization and architectural requirements.

3.  **Experimentation:** There's no single "best" activation function for all hidden layers in all scenarios. Theoretical properties are a guide, but empirical performance on your specific dataset and network architecture is the ultimate judge. Always be prepared to experiment with different activation functions as part of your hyperparameter tuning process.

4.  **Consistency within a Network:** While possible, it's generally uncommon and often unnecessary to mix different activation functions (like ReLU and ELU) randomly within the hidden layers of a standard network. Stick to one type for most hidden layers unless there's a specific architectural reason to differ. The output layer, however, will often have a different activation function suited to the task.

### 3.7 Conclusion

Activation functions are essential components of neural networks, providing the non-linearity required to learn complex patterns. While early functions like Sigmoid and Tanh paved the way, the advent of ReLU and its variants (Leaky ReLU, PReLU, ELU) significantly improved the trainability of deep networks by mitigating the vanishing gradient problem. The choice of activation function, especially for hidden layers, involves trade-offs between computational cost, gradient flow, output range properties, and potential issues like dying neurons. For output layers, the choice is directly linked to the specific prediction task. Understanding the characteristics and appropriate use cases of different activation functions is crucial for designing and training effective neural networks.
