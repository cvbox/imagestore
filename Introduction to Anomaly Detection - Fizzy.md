# Introduction to Anomaly Detection - Fizzy
**Anomaly Detection** (a.k.a **Outlier Detection**) is a process of detecting unexpected observations in specified datasets.

Anomaly detection has two basic assumptions:

-   Anomalies only occur very rarely in the data;
-   Their features differ from the normal instances significantly.  
    (Susan Li, 2019)

Different anomaly detection approaches shall be applied based on the characteristics of dataset and the purpose of the analysis. There are mainly two ways to deal with outliers in statistics and machine learning, namely **unsupervised learning** and **supervised learning**. Also the algorithms often been modified and integrated with other techniques to achieve specific goals in production environment, for example time series analysis.

## Statistical Models

### 3 Sigma Rule

For one dimension outlier detection we can simply use the 3σ3\\sigma-Rule.

Given the dataset X\\={x1,x2,...,xn}X = \\lbrace x_1,x_2,...,x_n \\rbrace, if XX follows normal distribution, then its probability density function is:

p(x,μ,σ2)\\=12πσe−12(x−μσ)2p(x,\\mu,\\sigma^2)=\\frac{1}{\\sqrt{2\\pi}\\sigma}e^{-\\frac{1}{2}(\\frac{x-\\mu}{\\sigma})^2}

from maximal likelihood, we know that:

μ^\\=x‾\\=∑i\\=1nxin\\hat{\\mu}=\\overline{x}=\\frac{\\sum\_{i=1}^{n} x\_{i}}{n}

σ^2\\=∑i\\=1n(xi−x‾)2n\\hat{\\sigma}^2=\\frac{\\sum\_{i=1}^{n} (x\_{i}-\\overline{x})^2}{n}

