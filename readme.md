# random <!-- omit in toc -->

> Seedable random number generator supporting many common distributions.

[![NPM](https://img.shields.io/npm/v/random.svg)](https://www.npmjs.com/package/random) [![Build Status](https://github.com/transitive-bullshit/random/actions/workflows/test.yml/badge.svg)](https://github.com/transitive-bullshit/random/actions/workflows/test.yml) [![Prettier Code Formatting](https://img.shields.io/badge/code_style-prettier-brightgreen.svg)](https://prettier.io)

Welcome to the most **random** module on npm! 😜

## Highlights <!-- omit in toc -->

- Simple API (_make easy things easy and hard things possible_)
- TypeScript supported!
- Seedable based on entropy or user input
- Plugin support for different pseudo random number generators (PRNGs)
- Sample from many common distributions
  - uniform, normal, poisson, bernoulli, etc
- Validates all user input via [ow](https://github.com/sindresorhus/ow)
- Integrates with [seedrandom](https://github.com/davidbau/seedrandom)
- Supports node >= 14 and browser

## Install <!-- omit in toc -->

```bash
npm install --save random
# or
yarn add random
# or
pnpm add random
```

## Usage <!-- omit in toc -->

```ts
import random from 'random'

// quick uniform shortcuts
random.float((min = 0), (max = 1)) // uniform float in [ min, max )
random.int((min = 0), (max = 1)) // uniform integer in [ min, max ]
random.boolean() // true or false

// uniform distribution
random.uniform((min = 0), (max = 1)) // () => [ min, max )
random.uniformInt((min = 0), (max = 1)) // () => [ min, max ]
random.uniformBoolean() // () => [ false, true ]

// normal distribution
random.normal((mu = 0), (sigma = 1))
random.logNormal((mu = 0), (sigma = 1))

// bernoulli distribution
random.bernoulli((p = 0.5))
random.binomial((n = 1), (p = 0.5))
random.geometric((p = 0.5))

// poisson distribution
random.poisson((lambda = 1))
random.exponential((lambda = 1))

// misc distribution
random.irwinHall(n)
random.bates(n)
random.pareto(alpha)
```

For convenience, several common uniform samplers are exposed directly:

```ts
random.float() // 0.2149383367670885
random.int(0, 100) // 72
random.boolean() // true
```

**All distribution methods return a thunk** (function with no params), which will return a series of independent, identically distributed random variables from the specified distribution.

```ts
// create a normal distribution with default params (mu=1 and sigma=0)
const normal = random.normal()
normal() // 0.4855465422678824
normal() // -0.06696771815439678
normal() // 0.7350852689834705

// create a poisson distribution with default params (lambda=1)
const poisson = random.poisson()
poisson() // 0
poisson() // 4
poisson() // 1
```

Note that returning a thunk here is more efficient when generating multiple
samples from the same distribution.

You can change the underlying PRNG or its seed as follows:

```ts
import seedrandom from 'seedrandom'

// change the underlying pseudo random number generator
// by default, Math.random is used as the underlying PRNG
random.use(seedrandom('foobar'))

// create a new independent random number generator (uses seedrandom under the hood)
const rng = random.clone('my-new-seed')

// create a second independent random number generator and use a seeded PRNG
const rng2 = random.clone(seedrandom('kittyfoo'))

// replace Math.random with rng.uniform
rng.patch()

// restore original Math.random
rng.unpatch()
```

You can also instantiate a fresh instance of `Random`:

```ts
import { Random } from 'random'
import seedrandom from 'seedrandom'

const rng = new Random()
const rng2 = new Random(seedrandom('tinykittens'))
```

## API <!-- omit in toc -->

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

#### Table of Contents <!-- omit in toc -->

<!-- no toc -->

- [Random](#random)
  - [rng](#rng)
  - [clone](#clone)
  - [use](#use)
  - [patch](#patch)
  - [unpatch](#unpatch)
  - [next](#next)
  - [float](#float)
  - [int](#int)
  - [integer](#integer)
  - [bool](#bool)
  - [boolean](#boolean)
  - [uniform](#uniform)
  - [uniformInt](#uniformint)
  - [uniformBoolean](#uniformboolean)
  - [normal](#normal)
  - [logNormal](#lognormal)
  - [bernoulli](#bernoulli)
  - [binomial](#binomial)
  - [geometric](#geometric)
  - [poisson](#poisson)
  - [exponential](#exponential)
  - [irwinHall](#irwinhall)
  - [bates](#bates)
  - [pareto](#pareto)

### [Random](https://github.com/transitive-bullshit/random/blob/e11a840a1cfe0f5bd9c43640f9645a0b28f61406/src/random.js#L36-L382)

Seedable random number generator supporting many common distributions.

Defaults to Math.random as its underlying pseudorandom number generator.

Type: `function (rng)`

- `rng` **(RNG | [function](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function))** Underlying pseudorandom number generator. (optional, default `Math.random`)

---

#### [rng](https://github.com/transitive-bullshit/random/blob/e11a840a1cfe0f5bd9c43640f9645a0b28f61406/src/random.js#L47-L49)

Type: `function ()`

---

#### [clone](https://github.com/transitive-bullshit/random/blob/e11a840a1cfe0f5bd9c43640f9645a0b28f61406/src/random.js#L61-L67)

- **See: RNG.clone**

Creates a new `Random` instance, optionally specifying parameters to
set a new seed.

Type: `function (args, seed, opts): Random`

- `args` **...any**
- `seed` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?** Optional seed for new RNG.
- `opts` **[object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)?** Optional config for new RNG options.

---

#### [use](https://github.com/transitive-bullshit/random/blob/e11a840a1cfe0f5bd9c43640f9645a0b28f61406/src/random.js#L87-L89)

Sets the underlying pseudorandom number generator used via
either an instance of `seedrandom`, a custom instance of RNG
(for PRNG plugins), or a string specifying the PRNG to use
along with an optional `seed` and `opts` to initialize the
RNG.

Type: `function (args)`

- `args` **...any**

Example:

```javascript
import random from 'random'

random.use('example_seedrandom_string')
// or
random.use(seedrandom('kittens'))
// or
random.use(Math.random)
```

---

#### [patch](https://github.com/transitive-bullshit/random/blob/e11a840a1cfe0f5bd9c43640f9645a0b28f61406/src/random.js#L94-L101)

Patches `Math.random` with this Random instance's PRNG.

Type: `function ()`

---

#### [unpatch](https://github.com/transitive-bullshit/random/blob/e11a840a1cfe0f5bd9c43640f9645a0b28f61406/src/random.js#L106-L111)

Restores a previously patched `Math.random` to its original value.

Type: `function ()`

---

#### [next](https://github.com/transitive-bullshit/random/blob/e11a840a1cfe0f5bd9c43640f9645a0b28f61406/src/random.js#L124-L126)

Convenience wrapper around `this.rng.next()`

Returns a floating point number in \[0, 1).

Type: `function (): number`

---

#### [float](https://github.com/transitive-bullshit/random/blob/e11a840a1cfe0f5bd9c43640f9645a0b28f61406/src/random.js#L138-L140)

Samples a uniform random floating point number, optionally specifying
lower and upper bounds.

Convence wrapper around `random.uniform()`

Type: `function (min, max): number`

- `min` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Lower bound (float, inclusive) (optional, default `0`)
- `max` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Upper bound (float, exclusive) (optional, default `1`)

---

#### [int](https://github.com/transitive-bullshit/random/blob/e11a840a1cfe0f5bd9c43640f9645a0b28f61406/src/random.js#L152-L154)

Samples a uniform random integer, optionally specifying lower and upper
bounds.

Convence wrapper around `random.uniformInt()`

Type: `function (min, max): number`

- `min` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Lower bound (integer, inclusive) (optional, default `0`)
- `max` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Upper bound (integer, inclusive) (optional, default `1`)

---

#### [integer](https://github.com/transitive-bullshit/random/blob/e11a840a1cfe0f5bd9c43640f9645a0b28f61406/src/random.js#L168-L170)

Samples a uniform random integer, optionally specifying lower and upper
bounds.

Convence wrapper around `random.uniformInt()`

Type: `function (min, max): number`

- `min` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Lower bound (integer, inclusive) (optional, default `0`)
- `max` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Upper bound (integer, inclusive) (optional, default `1`)

---

#### [bool](https://github.com/transitive-bullshit/random/blob/e11a840a1cfe0f5bd9c43640f9645a0b28f61406/src/random.js#L181-L183)

Samples a uniform random boolean value.

Convence wrapper around `random.uniformBoolean()`

Type: `function (): boolean`

---

#### [boolean](https://github.com/transitive-bullshit/random/blob/e11a840a1cfe0f5bd9c43640f9645a0b28f61406/src/random.js#L192-L194)

Samples a uniform random boolean value.

Convence wrapper around `random.uniformBoolean()`

Type: `function (): boolean`

---

#### [uniform](https://github.com/transitive-bullshit/random/blob/e11a840a1cfe0f5bd9c43640f9645a0b28f61406/src/random.js#L207-L209)

Generates a [Continuous uniform distribution](<https://en.wikipedia.org/wiki/Uniform_distribution_(continuous)>).

Type: `function (min, max): function`

- `min` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Lower bound (float, inclusive) (optional, default `0`)
- `max` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Upper bound (float, exclusive) (optional, default `1`)

---

#### [uniformInt](https://github.com/transitive-bullshit/random/blob/e11a840a1cfe0f5bd9c43640f9645a0b28f61406/src/random.js#L218-L220)

Generates a [Discrete uniform distribution](https://en.wikipedia.org/wiki/Discrete_uniform_distribution).

Type: `function (min, max): function`

- `min` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Lower bound (integer, inclusive) (optional, default `0`)
- `max` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Upper bound (integer, inclusive) (optional, default `1`)

---

#### [uniformBoolean](https://github.com/transitive-bullshit/random/blob/e11a840a1cfe0f5bd9c43640f9645a0b28f61406/src/random.js#L230-L232)

Generates a [Discrete uniform distribution](https://en.wikipedia.org/wiki/Discrete_uniform_distribution),
with two possible outcomes, `true` or \`false.

This method is analogous to flipping a coin.

Type: `function (): function`

---

#### [normal](https://github.com/transitive-bullshit/random/blob/e11a840a1cfe0f5bd9c43640f9645a0b28f61406/src/random.js#L245-L247)

Generates a [Normal distribution](https://en.wikipedia.org/wiki/Normal_distribution).

Type: `function (mu, sigma): function`

- `mu` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Mean (optional, default `0`)
- `sigma` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Standard deviation (optional, default `1`)

---

#### [logNormal](https://github.com/transitive-bullshit/random/blob/e11a840a1cfe0f5bd9c43640f9645a0b28f61406/src/random.js#L256-L258)

Generates a [Log-normal distribution](https://en.wikipedia.org/wiki/Log-normal_distribution).

Type: `function (mu, sigma): function`

- `mu` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Mean of underlying normal distribution (optional, default `0`)
- `sigma` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Standard deviation of underlying normal distribution (optional, default `1`)

---

#### [bernoulli](https://github.com/transitive-bullshit/random/blob/e11a840a1cfe0f5bd9c43640f9645a0b28f61406/src/random.js#L270-L272)

Generates a [Bernoulli distribution](https://en.wikipedia.org/wiki/Bernoulli_distribution).

Type: `function (p): function`

- `p` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Success probability of each trial. (optional, default `0.5`)

---

#### [binomial](https://github.com/transitive-bullshit/random/blob/e11a840a1cfe0f5bd9c43640f9645a0b28f61406/src/random.js#L281-L283)

Generates a [Binomial distribution](https://en.wikipedia.org/wiki/Binomial_distribution).

Type: `function (n, p): function`

- `n` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Number of trials. (optional, default `1`)
- `p` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Success probability of each trial. (optional, default `0.5`)

---

#### [geometric](https://github.com/transitive-bullshit/random/blob/e11a840a1cfe0f5bd9c43640f9645a0b28f61406/src/random.js#L291-L293)

Generates a [Geometric distribution](https://en.wikipedia.org/wiki/Geometric_distribution).

Type: `function (p): function`

- `p` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Success probability of each trial. (optional, default `0.5`)

---

#### [poisson](https://github.com/transitive-bullshit/random/blob/e11a840a1cfe0f5bd9c43640f9645a0b28f61406/src/random.js#L305-L307)

Generates a [Poisson distribution](https://en.wikipedia.org/wiki/Poisson_distribution).

Type: `function (lambda): function`

- `lambda` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Mean (lambda > 0) (optional, default `1`)

---

#### [exponential](https://github.com/transitive-bullshit/random/blob/e11a840a1cfe0f5bd9c43640f9645a0b28f61406/src/random.js#L315-L317)

Generates an [Exponential distribution](https://en.wikipedia.org/wiki/Exponential_distribution).

Type: `function (lambda): function`

- `lambda` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Inverse mean (lambda > 0) (optional, default `1`)

---

#### [irwinHall](https://github.com/transitive-bullshit/random/blob/e11a840a1cfe0f5bd9c43640f9645a0b28f61406/src/random.js#L329-L331)

Generates an [Irwin Hall distribution](https://en.wikipedia.org/wiki/Irwin%E2%80%93Hall_distribution).

Type: `function (n): function`

- `n` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Number of uniform samples to sum (n >= 0) (optional, default `1`)

---

#### [bates](https://github.com/transitive-bullshit/random/blob/e11a840a1cfe0f5bd9c43640f9645a0b28f61406/src/random.js#L339-L341)

Generates a [Bates distribution](https://en.wikipedia.org/wiki/Bates_distribution).

Type: `function (n): function`

- `n` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Number of uniform samples to average (n >= 1) (optional, default `1`)

---

#### [pareto](https://github.com/transitive-bullshit/random/blob/e11a840a1cfe0f5bd9c43640f9645a0b28f61406/src/random.js#L349-L351)

Generates a [Pareto distribution](https://en.wikipedia.org/wiki/Pareto_distribution).

Type: `function (alpha): function`

- `alpha` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Alpha (optional, default `1`)

---

## Todo <!-- omit in toc -->

- Distributions

  - [x] uniform
  - [x] uniformInt
  - [x] uniformBoolean
  - [x] normal
  - [x] logNormal
  - [ ] chiSquared
  - [ ] cauchy
  - [ ] fischerF
  - [ ] studentT
  - [x] bernoulli
  - [x] binomial
  - [ ] negativeBinomial
  - [x] geometric
  - [x] poisson
  - [x] exponential
  - [ ] gamma
  - [ ] hyperExponential
  - [ ] weibull
  - [ ] beta
  - [ ] laplace
  - [x] irwinHall
  - [x] bates
  - [x] pareto

- Generators

  - [x] pluggable prng
  - [ ] port more prng from boost
  - [ ] custom entropy

- Misc
  - [x] browser support via rollup
  - [x] basic docs
  - [x] basic tests
  - [x] test suite
  - [x] initial release!
  - [x] typescript support

## Related <!-- omit in toc -->

- [d3-random](https://github.com/d3/d3-random) - D3's excellent random number generation library.
- [seedrandom](https://github.com/davidbau/seedrandom) - Seedable pseudo random number generator.
- [random-int](https://github.com/sindresorhus/random-int) - For the common use case of generating uniform random ints.
- [random-float](https://github.com/sindresorhus/random-float) - For the common use case of generating uniform random floats.
- [randombytes](https://github.com/crypto-browserify/randombytes) - Random crypto bytes for Node.js and the browser.

## Credit <!-- omit in toc -->

Thanks go to [Andrew Moss](https://github.com/agmoss) for the TypeScript port and for helping to maintain this package!

Shoutout to [Roger Combs](https://github.com/rcombs) for donating the `random` npm package for this project!

Lots of inspiration from [d3-random](https://github.com/d3/d3-random) ([@mbostock](https://github.com/mbostock) and [@svanschooten](https://github.com/svanschooten)).

Some distributions and PRNGs are ported from C++ [boost::random](https://www.boost.org/doc/libs/1_66_0/doc/html/boost_random/reference.html#boost_random.reference.distributions).

## License <!-- omit in toc -->

MIT © [Travis Fischer](https://transitivebullsh.it)

Support my OSS work by <a href="https://twitter.com/transitive_bs">following me on twitter <img src="https://storage.googleapis.com/saasify-assets/twitter-logo.svg" alt="twitter" height="24px" align="center"></a>
