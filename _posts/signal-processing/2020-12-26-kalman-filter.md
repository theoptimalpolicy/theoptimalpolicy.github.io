## Kalman Filter Derivation

Aa
$$
\mathbf{x}_t=\mathbf{F}_{t}\mathbf{x}_{t-1}+\mathbf{B}_t\mathbf{u}_t+\mathbf{w}_t
$$
aa
$$
\mathbf{\hat{x}}_{t|t-1}=\mathbf{F}_{t}\mathbf{\hat{x}}_{t-1|t-1}+\mathbf{B}_t\mathbf{u}_t
$$
aa
$$
P_{t|t-1} = \mathbb{E}[(x_t-\hat{x}_{t|t-1})(x_t-\hat{x}_{t|t-1})^T]
$$
aa
$$
\left(\mathbf{x}_t- \mathbf{\hat{x}}_{t|t-1}\right)=\\
\left(\mathbf{F}_{t}\mathbf{x}_{t-1}+\mathbf{B}_t\mathbf{u}_t+\mathbf{w}_t\right)  - \left(\mathbf{F}_{t}\mathbf{\hat{x}}_{t-1|t-1}+\mathbf{B}_t\mathbf{u}_t\right) =\\
\mathbf{F}_t(\mathbf{x}_{t-1}-\mathbf{\hat{x}}_{t|t-1})+\mathbf{w}_t
$$
aa
$$
P_{t|t-1}  =\\
\mathbb{E}[\left(\mathbf{F}_t(\mathbf{x}_{t-1}-\mathbf{\hat{x}}_{t-1|t-1})+\mathbf{w}_t\right)\left(\mathbf{F}_t(\mathbf{x}_{t-1}-\mathbf{\hat{x}}_{t-1|t-1})+\mathbf{w}_t\right)^T]=\\
\mathbf{F}_t\mathbb{E}[(\mathbf{x}_{t-1}-\mathbf{\hat{x}}_{t-1|t-1})(\mathbf{x}_{t-1}-\mathbf{\hat{x}}_{t-1|t-1})^T]\mathbf{F}_t^T \\
+ \mathbf{F}_t\mathbb{E}[(\mathbf{x}_{t-1}-\mathbf{\hat{x}}_{t-1|t-1})\mathbf{w}_t^T]\\
+ \mathbb{E}[\mathbf{w}_t(\mathbf{x}_{t-1}-\mathbf{\hat{x}}_{t-1|t-1})^T]\mathbf{F}_t^T\\
+\mathbb{E}[\mathbf{w}_t\mathbf{w}_t^T]
$$
Noting that the *state estimation errors* and *process noise* are uncorrelated
$$
\mathbb{E}[(\mathbf{x}_{t-1}-\mathbf{\hat{x}}_{t-1|t-1})\mathbf{w}_t^T]=
\mathbb{E}[\mathbf{w}_t(\mathbf{x}_{t-1}-\mathbf{\hat{x}}_{t-1|t-1})^T]=0\\
$$
So...
$$
P_{t|t-1}  =
\mathbf{F}\mathbb{E}[(\mathbf{x}_{t-1}-\mathbf{\hat{x}}_{t-1|t-1})(\mathbf{x}_{t-1}-\mathbf{\hat{x}}_{t-1|t-1})^T]\mathbf{F}^T 
+\mathbb{E}[\mathbf{w}_t\mathbf{w}_t^T]
$$
Which trivially means:
$$
P_{t|t-1}=\mathbf{F}P_{t-1|t-1}\mathbf{F}^T 
+\mathbf{Q}_t\\
$$
So now we know
$$
\mathbf{\hat{x}}_{t|t-1}=\mathbf{F}_{t}\mathbf{\hat{x}}_{t-1|t-1}+\mathbf{B}_t\mathbf{u}_t\\
P_{t|t-1}=\mathbf{F}_tP_{t-1|t-1}\mathbf{F}_t^T 
+\mathbf{Q}_t\\
$$
But ,since at time $t$ we receive some kind of measurement, what if we'd be interested in knowing $\mathbf{\hat{x}}_{t|t}$ and $P_{t|t}$ ?

