---
description: What the OPcache optimiser does to your opcodes in PHP 8, how to control and inspect it, and what it means for Docket Cache files.
---

# OPcache Optimisations

OPcache does two jobs. The first, covered in the OPcache Extension article, is keeping compiled scripts in shared memory so PHP does not compile them again on every request. The second is the subject of this article: before a script is stored, OPcache rewrites its opcodes into a shorter, cheaper form. That rewriting step is the optimiser.

Because the optimiser runs once per file, at the moment the file is cached, its cost is paid a single time and every later request benefits from the result.

## Where the optimiser sits

A PHP file goes through these stages the first time it is requested:

| Stage | Done by | Result |
| --- | --- | --- |
| Parse | PHP compiler | Abstract syntax tree (AST) |
| Compile | PHP compiler | Opcodes for the file and each function in it |
| Optimise | OPcache | Fewer, simpler opcodes |
| Store | OPcache | Opcodes in shared memory |
| Execute | Zend VM, optionally the JIT | Output |

Two points follow from this layout.

First, the compiler already performs the simplest optimisations itself. Since PHP 7 it works on an AST and folds constant expressions as it goes, so `4 * 25` or `strlen('docket')` become literal values even with OPcache switched off. The optimiser picks up where the compiler stops.

Second, the optimiser sees one file at a time. It has no knowledge of what other files define, so a function, class or constant declared elsewhere is an unknown to it. It also runs before execution, which means the value of an ordinary variable such as a function argument is unknown too. Everything it does must be provably safe with only the information in the file in front of it.

## What it does

### Constant folding and propagation

The optimiser tracks values it can prove are constant and substitutes them wherever they are used. A local variable that is assigned a literal and never changed in an unpredictable way is treated as that literal, and expressions built from such values are evaluated ahead of time.

It also evaluates some internal function calls whose answer cannot change during the lifetime of the process. `function_exists()` on a built-in function and `extension_loaded()` are typical examples. The same call against a user-defined function is left alone, because that function may or may not have been loaded by another file.

### Dead code elimination

Once a condition is known to be always true or always false, the branch that can never run is removed, together with the test itself. Assignments to variables that are never read are dropped, and so is code that follows an unconditional `return`.

### Jump and control flow clean-up

Conditions, loops and `goto` compile to jump opcodes, and naive compilation often produces a jump whose target is another jump. The optimiser builds a control flow graph of each function, redirects such chains straight to their final target, inverts conditional jumps where that saves an instruction, and merges or removes blocks that have become empty.

### Data flow and type inference

From PHP 7.1 the optimiser converts each function into static single assignment (SSA) form and runs data-flow analysis over it. This lets it infer the possible types and value ranges of variables and temporaries. Later 7.x releases built two important passes on that foundation: sparse conditional constant propagation (SCCP), which combines constant propagation with branch analysis, and an SSA-based dead code elimination pass. Type information is also used to select specialised opcode handlers, for example an integer-only addition, and to drop type checks that are proven redundant.

### Function calls and inlining

Calling a function by name normally involves a lookup at runtime. When the target is known at compile time, either an internal function or one declared in the same file, the optimiser replaces the generic call sequence with a direct one. It also analyses the call graph of the file, so that what it learns about a function's return type can be used at its call sites.

Inlining in PHP is deliberately conservative. A function declared in the same file whose body does nothing but return a constant value can have its call replaced by that value. Do not expect general-purpose inlining of larger functions.

### Housekeeping

The remaining passes tidy up after the others: temporary variables are reused where their lifetimes do not overlap, unused variable slots are removed, NOP opcodes left behind by earlier passes are stripped out, identical literals are merged into one entry, and the stack size reserved for each call is adjusted.

## A worked example

The following file is small enough to follow by hand:

```php
<?php
function report($debug)
{
    if ($debug) {
        goto done;
    } else {
        echo "quiet\n";
    }

    done:
    echo "done\n";

    $count = 1;
    $count++;

    if (function_exists('array_merge') && extension_loaded('json')) {
        echo "ready\n";
    }

    define('APP_MODE', 'live');

    return $count + "42";
}
```

In our test on PHP 8.0 with the default optimisation level, the compiler produced 25 opcodes for `report()`. After the optimiser had run, 7 remained. This is what happened to each part:

| Source | After optimisation |
| --- | --- |
| `if ($debug) { goto done; } else { ... }` | The conditional jump followed by two plain jumps becomes a single inverted conditional jump over the `echo`. |
| `function_exists('array_merge') && extension_loaded('json')` | Both calls are evaluated at optimisation time. The condition is always true, so the test disappears and only the `echo` is kept. |
| `define('APP_MODE', 'live')` | The function call is replaced by the cheaper opcode that the `const` statement uses. |
| `$count = 1; $count++; return $count + "42";` | The value is tracked through all three statements. The function simply returns the integer `44`, and `$count` no longer exists. |
| Implicit `return null` at the end | Removed as unreachable. |

