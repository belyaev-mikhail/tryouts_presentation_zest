---
lang: ru

theme: metropolis


title: 'Тестирование с учётом покрытия для Котоеда'
author: Михаил Беляев
date: '\today'

csquotes: true
header-includes:
- |
  ```{=latex}
  \usepackage{csquotes}

  \newfontfamily\cyrillicfont{Fira Sans}
  \newfontfamily\cyrillicfonttt{Fira Mono}
  
  \setsansfont[BoldFont={Fira Sans Bold}, Scale=1.05]{Fira Sans}
  \setmonofont[Scale=0.95]{Fira Mono}

  \metroset{outer/numbering=fraction}
  \metroset{background=dark}
  \metroset{progressbar=frametitle}

  \definecolor{eigengrau}{rgb}{0.08627, 0.08627, 0.1137}
  \definecolor{gold}{rgb}{0.6509, 0.4863, 0.0001}
  
  \setbeamercolor{normal text}{bg=eigengrau,fg=white}
  \setbeamercolor{title separator}{fg=red}
  \setbeamercolor{frametitle}{bg=eigengrau,fg=gold}
  \setbeamercolor{progress bar}{bg=white,fg=red}
  ```

---

# О чём вообще речь

- Kotoed (существуетъ)
- Случайные тесты (гоняютъся)

# Подробнее

Случайное тестирование в котоеде реализовано в виде библиотеки `kcheck` и набора тестов

Примерно таких:

```kotlin
forAll { q1: Pair<@InRange(1, 8) Int, @InRange(1, 8) Int>,
          q2: Pair<@InRange(1, 8) Int, @InRange(1, 8) Int> ->
    val (x1, y1) = q1
    val (x2, y2) = q2

    expectEquals(
        ModelImplementation.queenThreatens(x1, y1, x2, y2),
        "x1" to x1, "y1" to y1,
        "x2" to x2, "y2" to y2
    )
    { queenThreatens(x1, y1, x2, y2) }
}
```

# Суть проекта

- `kcheck` использует для генерации данных достаточно простой подход на основе случайного поиска
- Есть в мире более продвинутые техники
  - Символьное исполнение
  - **Направленный поиск с помощью покрытия**

# Суть проекта

- Проект Zest реализует случайное тестирование с помощью покрытия на основе Jacoco
- Мы хотим попробовать то же самое, но для котоеда
  - Можно взять Zest и перегнать под него тесты
  - Можно разобраться в том, как он работает и модифицировать `kcheck`
