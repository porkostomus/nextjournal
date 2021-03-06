# Quadratic word problems (standard form)

## Fish and water temperature

The number of fish is a function of the water's temperature:

$\\Large{p(x)=-2x^2+40x-72}$

```bash id=d6e176cc-a1c4-43c2-b9aa-02f564d0c2ca
pip install sympy
```

**Which temperatures will result in **$0$** fish?**

```python id=71e7b177-5370-4415-8062-922023b3e2b4
import sympy as sp 
import matplotlib.pyplot as plt
import numpy as np

def plot(a, b, c, xax, yax):
  x = sp.Symbol('x')
  roots = sorted(sp.solveset(a*x**2 + b*x + c))
  xaxis = np.arange(roots[0], roots[1], 0.01)
  yaxis = a*xaxis**2 + b*xaxis + c
  fig, ax = plt.subplots()
  ax.plot(xaxis, yaxis)
  ax.set(xlabel=xax, ylabel=yax)
  ax.grid()
  return;

plot(-2, 40, -72, 'Temperature', 'Fish')
plt.gcf()
```

There are no fish when $p(x)=0$.

To find the zeros, we need to factor the equation as a product of 2 binomials. First divide by $a$ to get $x^2$ by itself:

```clojure id=31ca9b13-c15e-4989-b4a1-d0b18089333f
(defn divide-by-a [[a b c r]]
  [1 (/ b a) (/ c a) (/ r a)])

(defn div-by-a [s]
  (map #(/ % (first s)) s))

(div-by-a [-2 40 -72 0])
```

$\\Large{x^2-20x+36=0}$

We want to find the positive and negative integer factors of $c$:

```clojure id=3a270ac3-4048-43e4-9c97-3943b6c7b52f
(defn factors [n]
  (for [x (into (range (- (Math/abs n)) 0)
                (range 1 (inc (Math/abs n))))
        :when (zero? (rem n x))]
    [(int x) (int (/ n x))]))

(factors 0)
```

Now find the ones that add up to $b$and solve for $x=0$ by taking their negatives:

```clojure id=f99ca4ff-8bf0-460c-8043-7639641f483b
(defn zeros [[a b c r]]
  (map - (first (filter #(= b (apply + %))
                        (factors c)))))

(-> [-1 100 0 0]
  divide-by-a)

(zeros [1 -100 0 0])
```

## Profit and price of an item

Profit is a function of the price of an item:

$\\Large{P(x)=-2x^2+16x-24}$

```python id=b6d33576-bdce-4b60-a77a-18bd777c3483
plot(-2, 16, -24, 'Price', 'Profit')
plt.gcf()
```

**What price results in the maximum profit?**

Find the vertex's $x$\-coordinate by taking the average of the two zeros:

```clojure id=c675ce95-f97f-45a1-a3ad-c9d143c1a1aa
(defn average [x y]
  (/ (+ x y) 2))

(defn vert-x [[a b c r]]
  (apply average
       (-> [a b c r]
         divide-by-a
         zeros)))

(vert-x [-2 16 -24 0])
```

## Height of a thrown stone

Alain throws a stone off a bridge into a river below.

The stone's height, $x$ seconds after Alain threw it, is modeled by:

$h(x)=-5x^2+10x+15$

```python id=e9f4cea0-61b8-4017-9897-480daaebe979
plot(-5, 10, 15, 'Seconds', 'Height')
plt.gcf()
```

**What is the height of the stone at the time it is thrown?**

That's easy, substitute $0$for $x$ to get $15$.

## Height after takeoff

The spaceship's height (in meters), $x$ seconds after takeoff, is modeled by:

$h(x)=-2x^2+20x+48$

```python id=bdad01c0-1b0a-4126-908b-fb89f95ba0c9
plot(-2, 20, 48, 'Seconds', 'Height')
plt.gcf()
```

**What is the maximum height that the ship will reach?**

```clojure id=7bc9d87b-3ca6-4e4c-82e9-d64b979b1f4a
(vert-x [-2 20 48 0])
```

The vertex's $x$\-coordinate is $\\blueD 5$.

Now find $h(\\blueD{5})$:

```clojure id=57ea7e6e-7dd6-444b-ac1c-9edfdb1d7c3c
(defn solve [[a b c r] x]
  (+ (* (* x x) a)
     (* b x)
     c))

(solve [-2 20 48 0] 5)
```

## Object launched from platform

Its height, $x$ seconds after the launch, is modeled by:

$\\Large{h(x)=-5x^2+20x+60}$

**How many seconds after launch will the object land on the ground?**

```clojure id=3c8addfe-d253-47b3-ab96-c498a34d9d14
(->> [-5 20 60 0]
  divide-by-a
  zeros
  (apply max))
```

**What is the height of the object at the time of launch?**

```clojure id=f83c4935-c72b-467b-9e29-3e22cecad791
(solve [-5 20 60 0] 0)
```

