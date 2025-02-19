# 自学响应式编程(一)


{{< music auto="https://music.163.com/#/playlist?id=60198" >}}

# 功能测试文章

<img src="/images/lighthouse.jpg" alt="替代文本" width="600" height="200">

{{< image src="/images/lighthouse.jpg" caption="Lighthouse (`image`)" src_s="/images/lighthouse-small.jpg" src_l="/images/lighthouse-large.jpg" >}}



{{< mermaid >}}
graph LR;
    A[开始] -->|连接| B(Round edge)
    B --> C{第一个还是第二个}
    C -->|One| D[Result one]
    C -->|Two| E[Result two]
{{< /mermaid >}}

{{< mermaid >}}
journey
    title My working day
    section Go to work
      Make tea: 5: Me
      Go upstairs: 3: Me
      Do work: 1: Me, Cat
    section Go home
      Go downstairs: 5: Me
      Sit down: 5: Me
{{< /mermaid >}}

```java
images: []
tags: []
categories: []
```

```yaml
featuredImage: ""
featuredImagePreview: ""
```

- **tags**: 文章的标签.

- **categories**: 文章所属的类别.

  {{< music url="/music/123.mp3">}}

  

```yaml
lightgallery: true
```

$$ c = \pm\sqrt{a^2 + b^2} $$

\\[ f(x)=\int_{-\infty}^{\infty} \hat{f}(\xi) e^{2 \pi i \xi x} d \xi \\]

\begin{equation*}
  \rho \frac{\mathrm{D} \mathbf{v}}{\mathrm{D} t}=\nabla \cdot \mathbb{P}+\rho \mathbf{f}
\end{equation*}

\begin{equation}
  \mathbf{E}=\sum_{i} \mathbf{E}\_{i}=\mathbf{E}\_{1}+\mathbf{E}\_{2}+\mathbf{E}_{3}+\cdots
\end{equation}

\begin{align}
  a&=b+c \\\\
  d+e&=f
\end{align}

\begin{alignat}{2}
   10&x+&3&y = 2 \\\\
   3&x+&13&y = 4
\end{alignat}

\begin{gather}
   a=b \\\\
   e=b+c
\end{gather}

\begin{CD}
   A @>a\>> B \\\\
@VbVV @AAcA \\\\
   C @= D
\end{CD}

{{< admonition type=tip title="This is a tip" open=false >}} 一个 **技巧** 横幅 {{< /admonition >}} 或者 {{< admonition tip "This is a tip" false >}} 一个 **技巧** 横幅 {{< /admonition >}}

```markdown
{{< bilibili BV1Sx411T7QQ >}}
```

{{< bilibili BV1xz4y1A7MU >}}

{{< echarts >}}
{
  "title": {
    "text": "折线统计图",
    "top": "2%",
    "left": "center"
  },
  "tooltip": {
    "trigger": "axis"
  },
  "legend": {
    "data": ["测试1", "测试2", "测试3", "测试4", "测试5"],
    "top": "10%"
  },
  "grid": {
    "left": "5%",
    "right": "5%",
    "bottom": "5%",
    "top": "20%",
    "containLabel": true
  },
  "toolbox": {
    "feature": {
      "saveAsImage": {
        "title": "保存为图片"
      }
    }
  },
  "xAxis": {
    "type": "category",
    "boundaryGap": false,
    "data": ["周一", "周二", "周三", "周四", "周五", "周六", "周日"]
  },
  "yAxis": {
    "type": "value"
  },
  "series": [
    {
      "name": "测试1",
      "type": "line",
      "stack": "总量",
      "data": [120, 132, 101, 134, 90, 230, 210]
    },
    {
      "name": "测试2",
      "type": "line",
      "stack": "总量",
      "data": [220, 182, 191, 234, 290, 330, 310]
    },
    {
      "name": "测试3",
      "type": "line",
      "stack": "总量",
      "data": [150, 232, 201, 154, 190, 330, 410]
    },
    {
      "name": "测试4",
      "type": "line",
      "stack": "总量",
      "data": [320, 332, 301, 334, 390, 330, 320]
    },
    {
      "name": "测试5",
      "type": "line",
      "stack": "总量",
      "data": [820, 932, 901, 934, 1290, 1330, 1320]
    }
  ]
}
{{< /echarts >}}
