---
# You can also start simply with 'default'
#theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
#background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: Debugging a debugger
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# apply unocss classes to the current slide

# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-up
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
# <!-- Empty slide -->
layout: center
---

<style>
.headline {
    font-weight: 1000;
    text-align: center;
    font-size: 50px;
}

.headline-smol {
    font-weight: bold;
    text-align: center;
}

.addonestuff {
    font-weight: bold;
    text-align: center;
}
</style>


<h1 class="headline">Debuging a Debugger</h1><br />
<h2 class="headline-smol">Marco Sussitz</h2>

<div class="addonestuff">Software developer at Dynatrace</div>
<br /><br />
<div class="addonestuff">https://www.sussitzm.com</div>

---
layout: center
transition: none
---

<v-clicks at="1">
<div>
```java {all|all|2-5|4|7-14|9-10|all} twoslash
public class Main {
    public static void main(String[] args) {
        int number = 5;
        System.out.println(String.format("%s + 2 = %s", number, addTwo(number)));
    }

    public static Integer addTwo(int i) {
        try {
            int two = 2;
            return i - two;
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}
```
</div>
</v-clicks>

<!--
Office Story:
- Coffee machine
- Cannot hide on the toilet anymore


lets debug it together
-->

---
layout: center
transition: none
---

<video autoplay muted>
  <source src="./videos/firstVideo_problemSetup.mp4" type="video/mp4">
</video>

---
layout: center
---

<v-click>
  <h1>How do you debug a debugger?</h1>
</v-click>

---
layout: center
---

<v-click>
  <h1>Java Platform Debugger Architecture (JPDA)</h1>
</v-click>

<v-clicks>

- Java Debug Interface (JDI)
- Java Debug Wire Protocol (JDWP)
- Java Virtual Machine Tool Interface (JVM TI)

</v-clicks>

<!--
What is the first thing to do? Documentation
JDI: front end of the debugger written in java
JDWP: communication lay not bound to a language
- Writte my debugger in C#, or maybe I am a hippster then I can use rust or I hate my self then I would use C.
-->

---
layout: center
---
<v-click>
  <h1>“You might want to use JVM TI if your tool has very specialized functionality, not available in JDWP/JDI, that can only be performed in the debuggee process and you have a lot of time on your hands.” </h1>
  <h2 style="color:#5363f5; text-decoration: underline">https://www.oracle.com/java/technologies/javase/java-platform-debugger-architecture.html</h2>
</v-click>


---
layout: center
---

<style>
.grid-container-presentation {
    display: grid;
    grid-template-columns: repeat(5, 160px);
    grid-gap: 0px; /* Adds space between grid items */
    align-items: center; /* Aligns items vertically in the center */
}

.grid-item-presentation {
    display: grid;
    place-items: center;
    padding: 8px;
    width: 160px;
    height: 120px;
    background-color: #4d4d4d; /* Default color for grid items */
}

.big-box {
    background-color: #4d4d4d; /* Specific styling for the big boxes */
}

.placeholder-box {
    opacity: 0;
}

.thin-box {
    height: 10px; /* Same height as big boxes */
    background-color: #4d4d4d; /* Color for the thin box */
    display: flex;
}

section {
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 1em;
  color: white;
  font-family: Arial;
  font-weight: bold;
}
</style>

<div class="grid-container-presentation">
    <div class="grid-item-presentation big-box">
      <div>
        <div style="
          font-family: Arial;
          font-weight: bold;"> 
          Debugger<br /> Frontend (JDI) 
        </div>
      </div>
    </div>
    <div class="grid-item-presentation thin-box"></div>
    <div class="grid-item-presentation big-box">
      <section>
        Communication<br /> layer
      </section>
    </div>
    <div class="grid-item-presentation thin-box"></div>
    <div class="grid-item-presentation big-box">
      <section>
        Debugged<br /> JVM (JVMTI)
      </section>
    </div>
    <img src="./pictures/intelijIcon.png" alt="IntellijIcon" style="padding: 32px;"/>
    <div class="placeholder-box"></div>
    <div class="placeholder-box"></div>
    <div class="placeholder-box"></div>
    <img src="./pictures/javaIcon.png" alt="IntellijIcon" style="padding: 15px;"/>
</div>

---
layout: center
---

