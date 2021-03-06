<!-- ===================== Bắt đầu dịch Phần 1 ===================== -->
<!-- ========================================= REVISE PHẦN 1 - BẮT ĐẦU =================================== -->

<!--
# Dropout
-->

# *dịch tiêu đề phía trên*
:label:`sec_dropout`

<!--
Just now, in :numref:`sec_weight_decay`, we introduced the classical approach to regularizing statistical models by penalizing the $\ell_2$ norm of the weights.
In probabilistic terms, we could justify this technique by arguing that we have assumed a prior belief that weights take values from a Gaussian distribution with mean $0$.
More intuitively, we might argue that we encouraged the model to spread out its weights among many features and rather than depending too much on a small number of potentially spurious associations.
-->

*dịch đoạn phía trên*

<!--
## Overfitting Revisited
-->

## *dịch tiêu đề phía trên*

<!--
Faced with more features than examples, linear models tend to overfit.
But given more examples than features, we can generally count on linear models not to overfit.
Unfortunately, the reliability with which linear models generalize comes at a cost:
Naively applied, linear models do not take into account interactions among features.
For every feature, a linear model must assign either a positive or a negative weight, ignoring context.
-->

*dịch đoạn phía trên*

<!--
In traditional texts, this fundamental tension between generalizability and flexibility is described as the *bias-variance tradeoff*.
Linear models have high bias (they can only represent a small class of functions), but low variance (they give similar results across different random samples of the data).
-->

*dịch đoạn phía trên*

<!--
Deep neural networks inhabit the opposite end of the bias-variance spectrum.
Unlike linear models, neural networks, are not confined to looking at each feature individually.
They can learn interactions among groups of features.
For example, they might infer that “Nigeria” and “Western Union” appearing together in an email indicates spam but that separately they do not.
-->

*dịch đoạn phía trên*

<!--
Even when we have far more examples than features, deep neural networks are capable of overfitting.
In 2017, a group of researchers demonstrated the extreme flexibility of neural networks by training deep nets on randomly-labeled images.
Despite the absence of any true pattern linking the inputs to the outputs, they found that the neural network optimized by SGD, could label every image in the training set perfectly.
-->

*dịch đoạn phía trên*

<!--
Consider what this means.
If the labels are assigned uniformly at random and there are 10 classes, then no classifier can do better than 10% accuracy on holdout data.
The generalization gap here is a whopping 90%.
If our models so expressive that they can overfit this badly, then when should we expect them not to overfit?
The mathemtatical foundations for the puzzling generalization properties of deep networks remain open research questions, and we encourage the theoretically-oriented reader to dig deeperinto the topic.
For now, we turn to the more terrestrial investigation of practical tools that tend (empirically) to improve the generalization of deep nets.
-->

*dịch đoạn phía trên*

<!-- ===================== Kết thúc dịch Phần 1 ===================== -->

<!-- ===================== Bắt đầu dịch Phần 2 ===================== -->

<!-- ========================================= REVISE PHẦN 1 - KẾT THÚC ===================================-->

<!-- ========================================= REVISE PHẦN 2 - BẮT ĐẦU ===================================-->

<!--
## Robustness through Perturbations
-->

## *dịch tiêu đề phía trên*

<!--
Let's think briefly about what we expect from a good predictive model.
We want it to peform well on unseen data.
Classical generalization theory suggests that to close the gap between train and test performance, we should aim for a *simple* model.
Simplicity can come in the form of a small number of dimensions, as we explored when discussing linear models monomial basis functions :numref:`sec_model_selection`.
As we saw when discussing weight decay ($\ell_2$ regularization) :numref:`sec_weight_decay`, the (inverse) norm of the parameters represents another useful measure of simplicity.
Another useful notion of simplicity is smoothness, i.e., that the function should not be sensitive
to small changed to its inputs.
For instance, when we classify images, we would expect that adding some random noise to the pixels should be mostly harmless.
-->

*dịch đoạn phía trên*

<!--
In 1995, Christopher Bishop formalized this idea when he proved that training with input noise is equivalent to Tikhonov regularization :cite:`Bishop.1995`.
This work drew a clear mathematical connection between the requirement that a function be smooth (and thus simple), and the requirement that it be resilient to perturbations in the input.
-->

*dịch đoạn phía trên*

