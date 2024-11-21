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
- $`C_{X}`$ - A constant. Typically used to mean "the amount of $`X`$ you started off with".

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
    \frac{d}{dt}(N_{MK1}) = C_{MK2}r_{MK2} \\
    N_{MK1} = \int_{MK1}C_{MK2}r_{MK2}dt
}
```

Substituting the $`N_{MK1}`$ expression into $`\frac{d}{dt}(N_{cells}) = N_{MK1}r_{MK1}`$:

```math
\displaylines{
    \frac{d}{dt}(N_{cells}) = (\int_{MK1}C_{MK2}r_{MK2}dt)r_{MK1} \\
    N_{cells} = r_{MK2}r_{MK1}\int_{cells}\int_{MK1}C_{MK2} dt \\
    N_{cells} = r_{MK2}r_{MK1}\int_{cells}(C_{MK2}t + C_{MK1}) dt \\
    N_{cells} = r_{MK2}r_{MK1}(\frac{1}{2}C_{MK2}t^{2} + C_{MK1}t + C_{cells})
}
```

## 5. MK3 → MK2 → MK1 → Cells

Extrapolating the above pattern, $`N_{cells}`$ for a system where the highest level producers are MK3 generators:

```math
\displaylines{
    N_{cells} = r_{MK3}r_{MK2}r_{MK1}\int_{cells}\int_{MK1}\int_{MK2}C_{MK3} dt \\
    N_{cells} = r_{MK3}r_{MK2}r_{MK1}(\frac{1}{6}C_{MK3}t^{3} + \frac{1}{2}C_{MK2}t^{2} + C_{MK1}t + C_{cells})
}
```

A pattern can be discerned here: each higher-level generator's contribution to the number of cells at time $t$ is:
- proportional to a time component to the _power_ of the generator level, e.g. the time term for MK3 generators is $`t^{3}`$.
- proportional to factorial divisor of the generator level, e.g. the denominator for MK3 generators ($`C_{MK3}`$) is $`\frac{1}{3!} = \frac{1}{6}`$.

As a whole, an increase of $`r_{MKx}`$ has an equal effect on $`N_{cells}`$ _regardless of the generator level_.

## 6. General Form for Generator Scaling

With the above observations, we arrive at a general form for the productive impact for all generator levels on the overall cell amount across time.

```math
N_{cells}(t) = \prod_{x=1}^{8}r_{MKx} \sum_{x=0}^{8}\frac{1}{x!}C_{MKx}t^{x}
```

Where:
* $`x`$ is the generator level (consider $`C_{MK0}`$ to be equivalent to $`C_{cells}`$),
* $`\prod_{x=1}^{8}r_{MKx}`$ is the [product](https://en.wikipedia.org/wiki/Multiplication#Product_of_a_sequence) of all rates of production across _all_ generator levels,

```math
\prod_{x=1}^{8}r_{MKx} = r_{MK8}*r_{MK7}* ... *r_{MK2}*r_{MK1}
```

* and $`\sum_{x=0}^{8}\frac{1}{x!}C_{MKx}t^{x}`$ is the additive contribution of each generator to $`N_{cells}(t)`$.

```math
\sum_{x=0}^{8}\frac{1}{x!}C_{MKx}t^{x} = \frac{1}{40320}C_{MK8}t^{8} + \frac{1}{5040}C_{MK7}t^{7} + ... + \frac{1}{2}C_{MK2}t^{2} + C_{MK1}t + C_{cells}
```

In other words, each higher-level generator has a much greater impact on the overall cell amount at time $t$ due to having a time component that is proportional to the generator level, divided by that level's factorial.

## 7. Appendix

### 7.1. Scaling Contribution

- $`N_{cells}`$ coef. - Scaling contribution of the generator to number of cells at time $`t`$. This is the coefficient of $`N_{MKx}`$ in $`N_{cells}`$.
- $`\frac{d}{dt}(N_{cells})`$ coef. - Scaling contribution of the generator to the growth of cells at time $`t`$. This is the coefficient of $`N_{MKx}`$ in $`\frac{d}{dt}(N_{cells})`$.
- $`\mathbb{R} = \prod_{x=1}^{8}r_{MKx}`$ for brevity.

| Generator | $`N_{cells}`$, $`N_{MKx}`$ coef. | $`\frac{d}{dt}(N_{cells})`$, $`N_{MKx}`$ coef. |
| --- | --- | --- |
| MK1 | $`\mathbb{R}t`$ | $`\mathbb{R}`$ |
| MK2 | $`\frac{1}{2}\mathbb{R}t^{2}`$ | $`\mathbb{R}t`$ |
| MK3 | $`\frac{1}{6}\mathbb{R}t^{3}`$ | $`\frac{1}{2}\mathbb{R}t^{2}`$ |
| MK4 | $`\frac{1}{24}\mathbb{R}t^{4}`$ | $`\frac{1}{6}\mathbb{R}t^{3}`$ |
| MK5 | $`\frac{1}{120}\mathbb{R}t^{5}`$ | $`\frac{1}{24}\mathbb{R}t^{4}`$ |
| MK6 | $`\frac{1}{720}\mathbb{R}t^{6}`$ | $`\frac{1}{120}\mathbb{R}t^{5}`$ |
| MK7 | $`\frac{1}{5040}\mathbb{R}t^{7}`$ | $`\frac{1}{720}\mathbb{R}t^{6}`$ |
| MK8 | $`\frac{1}{40320}\mathbb{R}t^{8}`$ | $`\frac{1}{5040}\mathbb{R}t^{7}`$ |