First an illustration af an example problem:

![](images/KF1.PNG)

![](images/KF2.png)

![](images/KF3.png)

![](images/KF4.png)

![](images/KF5.png)

The prediction pdf represented by the red Gaussian function in Figure 3 is given by the equation
$$
y_1(r;\mu_1,\sigma_1)= \frac{1}{\sqrt{2\pi\sigma_1^2}}e^{-\frac{(r-\mu_1)^2}{2\sigma_1^2}}
$$
The measurement pdf represented by the blue Gaussian function in Figure 4 is given by
$$
y_2(r;\mu_2,\sigma_2)= \frac{1}{\sqrt{2\pi\sigma_2^2}}e^{-\frac{(r-\mu_2)^2}{2\sigma_2^2}}
$$
The information provided by these two pdfs is fused by multiplying the two together, i.e., considering the prediction and the measurement together (see Figure 5). The new *pdf* representing the fusion of the information from the prediction and measurement, and our best current estimate of the system, is therefore given by the product of these two Gaussian functions
$$
y_{\textit{fused}}(r;\mu_1,\sigma_1,\mu_2,\sigma_2) =
\frac{1}{\sqrt{2\pi\sigma_1^2}}e^{-\frac{(r-\mu_1)^2}{2\sigma_1^2}}\cdot\frac{1}{\sqrt{2\pi\sigma_2^2}}e^{-\frac{(r-\mu_2)^2}{2\sigma_2^2}}=\\
\frac{1}{2\pi\sigma_1\sigma_2}e^{-\beta }
$$
We see that
$$
\beta =\frac{(r-\mu_1)^2}{2\sigma_1^2}+\frac{(r-\mu_2)^2}{2\sigma_2^2} = \\
\frac{
(2\sigma_2^2r^2 +
2\sigma_2^2\mu_1^2 -
4\sigma_2^2\mu_1r) +
(2\sigma_1^2r^2 +
2\sigma_1^2\mu_2^2 -
4\sigma_1^2\mu_2r)
}{4\sigma_1^2\sigma_2^2} = \\

\frac{
(\sigma_2^2r^2 +
\sigma_2^2\mu_1^2 -
2\sigma_2^2\mu_1r) +
(\sigma_1^2r^2 +
\sigma_1^2\mu_2^2 -
2\sigma_1^2\mu_2r)
}{2\sigma_1^2\sigma_2^2} = \\

\frac{(\sigma_2^2+\sigma_1^2)r^2 -
2(\sigma_2^2\mu_1+\sigma_1^2\mu_2)r +
(\sigma_2^2\mu_1^2+\sigma_1^2\mu_2^2)}
{2\sigma_1^2\sigma_2^2} = \\

\frac{r^2 -
2\frac{(\sigma_2^2\mu_1+\sigma_1^2\mu_2)}{(\sigma_2^2+\sigma_1^2)}r +
\frac{(\sigma_2^2\mu_1^2+\sigma_1^2\mu_2^2)}{(\sigma_2^2+\sigma_1^2)}
}
{\frac{2\sigma_1^2\sigma_2^2}{(\sigma_2^2+\sigma_1^2)}}
$$
Daaamn, that's not a perfect square, so...
$$
\beta =\frac{(r-\mu_1)^2}{2\sigma_1^2}+\frac{(r-\mu_2)^2}{2\sigma_2^2} = \\

\frac{r^2 -
2\frac{(\sigma_2^2\mu_1+\sigma_1^2\mu_2)}{(\sigma_2^2+\sigma_1^2)}r +\left(\frac{\sigma_2^2\mu_1+\sigma_1^2\mu_2}{\sigma_1^2+\sigma_2^2}\right)^2
}
{\frac{2\sigma_1^2\sigma_2^2}{(\sigma_2^2+\sigma_1^2)}}
+
\frac{\frac{(\sigma_2^2\mu_1^2+\sigma_1^2\mu_2^2)}{(\sigma_2^2+\sigma_1^2)}-
\left(\frac{\sigma_2^2\mu_1+\sigma_1^2\mu_2}{\sigma_1^2+\sigma_2^2}\right)^2
}{\frac{2\sigma_1^2\sigma_2^2}{(\sigma_2^2+\sigma_1^2)}} = \\


