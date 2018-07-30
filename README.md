Programming subset of Category Theory Cheatsheet
------

**functor**: given `Producer[A]` with **output** `A`, allows post-processing values with `A => B` to get `Producer[B]`<br/>
**contravariant functor**: given `Consumer[A]` with **input** `A`, allows pre-processing values with `B => A` to get `Consumer[B]`<br/>
**profunctor**: given `Pipe[A, B]` with **input** `A` and **output** `B`, allow pre-processing **input** values with `C => A`, and post-processing **output** values with `B => D` to get `Pipe[C, D]`<br/>
**applicative**: gives `map2`, `map3` **...** `mapN`. where `map2: (F[A], F[B]), (A, B => C) => F[C]` **...** `mapN: (F[A] .. F[N]), (A .. N => Z) => F[Z]`<br/>
**monad**: gives `flatten: F[F[A]] => F[A]`. If you `map`, then `flatten`, you can chain operations: `flatten(maybeReadFile.map(maybeParseAsInteger)) = Maybe[Integer]`<br/>
**monoid**: gives `plus` and `zero`<br/>
**alternative**: gives `orElse: F[A], F[A] => F[A]`<br/>
**category**: function-like entity with `compose` and `identity`<br/>
**arrow**: category with `fork`/`join`<br/>
**comonad**: given `Cursor[A]` with **output** `A`, allows post-processing values under nested cursors with `Cursor[A] => B` to get `Cursor[B]`. i.e.
  ```scala
  List(1,1,2).extend(sumOfList) = List(4, 3, 2)
  // list's cursor = head + tail
  // sumOfList(List(1,1,2)) = 4
  // sumOfList(List(1,2))   = 3
  // sumOfList(List(2))     = 2 
  sumOfList: List[Int] => Int

  AST(nodes...).extend(typecheckNodeAndChildren) = AST(typednodes...) // the type of each node depends on the types of child nodes
  // ast's cursor = node + children
  typecheckNodeAndChildren: AST[Node] => TypedNode
  ```

**algebra**: interface + laws

**free X**: data that only represents the operations of an algebraic structure **X**, but doesn't do anything.<br/>
**free**: *colloq.* free monad<br/>
**cofree**: *colloq.* free comonad<br/>
**free monoid**: *colloq.* list<br/>
**yoneda**: *colloq.* free functor built from a functor<br/>
**coyoneda**: *colloq.* free functor convertible into a functor<br/>
**codensity**: *colloq.* continuations-based free monad<br/>
**day convolution**: representation of `map2`<br/>

**tensor**: binder of multiple arguments. default tensor is tuple: `(A, B) => C`

**on monoids**: `applicative`, `alternative`, `monad` and `category` are all specialized **monoids** with different tensors<br/>
&nbsp;&nbsp;&nbsp;&nbsp;**intuition**:
  * **monoid** tensor is `tuple`
    ```scala
     plus: (A, A) => A
     // if i can combine tuples of A then i can combine a tupleN of A!
     // i can squish any List[A] into one A!
    ```
  * **monad** tensor is `compose`. `compose F G = F[G[_]]`
    ```scala
     plus: F compose F => F  // flatten: F[F[_]] => F[_]
    // if i can combine composes of F then i can combine a composeN of F!
    // i can flatten any F[F[F[F[F[F[F... into one F!
    ```

  * **alternative** tensor is `higherTuple`. `higherTuple F G = (F[_], G[_])`
    ```scala
     plus: F higherTuple F => F  // orElse: F[_], F[_] => F[_]
     // if i can combine higherTuples of F then i can combine a higherTupleN of F!
     // i can choose from any paths (F orElse F orElse F orElse...) one successful F!
    ```

  * **applicative** tensor is `map2` (day convolution). `map2 F G = (F[A], G[B], (A, B) => C)`
    ```scala
     plus: F map2 F => F  // map2: (F[A], F[B], (A, B) => C) => F[C]
     // if i can combine map2s of F then i can combine a mapN of F!
     // i can zip any (F[A], F[B] ... F[N]) with an (A ... N => Z) to get one F[Z]!
    ```

**f-algebra**: `F[A] => A`, fold-like function<br/>
**f-coalgebra**: `A => F[A]`, unfold-like function<br/>
**recursion**: `fold`-like function calling itself with some context while `reducing` a context.<br/>
**corecursion**: `unfold`-like function calling itself with some context while `building up` a context<br/>

**cata**: `fold`<br/>
**ana**: `unfold`<br/>
**hylo**: `unfold` then `fold`<br/>
**meta**: `fold` then `unfold`<br/>
**prepro**: `map` then `fold`<br/>
**postpro**: `unfold` then `map`<br/>
**para**: `fold` with cursor<br/>
**zygohistoprepro**: dumb meme<br/>
**elgot algebra**: `unfold` then `fold` where `unfold` part can short-circuit<br/>
**elgot coalgebra**: `unfold` then `fold` with cursor, where cursor tail is mapped by elgot coalgebra<br/>

**benefits of FP**:<br/>
Most programming tasks are reduced to variants of `plus`.

**or even**:<br/>
Most programming tasks can be summarized as "do a bunch of stuff".<br/>
FP separates the `bunch` and the `do` parts.<br/>
Use any of the available `plus`es to bunch your program together, then write an interpreter for it and just `do` it.<br/>
And since we can have multiple interpreters, we also gain the ability to "do a bunch of different stuff"
