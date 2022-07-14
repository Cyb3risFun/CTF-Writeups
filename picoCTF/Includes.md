# _Includes_
## Description
Can you get the flag?
Go to this website and see what you can discover.
## Solution
Go to the website, there is a brief description of including files, and a `say hello` button
Click on the button, it pops an alert saying
>This code is in a separate file!

Base on the description on the page and the pop up alert message, it should be obvious that the flag is hidden in the file included, so I went to view the page source and found two file included, both of them included a part of the flag
`style.css`
```css
body {
  background-color: lightblue;
}

/*  picoCTF{1nclu51v17y_1of2_  */
```
`script.js`
```js

function greetings()
{
  alert("This code is in a separate file!");
}

//  f7w_2of2_6edef411}
```