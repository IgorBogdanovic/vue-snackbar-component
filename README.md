# vue-snackbar-component

### Demo
###### CodeSandbox: https://codesandbox.io/s/github/giga0/vue-snackbar-component
###### Full screen: https://2qxtg.codesandbox.io/

### Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

### Introduction
Snackbar informs user about temporary processes running or low priority actions.

It appear on bottom left corner of the window at 1rem from left and 1rem from bottom, on top of every other layers. Then it'll automatically disappear after a default time (2000ms) or duration time defined in payload of message object that needs to be shown.

Only one snackbar can be shown at a time.
It means the component queue the incoming messages to be able to show them in sequence when several are issued. Each time it receives a show-snackbar bus event, it appends the event payload in an internal array. After the dedicated time, the snackbar disappears and the corresponding message is flushed from the queue.

There are 3 variations of displaying snackbar:
1. simple
2. with color emphasis
3. with actions

The message always fit in one line and do not word-wrap.

Payload object sent when emitting show-snackbar bus event should look like this:
```javascript
{
    message: 'the text message to show.', // required, string
    emphasis: 'color', // optional, color for the little emphasis
    duration: number, // optional, in milliseconds, will override the default disparition time if given
    actions: [ // optional, or empty array for no action
        {
            label: 'action', // the displayed action name, string
            color: 'color', // optional, color of the text button, black is default
            callback: function // the function to execute when action is clicked
        },
        ...
    ]
}
```
