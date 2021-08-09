# 마크다운?

### 개요

- 2004년 존 그루버가 만든 텍스트 기반의 가벼운 마크업 언어

### 특징

- 워드프로세서(한글/MS word)는 다양한 서식과 구조를 지원하며, 문서에 즉각적으로 반영
- 마크다운은 가능한 읽을 수 있도록 최소한의 문법으로 구조화 (make it as readable as possible)
- 마크다운은 단순 텍스트 문법으로 내용을 작성하며, 다양한 환경에서 변환하여 보여짐

### 활용 예

- Github 등의 사이트에서는 파일명이 README.md인 것을 모두 보여줌
  - 오픈소스의 공식 문서를 작성하거나 개인 프로젝트의 프로젝트 소개서로 활용
- 다양한 기술블로그에서는 정적사이트생성기(Static site generator)
  - Jekyll, Gatsby, Hugo, Hexo 등을 통해 작성된 마크다운을 HTML, CSS, JS 파일 등으로 변환하고 Github pages 기능을 통해 호스팅 (외부 공개)
-  다양한 개발 환경 뿐만 아니라 일반 SW에서도 많이 사용되고 있음
  - Jupyter notebook, Notion

# Markdown Syntax

## Basic Syntax

### Headings

#을 개수에 따라  h1~h6까지 표현이 가능하다.

| 예                 |
| ------------------ |
| # Here's a Heading |

### Line Breaks

\<br>을 사용해서 문장을 구분할 수 있다. (Enter)

| 예                                                        |
| --------------------------------------------------------- |
| First line with the HTML tag after.<br>And the next line. |

### List

순서가 있는 리스트 1.또는 순서가 없는 리스트 -(dashes)로 구성되어있다.

### Fenced Code block

backtick 기호 3개를 활용하여 작성(\``` ```)

코드 블록에 특정 언어를 명시하면 Syntax Highlighting 적용 가능

```python
print('Hello world!')
```



### Inline Code block

코드 블록은 기호 1개를 인라인에 활용하여 작성(``)

| `nano` |
| ------ |

### Link

\[문자열](url)을 통해 링크를 작성 가능

\!\[문자열](url)을 통해 이미지를 사용 가능

### Blockquotes(인용문)

\>를 통해 인용문 작성

> Blockquotes

### text 강조

** : 굵게(bold) , *:기울임(Italic)을 통해 특정 글자들을 강조

| Markdown                              | Output                               |
| ------------------------------------- | ------------------------------------ |
| I just love \**bold text**.           | I just love **bold text**.           |
| Italicized text is the \*cat's meow*. | Italicized text is the *cat’s meow*. |

### 수평선

3개 이상의 asterisks (***), dashes (---), or underscores (___)

| ***                               |
| --------------------------------- |
| ---                               |
| _________________________________ |