![](https://i.loli.net/2020/11/21/IaCminQKZyuqpxW.png)

Then we create a μ±3σ\\mu \\pm 3\\sigma window contains 99.73% of the data. If a new point falls outside of the window (i.e. xk∉(μ^−3σ^,μ^+3σ^)x_k \\notin (\\hat{\\mu}-3\\hat{\\sigma},\\hat{\\mu}+3\\hat{\\sigma})), then such new data point can be perceived as an outlier.

### Boxlot

The boxplot can be applied to any form of distribution of the data. The define the boundary of the outlier detection, we first find the Q1 and Q3 then calculate the Interquartile Range (IQR = Q3 - Q1). The 2 outlier boundaries then defines as: Q1−λ∗IQR Q1- \\lambda \* IQR , Q3+λ∗IQR Q3+\\lambda \* IQR . (usually we take λ\\=1.5\\lambda = 1.5 )

![](https://i.loli.net/2020/11/21/V9tmDWJnERkcuIx.png)

> There are many other statistical models could perform the tasks of anomaly detection. I will probably introduce those in future posts.

## Distance Based Models

### Angle-Based Outlier Detection

![](https://i.loli.net/2020/11/21/VTmOXcGJEYFu42f.png)

Angle-Based model is not suitable for large datasets. But it is a basic way to detect outliers by simply comparing the angles between pairs of distance vectors to other points.

### Nearest Neighbour Approaches

For each data point xix_i, compute the distance to the kk-th nerest neighbour dk(xi)d\_{k}(x_i). Then for all data points, we diagnose the outliers are the points that have the larger distance. Therefore, the outliers are located in the more sparse neighbourhoods. However, this methods is not suitable for dataset that have modes with low density.

### Local Outlier Factor (LOF)

The LOF score is equal to ratio of average local density of the kk nearest neighbours of the instance and the local density of the data instance itself.

### Connectivity Outlier Factor (COF)

Outliers are points pp where average chaining distance acac-distkNN(p)(p)dist\_{kNN(p)}(p) is larger than the average chaining distance (acac-distdist) of their kk-nearest neighbourhood kNN(p)kNN(p).

COF identifies outliers as points whose neighourhoods are sparser than the neighbourhoods of their neighbours.

## Linear Models

### Principal Componenet Analysis

I will not try to explain what PCA is in here. The major argument here is that since the variances of the transformed data along the eigenvectors with small eigenvalues are low, significant deviations of the transformed data from the mean values along these directions may represent outliers.

Let RR be a p×pp \\times p sample correlation matrix computed from nn observations on each of pp random variables X1,...,XpX_1,...,X_p. If (λ1,e1),...,(λp,ep)(\\lambda_1,e_1),...,(\\lambda_p,e_p) are the pp eigenvalue-eigenvector pairs of RR, λ1≥...≥λp≥0\\lambda_1 \\geq ... \\geq \\lambda_p \\geq 0, then the ii-th sample principal component of an observation vector x\\=(x1,...,xp)\\pmb{x} = (x_1,...,x_p) is

yi\\=&lt;e_i,z>\\=∑k\\=1peik⋅zkfor i = 1,2,...,py\_{i} = &lt; \\pmb{e}\\\_i,\\pmb{z} > = \\sum\_{k=1}^{p} e\_{ik} \\cdot z_k \\quad \\text{for i = 1,2,...,p}

where ei\\=(ei1,...,eip)Te_i = (e\_{i1},...,e\_{ip})^{T} is the ii-th eigenvector. That means

y\\=(y1,...,yp)T\\=(ei1,...,eip)Tz\\pmb{y} = (y_1,...,y_p)^{T} = (e\_{i1},...,e\_{ip})^{T}\\pmb{z}

is the principal component of vector x\\pmb{x}.

z\\=(z1,...,zp)T\\pmb{z} = (z_1,...,z_p)^{T} is the vector of standardized observations defined as

zk\\=xk−x‾\_kskkk=1,...,pz_k = \\frac{x_k - \\overline{x}\\\_k}{\\sqrt{s\_{kk}}} \\quad \\text{k=1,...,p}

where x‾k \\overline{x}\_{k} and skk s\_{kk} are the sample mean and the sample variance of the variable XkX_k.

Consider the sample principal components y1,...,ypy_1,...,y_p of and observation x\\pmb{x}. The sum of the squares of the standardized principal component values

Score(x)\\=∑i\\=1pyi2λi\\=y12λ1+⋯+yp2λpScore(\\pmb{x}) = \\sum\_{i=1}^{p} \\frac{y\_{i}^2}{\\lambda_i} = \\frac{y\_{1}^2}{\\lambda_1} + \\cdots + \\frac{y\_{p}^2}{\\lambda_p}

is equivalent to the Mahalanobis distance of the observation x\\pmb{x} from the mean of the sample. An observation is outlier if fro a given significance level

∑i\\=1pyi2λi>χq2(α) where 1≤q≤p\\sum\_{i=1}^{p} \\frac{y\_{i}^{2}}{\\lambda\_{i}}>\\chi\_{q}^{2} (\\alpha) \\quad \\text {where} 1 \\leq q \\leq p

## Non-linear Models

### Replicator Neural Networks

The RNN in here is not referring the commonly known Recurrent Neural Networks. The Replicator Neural Networks, or autoencoders, are multi-layer feed-forward neural networks.

The input and output layers have the same number of nodes. Then the network is trained to learn identifying-mapping from inputs to outputs. Given a trained RNN, the reconstruction error is used as the outlier score: test instances incurring a high reconstruction error are considered outliers.

## Summaries

-   **Staistical Methods based OD**
    -   Normal data instances occur in high probability regions of a stochastic model, while anomalies occur in the low probability regions of the stochastic model.
-   **Classification based OD**
    -   A classifier that can distinguish between normal and anomalous classes can be learnt in the given feature space.
-   **Nearest Neighbour based OD**
    -   Normal data instances occur in dense neigbourhoods, while anomalies occur far from their closest neighbours.
-   **Clustering based OD**
    -   Normal data instances belong to a cluster in the data, while anomalies either do not belong to any cluster.
    -   Normal data instances lie close to their closest cluster centroid, while anomalies are far away from their closest cluster centroid.
    -   Normal data instances belong to large and dense clusters, while anomalies either belong to small or sparse clusters.
-   **PCA based OD**
    -   Data can be embedded into a lower dimensional subspace in which normal instances and anomalies appear significantly different.

## References

-   [https://zhuanlan.zhihu.com/p/30169110](https://zhuanlan.zhihu.com/p/30169110)
-   Charu C.Aggarwal (2013). Outlier Analysis. Springer.
-   Jiawei Han (2000). Data Mining: Concepts and Techniques.
-   Susan Li (2019). Anomaly Detection for Dummies. Retrieved from [https://towardsdatascience.com/anomaly-detection-for-dummies-15f148e559c1](https://towardsdatascience.com/anomaly-detection-for-dummies-15f148e559c1).

* * *

## Further Reading Resources

-   [Anomaly Detection for Dummies](https://towardsdatascience.com/anomaly-detection-for-dummies-15f148e559c1)
-   [Anomaly Detection Learning Resources](https://github.com/yzhao062/anomaly-detection-resources)

![](https://fizzy.cc/media/images/custom-authorAvatar.jpeg)

Simon

-   Hangzhou, China

I am a human learner. Yes, you can call me Simon. 
 [https://fizzy.cc/intro-anomaly-detection/](https://fizzy.cc/intro-anomaly-detection/)