<style>
.image {
   display: grid;
   place-items: center;
   align-content: end;
   font-weight: bold;
}

</style>
<v-click>
  <div class="image">
      <img src="./pictures/nowWhat.webp" alt="" style="position: absolute; inset: 0px; z-index: -10; filter: brightness(.9);"/>
      <h2 class="now">Now what?</h2>
  </div>
</v-click>

---
layout: center
---

<v-click>
  <h1>System.out.println(“TEST”);</h1>
</v-click>

<!--
I think every programmer has done this before and that is of course some good old print debugging.
-->

---
layout: center
transition: none
---

<div class="grid-container-presentation">
  <span v-mark.box.orange=1>
    <div class="grid-item-presentation big-box">
        <div>
          <div style="
            font-family: Arial;
            font-weight: bold;"> 
            Debugger<br /> Frontend (JDI) 
          </div>
          <div style="font-size: 0.7em">
            <v-click at="+2">Intellij with patched JDI</v-click>
          </div>
          <div style="font-size: 0.7em; opacity: 0">
            test
          </div>
        </div>
    </div>
  </span>
  <div class="grid-item-presentation thin-box"></div>
  <div class="grid-item-presentation big-box">
    <section>
      Communication<br /> layer
    </section>
  </div>
  <div class="grid-item-presentation thin-box"></div>
  <div class="grid-item-presentation big-box">
    <section>
      Debugged<br /> JVM (JVMTI)
    </section>
  </div>
  <img src="./pictures/intelijIcon.png" alt="IntellijIcon" style="padding: 32px;"/>
  <div class="placeholder-box"></div>
  <div class="placeholder-box"></div>
  <div class="placeholder-box"></div>
  <img src="./pictures/javaIcon.png" alt="IntellijIcon" style="padding: 15px;"/>
</div>

<!--
Focus on the front end our intellij.
Intellij is written in java and is probably using the JDI. So this could be our first access point.
-->

---
layout: center
transition: none
---

<video autoplay muted>
  <source src="./videos/bonusVideo_patchingJDI.mp4" type="video/mp4">
</video>

---
layout: center
---

<v-click>
  <img src="./pictures/stackTrace_1.png" alt="" />
</v-click>


---
layout: center
---
<img src="./pictures/stackTrace_2.png" alt="" />

---
layout: center
---

<div class="grid-container-presentation">
    <div class="grid-item-presentation big-box">
        <div>
          <div style="
            font-family: Arial;
            font-weight: bold;"> 
            Debugger<br /> Frontend (JDI) 
          </div>
          <div style="font-size: 0.7em">
            <span v-mark.strike-through.red=1>
              Intellij with patched JDI
            </span>
          </div>
          <div style="font-size: 0.7em;">
            <v-click at="+2">Local build IDE in debugging mode</v-click>
          </div>
        </div>
    </div>
    <div class="grid-item-presentation thin-box"></div>
    <div class="grid-item-presentation big-box">
      <section>
        Communication<br /> layer
      </section>
    </div>
    <div class="grid-item-presentation thin-box"></div>
    <div class="grid-item-presentation big-box">
      <section>
        Debugged<br /> JVM (JVMTI)
      </section>
    </div>
    <img src="./pictures/intelijIcon.png" alt="IntellijIcon" style="padding: 32px;"/>
    <div class="placeholder-box"></div>
    <div class="placeholder-box"></div>
    <div class="placeholder-box"></div>
    <img src="./pictures/javaIcon.png" alt="IntellijIcon" style="padding: 15px;"/>
</div>

<!--
Lets be real this print debugging is beneve us. We are better then that.
-->

---
layout: center
transition: none
---

<v-click>
  <img src="./pictures/inception.webp" alt="" />
</v-click>

---
layout: center
transition: none
---

<video autoplay muted>
  <source src="./videos/secondVideo_debuggedIndellij.mp4" type="video/mp4">
</video>

<!--
Do understand what all of those thinks mean we have to look at the documentation
-->

---
layout: center
---

<v-click>
  <h1>StackFrame Command Set (16)</h1>
</v-click>

<v-click>
  <h2> GetValues Command (1)</h2>
</v-click>


