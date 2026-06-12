---
title: 微分法
category: 定理
---

## 1. 定義
$x$の関数$f(x)$から導関数$f'(x)$を求めることを，$x$について**微分する**という．

## 2. $f'(x)$の符号と関数の増減
関数$y=f(x)$の値の増減は，
- $f'(x) < 0$となる$x$の範囲で減少し，
- $0 < f'(x)$となる$x$の範囲で増加する．

## 3. 公式

### 基本的な関数の微分

$$
(c)' = 0 \quad (\text{ただし } c \text{ は定数})
$$

$$
(x^n)' = n x^{n-1} \quad (\text{実数 } n)
$$

### 積と商の微分公式

$$
\{f(x)g(x)\}' = f'(x)g(x) + f(x)g'(x)
$$

$$
\left\{\frac{f(x)}{g(x)}\right\}' = \frac{f'(x)g(x) - f(x)g'(x)}{\{g(x)\}^2}
$$


$$
\left\{\frac{1}{g(x)}\right\}' = -\frac{g'(x)}{\{g(x)\}^2}
$$

### 合成関数・逆関数の微分公式

$$
\{f(g(x))\}' = f'(g(x)) \cdot g'(x)
$$

$$
\frac{dx}{dy} = \frac{1}{\frac{dy}{dx}}
$$

$$
\frac{dy}{dx} = \frac{dy}{du} \cdot \frac{du}{dx}
$$

$$
\frac{dy}{dx} = \frac{ \frac{dy}{d \theta} }{ \frac{dx}{d \theta} }
$$

### 第$n$次導関数の計算
$$
f''(x) = \frac{d}{dx} f'(x)
$$

### 三角関数・指数関数・対数関数の微分

| 関数 $f(x)$ | 導関数 $f'(x)$ |
| --- | --- |
| $\sin x$ | $\cos x$ |
| $\cos x$ | $-\sin x$ |
| $\tan x$ | $\frac{1}{\cos^2 x}$ |
| $e^x$ | $e^x$ （変化しない） |
| $a^x$ | $a^x \log e a \quad (a > 0, a \neq 1)$ |
| $\log x$ | $\frac{1}{x}$ |
| $\log_a x$ | $\frac{1}{x \log e a} \quad (a > 0, a \neq 1)$ |