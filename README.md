# pixi-layers - The UI Library for Pixilang
The aim of this project is to provide an easy to use and extend simple library with usual UI components that might need you in small projects on [Pixilang](http://www.warmplace.ru/soft/pixilang/).

pixi-layers manages mouse and  **touchscreen** events so you can run it on **every** device.

## Features
*   ✔️ touchscreen support
*   ✅ buttons activating by release
    *   ✔️ with text
    *   ✅ with icon
    *   ✅ with icon and label
    *   ✅ colored
    *   ❕ radio
*   ✅ number inputs
*   ✅ popup windows
*   ✅ joysticks
*   ✅ scrolling viewer

## Documentation is [here](DOCS.md)

## Usage example:
Running this example you should see 2 buttons and number in the centor on screen. You can increase or decrease the number by one by clicking on buttons.
```c
include "bin/core.pixi"
include "bin/layers.pixi"
include "bin/release_buttons.pixi"

counter = 0

UI = create_abstract_layer(2) // create ui array with 2 elements
// create left button
UI[0] = create_button_activated_by_release_with_text(
    -80, 40, // button center position on screen
    80, 80, // button size
    {
        counter - 1
    }, // on click sub from counter 1
    "-" // button label
)

// create right button
UI[1] = create_button_activated_by_release_with_text(
    80, 40,
    80, 80,
    {
        counter + 1
    }, // on click add to counter 1
    "+" // button label
)

fn gl_callback($u){
    // enter to gl draw mode
    set_screen(GL_SCREEN)
    // clear screen with black
    clear()
    // draw buttons
    UI.draw(UI)

    // create temporary string
    $ts = ""
    // write counter value
    sprintf($ts, "%d", counter)

    t_push_matrix()
    t_translate(0, -40, 0)
    t_scale(2,2,1)
    print($ts) // draw scaled text
    t_pop_matrix()
    // exit from gl mode
    set_screen(0)
}

// set draw callback
set_gl_callback(gl_callback, 0)

while(1) { // main loop
    while(get_event()) { // event loop
        // fast quit on QUIT event
        if EVT[EVT_TYPE] == EVT_QUIT {
            halt
        }
        check_mouse_and_touches_events(UI)
    }
    // draw on screen
    frame()
}
```
