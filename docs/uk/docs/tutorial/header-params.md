# Заголовки

Можливо визначити заголовки у той самий спосіб, що й параметри `Query`, `Path` та `Cookie`.

## Імпортуйте `Header`

Спочатку треба імтортувати модуль `Header`:

=== "Python 3.6 та вище"

    ```Python hl_lines="3"
    {!> ../../../docs_src/header_params/tutorial001.py!}
    ```

=== "Python 3.10 та вище"

    ```Python hl_lines="1"
    {!> ../../../docs_src/header_params/tutorial001_py310.py!}
    ```

## Об'явіть `Header`

Далі необхідно об'явити заголовки. Для цього використовується така сама структура, що й для `Path`, `Query` and `Cookie`.

Перший аргумент це значення за замовчуванням. Також можливо вказати додаткові валідації чи аннотації:

=== "Python 3.6 та вище"

    ```Python hl_lines="9"
    {!> ../../../docs_src/header_params/tutorial001.py!}
    ```

=== "Python 3.10 та вище"

    ```Python hl_lines="7"
    {!> ../../../docs_src/header_params/tutorial001_py310.py!}
    ```

!!! note "Технічні деталі"
    `Header` це споріднений до `Path`, `Query` та `Cookie` класс. Він наслідує спільний класс `Param`.

    Але пам'ятайте що коли ви імпортуєте `Query`, `Path`, `Header` та інші типи з `fastapi`, це насправді фабричні функції, що повертають конкретні класи.

!!! info "Інформація"
    Щоб об'явити заголовки необхідно використовувати `Header`, інакше параметри будуть інтерпретовані як параметри запиту (query parameters).

## Автоматична конвертація

`Header` has a little extra functionality on top of what `Path`, `Query` and `Cookie` provide.

Most of the standard headers are separated by a "hyphen" character, also known as the "minus symbol" (`-`).

But a variable like `user-agent` is invalid in Python.

So, by default, `Header` will convert the parameter names characters from underscore (`_`) to hyphen (`-`) to extract and document the headers.

Also, HTTP headers are case-insensitive, so, you can declare them with standard Python style (also known as "snake_case").

So, you can use `user_agent` as you normally would in Python code, instead of needing to capitalize the first letters as `User_Agent` or something similar.

If for some reason you need to disable automatic conversion of underscores to hyphens, set the parameter `convert_underscores` of `Header` to `False`:

=== "Python 3.6 та вище"

    ```Python hl_lines="10"
    {!> ../../../docs_src/header_params/tutorial002.py!}
    ```

=== "Python 3.10 та вище"

    ```Python hl_lines="8"
    {!> ../../../docs_src/header_params/tutorial002_py310.py!}
    ```

!!! warning
    Before setting `convert_underscores` to `False`, bear in mind that some HTTP proxies and servers disallow the usage of headers with underscores.

## Дублюючі заголовки

It is possible to receive duplicate headers. That means, the same header with multiple values.

You can define those cases using a list in the type declaration.

You will receive all the values from the duplicate header as a Python `list`.

For example, to declare a header of `X-Token` that can appear more than once, you can write:

=== "Python 3.6 та вище"

    ```Python hl_lines="9"
    {!> ../../../docs_src/header_params/tutorial003.py!}
    ```

=== "Python 3.9 та вище"

    ```Python hl_lines="9"
    {!> ../../../docs_src/header_params/tutorial003_py39.py!}
    ```

=== "Python 3.10 та вище"

    ```Python hl_lines="7"
    {!> ../../../docs_src/header_params/tutorial003_py310.py!}
    ```

If you communicate with that *path operation* sending two HTTP headers like:

```
X-Token: foo
X-Token: bar
```

The response would be like:

```JSON
{
    "X-Token values": [
        "bar",
        "foo"
    ]
}
```

## Підсумок

Declare headers with `Header`, using the same common pattern as `Query`, `Path` and `Cookie`.

And don't worry about underscores in your variables, **FastAPI** will take care of converting them.