<div v-after>
  <div v-click>
    <table>
      <thead>
        <tr>
          <th></th>
          <th></th>
          <th></th>
        </tr>
        <tr>
          <th><h3>Type</h3></th>
          <th><h3>Name</h3></th>
          <th><h3>Description</h3></th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><kbd>threadID</kbd></td>
          <td>thread</td>
          <td>The frame's thread.</td>
        </tr>
        <tr>
          <td><kbd>frameID</kbd></td>
          <td>frame</td>
          <td>The frame ID.</td>
        </tr>
        <tr>
          <td><kbd>int</kbd></td>
          <td>slots</td>
          <td>The number of values to get.</td>
        </tr>
      </tbody>
    </table>
  </div>
</div>

<!--
But what does this mean?
- We need to look deeper into the documentation
-->

---
layout: center
---

<v-click>
  <h1>Slots</h1>
</v-click>

<div v-after>
  <div v-click>
    <table>
      <thead>
        <tr>
          <th></th>
          <th></th>
          <th></th>
        </tr>
        <tr>
          <th><h3>Type</h3></th>
          <th><h3>Name</h3></th>
          <th><h3>Description</h3></th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><kbd>int</kbd></td>
          <td>slot</td>
          <td>The local variable's index in the frame.</td>
        </tr>
        <tr>
          <td><kbd>byte</kbd></td>
          <td>sigbyte</td>
          <td>A tag identifying the type of the variable.</td>
        </tr>
      </tbody>
    </table>
  </div>
</div>


---
layout: center
---

<v-click>
  <h1>Tag Constants</h1>
</v-click>

<div v-after>
  <div v-click=2>
    <table>
      <thead>
        <tr>
          <th></th>
          <th></th>
          <th></th>
        </tr>
        <tr>
          <th><h3>Name</h3></th>
          <th><h3>Value</h3></th>
          <th><h3>Description</h3></th>
        </tr>
      </thead>
      <tbody>
          <tr>
            <td><kbd>ARRAY</kbd></td>
            <td>91</td>
            <td>'[' - an array object (objectID size).</td>
          </tr>
        <tr>
          <td><kbd>BYTE</kbd></td>
          <td>66</td>
          <td>'B' - a byte value (1 byte).</td>
        </tr>
        <tr>
          <td><kbd>CHAR</kbd></td>
          <td>67</td>
          <td>'C' - a character value (2 bytes).</td>
        </tr>
        <tr>
          <td><kbd>OBJECT</kbd></td>
          <td>76</td>
          <td>'L' - an object (objectID size).</td>
        </tr>
        <tr>
          <td><kbd>FLOAT</kbd></td>
          <td>70</td>
          <td>'F' - a float value (4 bytes).</td>
        </tr>
        <tr>
          <td><kbd>DOUBLE</kbd></td>
          <td>68</td>
          <td>'D' - a double value (8 bytes).</td>
        </tr>
        <tr>
          <td><kbd>INT</kbd></td>
          <td>73</td>
          <td>'I' - an int value (4 bytes).</td>
        </tr>
      </tbody>
    </table>
  </div>
</div>

<!--
- We are intertesed in the bottom one 73
- And the Object 76
Those are the once we have seen while debugging.
-->

---
layout: center
---

<v-click>
  <h1>Tools to use:</h1>
</v-click>

<v-clicks>

- ASM Bytecode Viewer (intellij plugin)
- jclasslib Bytecode Viewer (intellij plugin)
- javap (part of the JDK)

</v-clicks>

<!--
Now what does this mean?
- For this we need to look at the bydecode
- Some tools to use for that
-->

---
layout: center
---
```yaml {all|2-3|4|7} twoslash
public class org.example.Main
  minor version: 0
  major version: 61
  flags: (0x0021) ACC_PUBLIC, ACC_SUPER
  this_class: #22                         // org/example/Main
  super_class: #2                         // java/lang/Object
  interfaces: 0, fields: 0, methods: 3, attributes: 1
```

