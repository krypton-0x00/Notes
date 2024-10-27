```cpp
#include <SFML/Graphics.hpp>
#include<SFML/System.hpp>
#include<SFML/Window.hpp>
#include<SFML/Audio.hpp>
#include<SFML/Network.hpp>
int main()
{                       //size,             title           with titlebar   with close btn      should be resizeable window
    sf::RenderWindow window(sf::VideoMode(640,480),"Game",sf::Style::Titlebar | sf::Style::Close | sf::Style::Resize);
    sf::Event ev;  //event

    //game loop
    while (window.isOpen()) {

        //event polling

        while (window.pollEvent(ev)) { //while we are getting events from the windows save it to the ev;

            switch (ev.type)
            {
            case sf::Event::Closed :
                window.close();
                break;
            case sf::Event::KeyPressed:
                if (ev.key.code == sf::Keyboard::Escape) {
                    window.close();
                }           
            default:
                break;
            }

        }
        //Update 

        //Render
        window.clear(); //clear old frame


        //Draw your game Here;



        window.display(); //Tell app that window is done drawing

    }

    return 0;
}
```

# WINDOW STYLES 
|   |   |
|---|---|
|`sf::Style::None`|No decoration at all (useful for splash screens, for example); this style cannot be combined with others|
|`sf::Style::Titlebar`|The window has a titlebar|
|`sf::Style::Resize`|The window can be resized and has a maximize button|
|`sf::Style::Close`|The window has a close button|
|`sf::Style::Fullscreen`|The window is shown in fullscreen mode; this style cannot be combined with others, and requires a valid video mode|
|`sf::Style::Default`|The default style, which is a shortcut for `Titlebar \| Resize \| Close`|
# Playing with the window

```cpp
// change the position of the window (relatively to the desktop)
window.setPosition(sf::Vector2i(10, 50));

// change the size of the window
window.setSize(sf::Vector2u(640, 480));

// change the title of the window
window.setTitle("SFML window");

// get the size of the window
sf::Vector2u size = window.getSize();
unsigned int width = size.x;
unsigned int height = size.y;

// check whether the window has the focus
bool focus = window.hasFocus();

...
```
## [Controlling the framerate](https://www.sfml-dev.org/tutorials/2.6/window-window.php#controlling-the-framerate)

Sometimes, when your application runs fast, you may notice visual artifacts such as tearing. The reason is that your application's refresh rate is not synchronized with the vertical frequency of the monitor, and as a result, the bottom of the previous frame is mixed with the top of the next one.  
The solution to this problem is to activate _vertical synchronization_. It is automatically handled by the graphics card, and can easily be switched on and off with the `setVerticalSyncEnabled` function:

```
window.setVerticalSyncEnabled(true); // call it once, after creating the window
```

After this call, your application will run at the same frequency as the monitor's refresh rate.

Sometimes `setVerticalSyncEnabled` will have no effect: this is most likely because vertical synchronization is forced to "off" in your graphics driver's settings. It should be set to "controlled by application" instead.

In other situations, you may also want your application to run at a given framerate, instead of the monitor's frequency. This can be done by calling `setFramerateLimit`:

```
window.setFramerateLimit(60); // call it once, after creating the window
```

Unlike `setVerticalSyncEnabled`, this feature is implemented by SFML itself, using a combination of [`sf::Clock`](https://www.sfml-dev.org/documentation/2.6.1/classsf_1_1Clock.php "sf::Clock documentation") and `sf::sleep`. An important consequence is that it is not 100% reliable, especially for high framerates: `sf::sleep`'s resolution depends on the underlying operating system and hardware, and can be as high as 10 or 15 milliseconds. Don't rely on this feature to implement precise timing.

Never use both `setVerticalSyncEnabled` and `setFramerateLimit` at the same time! They would badly mix and make things worse.