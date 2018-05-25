All the category theory used in programming in one page
------

**functor**: given `Producer[A]` with **output** `A`, allows post-processing values with `A => B` to get `Producer[B]`<br/>
**contravariant functor**: given `Consumer[A]` with **input** `A`, allows pre-processing values with `B => A` to get `Consumer[B]`<br/>
**profunctor**: given `Pipe[A, B]` with **input** `A` and **output** `B`, allow pre-processing **input** values with `C => A`, and post-processing **output** values with `B => D` to get `Pipe[C, D]`<br/>
**applicative**: gives `map2`, `map3` **...** `mapN`. where `map2: (F[A], F[B]), (A, B => C) => F[C]` **...** `mapN: (F[A] .. F[N]), (A .. N => Z) => F[Z]`<br/>
**monad**: gives `flatten: F[F[_]] => F[_]`. If you `map`, then `flatten`, you can chain operations: `flatten(maybeReadFile.map(maybeParseAsInteger)) = Maybe[Integer]`<br/>
**monoid**: gives `plus` and `zero`<br/>
**alternative**: gives `orElse: F[_], F[_] => F[_]`<br/>
**category**: function-like entity with `compose` and `identity`<br/>
**arrow**: category with `fork`/`join`<br/>
**comonad**: given `Cursor[A]` with **output** `A`, allows post-processing values under all nested cursors with `Cursor[A] => B` to get `Cursor[B]`. i.e.
  ```scala
  List(1,1,2).extend(sumOfList) = List(4, 3, 2)
  // list's cursor = head + tail
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
**yoneda**: *colloq.* free contravariant functor<br/>
**coyoneda**: *colloq.* free functor<br/>
**day convolution**: *colloq.* free map2<br/>

**tensor**: binder of multiple arguments. default tensor is tuple: `(A, B) => C`

**on monoids**: `applicative`, `alternative`, `monad` and `category` are all specialized **monoids** with different tensors<br/>
&nbsp;&nbsp;&nbsp;&nbsp;**intuition**:
  * **monoid** tensor: `tuple`
    ```scala
     plus: (A, A) => A
     // if i can combine tuples of A then i can combine a tupleN of A!
     // i can squish any List[A] into one A!
    ```
  * **monad** tensor: `compose`: `compose F G = F[G[_]]`
    ```scala
     plus: F compose F => F  // flatten: F[F[_]] => F[_]
    // if i can combine composes then i can combine a composeN!
    // i can flatten any F[F[F[F[F[F[F... into one F!
    ```

  * **alternative** tensor: `higherTuple`: `higherTuple F G = (F[_], G[_])`
    ```scala
     plus: F higherTuple F => F  // orElse: F[_], F[_] => F[_]
     // if i can combine higherTuples then i can combine a higherTupleN!
     // i can choose from any paths (F orElse F orElse F orElse...) a one successful F!
    ```

  * **applicative** tensor: `free map2`: `map2 F G = (F[a], G[b], (a, b) => c)`
    ```scala
     plus: F map2 F => F
     // if i can combine map2s of F then i can combine a mapN of F!
     // i can zip any (F[A], F[B] ... F[N]) with an (A ... N => Z) to get one F[Z]!
    ```

**cata**: `fold`<br/>
**ana**: `unfold`<br/>
**hylo**: `unfold` then `fold`<br/>
**meta**: `fold` then `unfold`<br/>
**prepro**: `map` then `fold`<br/>
**postpro**: `unfold` then `map`<br/>
**zygohistoprepro**: dumb meme<br/>
**elgot algebra**: `fold` with short-circuit<br/>
**elgot coalgebra**: `unfold` with short-circuit<br/>
**f-algebra**: `F[A] => A`, fold-like function<br/>
**f-coalgebra**: `A => F[A]`, unfold-like function<br/>
**recursion**: `fold`-like function calling itself with some context while `reducing` a context.<br/>
**corecursion**: `unfold`-like function calling itself with some context while `building up` a context<br/>

**benefit of FP**:<br/>
&nbsp;&nbsp;&nbsp;&nbsp;most programming tasks are reduced to variants of `plus`, lowering mental overhead