## Power generated from circuit

The power $P$generated by an electrical circuit (in watts) as a function of its current $c$ (in amperes) is modeled by:

$\\huge{P(c)=-12c^2+120c}$

**What is the maximum power generated by the circuit?**

To find the roots, we need to get it into factored form.

$\\Large{\\begin{aligned} P(c)&=0 \\\\\\\\ -12c^2+120c&=0 \\end{aligned}}$

Divide by`(" a")`:

```clojure id=d6d80294-caff-4e03-bfd1-4fb430475c34
(div-by-a [-12 120 0])
```

$c^2-10c=0$

Divide by the variable $c$and take the root of each term:

```clojure id=23394200-eb27-491b-b489-2cdc2f3aea12
(defn root [[b c]]
  (- (/ c b)))

(defn roots [eq1 eq2]
  [(root eq1) (root eq2)])

(defn factor-x [[a b c]]
  [[1 0] [1 (/ b a)]])

(apply roots (factor-x [1 -10 0]))
```

The vertex's $x$coordinate is the average of the 2 roots; which we will then plug into the equation for the $y$coordinate:

```clojure id=bb412fd1-481e-4108-88de-ae4bc7d210ea
(defn vertex [[a b c :as eq]]
  (let [div-a (div-by-a eq)
        roots (apply roots (factor-x div-a))
        x (apply average roots)
        y (+ (* (* x x)
                a)
             (* b x)
             c)]
    [x y]))

(vertex [-12 120 0])
```

## The garden's area

The garden's area $A$ is a function of the garden's width $w$:

$\\Large{A(w)=-w^2+100w}$

**What side width will produce the maximum garden area?**

```clojure id=26a82865-8c5f-4c01-b167-5397353188f0
(vertex [-1 100 0])
```

## Ball thrown off balcony

The ball's height (in meters above the ground), $x$ seconds after being thrown, is modeled by:

$\\Large{h(x)=-2x^2+4x+16}$

**How many seconds after being thrown will the ball hit the ground?**

```clojure id=e4cb38e7-0a58-4937-8fdd-fc0b2b43abd8
(->> [-2 4 16]
  div-by-a
  zeros
  (apply max))
```

## Diving altitude

A diver's altitude (in meters relative to sea level), $x$ seconds after diving, is modeled by:

$\\Large{d(x)=\\dfrac12x^2 -10x}$

**How many seconds after diving will the diver reach the lowest altitude?**

```clojure id=c3076add-8ab0-429f-a2e8-65ac65228ab5
(vertex [1/2 -10 0])
```

<details id="com.nextjournal.article">
<summary>This notebook was exported from <a href="https://nextjournal.com">https://nextjournal.com</a></summary>

