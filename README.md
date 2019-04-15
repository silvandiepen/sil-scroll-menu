# Scroll Trigger

A simple Vue Directive to trigger a class based on the element inview or not. 


### Install

Install the package
`npm install @sil/scroll-menu`


Import the package

`import ScrollMenu from '~/@sil/scroll-menu`

Define the component:

```js
	Vue.directive(ScrollMenu);
```

Use the component with default values:

```html
<any-element v-scroll-menu />	
```

Use the component with alternative values:
```html
<element v-scroll-menu="{bemClass: 'header', setClasses: true, offset: 80 }" ></element>
```

#### Styling

Some styling won't be set automatically. Don't forget to add this to your element. 

```css
.element {
	position: fixed;
	top: 0;
	width: 100%; 

	transition: transform 0.5s ease-in-out;
	will-change: transform;

	// In case of opacity change:
	transition: transform 0.5s ease-in-out, opacity 0.5s ease-in-out; 
	will-change: transform, opacity; 
}
```



### Settings

| Argument     | Default | Description                                                                     |
| ------------ | ------- | ------------------------------------------------------------------------------- |
| `debug`      | false   | Show debugging logs                                                             |
| `offset`     | 0       | On/Off top is triggered from the top (+ offset) of the page.                    |
| `bemClass`   | null    | Give a bemclass to automatically created BEM style classes                      |
| `setStyle`   | true    | Sets the transform and opacity (when opacity is true)                           |
| `setClasses` | false   | Sets the classes to the element. So you can style more on the given parameters. |


#### Classes

| Argument    | Default | Description                                  |
| ----------- | ------- | -------------------------------------------- |
| `offClass`  | `.off`  | Class given when the element is from the top |
| `onClass`   | `.on`   | Class given when the element is on the top   |
| `upClass`   | `.up`   | Class given when page is scrolling up        |
| `downClass` | `.down` | Class given when page is scrolling down      |


#### bemClass
When set, the element, classes will be overruled and automatically set with the given `bemClass` (string):

The end of the classes (up,down,etc..) are being used from the 'classes'.

- up: `[bemClass]--up`
- down: `[bemClass]--down`
- off: `[bemClass]--off`
- on: `[bemClass]--on`

default: `null`
