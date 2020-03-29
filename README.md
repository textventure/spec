# Introduction

This document is the specification for [textventure](https://textventure.org).

# Format

Textventure is written in [YAML](http://yaml.org/).

# Schema

On a high-level, textventure consists of the properties **\<id>**, **\<text>**, and **\<choice>**:

```yaml
# text with no choice
<id>: !!str <text>

# text with choice
<id>: !!map
  <text>: !!seq
  - <choice>: !!str <id>
```

| Property  | Description                                                 |
| :-------- | :---------------------------------------------------------- |
| \<id>     | Unique string. Maps to \<text>.                             |
| \<text>   | String. If a collection, maps to a sequence of \<choice>'s. |
| \<choice> | String. Maps to \<id>.                                      |

There's also an optional `_config` property:

```yaml
_config:
  start: start
  renderer: text
```

| Key      | Value(s)                             | Description                                 |
| :------- | :----------------------------------- | :------------------------------------------ |
| start    | start (_default_)<br>\*              | String. The start \<id>.                    |
| renderer | text (_default_)<br>html<br>markdown | String. Renderer for \<text> and \<choice>. |

# Example

Given the flowchart:

<!--
https://gist.github.com/remarkablemark/30d3974972e6fc3348fe3c58136e5aaa
-->
<img src="https://textventure.org/static/2018/2018-08-12-textventure-spec-example-flowchart.svg" alt="Flowchart of a textventure" width="430px">

The textventure would be written as:

```yaml
start:
  You come to a fork in the road.:
    - Turn left.: left
    - Turn right.: right

left: You turn left.

right: You turn right.
```

To add choices to the `right` node:

```yaml
right:
  You turn right.:
    - Choice A: choice_a
    - Choice B: choice_b

choice_a: Text for A.

choice_b: Text for B.
```

To rewrite the textventure in [Markdown](https://commonmark.org/):

```yaml
_config:
  renderer: markdown

start:
  ? |- # keeps newlines except for the last one
    You come to a fork in the road.

    ![Image of a fork in the road](fork-in-the-road.jpg)
  : # html entities need to be wrapped in quotes
    - '&larr; Turn _left_.': left
    - '&rarr; Turn _right_.': right

left: You turn left.

right: You turn right.
```