```edn nextjournal-metadata
{:nodes
 {"23394200-eb27-491b-b489-2cdc2f3aea12"
  {:compute-ref #uuid "1bb165d7-8d0c-48c9-9879-d971cdd6a454",
   :exec-duration 118,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "e1d916c0-0392-47e0-9fe7-bc184f741f59"]},
  "26a82865-8c5f-4c01-b167-5397353188f0"
  {:compute-ref #uuid "2ecd2edf-e2de-4647-8f91-650c192beaa4",
   :exec-duration 35,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "e1d916c0-0392-47e0-9fe7-bc184f741f59"]},
  "31ca9b13-c15e-4989-b4a1-d0b18089333f"
  {:compute-ref #uuid "4488318b-8da6-480e-a9a1-4f88738c8ff4",
   :exec-duration 205,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "e1d916c0-0392-47e0-9fe7-bc184f741f59"]},
  "3a270ac3-4048-43e4-9c97-3943b6c7b52f"
  {:compute-ref #uuid "3fef5063-c5ea-4a42-9545-96807a44cc86",
   :exec-duration 163,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "e1d916c0-0392-47e0-9fe7-bc184f741f59"]},
  "3c8addfe-d253-47b3-ab96-c498a34d9d14"
  {:compute-ref #uuid "f84ac629-db9e-4664-94dc-9d484959ac71",
   :exec-duration 48,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "e1d916c0-0392-47e0-9fe7-bc184f741f59"]},
  "57ea7e6e-7dd6-444b-ac1c-9edfdb1d7c3c"
  {:compute-ref #uuid "945d0f39-783a-414c-b126-d72d37682cbf",
   :exec-duration 67,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "e1d916c0-0392-47e0-9fe7-bc184f741f59"]},
  "6444c6b4-a50a-4f66-a7ad-1361b2d481d7"
  {:environment
   [:environment
    {:article/nextjournal.id
     #uuid "5b45e08b-5b96-413e-84ed-f03b5b65bd66",
     :change/nextjournal.id
     #uuid "5cc983f4-4ad6-43ee-abdc-44c24d0533ff",
     :node/id "0149f12a-08de-4f3d-9fd3-4b7a665e8624"}],
   :kind "runtime",
   :language "python",
   :type :nextjournal},
  "71e7b177-5370-4415-8062-922023b3e2b4"
  {:compute-ref #uuid "06b86151-db9a-469b-b019-27e7550bb762",
   :exec-duration 2286,
   :kind "code",
   :output-log-lines {:stdout 0},
   :refs (),
   :runtime [:runtime "6444c6b4-a50a-4f66-a7ad-1361b2d481d7"]},
  "7bc9d87b-3ca6-4e4c-82e9-d64b979b1f4a"
  {:compute-ref #uuid "184f770b-8412-4d40-825c-a7621f5c9f7a",
   :exec-duration 51,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "e1d916c0-0392-47e0-9fe7-bc184f741f59"]},
  "b6d33576-bdce-4b60-a77a-18bd777c3483"
  {:compute-ref #uuid "e5c70a54-92ae-4076-9f36-380ea9f520d3",
   :exec-duration 1841,
   :kind "code",
   :output-log-lines {:stdout 0},
   :refs (),
   :runtime [:runtime "6444c6b4-a50a-4f66-a7ad-1361b2d481d7"]},
  "bb412fd1-481e-4108-88de-ae4bc7d210ea"
  {:compute-ref #uuid "178e3310-0c0e-4b1c-96d2-c24db58bef5a",
   :exec-duration 78,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "e1d916c0-0392-47e0-9fe7-bc184f741f59"]},
  "bdad01c0-1b0a-4126-908b-fb89f95ba0c9"
  {:compute-ref #uuid "8e676cd1-63c4-41ef-a866-cee101141e00",
   :exec-duration 1662,
   :kind "code",
   :output-log-lines {:stdout 0},
   :refs (),
   :runtime [:runtime "6444c6b4-a50a-4f66-a7ad-1361b2d481d7"]},
  "c3076add-8ab0-429f-a2e8-65ac65228ab5"
  {:compute-ref #uuid "4937676b-7908-4121-8bad-0c69e6806475",
   :exec-duration 168,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "e1d916c0-0392-47e0-9fe7-bc184f741f59"]},
  "c675ce95-f97f-45a1-a3ad-c9d143c1a1aa"
  {:compute-ref #uuid "8c6df26d-ec54-4770-8472-279a56dd1481",
   :exec-duration 139,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "e1d916c0-0392-47e0-9fe7-bc184f741f59"]},
  "d6d80294-caff-4e03-bfd1-4fb430475c34"
  {:compute-ref #uuid "8ebabb63-6ba0-475f-9811-7f1e8f0d5485",
   :exec-duration 70,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "e1d916c0-0392-47e0-9fe7-bc184f741f59"]},
  "d6e176cc-a1c4-43c2-b9aa-02f564d0c2ca"
  {:compute-ref #uuid "8b8ab85f-0f96-4b83-ba5e-fbe62db00cba",
   :exec-duration 16873,
   :kind "code",
   :output-log-lines {:stdout 11},
   :refs (),
   :runtime [:runtime "6444c6b4-a50a-4f66-a7ad-1361b2d481d7"],
   :stdout-collapsed? true},
  "e1d916c0-0392-47e0-9fe7-bc184f741f59"
  {:environment
   [:environment
    {:article/nextjournal.id
     #uuid "5b45eb52-bad4-413d-9d7f-b2b573a25322",
     :change/nextjournal.id
     #uuid "5cd52af1-7a79-4804-a169-d6ffcdb6eb7a",
     :node/id "0ae15688-6f6a-40e2-a4fa-52d81371f733"}],
   :kind "runtime",
   :language "clojure",
   :type :nextjournal},
  "e4cb38e7-0a58-4937-8fdd-fc0b2b43abd8"
  {:compute-ref #uuid "849369df-0e92-45ae-802e-ec37f9fca1f1",
   :exec-duration 53,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "e1d916c0-0392-47e0-9fe7-bc184f741f59"]},
  "e9f4cea0-61b8-4017-9897-480daaebe979"
  {:compute-ref #uuid "d44ba4c7-a5ba-4707-8bb0-dbf3082957df",
   :exec-duration 1752,
   :kind "code",
   :output-log-lines {:stdout 0},
   :refs (),
   :runtime [:runtime "6444c6b4-a50a-4f66-a7ad-1361b2d481d7"]},
  "f83c4935-c72b-467b-9e29-3e22cecad791"
  {:compute-ref #uuid "f0357498-585f-49b0-94c4-e39ffee50e63",
   :exec-duration 37,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "e1d916c0-0392-47e0-9fe7-bc184f741f59"]},
  "f99ca4ff-8bf0-460c-8043-7639641f483b"
  {:compute-ref #uuid "a1464c79-5428-46c5-b147-0b830a72b71f",
   :exec-duration 116,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "e1d916c0-0392-47e0-9fe7-bc184f741f59"]}}}
```
</details>