\frac{r^2 -
2\frac{(\sigma_2^2\mu_1+\sigma_1^2\mu_2)}{(\sigma_2^2+\sigma_1^2)}r +\left(\frac{\sigma_2^2\mu_1+\sigma_1^2\mu_2}{\sigma_1^2+\sigma_2^2}\right)^2
}
{\frac{2\sigma_1^2\sigma_2^2}{(\sigma_2^2+\sigma_1^2)}}
+
\frac{\frac{(\sigma_2^2\mu_1^2+\sigma_1^2\mu_2^2)}{(\sigma_2^2+\sigma_1^2)}-
\frac{\sigma_2^4\mu_1^2+\sigma_1^4\mu_2^2+2\sigma_1^2\sigma_2^2\mu_1\mu_2}{\left(\sigma_2^2+\sigma_1^2\right)^2} 
}{\frac{2\sigma_1^2\sigma_2^2}{(\sigma_2^2+\sigma_1^2)}} = \\


\frac{r^2 -
2\frac{(\sigma_2^2\mu_1+\sigma_1^2\mu_2)}{(\sigma_2^2+\sigma_1^2)}r +\left(\frac{\sigma_2^2\mu_1+\sigma_1^2\mu_2}{\sigma_1^2+\sigma_2^2}\right)^2
}
{\frac{2\sigma_1^2\sigma_2^2}{(\sigma_2^2+\sigma_1^2)}}
+
\frac{\frac{\left(\sigma_2^2+\sigma_1^2\right)(\sigma_2^2\mu_1^2+\sigma_1^2\mu_2^2)-\left(\sigma_2^4\mu_1^2+\sigma_1^4\mu_2^2+2\sigma_1^2\sigma_2^2\mu_1\mu_2\right)}{\left(\sigma_2^2+\sigma_1^2\right)^2} 
}{\frac{2\sigma_1^2\sigma_2^2}{(\sigma_2^2+\sigma_1^2)}} = \\


\frac{r^2 -
2\frac{(\sigma_2^2\mu_1+\sigma_1^2\mu_2)}{(\sigma_2^2+\sigma_1^2)}r +\left(\frac{\sigma_2^2\mu_1+\sigma_1^2\mu_2}{\sigma_1^2+\sigma_2^2}\right)^2
}
{\frac{2\sigma_1^2\sigma_2^2}{(\sigma_2^2+\sigma_1^2)}}
+
\frac{\frac{(\sigma_2^4\mu_1^2+\sigma_2^2\sigma_1^2\mu_2^2+\sigma_2^2\sigma_1^2\mu_1^2+\sigma_1^4\mu_2^2)-\left(\sigma_2^4\mu_1^2+\sigma_1^4\mu_2^2+2\sigma_1^2\sigma_2^2\mu_1\mu_2\right)}{\left(\sigma_2^2+\sigma_1^2\right)^2} 
}{\frac{2\sigma_1^2\sigma_2^2}{(\sigma_2^2+\sigma_1^2)}} = \\

\frac{r^2 -
2\frac{(\sigma_2^2\mu_1+\sigma_1^2\mu_2)}{(\sigma_2^2+\sigma_1^2)}r +\left(\frac{\sigma_2^2\mu_1+\sigma_1^2\mu_2}{\sigma_1^2+\sigma_2^2}\right)^2
}
{\frac{2\sigma_1^2\sigma_2^2}{(\sigma_2^2+\sigma_1^2)}}
+
\frac{\frac{\sigma_2^4\mu_1^2+\sigma_2^2\sigma_1^2\mu_2^2+\sigma_2^2\sigma_1^2\mu_1^2+\sigma_1^4\mu_2^2
-
\sigma_2^4\mu_1^2-\sigma_1^4\mu_2^2-2\sigma_1^2\sigma_2^2\mu_1\mu_2}{\left(\sigma_2^2+\sigma_1^2\right)^2} 
}{\frac{2\sigma_1^2\sigma_2^2}{(\sigma_2^2+\sigma_1^2)}} = \\

