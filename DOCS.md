## Core
```c
include "bin/core.pixi"
```
### set_standart_uis_properties
Make from container an ui object.
```c
set_standart_uis_properties(
    $cont,
    $press, $move, $release,
    $save_state, $set_state,
    $draw,
    $remove
)
```
**Arguments:**
*   $cont = container - container that will be an ui object
*   $press, $move, $release = fn($self, $scancode_id, $evt_x, $evt_y) - event
callbacks
*   $save_state, $set_state = fn() - *TODO: add description*
*   $draw = fn($self) - drawing callback
*   $remove = fn($self) - deconstructor

**Result:**
void; It fills $cont properties:
*   press = $press
*   move = $move
*   release = $release
*   save_state = $save_state
*   set_state = $set_state
*   draw = $draw
*   remove = $remove
*   grab_mouse = (default) 0; changeable; can have next values:
    *   = -1 - ignore events
    *   = 0 - can grab touches, no grabbed
    *   \> 0 - grab touches
*   touches = container; *TODO: add info about touches*
*   states = container;  *TODO: add info about states*
    *   states.size - count elements in container
*   state_id = current state id

**Exapmles:** *TODO: add examples*
```c
```

-----------

### check_mouse_and_touches_events
Check EVT for mouse and touches events and pass it to ui object.
```c
$grabbed = check_mouse_and_touches_events($ui_object)
```
**Arguments:**
*   $ui_object = container - container must be passed through [set_standart_uis_properties](#set_standart_uis_properties)

**Result:**
0 | 1;
*   0 - event released or not grabbed;
*   1 - event grabbed

It calls
`$ui_object.[press|move|release]($ui_object, $scancode_id, EVT[EVT_X], EVT[EVT_Y])`;<br>
EVT_MOUSE\* events have `$scancode_id = 0`;<br>
EVT_TOUCH\* events have `$scancode_id = EVT[EVT_SCANCODE]`

**Exapmles:**
```c
/* usual usage */
// ui - an ui object
while(1){ // main loop

    while(get_event()){ // load new event to EVT

        // fast quit if EVT_QUIT emitted
        if EVT[EVT_TYPE] == EVT_QUIT {
            halt
        }
        check_mouse_and_touches_events(ui)
    }
}

/* Another example */
// ui1, ui2 - an ui objects
while(1){ // main loop

    while(get_event()){ // event loop

        // fast quit if EVT_QUIT emitted
        if EVT[EVT_TYPE] == EVT_QUIT {
            halt
        }

        // check events in ui1
        if check_mouse_and_touches_events(ui1) {
            continue
        }

        // if event was not grabbed by ui1 or just released,
        // check events in ui2
        if check_mouse_and_touches_events(ui2) {
            continue
        }
    }

    frame() // draw call
}
```

### resize_ui_object_touches
*TODO: add description*

### push_ui_object_state
*TODO: add description*

-----------

## Layers
```c
include "bin/core.pixi"
include "bin/layers.pixi"
```

### create_abstract_layer
Create an array of ui objects.
```c
$cont = create_abstract_layer(
    $number_of_elements
)
```
**Arguments:**
*   $number_of_elements = int - number of elements inside layer

**Result:**
$cont - [ui object](#set_standart_uis_properties)
of size `($number_of_elements)` filled with `-1` and had setted properties:
*   show = (default) 1; changeable; can have next values:
    *   <= 0 - dont draw its elements
    *   \> 0 - draw its elements in straight order skipping `-1`

**Exapmles:**
```c

UI = create_abstract_layer(N)

UI[0] = /* an ui object */
UI[1] = /* another ui object */
...
UI[N] =  /* last ui object */

fn gl_callback($u){
    // enter to gl draw mode
    set_screen(GL_SCREEN)

    // draw layer as usual ui object
    UI.draw(UI)

    // exit from gl mode
    set_screen(0)
}

// set gl callback that will be called on frame()
set_gl_callback(gl_callback, 0)

while(1){ // main loop
    while(get_event()) { // event loop

        // fast quit if EVT_QUIT emitted
        if EVT[EVT_TYPE] == EVT_QUIT {
            halt
        }
        check_mouse_and_touches_events(UI)
    }
    frame()
}
```

-----------
## Release buttons
```c
include "bin/core.pixi"
include "bin/release_buttons.pixi"
```

### create_button_activated_by_release
Description
```c
$button = create_button_activated_by_release(
    $x, $y,
    $width, $height,
    $on_click,
    $save_state_func, $set_state_func,
    $draw_func,
    $remove_func
)
```
**Arguments:**
*   $x, $y = int - center of button on screen
*   $width, $height = int - button size
*   $on_click = fn() - callback, it would be called on release event
*   $save_state_func, $set_state_func = fn() *TODO: add state description*
*   $draw_func = fn($self) - draw callback
*   $remove_func = fn($self) - deconstructor

**Result:**
$button - [ui object](#set_standart_uis_properties) of size `(1)`.<br>
In `$button[0]` it stores number of grabbed touches
which position is inside button; When button gets release event
it will call `$on_click` if `$button[0]` > 0;
So when you press on button, then move the cursor outside
and release it `$on_click` wouldn't be called.<br>
Also, `$button` have filled next properties:
*   x = $x
*   y = $y
*   width = $width
*   height = $height
*   on_click = $on_click
*   show = 1 - should be used by $draw_func

**Exapmles:** *TODO: add example*
```
```

-----------

### create_button_activated_by_release_with_text
Description
```c
$button = create_button_activated_by_release_with_text(
    $x, $y,
    $width, $height,
    $on_click,
    $text
)
```
**Arguments:**
*   $x, $y = int - center of button on screen
*   $width, $height = int - button size
*   $on_click = fn() - callback, it would be called on release event
*   $text = string - text inside button

**Result:**
$button - [button](#create_button_activated_by_release) with added properties:
*   text = $text; changeable - text that will be drawing in center of button;

**Exapmles:**
```c
counter = 0
button = create_button_activated_by_release_with_text(
    0, 40,
    256, 80, // place button in the center of screen
    {
        counter + 1
    }, // on click add to counter 1
    "Add 1" // button label
)

fn gl_callback($u){
    set_screen(GL_SCREEN)
    clear()
    // draw button
    button.draw(button)
    $ts = "" // create temporary string
    sprintf($ts, "%d", counter) // write value to string
    print($ts, 0, -40) // print string on screen
    set_screen(0)
}

set_gl_callback(gl_callback, 0)

while(1) { // main loop
    while(get_event()) { // event loop
        if EVT[EVT_TYPE] == EVT_QUIT { halt }
        check_mouse_and_touches_events(button)
    }
    frame()
}
```

-----------

### example_function
Description
```c
example_function(
    $arg0,
    $arg1,
    $arg2,
    $arg3
)
```
**Arguments:**
*   $arg0 = float | int - description
*   $arg1 = 0 | 1 - description
*   $arg2 = container - description
*   $arg3 = fn($a1,$a2) - description

**Result:**
void;

**Exapmles:**
```c
fn f(a, b) {
    logf("%d/n", a + b)
}
cont = new()
example_function(10, 0, cont, f)
```

-----------
