#diagramas #ejemplos #obsidian
Ejemplos para construir diferentes diagramas:
[Documentación externa](https://support.typora.io/Draw-Diagrams-With-Markdown/)
  para formato #SQL:
``` sql
SELECT * FROM ESQUEMA.TABLA;
```






```mermaid
classDiagram
      Animal <|-- Duck
      Animal <|-- Fish
      Animal <|-- Zebra
      Animal : +int age
      Animal : +String gender
      Animal: +isMammal()
      Animal: +mate()
      class Duck{
          +String beakColor
          +swim()
          +quack()
      }
```

## Pie Charts

```mermaid
pie
    title Pie Chart
    "Dogs" : 386
    "Cats" : 85
    "Rats" : 150 
```


### An example of a timeline[​](https://mermaid.js.org/syntax/timeline.html#an-example-of-a-timeline)

##### Code:

```mermaid
timeline
    title History of Social Media Platform
    2002 : LinkedIn
    2004 : Facebook
         : Google
    2005 : Youtube
    2006 : Twitter

```

title: Animal example
---
```mermaid
classDiagram
    note "From Duck till Zebra"
    Animal <|-- Duck
    note for Duck "can fly\ncan swim\ncan dive\ncan help in debugging"
    Animal <|-- Fish
    Animal <|-- Zebra
    Animal : +int age
    Animal : +String gender
    Animal: +isMammal()
    Animal: +mate()
    class Duck{
        +String beakColor
        +swim()
        +quack()
    }
    class Fish{
        -int sizeInFeet
        -canEat()
    }
    class Zebra{
        +bool is_wild
        +run()
    }

```


```mermaid
xychart-beta
    title "Sales Revenue"
    x-axis [jan, feb, mar, apr, may, jun, jul, aug, sep, oct, nov, dec]
    y-axis "Revenue (in $)" 4000 --> 11000
    bar [5000, 6000, 7500, 8200, 9500, 10500, 11000, 10200, 9200, 8500, 7000, 6000]
    line [5000, 6000, 7500, 8200, 9500, 10500, 11000, 10200, 9200, 8500, 7000, 6000]
    %%{init: { "themeVariables": {"xyChart": {"backgroundColor": "#000000","yAxisTickColor": "#000000" , "fontColor" : "red"} , "fontSize": "14px", "fontFamily": "Arial", "textColor": "#ff0000"} , "xyChart": {"width":"700"} }}%%
```


```mermaid
%%{init: {"themeVariables": {"fontSize": "14px", "fontFamily": "Arial", "textColor": "#ff0000"}}}%%
xychart
  x: "Category"
  y: "Value"
  series:
    - x: "A"
      y: 10
    - x: "B"
      y: 20
    - x: "C"
      y: 15
```
