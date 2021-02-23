---
title: PlantUML tips and tricks
excerpt: Tips and tricks for PlantUML
tags: 
  - plantuml
categories: Tips
---

I love [PlantUML]. It has it's limitations but if the size and the complexity of the diagrams can be contained, then it offers great benefits. The fact that the format is in text is so powerful in software engineering because it can be attached to any process with change management over source control. Also, it is great for [Confluence] when the [PlantUML Diagrams for Confluence] add-on is enabled.

With this post I want to share some tips and tricks from my experience using it.

# Layout orientation in component diagram
When using the component diagram, often the layout seems to have a mind of it's own. You can try to provide some guidelines to the rendering component by adding directions in the arrows (e.g. `-l->`) but when that doesn't work, I try to achieve an optimal layout by reorganizing the appearance of the components, especially when they are nested with e.g. `package`. It is a bit strange, but order in rendering seems to follow a reverse order of appearance in the script.

# Multiple and options flows in a component diagram
Sometimes, I want to have multiple flows within a component diagram. In this case and depending on the case, I use color coding with each arrow (e.g. `A -> B #Green : text` and possibly add a legend at the end

```
@startuml

legend
|= Color |= Flow |
|<back:#Green>   </back>| Flow 1 |
|<back:#Red>   </back>| Flow 2 |

endlegend

@enduml
```

Note that the spaces inside the `<back></back>` element are intentional and each space represent one vertical line and 3 represent almost a square.

When sequence is important then I use a numbering scheme like this

- Flow 1
  - `1.1 action`
  - `1.2 action`
- Flow 2
  - `2.1 action`
  - `2.2 action`

# Autonumber events in sequence diagram

One of the best features of sequence diagrams is the `autonumber` feature which will add a number before the text of each event as explained in the documentation

```
@startuml

autonumber
Bob -> Alice : Authentication Request
Bob <- Alice : Authentication Response

@enduml
```
!()[https://s.plantuml.com/imgw/img-319897966ce6fa63c1dfcd7844917b20.webp]

I prefer the `autonumber "<B>[00]"` the most. By default, you can use the command as many times as you want to restart the numbering in the sequence.

```
@startuml

autonumber "<B>[00]"
Bob -> Alice : Authentication Request
Bob <- Alice : Authentication Response

@enduml
```

# Text alignment in sequence diagram

In the above example, the lines can be brought closer together to make the diagram more dense with this skin parameter `responseMessageBelowArrow true`.

# Semi transparent group backgrounds in sequence diagrams

I like to group my participants in boxes and color code them because when the sequence is long, you can quickly identify the scope of the vertical you are looking at even when the participants are out of view. Problem is that every one of the `Grouping message` features (`group`,`alt/else`,`opt`,`loop`,`par`,`break`,`critical` and `group`) are rendered by default with a white solid background which hides the color of the box. This is annoying when the group overlaps many verticals and becomes one massive big white background. A solution for this is not easy. You could use the `#Transparent` but then the entire group's background becomes transparent and when big the context is lost.

This was not easy to find, but [PlantUML] skinning supports alpha channel and the background can be set through the skin. [Skin Parameters] are not  documented extensively and I was able to find the proper one `SequenceGroupBodyBackgroundColor` through this [page][All Skin Parameters]. After some experimentation, I've decided to use white with some tranparency using this value `#FFFFFF90`.


```
@startuml
TODO:

Example

@enduml
```

# Visualize all possible colors

[PlantUML] supports many different colors by name but it is kind of a maze. Use the `colors` command to render a picture with all colors

```
@startuml

colors

@enduml
```


[PlantUML]: https://plantuml.com/
[Confluence]: https://www.atlassian.com/software/confluence
[PlantUML Diagrams for Confluence]: https://marketplace.atlassian.com/apps/1215115/plantuml-diagrams-for-confluence?hosting=cloud&tab=overview
[Skin Parameters]: https://plantuml.com/skinparam
[All Skin Parameters]: https://plantuml-documentation.readthedocs.io/en/latest/formatting/all-skin-params.html