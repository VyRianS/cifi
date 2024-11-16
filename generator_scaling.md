# CIFI - Generator Scaling

This document is an attempt to mathematically quantify the effect of chained generators (generators producing lower level generators) on overall cell production.

- [Terminology](#terminology)
- [Only Cells, No Generators](#only-cells-no-generators)
- [MK1 Generators](#mk1-generators)
- [MK2 Generators](#mk2-generators)
- [General Form for Generator Scaling](#general-form-for-generator-scaling)

## Terminology

- $N_{X}$ - Amount of $X$.
- $r_{X}$ - Rate of generation by entity $X$. This quantity typically refers to the production of a resource one level lower, e.g. $r_{MK2}$ means the rate of production of MK1 generators by a single MK2 generator.
- $C_{X}$ - A constant. Typically used to mean "the amount of $X$ you started off with".

## Only Cells, No Generators

With no generators, there are no cells produced over time. The total number of cells ($N_{cells}$) is a constant ($C_{cells}$) across time ($t$), i.e. you end up with the same number of cells as you started off with.

```math
\displaylines{
    \frac{d}{dt}(N_{cells}) = 0 \\
    N_{cells} = C_{cells}
}
```

## MK1 Generators

Since there are no higher-level generators, no MK1 generators are produced over time.

```math
\displaylines{
    \frac{d}{dt}(N_{MK1}) = 0 \\
    N_{MK1} = C_{MK1}
}
```

MK1 generators produce cells, at a rate of the number of MK1 generators ($N_{MK1}$) multiplied by the rate of cell production per MK1 generator ($r_{MK1}$):

```math
\displaylines{
    \frac{d}{dt}(N_{cells}) = N_{MK1}r_{MK1} \\
    N_{cells} = N_{MK1}r_{MK1}t + C_{cells}
}
```

Substituting $N_{MK1} = C_{MK1}$:

```math
N_{cells} = C_{MK1}r_{MK1}t + C_{cells}
```

Intuitively, $N_{cells}$ is in the form $y = mx + C$, with cell growth over time _linearly_ proportional to the number of MK1 generators ($C_{MK1}$) you start off with and the rate at which each MK1 generator produces cells ($r_{MK1}$).

## MK2 Generators

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
    \frac{d}{dt}(N_{MK1}) = N_{MK2}r_{MK2} \\
    N_{MK1} = N_{MK2}r_{MK2}t + C_{MK1} = C_{MK2}r_{MK2}t + C_{MK1}
}
```

Substituting the above formulation for $N_{MK1}$ into $N_{cells} = N_{MK1}r_{MK1}t + C_{cells}$:

```math
\displaylines{
    N_{cells} = (C_{MK2}r_{MK2}t + C_{MK1})r_{MK1}t + C_{cells} \\
    N_{cells} = C_{MK2}r_{MK2}r_{MK1}t^{2} + C_{MK1}r_{MK1}t + C_{cells}
}
```

A pattern can be discerned here: each higher-level generator's contribution to the number of cells at time $t$ is:
- proportional to the multiplicative effect of _all_ rates of generator and cell production at its level and below it, e.g. the rate term for MK3 generators is $r_{MK3}r_{MK2}r_{MK1}$, and so on,
- proportional to a time component to the _power_ of the generator level, e.g. the time term for MK3 generators is $t^{3}$.

## General Form for Generator Scaling

With the above observations, we arrive at a general form for the productive impact for all generator levels on the overall cell amount across time.

```math
N_{cells} = N_{MK8}\mathbb{R}_{MK8}t^{8} + N_{MK7}\mathbb{R}_{MK7}t^{7} + ... N_{MK1}\mathbb{R}_{MK1}t + C_{cells}
```

Where $`\mathbb{R}_{MKX}`$ is the product of all rates of production at this generator level and below it:

```math
\mathbb{R}_{MK8} = r_{MK8}*r_{MK7}* ... *r_{MK2}*r_{MK1}
```

In other words, each higher-level generator has a much greater impact on the overall cell amount at time $t$ due to being able to scale with all rates of production at its level and below it, and having a time component that is proportional to the generator level.
