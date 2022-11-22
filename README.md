/* Made by: Distinguished */
// This is a simple draggable and resizable menu.
// PS(Dragging/resizing only works in full screen because of api) api gets mouse pos of screen not window... why?
// font(s)
g_render.create_font("medium_font", "verdana", 16, 500, 2 | 8);
// config items
g_menu.add_config_value_bool("script:menu_toggle", false);
// menu items
g_menu.add_checkbox("Menu Toggle", "script:menu_toggle"); // You can bind this to a button.
// settings
var settings = {
    menu: { // menu
        title: 'Window Name', // window title
        dragging: false, // used to loop dragging if mouse exits drag box
        resizing: false, // used to loop resizing if mouse exits drag box
        x: 100, // starting pos.x
        y: 100, // starting pos.y
        w: 400, // starting size.x
        h: 320, // starting size.y
        wMin: 400, // min size.x
        hMin: 380, // min size.y
    },
};
// inti
function inti() {
    // get screen size
    screen_x = g_render.get_screen_size()[0];
    screen_y = g_render.get_screen_size()[1];
    // get current mouse pos
    mouse_x = g_input.get_mouse_pos()[0];
    mouse_y = g_input.get_mouse_pos()[1];
} add_callback("on_render", "inti");
// render menu
function render_menu() {
    // title
    var title_width = g_render.get_text_width(settings.menu.title, "medium_font");
    // toggle
    var toggled = g_menu.get_config_value("script:menu_toggle");
    // dragging
    var dragging = g_input.get_key_down(1) & mouse_x >= settings.menu.x & mouse_x <= settings.menu.x + settings.menu.w & mouse_y >= settings.menu.y & mouse_y <= settings.menu.y + 21; // Set last number to size of drag bar size.y
    // dragging/drag loop
    if (toggled & dragging) {
        settings.menu.dragging = true;
    }
    // dragging loop
    if (settings.menu.dragging== true) {
        // Sorry for snap I don't want to do the math.
        settings.menu.x = mouse_x - settings.menu.w / 2;
        settings.menu.y = mouse_y - 10;
        if (!g_input.get_key_down(1)) {
            settings.menu.dragging = false;
        }
    }
    // resizing
    var resizing = g_input.get_key_down(1) & mouse_x <= settings.menu.x + settings.menu.w & mouse_x >= settings.menu.x + settings.menu.w - 10 & mouse_y <= settings.menu.y + settings.menu.h & mouse_y >= settings.menu.y + settings.menu.h - 10;
    // resizing/resize loop
    if (toggled & resizing) {
        settings.menu.resizing = true;
    }
    // resizing loop
    if (settings.menu.resizing) {
        settings.menu.w = mouse_x - settings.menu.x + 5;
        settings.menu.h = mouse_y - settings.menu.y + 5;
        if (!g_input.get_key_down(1)) {
            settings.menu.resizing = false;
        }
    }
    // in screen pos checks
    if (settings.menu.x < 0) { settings.menu.x = 0; }
    if (settings.menu.x + settings.menu.w > screen_x) { settings.menu.x = screen_x - settings.menu.w; }
    if (settings.menu.y < 0) { settings.menu.y = 0; }
    if (settings.menu.y + settings.menu.h > screen_y) { settings.menu.y = screen_y - settings.menu.h; }
    // in screen size checks
    if (settings.menu.w < 240) { settings.menu.w = 240; }
    if (settings.menu.w > screen_x) { settings.menu.w = screen_x; }
    if (settings.menu.h < 200) { settings.menu.h = 200; }
    if (settings.menu.h > screen_y) { settings.menu.h = screen_y; }
    // draw
    if (toggled) {
        // outline
        g_render.draw_rectangle_filled(settings.menu.x - 1, settings.menu.y - 1, settings.menu.w + 2, settings.menu.h + 2, 18, 18, 18, 255);
        // title background
        g_render.draw_rectangle_filled(settings.menu.x, settings.menu.y, settings.menu.w, 20, 29, 29, 29, 255);
        // accent line
        g_render.draw_rectangle_filled(settings.menu.x, settings.menu.y + 19, settings.menu.w, 1, 91, 134, 198, 255);
        // background
        g_render.draw_rectangle_filled(settings.menu.x, settings.menu.y + 21, settings.menu.w, settings.menu.h - 21, 25, 25, 25, 255);
        // resize
        g_render.draw_rectangle_filled(settings.menu.x + settings.menu.w - 10, settings.menu.y + settings.menu.h - 10, 10, 10, 18, 18, 18, 255);
        // title
        g_render.draw_string(settings.menu.x + settings.menu.w / 2 - title_width / 2, settings.menu.y + 1, settings.menu.title, "medium_font", 255, 255, 255, 255);
    }
} add_callback("on_render", "render_menu");
