# The generated [`ReactionSystem`](@ref) and [`Reaction`](@ref)s
The `@reaction_network` macro generates a [`ReactionSystem`](@ref) object, which
has a number of fields that can be accessed directly or via the [Catalyst.jl
API](@ref) (the recommended route). Below we list these components, with the recommended
API method listed first:

* [`species(rn)`](@ref) and `states(rn)` is a vector of all the chemical
  species within the system, each represented as a `ModelingToolkit.Term`.
* [`params(rn)`](@ref) and `parameters(rn)` is a vector of all the parameters
  within the system, each represented as a `ModelingToolkit.Sym`.
* [`reactions(rn)`](@ref) and `equations(rn)` is a vector of all the
  `Reaction`s within the system.
* `get_iv(rn)` is the independent variable of the
  system, usually `t` for time, represented as a `ModelingToolkit.Sym`.

Each `Reaction` within `reactions(rn)` has a number of subfields. For `rx` a
`Reaction` we have:
* `rx.substrates`, a vector of ModelingToolkit expressions storing each
  substrate variable.
* `rx.products`, a vector of ModelingToolkit expressions storing each product
  variable.
* `rx.substoich`, a vector storing the corresponding integer stoichiometry of
  each substrate species in `rx.substrates`.
* `rx.prodstoich`, a vector storing the corresponding integer stoichiometry of
  each product species in `rx.products`.
* `rx.rate`, a `Number, `ModelingToolkit.Sym` or ModelingToolkit expression
  representing the reaction rate. E.g., for a reaction like `k*X, Y --> X+Y`,
  we'd have `rate = k*X`.
* `rx.netstoich`, a vector of pairs mapping the ModelingToolkit expression for
  each species that changes numbers by the reaction to how much it changes. E.g.,
  for `k, X + 2Y --> X + W`, we'd have `rx.netstoich = [Y(t) => -2, W(t) => 1]`.
* `rx.only_use_rate`, a boolean that is `true` if the reaction was made with
  non-filled arrows and should ignore mass action kinetics. `false` by default.

Empty `ReactionSystem`s can be generated via [`make_empty_network`](@ref) or
[`@reaction_network`](@ref) with no arguments (giving one argument to the latter
will specify a system name). `ReactionSystem`s can be programmatically extended
using [`addspecies!`](@ref), [`addparam!`](@ref), [`addreaction!`](@ref),
[`@add_reactions`](@ref), or composed using `merge` and `merge!`.
