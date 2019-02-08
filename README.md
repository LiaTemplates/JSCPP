<!--

author:   André Dietrich
email:    andre.dietrich@ovgu.de
version:  1.2.0
language: en
narrator: US English Female

script:   https://felixhao28.github.io/JSCPP/dist/JSCPP.es5.min.js

@JSCPP.__eval
<script>
  try {
    var output = "";
    JSCPP.run(`@0`, `@1`, {stdio: {write: s => { output += s }}});
    output;
  } catch (msg) {
    var error = new LiaError(msg, 1);
    try {
        var log = msg.match(/(.*)\nline (\d+) \(column (\d+)\):.*\n.*\n(.*)/);
        var info = log[1] + " " + log[4];
        if (info.length > 80)
          info = info.substring(0,76) + "..."
        error.add_detail(0, info, "error", log[2]-1, log[3]);
    } catch(e) {}
    throw error;
    }
</script>
@end

@JSCPP.eval: @JSCPP.__eval(@input, )

@JSCPP.evalWithStdin: @JSCPP.__eval(@input,`@input(1)`)

-->

# jscpp_template


                         --{{0}}--
This document defines some basic macros for applying the JavaScript C++
interpreter [JSCPP](https://felixhao28.github.io/JSCPP) in
[LiaScript](https://LiaScript.github.io) to make Markdown code-blocks
executable.

__Try it on LiaScript:__

https://liascript.github.io/course/?https://raw.githubusercontent.com/liaScript/jscpp_template/master/README.md

__See the project on Github:__

https://github.com/liaScript/jscpp_template

                         --{{1}}--
There are three ways to use this template. The easiest way is to use the import
statement and the url of the raw text-file of the master branch or any other
branch or version. But you can also copy the required functionionality directly
into the header or your Markdown document, see therefor the [last slide](#4).
And of course, you could also clone this project and change it, as you wish.

                           {{1}}
1. Load the macros via

   `import: https://raw.githubusercontent.com/liaScript/jscpp_template/master/README.md`

2. Copy the definitions into your Project

3. Clone this repository on GitHub


## `@JSCPP.eval`


                         --{{0}}--
Add the macro `@JSCPP.eval` to the end a single C++ code-block to make it
executable and editable in LiaScript. The current code gets evaluate by the
JSCPP interpreter and the result is shown in a console below.


``` c
#include <iostream>
using namespace std;

int main() {
    int a = 12;
    int rslt = 0;
    for(int i=1; i<a; ++i) {
        rslt += i;
        cout << "rslt: " << rslt << endl;
    }
    cout << "final result = " << rslt << endl;
    return 0;
}
```
@JSCPP.eval



## `@JSCPP.evalWithStdin`

                         --{{0}}--
You can add stdin inputs via a second code-block and add the macro
`@JSCPP.evalWithStdin` to the end of this project, in order to pass these
strings to the interpreter.


```c
#include <iostream>
using namespace std;

int main() {
    int a;

    cin >> a;

    int rslt = 0;
    for(int i=1; i<a; ++i) {
        rslt += i;
        cout << "rslt: " << rslt << endl;
    }
    cout << "final result = " << rslt << endl;
    return 0;
}
```
``` text +stdin
5
```
@JSCPP.evalWithStdin


## Implementation

                         --{{0}}--
The code shows how the macros were implemented by calling the macro
`@JSCPP.__eval` with different default parameters. The script command at the top
defines the reference to the JSCPP javascript implementation that needs to be
called to load the interpreter.

``` html
script: https://felixhao28.github.io/JSCPP/dist/JSCPP.es5.min.js

@JSCPP.eval: @JSCPP.__eval(@input, )

@JSCPP.evalWithStdin: @JSCPP.__eval(@input,`@input(1)`)

@JSCPP.__eval
<script>
  try {
    var output = "";
    JSCPP.run(`@0`, `@1`, {stdio: {write: s => { output += s }}});
    output;
  } catch (msg) {
    var error = new LiaError(msg, 1);

    try {
        var log = msg.match(/(.*)\nline (\d+) \(column (\d+)\):.*\n.*\n(.*)/);
        var info = log[1] + " " + log[4];

        if (info.length > 80)
          info = info.substring(0,76) + "..."

        error.add_detail(0, info, "error", log[2]-1, log[3]);
    } catch(e) {}

    throw error;
    }
</script>
@end
```

                         --{{1}}--
If you want to minimize loading effort in your LiaScript project, you can also
copy this code and paste it into your main comment header, see the code in the
raw file of this document.

{{1}} https://raw.githubusercontent.com/liaScript/jscpp_template/master/README.md