\frac{r^2 -
2\frac{(\sigma_2^2\mu_1+\sigma_1^2\mu_2)}{(\sigma_2^2+\sigma_1^2)}r +\left(\frac{\sigma_2^2\mu_1+\sigma_1^2\mu_2}{\sigma_1^2+\sigma_2^2}\right)^2
}
{\frac{2\sigma_1^2\sigma_2^2}{(\sigma_2^2+\sigma_1^2)}}
+
\frac{\frac{\sigma_2^2\sigma_1^2\mu_2^2+\sigma_2^2\sigma_1^2\mu_1^2
-
2\sigma_1^2\sigma_2^2\mu_1\mu_2}{\left(\sigma_2^2+\sigma_1^2\right)^2} 
}{\frac{2\sigma_1^2\sigma_2^2}{(\sigma_2^2+\sigma_1^2)}} = \\

\frac{r^2 -
2\frac{(\sigma_2^2\mu_1+\sigma_1^2\mu_2)}{(\sigma_2^2+\sigma_1^2)}r +\left(\frac{\sigma_2^2\mu_1+\sigma_1^2\mu_2}{\sigma_1^2+\sigma_2^2}\right)^2
}
{\frac{2\sigma_1^2\sigma_2^2}{(\sigma_2^2+\sigma_1^2)}}
+
\frac{\frac{\sigma_1^2\sigma_2^2\left(\mu_2^2+\mu_1^2
-2\mu_1\mu_2\right)}{\left(\sigma_2^2+\sigma_1^2\right)^2} 
}{\frac{2\sigma_1^2\sigma_2^2}{(\sigma_2^2+\sigma_1^2)}} = \\



\frac{r^2 -
2\frac{(\sigma_2^2\mu_1+\sigma_1^2\mu_2)}{(\sigma_2^2+\sigma_1^2)}r +\left(\frac{\sigma_2^2\mu_1+\sigma_1^2\mu_2}{\sigma_1^2+\sigma_2^2}\right)^2
}
{\frac{2\sigma_1^2\sigma_2^2}{(\sigma_2^2+\sigma_1^2)}}
+
\frac{\left(\mu_2^2+\mu_1^2-2\mu_1\mu_2\right) 
}{2(\sigma_2^2+\sigma_1^2)} = \\



\frac{\left(r-\frac{(\sigma_2^2\mu_1+\sigma_1^2\mu_2)}{(\sigma_2^2+\sigma_1^2)}\right)^2
}
{\frac{2\sigma_1^2\sigma_2^2}{(\sigma_2^2+\sigma_1^2)}}
+
\frac{\left(\mu_2-\mu_1\right)^2
}{2(\sigma_2^2+\sigma_1^2)}\\

\mu_{\textit{fused}} = \frac{\mu_1\sigma_2^2+\mu_2\sigma_1^2}{\sigma_1^2+\sigma_2^2}\\
\sigma_{\textit{fused}}^2 = \frac{\sigma_1^2\sigma_2^2}{(\sigma_2^2+\sigma_1^2)}
$$
So we have
$$
\beta =  \frac{\left(r-\mu_{\textit{fused}}\right)^2
}
{2\sigma_{\textit{fused}}^2}
+
\frac{\left(\mu_2-\mu_1\right)^2
}{2(\sigma_2^2+\sigma_1^2)}
$$
And...
$$
y_{\textit{fused}}(r;\mu_1,\sigma_{\textit{fused}},\mu_{\textit{fused}}) =\frac{1}{2\pi\sigma_1\sigma_2}e^{-\beta } = \\

\frac{1}{2\pi\sigma_1\sigma_2}e^{-\left(\frac{\left(r-\mu_{\textit{fused}}\right)^2
}
{2\sigma_{\textit{fused}}^2}
+
\frac{\left(\mu_2-\mu_1\right)^2
}{2(\sigma_2^2+\sigma_1^2)}\right) }
= \\

\frac{1}{2\pi\sigma_1\sigma_2}

