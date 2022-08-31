
<p align="center">
    <img width="180" src="https://i.ibb.co/9HLzCP5/default.png" alt="Vue Drawble logo">
</p>

Vue Drawble is a simple and lightweight Vue Component aiming to help you render your data in a table in a dead simple way.

(Yes the logo has been generated with NameCheap lol)

## Features

- Renders a table
- Customize the column template
- Choose which data you want to display

### Upcoming (by priority)

- Filter
- Search
- Collapsable panel
- Typescript
- Easily style with your preferred framework
- Improve code


## Documentation

Examples are available in the src/App.vue file. (More to come)

To get started with Vue Drawble, first install the package with your favorite package manager :

```bash
npm i vue-drawble
```

Then you can import the VueDrawble component
```js
import { VueDrawble } from 'vue-drawble';
```

To use this component, simply add it to your template like this
```js
<VueDrawble />
```

This component requires two props to work : Columns, and Rows.

Here's the list of props you can pass to VueDrawble :
- Columns : An array or an object -> It correspond to the title of your table columns and should match one of your key from your rows.
- Rows : An array of object -> A JSON
- firstLetterUppercase -> Boolean : Will automatically convert your Columns and display them with an uppercase first letter.

Example : 

```js
const rows = ref([
    {
        id: '1',
        name: 'My first command',
        description: 'I print Hello World on the Terminal',
        'command cli': `echo 'Hello World'`
    },
    {
        id: '2',
        name: 'My second command',
        description: 'I print Bonjour on the Terminal',
        'command cli': `echo 'Bonjour'`
    },
]);

<VueDrawble :rows="rows" :columns="['id', 'command cli', 'description']" />
```

#### Render :

| id | command cli        | description                         |
|----|--------------------|-------------------------------------|
| 1  | echo 'Hello World' | I print Hello World on the Terminal |
| 2  | echo 'Bonjour'     | I print Bonjour on the Terminal     |

Here you can see that the name column is missing, because I didn't provided it in the columns array.

By using the columns props with an array, Vue Drawble assume that the key in your rows is correct and you don't want to rename it, but that's not always true, this is why you can also provide an Object to the prop columns :
```js
<VueDrawble :rows="rows" :columns="{id: 'ID', name: 'Name', 'command cli': 'Command to execute'}">
```

When you provide an object, the key will be used to match with the rows, while the value will be displayed in the table header :

#### Render :

| ID | Name              | Command to execute |
|----|-------------------|--------------------|
| 1  | My first command  | echo 'Hello World' |
| 2  | My second command | echo 'Bonjour'     |

### Custom template

Sometimes rendering a table is not enough and you'd like to add link or some css to some columns.

To do that, you can use the template VueDrawble is dynamically creating with your data.
For example, let's say we would like to add an anchor on our column name, we would do it like that :

```js
<VueDrawble :rows="rows" :columns="{id: 'ID', name: 'Name', 'command cli': 'Command to execute'}">
    <template #name="{ name }"><a href="#">{{ name }}</a></template>
</VueDrawble>
```

The slot name you must provide is the key of your rows and it allows you to do an object destructuration to get all the data you need from your row.

It can be a problem if your key is a composed of two word, for example : 'command cli', this is why VueDrawble internally convert those keys to camelCase.

So let's say you want to update the template for the column 'command cli', you would write your template like that

```js
<template #commandCli="{ 'command cli': commandCli  }"><span style="color: red;">{{ commandCli }}</span></template>
```

Here we are simply extracting the 'command cli' variable and renaming it 'commandCli' and using it in our template.

You could also get the id or the name like that :

```js
<template #commandCli="{ id, name, 'command cli': commandCli  }"><span style="color: red;"> {{ id }} {{ name }}</span>{{ commandCli }}</template>
```

## Authors

- [@Kayoshi-dev](https://github.com/Kayoshi-dev)


## Contributing

Contributions are always welcome!

Before contributing please make an issues and check if someone is already working on the functionnality you want to implement! (Or the bug you want to fix :p)

