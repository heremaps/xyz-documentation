# Interact with your data

It's very easy to interact with data that get shown by adding an event listener to the `display`.
Here we're going to change the style of the feature that the user clicks on so that it highlights
in a different color.

1. Define the style to use for the highlight

    ```javascript
    var clickedFeature;
    // Define new line style
    var selectedStyle = [{
        zIndex: 0,
        type: "Line",
        opacity: 0.7,
        strokeWidth: 16,
        stroke: "#FFFFFF"
    }];
    ```

2. Then remember which feature is (or was clicked) so that it can revert to the normal style when another is selected

    ```javascript
    var clickedFeature;
    ```

3. Finally, add an event listener function to the `display` to fire when the mouse button is released revert the previous selection back to normal. If a feature was actually selected `ev.target` will contain it, and this can be used set it to the highlight style.

    ```javascript
    // add event listener to pointerup
    display.addEventListener('pointerup', function(ev){
    // Restore default feature style
      if(clickedFeature)
          linkLayer.setStyleGroup(clickedFeature);

    // If a feature is clicked
      if(ev.target){
        clickedFeature = ev.target;

        // Set new feature style if mouse clicks on a feature
        linkLayer.setStyleGroup(clickedFeature, selectedStyle);
      }
    });
    ```