---
layout: center
---
```yaml {all|2-3|14} twoslash
Constant pool:
   #1 = Methodref          #2.#3          // java/lang/Object."<init>":()V
   #2 = Class              #4             // java/lang/Object
   #3 = NameAndType        #5:#6          // "<init>":()V
   #4 = Utf8               java/lang/Object
   #5 = Utf8               <init>
   #6 = Utf8               ()V
   #7 = Fieldref           #8.#9          // java/lang/System.out:Ljava/io/PrintStream;
   #8 = Class              #10            // java/lang/System
   #9 = NameAndType        #11:#12        // out:Ljava/io/PrintStream;
  #10 = Utf8               java/lang/System
  #11 = Utf8               out
  #12 = Utf8               Ljava/io/PrintStream;
  #13 = String             #14            // %s + 2 = %s
  #14 = Utf8               %s + 2 = %s
  #15 = Methodref          #16.#17        // java/lang/Integer.valueOf:(I)Ljava/lang/Integer;
  #16 = Class              #18            // java/lang/Integer
  #17 = NameAndType        #19:#20        // valueOf:(I)Ljava/lang/Integer;
  #18 = Utf8               java/lang/Integer
  #19 = Utf8               valueOf
  #20 = Utf8               (I)Ljava/lang/Integer;
  #21 = Methodref          #22.#23        // org/example/Main.addTwo:(I)Ljava/lang/Integer;
  #22 = Class              #24            // org/example/Main
  #23 = NameAndType        #25:#20        // addTwo:(I)Ljava/lang/Integer;
  #24 = Utf8               org/example/Main
  #25 = Utf8               addTwo
  #26 = Methodref          #27.#28        // java/lang/String.format:(Ljava/lang/String;[Ljava/lang/Object;)Ljava/lang/String;
```

---
layout: center
---

<div grid="~ cols-2 gap-4">

```yaml{all|2-3|4-8|9-10|11-13}{at:1}
public org.example.Main()
  descriptor: ()V
  flags: (0x0001) ACC_PUBLIC
  Code:
    stack=1, locals=1, args_size=1
        0: aload_0
        1: invokespecial #1
        4: return
    LineNumberTable:
      line 3: 0
    LocalVariableTable:
      Start  Length  Slot  Name   Signature
          0       5     0  this   Lorg/example/Main
```

```java{all|1|2|none}{at:1}
public Main() {
  super();
}
```
</div>

<!--
18:00
-->

---
layout: center
---

<div grid="~ cols-2 gap-4">

````md magic-move {lines: true}
```yaml{all|1|2-3|4}
public static void main(java.lang.String[]);
  descriptor: ([Ljava/lang/String;)V
  flags: (0x0009) ACC_PUBLIC, ACC_STATIC
  Code:
    {..........}
    LineNumberTable:
      line 5: 0
      line 6: 2
      line 7: 31
    LocalVariableTable:
      Start  Length  Slot  Name   Signature
          0      32     0  args   [Ljava/lang/String;
          2      30     1 number   I
```

```yaml{3-23|8|19|21|all}
public static void main(java.lang.String[]);
  {..........}
  Code:
    stack=6, locals=2, args_size=1
      0: iconst_5
      1: istore_1
      2: getstatic     #7                  
      5: ldc           #13                 
      7: iconst_2
      8: anewarray     #2                 
      11: dup
      12: iconst_0
      13: iload_1
      14: invokestatic  #15                 
      17: aastore
      18: dup
      19: iconst_1
      20: iload_1
      21: invokestatic  #21                 
      24: aastore
      25: invokestatic  #26                 
      28: invokevirtual #32                 
      31: return
    {..........}
```

```yaml{all|6-9|10-13|12|12|13}
public static void main(java.lang.String[]);
  descriptor: ([Ljava/lang/String;)V
  flags: (0x0009) ACC_PUBLIC, ACC_STATIC
  Code:
    {..........}
    LineNumberTable:
      line 5: 0
      line 6: 2
      line 7: 31
    LocalVariableTable:
      Start  Length  Slot  Name   Signature
          0      32     0  args   [Ljava/lang/String;
          2      30     1 number   I
```
````

```java{all|1|1|all|all|5|7|4|all|all|none|none|none|1|2}{at:1}
public static void main(String[] args) {
    int number = 5;
    System.out.println(
      String.format(
        "%s + 2 = %s",
        number,
        addTwo(number)));
}
```

</div>

<!--
ldc load constant onto the operator stack
-->

---
layout: center
---

<div grid="~ cols-2 gap-4">