e^{-\frac{\left(r-\mu_{\textit{fused}}\right)^2
}
{2\sigma_{\textit{fused}}^2}}

e^{-
\frac{\left(\mu_2-\mu_1\right)^2
}{2(\sigma_2^2+\sigma_1^2)} }

= \\
\frac{\sigma_{\textit{fused}}}{\sigma_{\textit{fused}}}
\cdot
\frac{1}{2\pi\sigma_1\sigma_2}

e^{-\frac{\left(r-\mu_{\textit{fused}}\right)^2
}
{2\sigma_{\textit{fused}}^2}}

e^{-
\frac{\left(\mu_2-\mu_1\right)^2
}{2(\sigma_2^2+\sigma_1^2)} } = \\

\frac{\sqrt{\frac{\sigma_1^2\sigma_2^2}{(\sigma_2^2+\sigma_1^2)}}}{\sigma_{\textit{fused}}}
\cdot
\frac{1}{2\pi\sigma_1\sigma_2}

e^{-\frac{\left(r-\mu_{\textit{fused}}\right)^2
}
{2\sigma_{\textit{fused}}^2}}

e^{-
\frac{\left(\mu_2-\mu_1\right)^2
}{2(\sigma_2^2+\sigma_1^2)} } = \\

\frac{\frac{\sigma_1\sigma_2}{\sqrt{(\sigma_2^2+\sigma_1^2)}}}{\sigma_{\textit{fused}}}
\cdot
\frac{1}{2\pi\sigma_1\sigma_2}

e^{-\frac{\left(r-\mu_{\textit{fused}}\right)^2
}
{2\sigma_{\textit{fused}}^2}}

e^{-
\frac{\left(\mu_2-\mu_1\right)^2
}{2(\sigma_2^2+\sigma_1^2)} } = \\


\frac{\frac{1}{\sqrt{(\sigma_2^2+\sigma_1^2)}}}{\sigma_{\textit{fused}}}
\cdot
\frac{1}{2\pi}

e^{-\frac{\left(r-\mu_{\textit{fused}}\right)^2
}
{2\sigma_{\textit{fused}}^2}}

e^{-
\frac{\left(\mu_2-\mu_1\right)^2
}{2(\sigma_2^2+\sigma_1^2)} } = \\



\frac{1}{2\pi\sqrt{(\sigma_2^2+\sigma_1^2)}\cdot\sigma_{\textit{fused}}}
e^{-\frac{\left(r-\mu_{\textit{fused}}\right)^2
}
{2\sigma_{\textit{fused}}^2}}

e^{-
\frac{\left(\mu_2-\mu_1\right)^2
}{2(\sigma_2^2+\sigma_1^2)} } = \\


\frac{1}{\sqrt{2\pi}\sigma_{\textit{fused}}}
e^{-\frac{\left(r-\mu_{\textit{fused}}\right)^2
}
{2\sigma_{\textit{fused}}^2}}
\underset{S_{12}}{\underbrace{
\frac{1}{\sqrt{2\pi(\sigma_2^2+\sigma_1^2)}}
e^{-
\frac{\left(\mu_2-\mu_1\right)^2
}{2(\sigma_2^2+\sigma_1^2)} }}} = \\


\frac{S_{12}}{\sqrt{2\pi}\sigma_{\textit{fused}}}
e^{-\frac{\left(r-\mu_{\textit{fused}}\right)^2
}
{2\sigma_{\textit{fused}}^2}}
$$
Where $S_{12}$ is a *scaling factor* (DOVE FINISCE STO SCALING FACTOR)

So, just to remind it, we have:
$$
\mu_{\textit{fused}} = \frac{\mu_1\sigma_2^2+\mu_2\sigma_1^2}{\sigma_1^2+\sigma_2^2}=\mu_1 + \frac{\sigma_1^2(\mu_2-\mu_1)}{\sigma_1^2+\sigma_2^2}
$$

$$
\sigma_{\textit{fused}}^2 = \frac{\sigma_1^2\sigma_2^2}{(\sigma_2^2+\sigma_1^2)}=\sigma_1^2-\frac{\sigma_1^4}{\sigma_1^2+\sigma_2^2}
$$

