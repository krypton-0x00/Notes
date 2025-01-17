Let me explain how the button disabling works step by step:

1. First, in the `ChatMessage.tsx`, we added a `disabled` property to the Button interface:

Code

CopyInsert

`interface Button {   title: string;   payload: string;   disabled?: boolean;  // New property to track disabled state }`

2. In the `handleButtonClick` function in `ChatInterface.tsx`, here's what happens when a button is clicked:

Code

CopyInsert

`setMessages(prev => prev.map(msg => {   // Check if this message has buttons   if (msg.buttons) {     return {       ...msg,  // Keep all other message properties       buttons: msg.buttons.map(btn => ({         ...btn,  // Keep all button properties         disabled: true,  // Set ALL buttons to disabled       })),     };   }   return msg;  // If no buttons, return message as is }))`

Let's break down what this code does:

a. `setMessages(prev => ...)` - Takes the previous state of all messages b. `.map(msg => ...)` - Goes through each message c. `if (msg.buttons)` - Checks if this message has any buttons d. If it has buttons:

- Creates a new message object with `...msg`
- Updates its buttons array using `buttons.map`
- For each button, creates a new button object with `...btn`
- Sets `disabled: true` for every button in that message e. If no buttons, returns the message unchanged

So when you click any button:

1. It finds the message containing the clicked button
2. It updates ALL buttons in that message to have `disabled: true`
3. The disabled buttons get styled with reduced opacity and can't be clicked thanks to the CSS in `ChatMessage.tsx`:

Code

CopyInsert

`className={cn(   // ... other styles ...   button.disabled && "opacity-50 cursor-not-allowed hover:bg-highlight/20" )}`

This ensures that once you make a selection from a group of options, all options in that group become unclickable.