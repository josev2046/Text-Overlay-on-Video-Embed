A rapid prototype demonstrating how to overlay user-provided text on top of a Vimeo embedded video. It leverages standard HTML, CSS for styling and positioning, and JavaScript for dynamic text updates.

[![DOI](https://zenodo.org/badge/981996757.svg)](https://doi.org/10.5281/zenodo.15387005)

## How does it work

The core functionality can be broken down as follows, aligning with the sequence diagram below:

![image](https://github.com/user-attachments/assets/04b14cbb-562b-4d9c-8fea-3402d3bd9586)


~~~```plantuml
@startuml
participant "JavaScript Logic" as JS
participant "Overlay Element" as Overlay
participant "Video Container" as VideoContainer

JS -> JS: User enters text
note right of JS: Text input field is updated.
JS -> Overlay: Updates text content
note right of Overlay: The content of the overlay element is changed.
Overlay -> VideoContainer: Displayed on top
note right of VideoContainer: Due to CSS positioning (e.g., absolute, z-index).
@enduml
~~~

## User input: 
The user interacts with a standard HTML `<input type="text">` field, entering the desired overlay text.

## JavaScript logic:
An event listener is attached to the `input` event of the text input field.

When the user types, this event is triggered, and the JavaScript code retrieves the current value from the input field.

The JavaScript then targets a `<div>` element with the ID `text-overlay`. This div is designated as the visual container for the overlay text.

The `textContent` property of the `text-overlay` element is updated with the text entered by the user.

## Overlay element:
The `text-overlay` `div` is styled using CSS to achieve its overlay effect. Key CSS properties include:

`position: absolute;`: This takes the element out of the normal document flow, allowing it to be positioned relative to its nearest positioned ancestor (`#overlay-container`).

`top: 50%;`, `left: 50%;`, `transform: translate(-50%, -50%);`: These properties work together to centre the overlay text element both horizontally and vertically within its container.

`color: white;`, `font-size`, `font-weight`, `padding`, `background-color`: These properties control the visual appearance of the text.

`z-index: 10;`: This ensures the overlay element is stacked above the `<iframe>` containing the Vimeo player.

`pointer-events: none;`: This crucial property allows user interaction with the Vimeo player controls beneath the overlay.


## Video container:
The Vimeo `<iframe>` embed is placed within a `<div>` with the ID `vimeo-embed`. This acts as the container for the video player.

A parent `<div>` with the ID `overlay-container` is used as the relative positioning context for the absolute positioning of the `text-overlay`. JavaScript also manages the dimensions of this container to maintain responsiveness with the Vimeo embed.

The CSS for `#vimeo-embed` ensures the `<iframe>` takes up the full width of its container, and the padding-bottom trick maintains the video's aspect ratio.

In essence, the user types text, JavaScript dynamically updates a styled `<div>`, and CSS ensures this `<div>` is visually positioned on top of the Vimeo embed.



## Potential Vimeo SDK integration
Extending this to have the text overlay interact with the Vimeo player (e.g., displaying specific text at certain timestamps, hiding the overlay during playback) is indeed possible by leveraging the Vimeo Player SDK (`<script src="https://player.vimeo.com/api/player.js"></script>`).

Conceptually, this would involve:

Initialising the Player: Using JavaScript to create an instance of the Vimeo player object, allowing programmatic control.

Listening for Player Events: Attaching event listeners (e.g., for `play`, `pause`, `timeupdate`, `ended`) provided by the SDK.

Conditional Logic: Implementing JavaScript logic within these event listeners to modify the `textContent` or the visibility (`display` style) of the `text-overlay` element based on the player's state or the current playback time.

Now, while the inclusion of the SDK script tag is present in the current code, the actual interaction logic would primarily reside within the JavaScript. It's more about utilising the API provided by the SDK through JavaScript rather than the SDK inherently handling the overlay display itself. The core mechanism of updating the `text-overlay` element based on conditions (whether those conditions are user input or player state) remains a JavaScript task, with CSS still responsible for the visual presentation and positioning of the overlay. Therefore, while the trigger for changes in the overlay can come from the Vimeo player's state via the SDK, the display and manipulation of the overlay element remain within the realm of JavaScript and CSS.