These last two equations represent the measurement update steps of the *Kalman* filter algorithm, as will be shown explicitly below. However, to present a more general case, we need to consider an extension to this example.

In the example above, it was assumed that the predictions and measurements were made in the same coordinate frame and in the same units. This has resulted in a particularly concise pair of equations representing the prediction and measurement update stages. It is important to note however that in reality a function is usually required to map predictions and measurements into the same domain. In a more realistic extension to our example, the position of the train will be predicted directly as a new distance along the railway line in units of meters, but the time of flight measurements are recorded in units of seconds. To allow the prediction and measurement pdfs to be multiplied together, one must be converted into the domain of the other, and it is standard practice to map the predictions into the measurement domain via the transformation matrix $\mathbf{H}_t$.

We now revisit $(10)$ and $(11)$ and, instead of allowing $y_1$ and $y_2$ to both represent values in meters along the railway track, we consider the distribution $y_2$ to represent the time of flight in seconds for a radio signal propagating from a transmitter positioned at $x = 0$ to the antenna on the train. The spatial prediction *pdf* $y_1$ is converted into the measurement domain by scaling the function by $c$, the speed of light. Equations $(10)$ and $(11)$ therefore must be rewritten as
$$
y_1(s;\mu_1,\sigma_1)= \frac{1}{\sqrt{2\pi\sigma_1^2}}e^{-\frac{(s-\frac{\mu_1}{c})^2}{2\left(\frac{\sigma_1}{c}\right)^2}}
$$

$$
y_2(s;\mu_2,\sigma_2)= \frac{1}{\sqrt{2\pi\sigma_2^2}}e^{-\frac{(s-\mu_2)^2}{2\sigma_2^2}}
$$

where both distributions are now defined in the measurement domain, radio signals propagate along the time "$s$" axis, and the measurement unit is the second.

Following the derivation as before we now find
$$
\frac{\mu_{\textit{fused}}}{c} = \frac{\mu_1}{c} + \frac{\left(\frac{\sigma_1}{c}\right)^2\left(\mu_2-\frac{\mu_1}{c}\right)}{\left(\frac{\sigma_1}{c}\right)^2+\sigma_2^2}\\

\implies\\

\mu_{\textit{fused}} = \mu_1 + \left(\frac{\frac{\sigma_1^2}{c}}{\left(\frac{\sigma_1}{c}\right)^2+\sigma_2^2}\right)\left(\mu_2-\frac{\mu_1}{c}\right)
$$
Substituting $H = \frac{1}{c}$ and $K = \frac{H\sigma_1^2}{H^2\sigma_1^2+\sigma_2^2}$ results in
$$
\mu_{\textit{fused}} = \mu_1 + K\cdot(\mu_2 - H\mu_1)
$$
Similarly the fused variance estimate becomes
$$
\frac{\sigma_{\textit{fused}}^2}{c^2} = \left(\frac{\sigma_1}{c}\right)^2 - \frac{\left(\frac{\sigma_1}{c}\right)^4}{\left(\frac{\sigma_1}{c}\right)^2+\sigma_2^2}\\

\implies\\

\sigma_{\textit{fused}}^2 = \sigma_1^2 - \left(\frac{\frac{\sigma_1^2}{c}}{\left(\frac{\sigma_1}{c}\right)^2+\sigma_2^2}\right)\frac{\sigma_1^2}{c}\\
$$

$$
\sigma_{\textit{fused}}^2 = \sigma_1^2 - KH\sigma_1^2
$$

We can now compare certain terms resulting from this scalar derivation with the standard vectors and matrices used in the *Kalman filter* algorithm:

- $\mu_{\textit{fused}} \to \mathbf{\hat{x}_{t|t}}$  :

	The state vector *following* data fusion.

- $\mu_1\to \mathbf{\hat{x}_{t|t-1}}$ :

	The state vector *before* data fusion, i.e., the prediction.

- $\sigma_{\textit{fused}}^2\to\mathbf{P}_{t|t}$ :

	The covariance matrix (confidence) *following* data fusion.

