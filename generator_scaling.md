# CIFI - Generator Scaling

This document is an attempt to mathematically quantify the effect of chained generators (generators producing lower level generators) on overall cell production.

- [1. Terminology](#1-terminology)
- [2. Only Cells, No Generators](#2-only-cells-no-generators)
- [3. MK1 → Cells](#3-mk1--cells)
- [4. MK2 → MK1 → Cells](#4-mk2--mk1--cells)
- [5. MK3 → MK2 → MK1 → Cells](#5-mk3--mk2--mk1--cells)
- [6. General Form for Generator Scaling](#6-general-form-for-generator-scaling)
- [7. Appendix](#7-appendix)
  - [7.1. Scaling Contribution](#71-scaling-contribution)

## 1. Terminology

- $`N_{X}`$ - Amount of $`X`$.
- $`r_{X}`$ - Rate of generation by entity $`X`$. This quantity typically refers to the production of a resource one level lower, e.g. $`r_{MK2}`$ means the rate of production of MK1 generators by a single MK2 generator.
- $`C_{X}`$ - A constant. Typically used to mean the initial amount of $`X`$ at tick $`0`$.

## 2. Only Cells, No Generators

With no generators, there are no cells produced over time. The total number of cells ($`N_{cells}`$) is a constant ($`C_{cells}`$) across time ($`t`$), i.e. you end up with the same number of cells as you started off with.

```math
\displaylines{
    \frac{d}{dt}(N_{cells}) = 0 \\
    N_{cells} = C_{cells}
}
```

## 3. MK1 → Cells

Since there are no higher-level generators, no MK1 generators are produced over time.

```math
\displaylines{
    \frac{d}{dt}(N_{MK1}) = 0 \\
    N_{MK1} = C_{MK1}
}
```

MK1 generators produce cells, at a rate of the number of MK1 generators ($`N_{MK1}`$) multiplied by the rate of cell production per MK1 generator ($`r_{MK1}`$):

```math
\displaylines{
    \frac{d}{dt}(N_{cells}) = C_{MK1}r_{MK1} \\
    N_{cells} = \int_{cells}C_{MK1}r_{MK1}dt = C_{MK1}r_{MK1}t + C_{cells}
}
```

Intuitively, $`N_{cells}`$ is in the form $`y = mx + C`$, with cell growth over time _linearly_ proportional to the number of MK1 generators ($`C_{MK1}`$) you start off with and the rate at which each MK1 generator produces cells ($`r_{MK1}`$).

## 4. MK2 → MK1 → Cells

Since there are no higher-level generators, no MK2 generators are produced over time.

```math
\displaylines{
    \frac{d}{dt}(N_{MK2}) = 0 \\
    N_{MK2} = C_{MK2}
}
```

MK2 generators produce MK1 generators.

```math
\displaylines{
    \frac{d}{dt}(N_{MK1}) = N_{MK2}r_{MK2} = C_{MK2}r_{MK2} \\
    N_{MK1} = \int_{MK1}C_{MK2}r_{MK2}dt = C_{MK2}r_{MK2}t + C_{MK1}
}
```

Substituting the $`N_{MK1}`$ expression into $`\frac{d}{dt}(N_{cells}) = N_{MK1}r_{MK1}`$:

```math
\displaylines{
    \frac{d}{dt}(N_{cells}) = (C_{MK2}r_{MK2}t + C_{MK1})r_{MK1} \\
    N_{cells} = \int_{cells} (C_{MK2}r_{MK2}t + C_{MK1})r_{MK1} dt \\
    N_{cells} = \frac{1}{2}C_{MK2}r_{MK2}r_{MK1}t^{2} + C_{MK1}r_{MK1}t + C_{cells}
}
```

## 5. MK3 → MK2 → MK1 → Cells

Extrapolating the above pattern, $`N_{cells}`$ for a system where the highest level producers are MK3 generators:

```math
\displaylines{
    N_{cells} = \frac{1}{6}C_{MK3}r_{MK3}r_{MK2}r_{MK1}t^{3} + \frac{1}{2}C_{MK2}r_{MK2}r_{MK1}t^{2} + C_{MK1}r_{MK1}t + C_{cells}
}
```

A pattern can be discerned here: each higher-level generator's contribution to the number of cells at time $t$ is:
- proportional to a time component to the _power_ of the generator level, e.g. the time term for MK3 generators is $`t^{3}`$.
- proportional to factorial divisor of the generator level, e.g. the denominator for MK3 generators ($`C_{MK3}`$) is $`\frac{1}{3!} = \frac{1}{6}`$.
- proportional to the product of the generators rates at that level and below it.

## 6. General Form for Generator Scaling

With the above observations, we arrive at a general form for the productive impact for all generator levels on the overall cell amount across time.

```math
N_{cells}(t) = \sum_{x=1}^{8}\frac{\mathbb{R}_{MKx}}{x!}C_{MKx}t^{x} + C_{cells}
```

Where:
* $`x`$ is the generator level,
* $`\mathbb{R}_{MKx}`$ is the rate product of generator $`MKx`$ and all generator levels below it,

```math
\displaylines{
    \mathbb{R}_{MK8} = r_{MK8}*r_{MK7}* ... *r_{MK2}*r_{MK1} \\
    \mathbb{R}_{MK7} = r_{MK7}*r_{MK6}* ... *r_{MK2}*r_{MK1} \\
    ... \\
    \mathbb{R}_{MK2} = r_{MK2}*r_{MK1} \\
    \mathbb{R}_{MK1} = r_{MK1}
}
```

* and $`\sum_{x=1}^{8}\frac{\mathbb{R}_{MKx}}{x!}C_{MKx}t^{x}`$ is the additive contribution of each generator to $`N_{cells}(t)`$.

```math
\sum_{x=1}^{8}\frac{\mathbb{R}_{MKx}}{x!}C_{MKx}t^{x} = \frac{\mathbb{R}_{MK8}}{40320}C_{MK8}t^{8} + \frac{\mathbb{R}_{MK7}}{5040}C_{MK7}t^{7} + ... + \frac{\mathbb{R}_{MK2}}{2}C_{MK2}t^{2} + \mathbb{R}_{MK1}C_{MK1}t
```

In other words, each higher-level generator has a much greater impact on the overall cell amount at time $t$ due to having a time component that is proportional to the generator level, divided by that level's factorial.

## 7. Appendix

### 7.1. Scaling Contribution

- $`N_{cells}`$ coef. - Scaling contribution of the generator to number of cells at time $`t`$. This is the coefficient of $`N_{MKx}`$ in $`N_{cells}`$.
- $`\frac{d}{dt}(N_{cells})`$ coef. - Scaling contribution of the generator to the growth of cells at time $`t`$. This is the coefficient of $`N_{MKx}`$ in $`\frac{d}{dt}(N_{cells})`$.

| Generator | $`N_{cells}`$, $`N_{MKx}`$ coef. | $`\frac{d}{dt}(N_{cells})`$, $`N_{MKx}`$ coef. | $`\mathbb{R}_{MKx}`$ |
| :---: | :---: | :---: | :---: |
| MK1 | $`\mathbb{R}_{MK1}t`$ | $`\mathbb{R}_{MK1}`$ | $`r_{MK1}`$ |
| MK2 | $`\frac{1}{2}\mathbb{R}_{MK2}t^{2}`$ | $`\mathbb{R}_{MK2}t`$ | $`r_{MK2}*r_{MK1}`$ |
| MK3 | $`\frac{1}{6}\mathbb{R}_{MK3}t^{3}`$ | $`\frac{1}{2}\mathbb{R}_{MK3}t^{2}`$ | $`r_{MK3}*r_{MK2}*r_{MK1}`$ |
| MK4 | $`\frac{1}{24}\mathbb{R}_{MK4}t^{4}`$ | $`\frac{1}{6}\mathbb{R}_{MK4}t^{3}`$ | $`r_{MK4}* ...*r_{MK1}`$ |
| MK5 | $`\frac{1}{120}\mathbb{R}_{MK5}t^{5}`$ | $`\frac{1}{24}\mathbb{R}_{MK5}t^{4}`$ | $`r_{MK5}*...*r_{MK1}`$ |
| MK6 | $`\frac{1}{720}\mathbb{R}_{MK6}t^{6}`$ | $`\frac{1}{120}\mathbb{R}_{MK6}t^{5}`$ | $`r_{MK6}*...*r_{MK1}`$ |
| MK7 | $`\frac{1}{5040}\mathbb{R}_{MK7}t^{7}`$ | $`\frac{1}{720}\mathbb{R}_{MK7}t^{6}`$ | $`r_{MK7}*...*r_{MK1}`$ |
| MK8 | $`\frac{1}{40320}\mathbb{R}_{MK8}t^{8}`$ | $`\frac{1}{5040}\mathbb{R}_{MK8}t^{7}`$ | $`r_{MK8}*...*r_{MK1}`$ |