````md magic-move{lines: true}
```yaml{all|2-3|4}
public static java.lang.Integer addTwo(int);
  descriptor: (I)Ljava/lang/Integer;
  flags: (0x0009) ACC_PUBLIC, ACC_STATIC
  Code:
    {..........}
    Exception table:
        from    to  target type
            0     8     9   Class java/lang/Exception
    LineNumberTable:
      line 11: 0
      line 12: 2
      line 13: 9
      line 14: 10
    LocalVariableTable:
      Start  Length  Slot  Name   Signature
          2       7     1   two   I
          10      9     1     e   Ljava/lang/Exception;
          0      19     0     i   I
    StackMapTable: number_of_entries = 1
      frame_type = 73 /* same_locals_1_stack_item */
        stack = [ class java/lang/Exception ]
```

```yaml{3-23|9|12-17}
public static java.lang.Integer addTwo(int);
  {..........}
  Code:
    stack=3, locals=2, args_size=1
        0: iconst_2
        1: istore_1
        2: iload_0
        3: iload_1
        4: isub
        5: invokestatic  #15
        8: areturn
        9: astore_1
      10: new           #40
      13: dup
      14: aload_1
      15: invokespecial #42
      18: athrow
    {..........}
```

```yaml{all|6-8|8|8|8|9-13|14-18|18|16|17|19-21}
public static java.lang.Integer addTwo(int);
  descriptor: (I)Ljava/lang/Integer;
  flags: (0x0009) ACC_PUBLIC, ACC_STATIC
  Code:
    {..........}
    Exception table:
        from    to  target type
            0     8     9   Class java/lang/Exception
    LineNumberTable:
      line 11: 0
      line 12: 2
      line 13: 9
      line 14: 10
    LocalVariableTable:
      Start  Length  Slot  Name   Signature
          2       7     1   two   I
          10      9     1     e   Ljava/lang/Exception;
          0      19     0     i   I
    StackMapTable: number_of_entries = 1
      frame_type = 73 /* same_locals_1_stack_item */
        stack = [ class java/lang/Exception ]
```
````

```java{all|1|all|all|4|5-7|all|none|none|3-4|5-7|none|none|1|3|5|none}{at:1}
public static Integer addTwo(int i) {
    try {
        int two = 2;
        return i - two;
    } catch (Exception e) {
        throw new RuntimeException(e);
    }
}
```
</div>

<!--
StackMap table java 6+
-->

---
layout: center
---

<v-click>
  <img src="./pictures/inception_2.webp" alt="" />
  <h1 style="text-align: center;">We need to go deeper</h1>
</v-click>


---
layout: center
---

<v-click>
  <h1>Java Platform Debugger Architecture (JPDA)</h1>
</v-click>

<div v-after>
  <ul>
    <li>Java Debug Interface (JDI)</li>
    <li>Java Debug Wire Protocol (JDWP)</li>
      <li>
        <span v-mark.underscore.orange=2>
          Java Virtual Machine Tool Interface (JVM TI)
        </span>
      </li>
  </ul>
</div>


---
layout: center
transition: none
---

<v-click at="1">
  <div class="grid-container-presentation">
    <div class="grid-item-presentation big-box">
      <div>
        <div style="
          font-family: Arial;
          font-weight: bold;"> 
          Debugger<br /> Frontend (JDI) 
        </div>
      </div>
    </div>
    <div class="grid-item-presentation thin-box"></div>
    <div class="grid-item-presentation big-box">
      <section>
        Communication<br /> layer
      </section>
    </div>
    <div class="grid-item-presentation thin-box"></div>
    <span v-mark.box.orange=2>
      <div class="grid-item-presentation big-box">
        <div> 
          <div>
            Debugged<br /> JVM (JVMTI)
          </div>
          <div style="font-size: 0.7em;">
            <v-click at="+3">Patched openJDK More logs from JVMTI
              </v-click>
          </div>
        </div>
      </div>
    </span>
    <img src="./pictures/intelijIcon.png" alt="IntellijIcon" style="padding: 32px;"/>
    <div class="placeholder-box"></div>
    <div class="placeholder-box"></div>
    <div class="placeholder-box"></div>
    <img src="./pictures/javaIcon.png" alt="IntellijIcon" style="padding: 15px;"/>
  </div>
</v-click>



---
layout: center
transition: none
---

