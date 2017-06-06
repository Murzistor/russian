# Строковые mixin'ы

Выражение `mixin` принимает заданную строку, компилирует её
и генерирует соответствующие инструкции. Это механизм,
работающий исключительно **во время компиляции**, и, соответственно,
работает только со строками, доступными во время компиляции.
Сравнение со зловещим `eval` из JavaScript здесь неуместно.

    mixin("int b = 5");
    assert(b == 5); // компилируется вполне
                    // обычно

`mixin` так же работает со строками, собранными динамически,
естественно, если они не зависят от значений, доступных
только во время исполнения.

`mixin` совместно с **CTFE** из следующего раздела
позволяют писать впечатляющие библиотеки, наподобие
[Pegged](https://github.com/PhilippeSigaud/Pegged),
генерирующей парсер грамматики, определённой в виде
строки в исходном коде.

### Подробнее

- [Mixins in D](https://dlang.org/spec/template-mixin.html)

## {SourceCode}

```d
import std.stdio : writeln;

auto calculate(string op, T)(T lhs, T rhs)
{
    return mixin("lhs " ~ op ~ " rhs");
}

void main()
{
    // Инноваторское решение для Hello World!
    mixin(`writeln("Hello World");`);

    // передать необходимый оператор как
    // параметр шаблона.
    writeln("5 + 12 = ", calculate!"+"(5,12));
    writeln("10 - 8 = ", calculate!"-"(10,8));
    writeln("8 * 8 = ", calculate!"*"(8,8));
    writeln("100 / 5 = ", calculate!"/"(100,5));
}
```