The only thing left to decide at runtime is the one value the optimiser could not know: `$debug`.

## Controlling the optimiser

The passes are selected by the `opcache.optimization_level` bitmask. Pass *n* is bit *n − 1*.

| Pass | Bit | Purpose |
| --- | --- | --- |
| 1 | `0x0001` | Simple local optimisations: constant substitution, expression evaluation, constant function calls |
| 3 | `0x0004` | Jump optimisation |
| 4 | `0x0008` | Function call optimisation |
| 5 | `0x0010` | Control flow graph (CFG) based optimisation |
| 6 | `0x0020` | Data-flow (DFA/SSA) based optimisation |
| 7 | `0x0040` | Call graph optimisation |
| 8 | `0x0080` | SCCP constant propagation |
| 9 | `0x0100` | Temporary variable reuse |
| 10 | `0x0200` | NOP removal |
| 11 | `0x0400` | Merge equal constants |
| 12 | `0x0800` | Adjust used stack |
| 13 | `0x1000` | Remove unused variables |
| 14 | `0x2000` | Dead code elimination |
| 15 | `0x4000` | Collect constants (unsafe, off by default) |
| 16 | `0x8000` | Inline functions |

The default is:

```ini
opcache.optimization_level=0x7FFEBFFF
```

That value, unchanged since PHP 7.3, enables every pass considered safe. The two cleared bits are pass 15 and a further flag (`0x10000`) that tells the optimiser to ignore the possibility of operator overloading. Both are marked unsafe in the PHP source because they can change the behaviour of valid code.

Our advice is to leave the default alone. Setting the level to `0` is useful only for ruling the optimiser out while debugging, and switching on the unsafe bits trades correctness for a gain you are unlikely to measure.

## Seeing it for yourself

`opcache.opt_debug_level`, available since PHP 7.1, prints the opcodes to standard error. `0x10000` shows them as the compiler produced them and `0x20000` shows them after optimisation:

```shell
php -d opcache.enable_cli=1 -d opcache.file_update_protection=0 -d opcache.opt_debug_level=0x20000 report.php
```

{% hint style="info" %}
By default `opcache.file_update_protection` stops OPcache from caching a file modified less than two seconds ago. A file you have just saved is therefore neither cached nor optimised, and nothing is printed. Setting it to `0` for the test run avoids that.
{% endhint %}

## How the JIT relates

The JIT compiler, added in PHP 8.0, is part of OPcache but is not another optimiser pass. The optimiser turns opcodes into better opcodes, which the Zend VM then interprets. The JIT goes a step further and translates opcodes that are executed often into native machine code, stored in a separate buffer sized by `opcache.jit_buffer_size`.

The two are connected: the JIT reuses the SSA form and type inference described above to decide what machine code it can safely generate. The optimiser, however, is always active when OPcache is enabled, whereas the JIT is opt-in. As of PHP 8.4 `opcache.jit` defaults to `disable`; in PHP 8.0 to 8.3 the JIT stayed off because the buffer size defaulted to `0`.

The JIT pays off for CPU-bound code such as tight loops and arithmetic. A typical WordPress request spends its time in database queries, file access and string handling, where there is little for the JIT to gain.

## What this means for Docket Cache

Docket Cache stores each object cache entry as a plain PHP file that returns an array. When the entry holds only scalars and arrays, the file looks like this in essence:

```php
<?php
return array(
    'group' => 'options',
    'key'   => 'posts_per_page',
    'type'  => 'integer',
    'data'  => 10,
);
```

Here the heavy lifting is done before the optimiser is involved. An array made up entirely of literals is built by the compiler as a single constant, so the whole file compiles to one opcode: return that array. OPcache stores the array in shared memory, and reading the entry on later requests means executing that one opcode with no parsing and no rebuilding of the data. Docket Cache also asks OPcache to compile a cache file as soon as it has been written.

The optimiser has almost nothing to add. It removes the unreachable implicit `return` that the compiler appends to every file, and that is all. There are no branches to prune, no calls to resolve and no variables to analyse. For the same reason the JIT brings no benefit to cache files: a single opcode is not a hot loop.

Entries that contain objects are different. Those are exported as PHP code that recreates the object, so the file has a handful of opcodes to execute on each read. They are still served from shared memory without recompilation.

In practice:

* The speed of Docket Cache comes from OPcache's shared memory storage, not from the optimiser or the JIT. You do not need to tune `opcache.optimization_level` or enable the JIT for it.
* The optimiser still earns its keep on the rest of the request: WordPress core, plugins and themes are ordinary PHP code and get every optimisation described above.
* What deserves your attention is that OPcache has enough memory and key slots for your cache files. Those settings are covered in the OPcache Extension article.