<video autoplay muted>
  <source src="./videos/thirdVideo_JvmtiChanges.mp4" type="video/mp4">
</video>

<!--
fix trasition
-->

---
layout: center
transition: none
---

<video autoplay muted>
  <source src="./videos/fourthVideo_debuggerRunWithMoreInfo.mp4" type="video/mp4">
</video>

---
layout: center
transition: none
---

<video autoplay muted>
  <source src="./videos/fifthVideo_BasicTypsLookUp.mp4" type="video/mp4">
</video>

---
layout: center
transition: none
---

<v-clicks>
```java{all|8,10}
enum BasicType {
  T_BOOLEAN     = JVM_T_BOOLEAN,
  T_CHAR        = JVM_T_CHAR,
  T_FLOAT       = JVM_T_FLOAT,
  T_DOUBLE      = JVM_T_DOUBLE,
  T_BYTE        = JVM_T_BYTE,
  T_SHORT       = JVM_T_SHORT,
  T_INT         = JVM_T_INT,
  T_LONG        = JVM_T_LONG,
  T_OBJECT      = 12,
  T_ARRAY       = 13,
  T_VOID        = 14,
  T_ADDRESS     = 15,
  T_NARROWOOP   = 16,
  T_METADATA    = 17,
  T_NARROWKLASS = 18,
  T_CONFLICT    = 19,
  T_ILLEGAL     = 99
};
```
</v-clicks>

---
layout: center
---

```yaml{all|14-18}
public static java.lang.Integer addTwo(int);
  descriptor: (I)Ljava/lang/Integer;
  flags: (0x0009) ACC_PUBLIC, ACC_STATIC
  Code:
    {..........}
    Exception table:
        from    to  target type
            0     8     9   Class java/lang/Exception
    LineNumberTable:
      line 11: 0
      line 12: 2
      line 13: 9
      line 14: 10
    LocalVariableTable:
      Start  Length  Slot  Name   Signature
          2       7     1   two   I
          10      9     1     e   Ljava/lang/Exception;
          0      19     0     i   I
    StackMapTable: number_of_entries = 1
      frame_type = 73 /* same_locals_1_stack_item */
        stack = [ class java/lang/Exception ]
```

<!--
27:00
-->

---
layout: center
---

<v-click>
  <h1>Chapter 4. The class File Format</h1>
</v-click>

<div >
  <ul>
    <li v-click>The value of start_pc must be a valid index into the code array of this Code attribute and must be the index of the opcode of an instruction.</li>
    <li v-click>The value of start_pc + length must either be a valid index into the code array of this Code attribute and be the index of the opcode of an instruction, or it must be the first index beyond the end of that code array.</li>
  </ul>
</div>

---
layout: center
---

<div grid="~ cols-2 gap-4">

```yaml{all|none|none|7-12|none|13-17|none|5-17|none}
public static java.lang.Integer addTwo(int);
  {..........}
  Code:
    stack=3, locals=2, args_size=1
        0: iconst_2
        1: istore_1
        2: iload_0
        3: iload_1
        4: isub
        5: invokestatic  #15
        8: areturn
        9: astore_1
      10: new           #40
      13: dup
      14: aload_1
      15: invokespecial #42
      18: athrow
    {..........}
```

```yaml{all|none|16|16|17|17|18|18|none|16-17}{at:1}
public static java.lang.Integer addTwo(int);
  descriptor: (I)Ljava/lang/Integer;
  flags: (0x0009) ACC_PUBLIC, ACC_STATIC
  Code:
    {..........}
    Exception table:
        from    to  target type
            0     8     9   Class java/lang/Exception
    LineNumberTable:
      line 11: 0
      line 12: 2
      line 13: 9
      line 14: 10
    LocalVariableTable:
      Start  Length  Slot  Name   Signature
          2       7     1   two   I
          10      9     1     e   Ljava/lang/Exception;
          0      19     0     i   I
    StackMapTable: number_of_entries = 1
      frame_type = 73 /* same_locals_1_stack_item */
        stack = [ class java/lang/Exception ]
```

</div>

---
layout: center
---

<style>
.headline {
    font-weight: 1000;
    text-align: center;
    font-size: 50px;
}

.headline-smol {
    font-weight: bold;
    text-align: center;
}

