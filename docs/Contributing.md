# Contributing to Vega Tooltip

Thank you for contributing to Vega Tooltip!

Before you start, please make sure you have `npm` and follow through our [how-to-run instructions](../README.md#run).

Please start by looking at our demonstration page, which contains many examples of tooltip working with Vega and Vega-Lite visualizations:
- [index.html](../index.html)
- [examples.js](../example.js)
    - Contains two functions that demonstrates how to use the tooltip APIs
        - `addVlExample(path, id, options)`
        - `addVgExample(path, id, options)`

For now, our library only contains:
- [vg-tooltip.css](../src/vg-tooltip.css)
- [vg-tooltip.js](../src/vg-tooltip.js) <-- The main source code
    - In the beginning,
        ```js
        window.vg.tooltip = ...
        window.vl.tooltip = ...
        ```
      we add `tooltip` under `vg` and `vl` namespace.
      Note that `tooltip` contains a `destroy()` function, which will let users free up memory after they are done with tooltip (e.g., after they destroy a visualization).
    - `supplementOptions(options, vlSpec)` <-- Vega Lite only
        - `getFieldOption()`
        - `getFieldDef()`
        - `supplementFieldOption()` <-- Just supplement a single field
    - Three functions registered as Vega View event listeners `mouseover`, `mousemove`, `mouseout` in the beginning of the file.
        - `init()`
        - `update()`
        - `clear()` 
        - Note that these functions allow users to provide custom-callbacks through `options.onAppear()`, `options.onMove()`, and `options.onDisappear()`
    - As you might guess, `init()` is the function that does most of tooltip's work: 
        - `getTooltipData()` <-- prepares data for the tooltip
            - `prepareAllFieldsData()` <-- if user didn't specify which fields to show, tooltip shows all fields in the spec
            - `prepareCustomFieldsData()` <-- otherwise, show user-specified fields in tooltip
            - When preparing data, we handle some special cases:
                - temporal fields: `removeDuplicateTimeFields()`
                - binned fields: `combineBinFields()`
                - lines and area marks: `dropFieldsForLineArea()`
            - We also have two data-formating functions, depending on whether user provides custom-format for their tooltip:
                - `customFormat()`
                - `autoFormat()`
    - After data preparation, we have some functions responsible for rendering the tooltip:
        - `bindData()` <-- bind our prepared data to tooltip placeholder (in html)
        - `updatePosition()` <-- by default, tooltip has a 10px offset from the mouse cursor
        - `updateColorTheme()` <-- dark theme, light theme, or provide your custom theme