<!--
Then, in 2014, Srivastava et al. :cite:`Srivastava.Hinton.Krizhevsky.ea.2014` developed a clever idea for how to apply Bishop's idea to the *internal* layers of the network, too.
Namely, they proposed to inject noise into each layer of the network before calculating the subsequent layer during training.
They realized that when training a deep network with many layers, enforcing smoothness just on the input-output mapping.
-->

*dịch đoạn phía trên*

<!--
Their idea, called *dropout*, involves injecting noise while computing each internal layer during forward propagation, and it has become a standard technique for training neural networks.
The method is called *dropout* because we literally *drop out* some neurons during training.
Throughout training, on each iteration, standard dropout consists of zeroing out some fraction (typically 50%) of the nodes in each layer before calculating the subsequent layer.
-->

*dịch đoạn phía trên*

<!--
To be clear, we are imposing our own narrative with the link to Bishop.
The original paper on dropout offers intuition through a surprising analogy to sexual reproduction.
The authors argue that neural network overfitting is characterized by a state in which each layer an relies on a specifc pattern of activations in the previous layer, calling this condition *co-adaptation*.
Dropout, they claim, breaks up co-adaptation just as sexual reproduction is argued to break up co-adapted genes.
-->

*dịch đoạn phía trên*

<!--
The key challenge then is *how* to inject this noise.
One idea is too inject the noise in an *unbiased* manner so that the expected value of each layer---fixing the others equal to the value it would have taken absent noise.
-->

*dịch đoạn phía trên*

<!-- ===================== Kết thúc dịch Phần 2 ===================== -->

<!-- ===================== Bắt đầu dịch Phần 3 ===================== -->