.addonestuff {
    font-weight: bold;
    text-align: center;
}
</style>

<v-click>
  <h1 class="headline">Truths are Illusions</h1><br />
  <h2 class="headline-smol ">-Friedrich Nietzsche</h2>
</v-click>


---
layout: center
---

<v-clicks>
```yaml{all}
public static java.lang.Integer addTwo(int);
  descriptor: (I)Ljava/lang/Integer;
  flags: (0x0009) ACC_PUBLIC, ACC_STATIC
  Code:
    {..........}
    Exception table:
        from    to  target type
            0     8     9   Class java/lang/Exception
    LineNumberTable:
      line 11: 0
      line 12: 2
      line 13: 9
      line 14: 10
    LocalVariableTable:
      Start  Length  Slot  Name   Signature
          2       7     1   two   I
          10      9     1     e   Ljava/lang/Exception;
          0      19     0     i   I
    StackMapTable: number_of_entries = 1
      frame_type = 73 /* same_locals_1_stack_item */
        stack = [ class java/lang/Exception ]
```
</v-clicks>

---
layout: center
---


<v-click>
  <h1>Java agents</h1>
</v-click>

<div >
  <ul>
    <li v-click>java.lang.instrument</li>
    <li v-click>Provides services that allow Java programming language agents to instrument programs running on the JVM.</li>
  </ul>
</div>

---
layout: center
---

```java{all|1|2-3|4|5|6|7|8|9|10-14|13,16}
public class WriteLoadedClassToDiskAgent implements ClassFileTransformer {
    @Override
    public byte[] transform(
            ClassLoader loader,
            String className,
            Class<?> classBeingRedefined,
            ProtectionDomain protectionDomain,
            byte[] classfileBuffer) {
        if (className.contains("org/example/Main")) {
            try {
                FileUtils.writeByteArrayToFile(new File("Main.class"), classfileBuffer);
            } catch (IOException e) {
                throw new RuntimeException("Cannot write classfile");
            }
        }
        return null;
    }
}
```

---
layout: center
transition: none
---

<video autoplay muted>
  <source src="./videos/sixVideo_runWithWriteClassToDisk.mp4" type="video/mp4">
</video>

<!--
fix the transition
-->

---
layout: center
transition: none
---

<div grid="~ cols-2 gap-4">

```yaml{all|16-18}
public static java.lang.Integer addTwo(int);
  descriptor: (I)Ljava/lang/Integer;
  flags: (0x0009) ACC_PUBLIC, ACC_STATIC
  Code:
    {..........}
    Exception table:
        from    to  target type
            0     8     9   Class java/lang/Exception
    LineNumberTable:
      line 11: 0
      line 12: 2
      line 13: 9
      line 14: 10
    LocalVariableTable:
      Start  Length  Slot  Name   Signature
          2       7     1   two   I
          10      9     1     e   Ljava/lang/Exception;
          0      19     0     i   I
    StackMapTable: number_of_entries = 1
      frame_type = 73 /* same_locals_1_stack_item */
        stack = [ class java/lang/Exception ]
```

```yaml{all|16-18}{at:1}
public static java.lang.Integer addTwo(int);
  descriptor: (I)Ljava/lang/Integer;
  flags: (0x0009) ACC_PUBLIC, ACC_STATIC
  Code:
    {..........}
    Exception table:
        from    to  target type
            0     8     9   Class java/lang/Exception
    LineNumberTable:
      line 11: 0
      line 12: 2
      line 13: 9
      line 14: 10
    LocalVariableTable:
      Start  Length  Slot  Name   Signature
          0       9     1   two   I
          0      19     1     e   Ljava/lang/Exception;
          0      19     0     i   I
    StackMapTable: number_of_entries = 1
      frame_type = 73 /* same_locals_1_stack_item */
        stack = [ class java/lang/Exception ]
```

</div>


---
layout: center
transition: none
---

<video autoplay muted>
  <source src="./videos/seventhVideo_FixBug.mp4" type="video/mp4">
</video>

---
layout: center
---


