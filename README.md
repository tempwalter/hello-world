# hello-world
my first repository
<!-- toc -->

- [Sequence Diagram](#sequence-diagram)
  - [语法](#语法)
    - [Participants](#participants)
    - [Messages](#messages)
    - [Notes](#notes)
    - [Loop](#loop)
    - [Alt](#alt)
    - [Styling](#styling)
    - [Configuration](#configuration)
  - [Message to self In loop](#message-to-self-in-loop)
  - [Loops,alt and opt](#loopsalt-and-opt)
- [Basic sequence diagram](#basic-sequence-diagram)

<!-- tocstop -->

## Sequence Diagram

> A Sequence diagram is an interaction diagram that shows how processes operate with one another and in what order

### 语法
#### Participants
- 需要定义在序列图的一开始，主要是用来标书序列图中各个成员的顺序的

#### Messages
- Messages can be of two displayed either solid or with a dotted line.

> [Actor][Arrow][Actor]:Message text

There are six types of arrows currently supported:
> -> which will render a solid line with arrow
```
sequenceDiagram
    Alice ->Bob: Hello!
```
> --> which will render a dotted line with arrow
```
sequenceDiagram
    Alice -->Bob: Hello!
```
> ->> which will render a solid line with arrowhead
```
sequenceDiagram
    Alice ->>Bob: Hello!
```
> -->> which will render a dotted line with arrowhead
```
sequenceDiagram
    Alice -->>Bob: Hello!
```
> -x which will render a solid line with a cross at the end (async)
```
sequenceDiagram
    Alice -x Bob: Hello!
```
> --x which will render a dotted line with a cross at the end (async)
```
sequenceDiagram
    Alice --x Bob: Hello!
```

#### Notes
It is possible to add notes to a sequence diagram.

> Note [ right of | left of | over ] [Actor]: Text in note content

#### Loop
```
loop loop text
... statements...
end
```

#### Alt
it is possible to express alternative paths in a sequence diagram.
```
alt Descrption text
...statements...
else
... statements...
end
```
or if there is sequence that is optional(if without else)
```
opt Description text
...statements...
end
```
See the example below
```
sequenceDiagram
    participant Alice
    participant Bob

    Alice->>John: What date is today?
    John->>Bob: Help me check the numbers...
    Note over Bob,John: Bob check the calender number for John
    loop numbers check
        alt is 1
            Bob-->John: number is 1
            John-->>Alice: Today is Monday!
        else is 2
            Bob-->John: number is 2
            John-->>Alice: Today is Tuesday!
        end
        alt is 3
            Bob-->John: number is 3
            John-->>Alice: Today is Wednesday!
        else is 4
            Bob-->John: number is 4
            John-->>Alice: Today is Thirday!
        end
        alt is 5
            Bob-->John: number is 5
            John-->>Alice: Today is Friday!
       else is 6
            Bob-->John: number is 6
            John-->>Alice: Today is Saturday!
        end
        opt John response to Bob
            John->>Bob: Thanks!
        end
    end
```

#### Styling
Styling of the a sequence diagram is done by defining a number of css classes. During rendering these classes are extracted from the

Class used
| class        | Description                              |
| ------------ | ---------------------------------------- |
| actor        | Style for the actor box at the top of the diagram |
| text.actor   | Style for text in the actor box at the top of the diagram |
| actor-line   | The vertical line for an actor           |
| messageLin0  | Styles for the solid message line        |
| messageline1 | Styles fot he dotted message line        |
| messageText  | Defines styles for the text on the message arrows |
| labelBox     | Defines styles label to left in a loop   |
| labelText    | Styles for the text in label for loops   |
| loopText     | Styles for the text in the loop box      |
| loopLine     | Defines styles for the lines in the loop box |
| note         | Styles for the note box                  |
| noteText     | Styles for the text on in the note boxes |

Sample stylesheet
```css
body {
    background: white;
}

.actor {
    stroke: #CCCCFF;
    fill: #ECECFF;
}
text.actor {
    fill:black;
    stroke:none;
    font-family: Helvetica;
}

.actor-line {
    stroke:grey;
}

.messageLine0 {
    stroke-width:1.5;
    stroke-dasharray: "2 2";
    marker-end:"url(#arrowhead)";
    stroke:black;
}

.messageLine1 {
    stroke-width:1.5;
    stroke-dasharray: "2 2";
    stroke:black;
}

#arrowhead {
    fill:black;

}

.messageText {
    fill:black;
    stroke:none;
    font-family: 'trebuchet ms', verdana, arial;
    font-size:14px;
}

.labelBox {
    stroke: #CCCCFF;
    fill: #ECECFF;
}

.labelText {
    fill:black;
    stroke:none;
    font-family: 'trebuchet ms', verdana, arial;
}

.loopText {
    fill:black;
    stroke:none;
    font-family: 'trebuchet ms', verdana, arial;
}

.loopLine {
    stroke-width:2;
    stroke-dasharray: "2 2";
    marker-end:"url(#arrowhead)";
    stroke: #CCCCFF;
}

.note {
    stroke: #decc93;
    stroke: #CCCCFF;
    fill: #fff5ad;
}

.noteText {
    fill:black;
    stroke:none;
    font-family: 'trebuchet ms', verdana, arial;
    font-size:14px;
}

```

#### Configuration
Is it possible to adjust the margins for rendering the sequence diagram.

This is done by defining mermaid.sequenceConfig or by the CLI to use a json file with the configuration. How to use
the CLI is described in the mermaidCLI page.
mermaid.sequenceConfig can be set to a JSON string with config parameters or the corresponding object.
```
mermaid.sequenceConfig = {
    diagramMarginX:50,
    diagramMarginY:10,
    boxTextMargin:5,
    noteMargin:10,
    messageMargin:35,
    mirrorActors:true
    };
```
Possible configration params:
| Param           | Descriotion	Default                      | value |
| --------------- | ---------------------------------------- | ----- |
| mirrorActor     | Turns on/off the rendering of actors below the diagram as well as above it | false |
| bottomMarginAdj | Adjusts how far down the graph ended. Wide borders styles with css could generate unwantewd clipping which is why this config param exists. | 1     |



### Message to self In loop
```
sequenceDiagram
    participant Alice
    participant Bob
    Alice->>John: Hello John, how are you?
    loop Healthcheck
        John->>John: Fight against hypochondria
    end
    Note right of John: Rational thoughts<br/>prevail...
    Note over Alice,John: typical interaction!
    John-->>Alice: Great!
    John->>Bob: How about you?
    Note left of Bob: Bob thinks
    Bob-->>John: Jolly good!
```

### Loops,alt and opt
```
sequenceDiagram
    loop Daily query
        Alice->>Bob: Hello Bob, how are you?
        alt is 1
            Bob-->>Alice: Not so good:(
        else is well
            Bob-->>Alice: Feeling fresh like a daisy
        end

        opt Extra response
            Bob->>Alice: Thanks for asking
        end
    end
```
## Basic sequence diagram
```
    sequenceDiagram
        Alice->>Bob: Hello Bob, how are you?
        Bob-->>Alice: How about you, John?
        Bob--x Alice: I am good thanks!
        Bob-x John: I amd good thanks!
        Note right of John: Bob thinks a long<br/>long time
        Bob-->Alice: Checking with John...
        Alice->John: Yes...John,how are you?
```
