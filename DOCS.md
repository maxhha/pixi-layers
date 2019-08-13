### Functions

##### set_standart_uis_properties
Make from container an ui object.
```
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
*   $press, $move, $release = fn($self, $scancode_id, $evt_x, $evt_y) - event callbacks
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
```
```

-----------

##### check_mouse_and_touches_events
Check EVT for mouse and touches events and pass it to ui object.
```
check_mouse_and_touches_events( $ui_object )
```
**Arguments:**
*   $ui_object = container - container must be passed through [set_standart_uis_properties](#set_standart_uis_properties)

**Result:**
void; It calls `$ui_object.[press|move|release]($ui_object, $scancode_id, EVT[EVT_X], EVT[EVT_Y])`;<br>
EVT_MOUSE\* events have `$scancode_id = 0`;<br>
EVT_TOUCH\* events have `$scancode_id = EVT[EVT_SCANCODE]`

**Exapmles:** *TODO: add examples*
```
```

-----------

##### example_function
Description
```
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
```
fn f(a, b) {
    logf("%d/n", a + b)
}
cont = new()
example_function(10, 0, cont, f)
```

-----------