```java{all|4|6,8|6}
public class CorruptDebuggerAgent {
    private static final boolean ENABLE_CORRUPTION = false;

    public static void premain(String arguments, Instrumentation instrumentation) {
        if (ENABLE_CORRUPTION) {
            instrumentation.addTransformer(new CorruptDebuggerTransformer());
            if (arguments != null && arguments.contains("WriteToDisk")) {
                instrumentation.addTransformer(new WriteLoadedClassToDiskAgent());
            }
        }
    }
}
```

---
layout: center
---

```java{all|2-6|7|10}
@Override
public byte[] transform(ClassLoader loader,
                        String className,
                        Class<?> classBeingRedefined,
                        ProtectionDomain protectionDomain,
                        byte[] classfileBuffer) {
    if (className.contains("org/example/Main")) {
        ClassReader classReader = new ClassReader(classfileBuffer);
        final ClassWriter classWriter = new ClassWriter(classReader, ClassWriter.COMPUTE_FRAMES | ClassWriter.COMPUTE_MAXS);
        classReader.accept(new CorruptClassVisitor(classWriter, className, "addTwo"), ClassReader.EXPAND_FRAMES);
        return classWriter.toByteArray();
    }
    return classfileBuffer;
}
```

---
layout: center
---

```java{all|4|5}
@Override
public MethodVisitor visitMethod(int access, String name, String desc,
                                  String signature, String[] exceptions) {
    if (name.equals("addTwo")) {
        return new CorruptMethodVisitor(super.visitMethod(access, name, desc, signature, exceptions), className, name, methodName);
    }
    return super.visitMethod(access, name, desc, signature, exceptions);
}
```

---
layout: center
---


````md magic-move{lines: true}
```java{all|12-20|12-15}
public class CorruptMethodVisitor extends MethodVisitor {
    private Label start;
    private Label end;

    private int startLabelineNumber = 11;
    private int endLabelLineNumber = 14;

    public CorruptMethodVisitor(MethodVisitor methodVisitor, String className, String methodName, String methodToVisit) {
        super(Opcodes.ASM4, methodVisitor);
    }

    @Override
    public void visitLineNumber(int line, Label label) {
        //Code
    }

    @Override
    public void visitLocalVariable(String name, String descriptor, String signature, Label start, Label end, int index) {
        //Code
    }
}
```

```java{12-20|14|17-19|22-25}
public class CorruptMethodVisitor extends MethodVisitor {
    private Label start;
    private Label end;

    private int startLabelineNumber = 11;
    private int endLabelLineNumber = 14;

    public CorruptMethodVisitor(MethodVisitor methodVisitor, String className, String methodName, String methodToVisit) {
        super(Opcodes.ASM4, methodVisitor);

    }

    @Override
    public void visitLineNumber(int line, Label label) {
        super.visitLineNumber(line, label);

        if (line == startLabelineNumber) {
            this.start = label;
        }
    }

    @Override
    public void visitLocalVariable(String name, String descriptor, String signature, Label start, Label end, int index) {
        //Code
    }
}
```

```java{17-20}
public class CorruptMethodVisitor extends MethodVisitor {
    private Label start;
    private Label end;

    private int startLabelineNumber = 11;
    private int endLabelLineNumber = 14;

    public CorruptMethodVisitor(MethodVisitor methodVisitor, String className, String methodName, String methodToVisit) {
        super(Opcodes.ASM4, methodVisitor);
    }

    @Override
    public void visitLineNumber(int line, Label label) {
        //Code
    }

    @Override
    public void visitLocalVariable(String name, String descriptor, String signature, Label start, Label end, int index) {
        super.visitLocalVariable(name, descriptor, signature, this.start, end, index);
    }
}
```
````

---
layout: center
---

<v-clicks at="1">
<div>
```java {all|all|8-13|9-10|11-13|8-13} twoslash
public class Main {
    public static void main(String[] args) {
        int number = 5;
        System.out.println(String.format("%s + 2 = %s", number, addTwo(number)));
    }

    public static Integer addTwo(int i) {
        try {
            int two = 2;
            return i - two;
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}
```
</div>
</v-clicks>

---
layout: center
---

<v-click>
  <h1>When is this usefull?</h1>
</v-click>

<!--
todo move slide and fix error
-->

---
layout: center
---

<v-click>
  <h1>How does a debugger work?</h1>
</v-click>

<v-clicks>

- LineNumberTable
- LocalVariableTable

</v-clicks>

---
layout: center
---
