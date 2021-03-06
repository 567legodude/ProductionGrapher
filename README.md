This is a project I threw together to aid in drawing parse trees.
The application is not perfect and may have some bugs, but it works enough for what I need.

The application uses DOT in order to generate the image of the tree.

### Installation
1. Run `java -version` to check what version of Java you have.
2. Download the application from the [Releases](https://github.com/567legodude/ProductionGrapher/releases) page.
3. Download and install [Graphviz](https://graphviz.org/download/) and then find the location of the DOT program.
    1. For example, on Windows, DOT may be located at `C:\Program Files\Graphviz\bin\dot.exe`.
    2. Copy the path to the DOT program and paste it in a text file called `dot.txt`. This file should be in the same
       directory as the application jar. If this file exists, the program will autofill the "Path
       to DOT" textbox with the content. The textbox must be filled with the path to the dot program in order
       to generate the image.
4. If your version of Java is between 8 and 10, the application works as-is. Launch the application using
   the file `run_java8-10` or with the command `java -cp "ProductionGrapher.jar" Grapher`.
5. For versions of Java 11+, additional modules are required.
    1. Download [JavaFX SDK](https://gluonhq.com/products/javafx/) version 11.0.2
    2. Put the folder `javafx-sdk-11.0.2` in the same directory as the application jar.
    3. Launch the application using the file `run_java11+` or with the command `java -cp "ProductionGrapher.jar" --module-path "javafx-sdk-11.0.2/lib" --add-modules=javafx.controls Grapher`.

### Usage
The large textbox on the left is where production rules go.  
Rules have the format `<rule> ::= options`  
`options` are the possible values of the production rule separated by pipes `|`. Pipes that are
adjacent to another pipe or escaped with a backslash will not split values.  
**Shortcut**: Ctrl+Click in the textbox to select a file to load into the textbox. Files
can also be dragged and dropped into this textbox to load a predefined set of rules.

The small textbox on the left shows a number next to the production rules in the large textbox.
These numbers are used when referring to a particular production rule.

The resulting tree is shown on the right side. Right-clicking in this area will open a dialog
to save the image.  
The graph can be exported in the following formats:
* Image formats: .jpeg, .png, .gif, .svg, and .bmp
* PDF File
* DOT File; The text that is given to DOT in order to generate the graph.

The following terminology is used in this section:
* **Production ID**: The number that appears in the small text box next to a production rule.
* **Node ID**: The number that appears next to a node in the graph image.
* **Simple pattern**: A simple pattern is used to locate a particular production value. By default,
  the pattern matches any production rule that contains all the characters in the pattern. The characters
  may be anywhere in the rule but must be in the same order. For example, the pattern `{}` will match any
  production value that contains both brackets as long as they appear in that order, such as `{ <expr> }`.
  Surrounding the pattern in single quotes will match any production value that contains the pattern as a substring.
  Surrounding the pattern in double quotes will match any production value that is exactly equal to the pattern.
* **Linear path**: A linear path means traveling straight through single production rules from a node
  to the target value. The application will not make any decisions for the user, and as such will
  not work for ambiguous grammars. There should be exactly one way to connect a given node
  to the target value. If there is more than one way to create a parse tree from the starting
  node to the target value, the search will fail.
* **Relevant node**: As the tree is being built, non-terminal nodes will be added to a list. The current relevant
  node will be indicated with a box. Most quick actions and shortcuts will be performed on the current relevant node.

Command to build and manipulate the tree are done in the textbox at the bottom.  
The following actions can be used to manipulate the tree:
1. Entering `+N` where `N` is the number of a production rule will insert a node into the graph
for the particular production rule. This is used to begin a new tree.
2. Enter `-N` to delete the node and its children with node ID `N`.
3. Enter a node ID for a non-terminal to enter selection mode for that production rule. The large textbox will change to show
   the possible values that the selected production rule can expand into.
    * Shortcut: Press the down arrow while the text box is empty to enter selection mode for the current relevant node.
    * The following commands are used while in selection mode:
        1. Enter the line number next to a value to expand the selected node into that value.
        2. Enter `-` to exit selection mode.
            * Shortcut: Press the up arrow to cancel selection mode.
        3. Press the left arrow while the text box is empty to quick-select the first possible option.
        4. Press the right arrow or down arrow while the text box is empty to quick-select the last possible option.
        5. Enter a simple pattern and hit tab to select the value that matches. A value will only be selected
            if exactly one of the values matches the pattern.
4. Enter `s N pattern` where `N` is a node ID and `pattern` is a simple pattern to create a linear
path from node N to the production value matching the pattern.
    * Shortcut: Simply enter the pattern and hit the tab key. N will be equal to the current relevant node. (This
      variation must be used for a pattern which contains spaces.)
5. Enter `r` to redraw the tree (including node IDs)
6. Enter `o` to redraw the tree without any extras, including the relevant node indicator and node IDs. This is how
the tree will appear when exported.
7. Enter `f` to redraw the tree with full extras. This is like the `r` command but will also display the node IDs
of terminal symbols.
8. Enter `~N` where `N` is a node ID to unlink the specified node from its parent.
9. Enter `=N` where `N` is a node ID to set that node as the relevant node.
10. Enter `*` to refresh the relevant nodes. If a parent to a relevant node was deleted, or the relevant node was
otherwise lost, this will clear the list of relevant nodes and re-add all non-terminals which have no children.
11. Enter `P C`, `P -- C`, or `P -> C` where `P` and `C` are node IDs to manually add a link between a parent and child node.
12. Press the up arrow to skip the current relevant node and move on to the next one. The skipped node will NOT
automatically become relevant again.