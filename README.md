# Cocktails

A website that fetches cocktail recipes from [The Cocktail DB API](https://www.thecocktaildb.com/api.php) and displays them in a website.

## Setting up the project

1. Initialize npm package management `$ npm init -y`

2. Install [parcel](https://parceljs.org/) through npm `$ npm install --save-dev parcel-bundler`

3. Change the script in the package.json file
```json
{
  "scripts": {
    "dev": "parcel index.html",
    "build": "parcel build index.html"
  }
}
```

4. Create a index.html file `touch index.html` with DOCTYPE, head, body and the title Cocktails
```html
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Cocktails</title>
</head>
<body>
</body>
</html>
```

5. Test dev script: `npm run dev`

If you open http://localhost:1234 in your browser, you should see the empty website with the title Cocktails in the tab header.

6. Test build script: `npm run build`

This should create a dist folder (short for distribution) which will contain an index.html file.

7. Create a style.css file in the root folder and include it right before the closing `</head>` tag. Test whether it's working by setting the body background-color.

index.html
```html
<link rel="stylesheet" href="style.css"">
```

style.css
```css
body {
   background-color: blanchedalmond;
}
```

8. Add a index.js file and include it in your html file right before the closing `</body>` tag. Test whether it's working by calling `alert`

index.html
```html
<script src="index.js">
```

index.js
```js
alert ("Hello from index.js");
```

7. Add a .gitignore file and exclude the node_modules, dist and .cache folder
```
node_modules/
dist/
.cache/
```

8. Initialize git: `git init` and commit everything.

## Create a html element through javascript

1. Get rid of the `alert`
2. Create a javascript function called `createCocktailCard`
which takes three arguments: `name`, `imageUrl` and `id`

It should return this div:
```html
<div class="card" data-id={{id}}>
   <h3>{{name}}</h3>
   <img src="{{imageUrl}}">
</div>
```

index.js
```js
function createCocktailCard (name, imageUrl, id) {
   var result = document.createElement ('div');

   // Setting the class
   result.classList.add ('card');

   // Setting the data-id attribute
   result.dataset.id = id;
   
   var h3 = document.createElement ('h3');
   h3.textContent = name;
   result.appendChild (h3);

   var img = document.createElement ('img');
   img.src = imageUrl;
   result.appendChild (img);

   return result;
}
```

3. Add one cocktail to the body element by calling the function with some arguments

```js
var testCocktailCard = createCocktailCard ("Stinger", "https:\/\/www.thecocktaildb.com\/images\/media\/drink\/2ahv791504352433.jpg", 17193);

document.body.appendChild (testCocktailCard);
```

## Query an element with a selector

We don't want to add the cocktail cards directly to the body, instead we want to create a container with a list element inside

1. Edit the html to create a div with the id main-container and inside the container a div with the id cocktail-list

```html
   <div id="main-container">
      <div id="cocktail-list"></div>
   </div>
```

2. We can now select the element using javascript and add our cocktail card to the cocktail-list instead of the body

```js
var cocktailList = document.querySelector ('#cocktail-list');
cocktailList.appendChild (testCocktailCard);
```

## Creating a list of elements from mack data

1. Add a fake apiResponse at the beginning of your index.js file

index.js
```js
var apiResponse = {
   "drinks": [
      {
         "strDrink": "3-Mile Long Island Iced Tea",
         "strDrinkThumb": "https:\/\/www.thecocktaildb.com\/images\/media\/drink\/rrtssw1472668972.jpg",
         "idDrink": "15300"
       },
       {
         "strDrink": "69 Special",
         "strDrinkThumb": "https:\/\/www.thecocktaildb.com\/images\/media\/drink\/vqyxqx1472669095.jpg",
         "idDrink": "13940"
       },
       {
         "strDrink": "A1",
         "strDrinkThumb": "https:\/\/www.thecocktaildb.com\/images\/media\/drink\/2x8thr1504816928.jpg",
         "idDrink": "17222"
       },
       {
         "strDrink": "Abbey Cocktail",
         "strDrinkThumb": "https:\/\/www.thecocktaildb.com\/images\/media\/drink\/quyyuw1472811568.jpg",
         "idDrink": "17834"
       },
       {
         "strDrink": "Abbey Martini",
         "strDrinkThumb": "https:\/\/www.thecocktaildb.com\/images\/media\/drink\/2mcozt1504817403.jpg",
         "idDrink": "17223"
       },
       {
         "strDrink": "Ace",
         "strDrinkThumb": "https:\/\/www.thecocktaildb.com\/images\/media\/drink\/l3cd7f1504818306.jpg",
         "idDrink": "17225"
       },
       {
         "strDrink": "Adam & Eve",
         "strDrinkThumb": "https:\/\/www.thecocktaildb.com\/images\/media\/drink\/vfeumw1504819077.jpg",
         "idDrink": "17226"
       },
       {
         "strDrink": "Addison",
         "strDrinkThumb": "https:\/\/www.thecocktaildb.com\/images\/media\/drink\/yzva7x1504820300.jpg",
         "idDrink": "17228"
       }
   ]
};
```

2. Test whether this worked by doing a console.log of apiResponse
```js
console.log(apiResponse);
```

3. Now we want to get the drinks from the response and loop over them
```js
var drinks = apiResponse.drinks;

for (var i = 0; i < drinks.length; i++) {
   var drink = drinks[i];
   console.log (drink);
}
```

4. Now instead of appending our one testCocktailCard let's append a cocktail card in every iteration of the loop.

We first have to get our individual properties from the drink. We have to look at the mock data to figure out which property names the apiResponse uses.

```js
var drink = drinks[i];
var name = drink.strDrink;
var imageUrl = drink.strDrinkThumb;
var id = drink.idDrink;
```

Then we can call our createCocktailCard function and append the element it returns to the cocktailList.

```js
for (var i = 0; i < drinks.length; i++) {
   var drink = drinks[i];
   var name = drink.strDrink;
   var imageUrl = drink.strDrinkThumb;
   var id = drink.idDrink;

   var cocktailCard = createCocktailCard (name, imageUrl, id);
   cocktailList.appendChild (cocktailCard);
}
```
5. Clean up your index.js file. Get rid of the console.log(apiResponse) and the testCocktailCard.
