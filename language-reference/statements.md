# Statements \(文\)

最終更新日:

Swift では、単純な文、コンパイラ制御文、および制御フロー文の 3 種類の文があります。単純な文は最も一般的で、式または宣言で構成されています。コンパイラ制御文により、プログラムはコンパイラの動作の側面を変更し、条件付きコンパイルブロックと行制御文を含めます。

制御フロー文は、プログラム内の実行の流れを制御するために使用されます。ループ文、分岐文、および制御転送文を含む、Swift にはいくつかの種類の制御フロー文があります。ループ文はコードブロックを繰り返し実行できるようにし、分岐文は特定の条件が満たされたときにのみ特定のコードブロックを実行できるようにし、制御転送文はコードが実行される順序を変更する方法を提供します。さらに、Swift は、スコープを導入し、エラーをキャッチして処理するための `do` 文、および現在のスコープが終了する直前のクリーンアップアクションを実行するための `defer` 文を提供します。

セミコロン(`;`)は任意の文の後に書くことができ、同じ行に複数の文が書かれている場合に、それらを分離するために使用されます。

> GRAMMAR OF A STATEMENT  
> statement → [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression)  `;`<sub>*opt*</sub>  
> statement → [declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_declaration)  `;`<sub>*opt*</sub>  
> statement → [loop-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar_loop-statement)  `;`<sub>*opt*</sub>  
> statement → [branch-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar_branch-statement)  `;`<sub>*opt*</sub>  
> statement → [labeled-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar_labeled-statement)  `;`<sub>*opt*</sub>  
> statement → [control-transfer-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar_control-transfer-statement)  `;`<sub>*opt*</sub>  
> statement → [defer-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar_defer-statement)  `;`<sub>*opt*</sub>  
> statement → [do-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar_do-statement)  `;`<sub>*opt*</sub>  
> statement → [compiler-control-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar_compiler-control-statement)  
> statements → [statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar_statement)  [statements](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar_statements)<sub>*opt*</sub>

## Loop Statements(ループ文)

ループ文では、ループで指定された条件に応じて、コードブロックを繰り返し実行できます。Swift には 3 つのループ文があります。`for-in` 文、`while` 文、および `repeat-while` 文です。

ループ文内の制御フローは、`break` 文と `continue` 文によって変更でき、下記の [Break Statement](#break-statementbreak文) 文と条件文[Continue Statement](#continue-statementcontinue文)で説明します。

> GRAMMAR OF A LOOP STATEMENT  
> loop-statement → [for-in-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar_for-in-statement)  
> loop-statement → [while-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar_while-statement)  
> loop-statement → [repeat-while-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar_repeat-while-statement)

### For-In Statement(For-In文)

`for-in` 文は、[Sequence](https://developer.apple.com/documentation/swift/sequence)に準拠したコレクション(または任意の型)内の各項目に対して 1 回のコードブロックを実行できます。

`for-in` 文は次の形式です。

![For-In文](./../.gitbook/assets/for-in_statement.png)

`makeIterator()` メソッドは、[IteratorProtocol](https://developer.apple.com/documentation/swift/iteratorprotocol)プロトコルに準拠した型のイテレータ型の値を取得するために、collection 式で呼び出されます。プログラムは、イテレータ上で `next()` メソッドを呼び出すことによってループの実行を開始します。返された値が `nil` ではない場合、item に代入され、プログラムは statements を実行し、再びループの先頭から実行を続行します。それ以外の場合、プログラムは値の代入や、statements を実行せず、 `for-in` 文の実行は終了しています。

> GRAMMAR OF A FOR-IN STATEMENT  
> for-in-statement → `for` `case` $$_{opt}$$ [pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar_pattern)  `in` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression)  [where-clause](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar_where-clause) $$_{opt}$$ [code-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_code-block)

### While Statement(While文)

### Repeat-While Statement(Repeat-While文)

## Branch Statements(分岐文)

### If Statement(If文)

### Guard Statement(Guard文)

### Switch Statement(Switch文)

#### Switch Statements Must Be Exhaustive(Switch文は網羅的でなければならない)

---

#### Switching Over Future Enumeration Cases(未来の列挙型ケースへの切り替え)

---

#### Execution Does Not Fall Through Cases Implicitly(暗黙的にFallthroughしない)

---

## Labeled Statement(ラベルあり文)

## Control Transfer Statements(制御転送文)

### Break Statement(Break文)

### Continue Statement(Continue文)

### Fallthrough Statement(Fallthrough文)

### Return Statement(Return文)

### Throw Statement(Throw文)

## Defer Statement(Defer文)

## Do Statement(Do文)

## Compiler Control Statements(コンパイラ制御文)

### Conditional Compilation Block(条件付きコンパイルブロック)

### Line Control Statement(行制御文)

### Compile-Time Diagnostic Statement(コンパイル時診断文)

## Availability Condition(Availability条件)
