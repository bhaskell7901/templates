# Title

Welcome and promo/marketing content (100,00 ft view).  Include relevant info to get the user started fast.

See the [Deployed Site](https://bhaskell7901.github.io/deployed-site-url)


## Table of Contents

*Will probable be included a lot since the doc is section heaving and it's easy to click to the section that might be useful for the reader (assuming a project grader is the primary reader for this doc)*
1. [Technology Used](#technology-used)
1. [Overview and Strategies](#overview-and-strategies)
1. [Additional Features](#additional-features)
1. [Usage](#usage)
1. [Learning Points](#learning-points)
1. [Author Info](#author-info)


## Technology Used 

*Table of relevant technology used.  This could be frameworks, APIs, languages, etc.  This template should be updated as new tech is used, and removed based on the application of the README for a project*
| Technology Used         | Resource URL           | 
| ------------- |:-------------:| 
| HTML    | [https://developer.mozilla.org/en-US/docs/Web/HTML](https://developer.mozilla.org/en-US/docs/Web/HTML) | 
| CSS     | [https://developer.mozilla.org/en-US/docs/Web/CSS](https://developer.mozilla.org/en-US/docs/Web/CSS)      |   
| Git | [https://git-scm.com/](https://git-scm.com/)     |  


## Overview and Strategies

**
A general overview of the Password Generator flow is:
1. Create a ```characterTypeList``` object
2. Create a ```passwordGenerator``` object
3. Pass in a ```characterTypeList``` object to set the language
4. Get user input to define a ```formDataObj``` object (aka password definition)
5. Pass in a ```formDataObj``` 
6. Generate a password with the given language and definition

For the Password Generator project, I wanted to create a password generator that was flexible enough to be used by people anywhere, so I focused on making the application to be extensible from a language perspective.  A ```characterTypeList``` object is used to encapsulate the language to be used to generate a password.  This could be contextual to a country, a region, or just a language preference by the user.  A ```formDataObj``` object is use to determine, given a language definition, what characteristics of that language's characters should be used to generate the password, i.e.: how long it should be, inclusion of special characters, etc.  The ```passwordGenerator``` object uses these two objects to generate a random password that is language specific and criteria specific to the user.

I also wanted to explore form data, and capturing form data from a webpage.  I expanded on the HTML and added a form to get user data to build a ```formDataObj``` object to pass to the ```passwordGenerator``` object.  The form functionality is explained in more detail below.

One of the criteria for the project was to incorporate ```alert```, ```prompt```, and ```confirm``` window inputs.  To meet the criteria I enabled it as a feature on the form.  Once the checkbox is checked, the form inputs are ignored and the user will be prompted for there input.


## The 'formDataObj' Object

The ```formDataObj``` is the object used to capture the user input.  The basic structure looks like:

```javascript

formDataObj = {
    meta : {},
    data : {}
}

```

The ```meta``` property contains the password length a boolean flag to determine whether to require at least one instance of a character set.  The ```data``` property contains keys as strings to the ```characterTypeList``` object.  They are used to determine what character sets to use.  If if they are not present in the data propery, then they are not used.

Here is what a ```formDataObj``` might look like:

```javascript

formDataObj = {
    meta : {
        charCount : "128",
        includeOneOfEach : true,
    },
    data : {
        specChars : "specChars",
        numChars : "numChars",
        capChars : "capChars", 
        lwrChars : "lwrChars"
    }
};

```

## The 'characterTypeList' Object

The ```characterTypeList``` is used to define the pool of characters that can be used to generate a password.  The basic structure looks like:

```javasctript

characterTypeList = { 
    specChars : [],
    numChars : [],
    capChars : [], 
    lwrChars : [], 
};

```

Each property is an array of single characters.  The property names are used by the formDataObj to determine when to include the arrays in password generation.


## The 'passwordGenerator' Object

The passwordGenerator flow:

Once the ```passwordGenerator``` is created, a ```characterTypeList``` is passed in using the ```setPasswordCharacterTypeList()``` function to set the language for the password to be generated from.  If the ```characterTypeList``` has the proper formatting (an object list of single character arrays), then it is added to the property of the same name and the ```hasCharLists``` value is set to true and the function returns true.  

If not, then ```hasCharLists``` 

### Functions

```setPasswordCharacterTypeList(obj)``` - Validates the object being passed in is a ```characterTypeList``` sets the object in the generator if true

```getPasswordCharacterTypeList()``` - Returns a copy of this object's ```characterTypeList```

```getFormData()``` - Gets the form data from the HTML page and returns a ```formDataObj```

```hasFormattedTypeLists(charTypesLists)``` - Validates if a ```characterTypeList``` is properly formatted

```characterTypeMatches(charTypes)``` - Compares the index references from the ```data``` property of the ```formDataObj``` to the ```characterTypeList``` and returns matching arrays for generating passwords

```getRandomIndex(indexLength)``` - A function to randomly gnerate a number to refer to a character array

```isImpossibleCondition( charTypes, pswdLength, boolIncludeAtLeastOne )``` - Tests for an impossible condition where a user may specify using more character sets than there are characters in the password.  An exmple would be specifying using the sets: special characters, numbers and upper case letters; including at least one of each character and setting the password length to 2.  In this case, there are more characters sets than there are password chracters and we cannot ensure at least one of each character in a character set will be used.  For this assingment, that condition is not possible.  However, I wanted the ```passwordGenerator``` to be flexible in other use cases.

```getCompleteCharacterArray(matchList)``` - Concatenates all the character set arrays into a single array to ensure more random character generation.  All of the characters are contained in sets.  A strategy might be to randomly select a set, then randomly select a character within that set.  However, this would lead to unwanted character weighting making a less secure password.  I chose to combine all the usable to characters into a single array and randomly select a character from the single array.


```validNumber(formDataObj)``` - Determines whether the input password length is a number or not.

```promptForNumber()``` - Prompts the user to input a number

```promptForData``` - Confirms if the user wants to include which character sets

```getInputFromPrompts``` - Handles the prompting feature of the ```passwordGenerator``` for getting user input through window prompts

```generatePassword``` - Generates a random password 


## Password Generator: Behind the Scenes

Behind the scenes, the Password Generator creates 3 ```characterTypeList``` and ```formDataObj``` objects.  One using English characters, one using Cyrillic characters, and one using Devanagari (Hindi).  A single ```passwordGenerator``` object is created and used for the demo and for the live site example.

On page loading, a demo is run using ```console.log``` to show password generation outputs after swapping languages.  The language is then put back to English for the live site.  The outputs of the demo can be viewed in the console.


## Additional Features

* Ability to switch between using the form and prompting using the *Enable Prompting* checkbox
* Ability to force inclusiveness or not for all character types chosen using the *Inclusive* checkbox
* Using ```get``` and ```set``` functions to set language and evaluate the language externally

Three ways to get input:
1. Using form input
2. Using prompts
3. Passing a ```formDataObj``` to ```passwordGenerator``` as an optional parameter to the ```generatePassword``` function


## Usage 

To use the Password Generator, just navigate to the [Live Site](https://bhaskell7901.github.io/pswd-generator/).  Then use the options in the form to determine how you want to generate a password.

![Password Generator image](https://github.com/bhaskell7901/pswd-generator/blob/main/assets/images/pswd-generator.png)

**Enable Prompting** - select this option if you want to be prompted for input.  Form data will be ignored when generating passwords through prompting.

**Number** - the number of characters to use in your password.

**Special**  - the special set of characters.

**Numbers** - the numerical characters.

**Caps** - captial letters.

**Lower** - lower case letters.

Click *Generate Password* to get your password!


## Learning Points 

I learned a lot about JavaScript objects from this project, and about getting data from multiple sources and still formatting it in a way that can be used again and again.  When I started the project, I was very familiar with arrays and how to use them.  I actually finished the project early and decided to refactor my code to utilize JavaScript object.  The first round of code is in the project in the [scripts.js](https://github.com/bhaskell7901/pswd-generator/blob/main/assets/scripts/script.js) file.  This project is using the second file, [scripts2.js](https://github.com/bhaskell7901/pswd-generator/blob/main/assets/scripts/scripts2.js).

One of the areas where I want to continue to push myself is utilizing Github to organzie my workflow.  The issue tracker and branches are an area I'm looking to start incoroporating into my next projects.



## Author Info

### Brandon Haskell

* [LinkedIn](https://www.linkedin.com/in/BrandonDHaskell)
* [Github](https://github.com/bhaskell7901)
