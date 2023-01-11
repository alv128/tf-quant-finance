<!--
This file is generated by a tool. Do not edit directly.
For open-source contributions the docs will be updated automatically.
-->

*Last updated: 2023-01-03.*

<div itemscope itemtype="http://developers.google.com/ReferenceObject">
<meta itemprop="name" content="tf_quant_finance.experimental.american_option_pricing.andersen_lake.exercise_boundary.boundary_numerator" />
<meta itemprop="path" content="Stable" />
</div>

# tf_quant_finance.experimental.american_option_pricing.andersen_lake.exercise_boundary.boundary_numerator

<!-- Insert buttons and diff -->

<table class="tfo-notebook-buttons tfo-api" align="left">
</table>

<a target="_blank" href="https://github.com/google/tf-quant-finance/blob/master/tf_quant_finance/experimental/american_option_pricing/exercise_boundary.py">View source</a>



Calculates the numerator of the exercise boundary function of an American option.

```python
tf_quant_finance.experimental.american_option_pricing.andersen_lake.exercise_boundary.boundary_numerator(
    tau_grid, b, k, r, q, sigma, integration_num_points=32, dtype=None
)
```



<!-- Placeholder for "Used in" -->

Calculates the numerator part of the calculation required to get the exercise
boundary function of an American option. This corresponds to `N` in formula
(3.7) in the paper [1].

#### References
[1] Leif Andersen, Mark Lake and Dimitri Offengenden. High-performance
American option pricing. 2015
https://engineering.nyu.edu/sites/default/files/2019-03/Carr-adjusting-exponential-levy-models.pdf#page=54

#### Example
```python
  dtype = tf.float64
  tau_grid = tf.constant([[0., 0.5, 1.], [0., 1., 2.],  [0., 3., 6.]],
                         dtype=dtype)
  k = tf.constant([1, 2, 2], dtype=dtype)
  r = tf.constant([0.01, 0.02, 0.04], dtype=dtype)
  q = tf.constant([0.01, 0.02, 0.0], dtype=dtype)
  sigma = tf.constant([0.1, 0.15, 0.05], dtype=dtype)
  k_exp = k[:, tf.newaxis, tf.newaxis]
  r_exp = r[:, tf.newaxis, tf.newaxis]
  q_exp = q[:, tf.newaxis, tf.newaxis]
  epsilon = machine_eps(dtype)
  def b_0(tau_grid_exp):
    one = tf.constant(1.0, dtype=dtype)
    return tf.ones_like(tau_grid_exp) * k_exp_exp * tf.where(
        tf.math.abs(q_exp_exp) < epsilon, one,
        tf.math.minimum(one, r_exp_exp / q_exp_exp))
  integration_num_points = 32
  boundary_numerator(tau_grid, b_0, k, r, q, sigma, integration_num_points)
  # Returns a tensor of shape [3, 3].
```

#### Args:


* <b>`tau_grid`</b>: Grid of values of shape `[num_options, grid_num_points]`
  indicating the time left until option maturity.
* <b>`b`</b>: Represents the exercise boundary function for the option. Receives
  `Tensor` of rank `tau_grid.rank + 1` and returns a `Tensor` of same shape.
* <b>`k`</b>: Same dtype as `tau_grid` with shape `num_options` representing the strike
  price of the option.
* <b>`r`</b>: Same shape and dtype as `k` representing the annualized risk-free
  interest rate, continuously compounded.
* <b>`q`</b>: Same shape and dtype as `k` representing the dividend rate.
* <b>`sigma`</b>: Same shape and dtype as `k` representing the volatility of the
  option's returns.
* <b>`integration_num_points`</b>: The number of points used in the integration
  approximation method.
  Default value: 32.
* <b>`dtype`</b>: If supplied, the dtype for all input tensors. Result will have the
  same dtype.
  Default value: None which maps to dtype of `tau_grid`.


#### Returns:

`Tensor` of shape `[num_options, grid_num_points]`, containing a partial
result for calculating the exercise boundary.