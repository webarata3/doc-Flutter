# highlight

``` markdown
markdown_extensions:
  - pymdownx.highlight:
      linenums: true
```

``` java
public class Test {
    public static void main(String[] args) {
        System.out.println("テスト");
    }
}
```

``` python hl_lines="2 3"
def bubble_sort(items):
    for i in range(len(items)):
        for j in range(len(items) - 1 - i):
            if items[j] > items[j + 1]:
                items[j], items[j + 1] = items[j + 1], items[j]
```

# Key

``` markdown
markdown_extensions:
  - pymdownx.keys
```

++ctrl+alt+del++

++ctrl+C++

# admonition

``` markdown
markdown_extensions:
  - admonition
```

!!! note
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.

!!! note "タイトル帰るよ"
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.

!!! abstract
    test

!!! info
    test

!!! tips
    test

!!! success
    test

!!! question
    test

!!! warning
    test

!!! failure
    test

!!! danger
    test

!!! bug
    test

!!! example
    test

!!! quote
    test

# 定義リスト

``` markdown
markdown_extensions:
  - def_list
```

定義語
:    ここに説明を書きます