<!--
In Bishop's work, he added Gaussian noise to the inputs to a linear model:
At each training iteration, he added noise sampled from a distribution with mean zero $\epsilon \sim \mathcal{N}(0,\sigma^2)$ to the input $\mathbf{x}$, 
yielding a perturbed point $\mathbf{x}' = \mathbf{x} + \epsilon$.
In expectation, $E[\mathbf{x}'] = \mathbf{x}$.
-->

*dịch đoạn phía trên*

<!--
In standard dropout regularization, one debiases each layer by normalizing by the fraction of nodes that were retained (not dropped out).
In other words, dropout with *dropout probability* $p$ is applied as follows:
-->

*dịch đoạn phía trên*

$$
\begin{aligned}
h' =
\begin{cases}
    0 & \text{ with probability } p \\
    \frac{h}{1-p} & \text{ otherwise}
\end{cases}
\end{aligned}
$$

<!--
By design, the expectation remains unchanged, i.e., $E[h'] = h$.
Intermediate activations $h$ are replaced by a random variable $h'$ with matching expectation.
-->

*dịch đoạn phía trên*

<!-- ========================================= REVISE PHẦN 2 - KẾT THÚC ===================================-->

<!-- ========================================= REVISE PHẦN 3 - BẮT ĐẦU ===================================-->

<!--
## Dropout in Practice
-->

## *dịch tiêu đề phía trên*

<!--
Recall the multilayer perceptron (:numref:`sec_mlp`) with a hidden layer and 5 hidden units.
Its architecture is given by
-->

*dịch đoạn phía trên*

$$
\begin{aligned}
    h & = \sigma(W_1 x + b_1), \\
    o & = W_2 h + b_2, \\
    \hat{y} & = \mathrm{softmax}(o).
\end{aligned}
$$

<!--
When we apply dropout to a hidden layer, zeroing out each hidden unit with probability $p$, the result can be viewed as a network containing only a subset of the original neurons.
In :numref:`fig_dropout2`, $h_2$ and $h_5$ are removed.
Consequently, the calculation of $y$ no longer depends on $h_2$ and $h_5$ and their respective gradient also vanishes when performing backprop.
In this way, the calculation of the output layer cannot be overly dependent on any one element of $h_1, \ldots, h_5$.
-->

*dịch đoạn phía trên*

<!--
![MLP before and after dropout](../img/dropout2.svg)
-->

![*dịch chú thích ảnh phía trên*](../img/dropout2.svg)
:label:`fig_dropout2`

<!--
Typically, ***we disable dropout at test time***.
Given a trained model and a new example, we do not drop out any nodes (and thus do not need to normalize).
However, there are some exceptions: some researchers use dropout at test time as a heuristic for estimating the *uncertainty* of neural network predictions: 
if the predictions agree across many different dropout masks, then we might say that the network is more confident.
For now we will put off uncertainty estimation for subsequent chapters and volumes.
-->

*dịch đoạn phía trên*

<!-- ===================== Kết thúc dịch Phần 3 ===================== -->

<!-- ===================== Bắt đầu dịch Phần 4 ===================== -->

<!--
## Implementation from Scratch
-->

## *dịch tiêu đề phía trên*

<!--
To implement the dropout function for a single layer, we must draw as many samples from a Bernoulli (binary) random variable as our layer has dimensions, 
where the random variable takes value $1$ (keep) with probability $1-p$ and $0$ (drop) with probability $p$.
One easy way to implement this is to first draw samples from the uniform distribution $U[0, 1]$, then we can keep those nodes for which the corresponding sample is greater than $p$, dropping the rest.
-->

*dịch đoạn phía trên*

<!--
In the following code, we implement a `dropout_layer` function that drops out the elements in the `ndarray` input `X` with probability `dropout`, 
rescaling the remainder as described above (dividing the survivors by `1.0-dropout`).
-->

*dịch đoạn phía trên*

```{.python .input  n=1}
import d2l
from mxnet import autograd, gluon, init, np, npx
from mxnet.gluon import nn
npx.set_np()

def dropout_layer(X, dropout):
    assert 0 <= dropout <= 1
    # In this case, all elements are dropped out
    if dropout == 1:
        return np.zeros_like(X)
    mask = np.random.uniform(0, 1, X.shape) > dropout
    return mask.astype(np.float32) * X / (1.0-dropout)
```

<!--
We can test out the `dropout_layer` function on a few examples.
In the following lines of code, we pass our input `X` through the dropout operation, with probabilities 0, 0.5, and 1, respectively.
-->

*dịch đoạn phía trên*

```{.python .input  n=2}
X = np.arange(16).reshape(2, 8)
print(dropout_layer(X, 0))
print(dropout_layer(X, 0.5))
print(dropout_layer(X, 1))
```

<!-- ========================================= REVISE PHẦN 3 - KẾT THÚC ===================================-->

<!-- ========================================= REVISE PHẦN 4 - BẮT ĐẦU ===================================-->

<!--
### Defining Model Parameters
-->

### *dịch tiêu đề phía trên*

<!--
Again, we work with the Fashion-MNIST dataset introduced in :numref:`sec_softmax_scratch`.
We define a multilayer perceptron with two hidden layers containing 256 outputs each.
-->

*dịch đoạn phía trên*

```{.python .input  n=3}
num_inputs, num_outputs, num_hiddens1, num_hiddens2 = 784, 10, 256, 256

W1 = np.random.normal(scale=0.01, size=(num_inputs, num_hiddens1))
b1 = np.zeros(num_hiddens1)
W2 = np.random.normal(scale=0.01, size=(num_hiddens1, num_hiddens2))
b2 = np.zeros(num_hiddens2)
W3 = np.random.normal(scale=0.01, size=(num_hiddens2, num_outputs))
b3 = np.zeros(num_outputs)

params = [W1, b1, W2, b2, W3, b3]
for param in params:
    param.attach_grad()
```

<!-- ===================== Kết thúc dịch Phần 4 ===================== -->

<!-- ===================== Bắt đầu dịch Phần 5 ===================== -->

<!--
### Defining the Model
-->

### Định nghĩa Mô hình

<!--
The model below applies dropout to the output of each hidden layer (following the activation function).
We can set dropout probabilities for each layer separately. A common trend is to set a lower dropout probability closer to the input layer.
Below we set it to 0.2 and 0.5 for the first and second hidden layer respectively.
By using the `is_training` function described in :numref:`sec_autograd`, we can ensure that dropout is only active during training.
-->

Mô hình bên dưới áp dụng dropout cho đầu ra của mỗi tầng ẩn (theo sau hàm kích hoạt).
Ta có thể đặt các giá trị xác suất dropout riêng biệt cho mỗi tầng. Một xu hướng chung là đặt xác suất dropout thấp hơn cho tầng ở gần với tầng đầu vào hơn.
Bên dưới ta đặt xác suất dropout bằng 0.2 và 0.5 tương ứng cho tầng ẩn thứ nhất và thứ hai.
Bằng cách sử dụng hàm `is_training` mô tả ở :numref:`sec_autograd`, ta có thể chắc chắn rằng dropout chỉ được kích hoạt trong quá trình huấn luyện.

```{.python .input  n=4}
dropout1, dropout2 = 0.2, 0.5

def net(X):
    X = X.reshape(-1, num_inputs)
    H1 = npx.relu(np.dot(X, W1) + b1)
    # Use dropout only when training the model
    if autograd.is_training():
        # Add a dropout layer after the first fully connected layer
        H1 = dropout_layer(H1, dropout1)
    H2 = npx.relu(np.dot(H1, W2) + b2)
    if autograd.is_training():
        # Add a dropout layer after the second fully connected layer
        H2 = dropout_layer(H2, dropout2)
    return np.dot(H2, W3) + b3
```

<!--
### Training and Testing
-->

### Huấn luyện và Kiểm tra

<!--
This is similar to the training and testing of multilayer perceptrons described previously.
-->

Việc này tương tự với quá trình huấn luyện và kiểm tra của các perceptron đa tầng trước đây.

```{.python .input  n=5}
num_epochs, lr, batch_size = 10, 0.5, 256
loss = gluon.loss.SoftmaxCrossEntropyLoss()
train_iter, test_iter = d2l.load_data_fashion_mnist(batch_size)
d2l.train_ch3(net, train_iter, test_iter, loss, num_epochs,
              lambda batch_size: d2l.sgd(params, lr, batch_size))
```

<!-- ========================================= REVISE PHẦN 4 - KẾT THÚC ===================================-->

<!-- ========================================= REVISE PHẦN 5 - BẮT ĐẦU ===================================-->

<!--
## Concise Implementation
-->

## Triển khai súc tích

<!--
Using Gluon, all we need to do is add a `Dropout` layer (also in the `nn` package) after each fully-connected layer, passing in the dropout probability as the only argument to its constructor.
During training, the `Dropout` layer will randomly drop out outputs of the previous layer (or equivalently, the inputs to the subsequent layer) according to the specified dropout probability.
When MXNet is not in training mode, the `Dropout` layer simply passes the data through during testing.
-->

Bằng việc sử dụng Gluon, tất cả những gì ta cần làm là thêm một tầng `Dropout` (cũng nằm trong gói `nn`) vào sau mỗi tầng kết nối đầy đủ và truyền vào xác suất dropout, đối số duy nhất của hàm khởi tạo.
Trong quá trình huấn luyện, hàm `Dropout` sẽ bỏ ngẫu nhiên một số đầu ra của tầng trước (hay tương đương với đầu vào của tầng tiếp theo) dựa trên xác suất dropout được định nghĩa trước đó.

```{.python .input  n=6}
net = nn.Sequential()
net.add(nn.Dense(256, activation="relu"),
        # Add a dropout layer after the first fully connected layer
        nn.Dropout(dropout1),
        nn.Dense(256, activation="relu"),
        # Add a dropout layer after the second fully connected layer
        nn.Dropout(dropout2),
        nn.Dense(10))
net.initialize(init.Normal(sigma=0.01))
```

<!--
Next, we train and test the model.
-->

Tiếp theo, ta huấn luyện và kiểm tra mô hình.

```{.python .input  n=7}
trainer = gluon.Trainer(net.collect_params(), 'sgd', {'learning_rate': lr})
d2l.train_ch3(net, train_iter, test_iter, loss, num_epochs, trainer)
```

<!-- ===================== Kết thúc dịch Phần 5 ===================== -->

<!-- ===================== Bắt đầu dịch Phần 6 ===================== -->

<!--
## Summary
-->

## Tóm tắt

<!--
* Beyond controlling the number of dimensions and the size of the weight vector, dropout is yet another tool to avoid overfitting. Often all three are used jointly.
* Dropout replaces an activation $h$ with a random variable $h'$ with expected value $h$ and with variance given by the dropout probability $p$.
* Dropout is only used during training.
-->

* Ngoài phương pháp kiểm soát số chiều và kích thước của vector trọng số, dropout cũng là một công cụ khác để tránh tình trạng quá khớp. Thông thường thì cả ba cách được sử dụng cùng nhau.
* Dropout thay thế giá trị kích hoạt $h$ bằng một biến ngẫu nhiên $h'$ với giá trị kỳ vọng $h$ và phương sai bằng xác suất dropout $p$.
* Dropout chỉ được sử dụng trong quá trình huấn luyện.


<!--
## Exercises
-->

## Bài tập

<!--
1. What happens if you change the dropout probabilities for layers 1 and 2? In particular, what happens if you switch the ones for both layers? 
Design an experiment to answer these questions, describe your results quantitatively, and summarize the qualitative takeaways.
2. Increase the number of epochs and compare the results obtained when using dropout with those when not using it.
3. What is the variance of the activations in each hidden layer when dropout is and is not applied? Draw a plot to show how this quantity evolves over time for both models.
4. Why is dropout not typically used at test time?
5. Using the model in this section as an example, compare the effects of using dropout and weight decay. 
What happens when dropout and weight decay are used at the same time? Are the results additive, are their diminish returns or (worse), do they cancel each other out?
6. What happens if we apply dropout to the individual weights of the weight matrix rather than the activations?
7. Invent another technique for injecting random noise at each layer that is different from the standard dropout technique. 
Can you develop a method that outperforms dropout on the FashionMNIST dataset (for a fixed architecture)?
-->

1. Điều gì xảy ra nếu bạn thay đổi xác suất dropout của tầng 1 và 2? Cụ thể, điều gì xảy ra nếu bạn tráo đổi xác suất của hai tầng này? Thiết kế một thí nghiệm để trả lời những câu hỏi này, mô tả các kết quả một cách định lượng và tóm tắt các bài học định tính.
2. Tăng số lượng epoch và so sánh các kết quả thu được khi sử dụng và khi không sử dụng dropout.
3. Tính toán phương sai của các giá trị kích hoạt ở mỗi tầng ẩn khi sử dụng và khi không sử dụng dropout. Vẽ biểu đồ thể hiện cách giá trị phương sai này thay đổi theo thời gian cho cả hai mô hình.
4. Tại sao dropout thường không được sử dụng tại bước kiểm tra?
5. Sử dụng mô hình trong phần này làm ví dụ, so sánh hiệu quả của việc sử dụng dropout và phân rã trọng số.
Điều gì xảy ra khi dropout và phân rã trọng số được sử dụng cùng một lúc? Hai phương pháp này bổ trợ cho nhau, làm giảm hiệu quả của nhau hay (tệ hơn) loại trừ lẫn nhau?
6. Điều gì xảy ra nếu chúng ta áp dụng dropout cho các trọng số riêng lẻ của ma trận trọng số thay vì các giá trị kích hoạt?
7. Hãy phát minh một kỹ thuật khác với kỹ thuật dropout tiêu chuẩn để thêm nhiễu ngẫu nhiên ở mỗi tầng.
Bạn có thể phát triển một phương pháp cho kết quả tốt hơn dropout trên bộ dữ liệu FashionMNIST không (với cùng một kiến trúc)?

<!-- ===================== Kết thúc dịch Phần 6 ===================== -->

<!-- ========================================= REVISE PHẦN 5 - KẾT THÚC ===================================-->

<!--
## [Discussions](https://discuss.mxnet.io/t/2343)
-->

## Thảo luận
* [Tiếng Anh](https://discuss.mxnet.io/t/2343)
* [Tiếng Việt](https://forum.machinelearningcoban.com/c/d2l)

## Những người thực hiện
Bản dịch trong trang này được thực hiện bởi:
<!--
Tác giả của mỗi Pull Request điền tên mình và tên những người review mà bạn thấy
hữu ích vào từng phần tương ứng. Mỗi dòng một tên, bắt đầu bằng dấu `*`.

Lưu ý:
* Nếu reviewer không cung cấp tên, bạn có thể dùng tên tài khoản GitHub của họ
với dấu `@` ở đầu. Ví dụ: @aivivn.

* Tên đầy đủ của các reviewer có thể được tìm thấy tại https://github.com/aivivn/d2l-vn/blob/master/docs/contributors_info.md.
-->

* Đoàn Võ Duy Thanh
<!-- Phần 1 -->
*

<!-- Phần 2 -->
*

<!-- Phần 3 -->
*

<!-- Phần 4 -->
*

<!-- Phần 5 -->
* Nguyễn Duy Du
* Phạm Minh Đức

<!-- Phần 6 -->
* Nguyễn Duy Du
* Phạm Minh Đức
