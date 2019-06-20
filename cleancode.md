# Writing clean and readable code

Code discipline is important for every developer, whether you work alone or in a group. I learned from own experience that it is easy to write messy and hard to read code fast. When you are lazy like me, it's easy to define a variable with a single letter like an "o" or a weird method name that doesn't mean much. It might make sense when you write it, but it might not in a few days or months. Let's see how we can fix this.

## Coding conventions & consitency 

Pick a coding convention and stick with it. [The google style guide is a good start](https://google.github.io/styleguide/jsguide.html). Avoid mixing multiple conventions to avoid confusion, especially when working in groups. In Javascript you don't have to end a line with a semicolon, but if you do this. Do it everywhere to keep your code consistent and clean.

## Plan your code/functions

When you use wireflows, or write down what the function should do. It would be easier write the code for the function. Also when you do this, you might come up with better method and variable names. 
An example flowchart:
![Flowchart](https://www.programming9.com/images/Raptor_Images/flowchart-2s_complement1-programming9.jpg)

## Break down your functions

When writing a function it's you might endup like me and make the function do multiple things. This is bad, a function should only complete a single task. Let's look at an example.

```javascript

const generateRandomString = function() {
	const characters = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefgijklmnopqrstuvwxyz";
	let randomString = "";

	for(let i = 0; i < 10; i++) {
		randomString += characters[Math.floor(Math.random()*characters.length)];
	}

	return randomString;

};


```

This code looks ok, but it's not performing a single function. It does generate a random string, but also a random number. We can break down this code into two seperate functions like this. 

```javascript

const generateRandomNumber = function(length) {
	return Math.floor(Math.random()*length);
};

const generateRandomString = function() {
	const characters = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefgijklmnopqrstuvwxyz";
	let randomString = "";

	for(let i = 0; i < 10; i++) {
		randomString += characters[generateRandomNumber(characters.length)];
	}

	return randomString;
}


```

And now we have two functions! This example is just a few lines of code. So you won't really see the difference. But as the function grows, its complexity grows. Plus as a bonus, if you break down your functions. They are re-usable!

## Comment your code

Last but not least, you should always comment code. Sometimes we see this as pointless, but it's not. I often than not forget to comment my code. And when the project gets bigger and bigger, you might lose track of what you were doing. I sometimes spend way to long to figure out what something does, or why it does what it does. To prevent this, you should obviously write comments. Also, a good tool for this is [JSDoc](https://devdocs.io/jsdoc/about-getting-started). With JSDoc you are able to generate documentation trough code comments. 

## Conclusion

It's fun to write code ofcourse, but when you write messy and uncommented code it will be harder to read and to maintain. If you keep good practices and code discipline from the start, a lot of time could be spared and confusion avoided. 