- $\sigma_1^2\to\mathbf{P}_{t|t-1}$ :

	The covariance matrix (confidence) *before* data fusion.

- $\mu_2 \to \mathbf{z}_t$ :

	The *measurement* vector.

- $\sigma_2^2\to \mathbf{R}_t$ :

	The uncertainty matrix associated with a noisy set of *measurements*.

- $H\to\mathbf{H}_t$ :

	The *transformation matrix* used to map *state vector parameters* into the *measurement* domain.

- $K = \frac{H\sigma_1^2}{H^2\sigma_1^2+\sigma_2^2} \to \mathbf{K}_t = \mathbf{P}_{t|t-1}\mathbf{H}_t^T\left(\mathbf{H}_t\mathbf{P}_{t|t-1}\mathbf{H}_t^T+\mathbf{R}_t\right)^{-1}$ :

	The *Kalman gain*.

It is now easy to see how the standard *Kalman filter equations* relate to $(22)$ and $(24)$ derived above:
$$
\mu_{\textit{fused}} = \mu_1 + \left(\frac{H\sigma_1^2}{H^2\sigma_1^2+\sigma_2^2}\right)\left(\mu_2-H\mu_1\right)\\
\implies
\mathbf{\hat{x}}_{t|t} = \mathbf{\hat{x}}_{t|t-1} + \mathbf{K}_t\left(\mathbf{z}_t-\mathbf{H}_t\mathbf{\hat{x}}_{t|t-1}\right)
$$

$$
\sigma_{\textit{fused}}^2 = \sigma_1^2 - \left(\frac{H\sigma_1^2}{H^2\sigma_1^2+\sigma_2^2}\right)H\sigma_1^2\\
\implies\\
\mathbf{P}_{t|t} = \mathbf{P}_{t|t-1} - \mathbf{K}_t\mathbf{H}_t\mathbf{P}_{t|t-1}
$$

Finally here are our key equations:
$$
\mathbf{\hat{x}}_{t|t-1}=\mathbf{F}_{t}\mathbf{\hat{x}}_{t-1|t-1}+\mathbf{B}_t\mathbf{u}_t
$$

$$
\mathbf{\hat{x}}_{t|t} = \mathbf{\hat{x}}_{t|t-1} + \mathbf{K}_t\underset{\text{Innovation!}}{\underbrace{\left(\mathbf{z}_t-\mathbf{H}_t\mathbf{\hat{x}}_{t|t-1}\right)}}
$$

$$
\mathbf{P}_{t|t-1}=\mathbf{F}_t\mathbf{P}_{t-1|t-1}\mathbf{F}_t^T 
+\mathbf{Q}_t
$$

$$
\mathbf{P}_{t|t} = \mathbf{P}_{t|t-1} - \mathbf{K}_t\mathbf{H}_t\mathbf{P}_{t|t-1}
$$

$$
\mathbf{K}_t = \mathbf{P}_{t|t-1}\mathbf{H}_t^T\left(\mathbf{H}_t\mathbf{P}_{t|t-1}\mathbf{H}_t^T+\mathbf{R}_t\right)^{-1}
$$

Let's comment equation $(28)$:

----

*The new estimate $\mathbf{\hat{x}}_{t|t}$ is equal to the old estimate $\mathbf{\hat{x}}_{t-1|t-1}$ multiplied by the state transition matrix $\mathbf{F_t}$ plus a weighting factor $\mathbf{K}_t$ (Kalman Gain) times the innovation introduced by a measurement $\mathbf{z}_{t}$. We can see that in the innovation expression the matrix $\mathbf{H}$ just represents the transformation matrix used to map the state vector parameters into the measurement domain (of course you have to compare $\mathbf{z}_{t}$ and $\mathbf{F}_t\mathbf{\hat{x}}_{t-1|t-1}$ in the same domain!). Moreover if you see how the Kalman Gain is calculated it's trivial to understand that in corrispondence of high uncertainty in the measurement  $(\mathbf{R}_t$ is high $)$ the Kalman Gain $\mathbf{K}_t$ becomes tiny $\to$ low trust in the measurement!*

---