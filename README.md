<img src="https://jungsoft.io/static/media/jungsoft_logo.c44eaf52.png" width="300px"/>

# Frontend Style Guide

## Introduction

The goal of this guide is to help our team to understand and
follow code style best practices, maintaining a pattern.

# :pushpin: Table of Contents

* [Components Definition](#components-definition)
* [Project Organization](#project-organization)
* [Code Standarts](#code-standards)
* [Styles Patterns](#styles-patterns)

## Components Definition 

All components should be definied within a directory. The component name should be equal to the directory 
and also would have some variants according to his type. 

```
LeadsList/
├── LeadsList.tsx
```

If it has some logic that would lead to a separation of concerns between presentational 
and container components, the structure should be as following: 
```
LeadsList/
├── LeadsListContainer.tsx
├── LeadsListContent.tsx
```

## Project Organization 

The source code of a project should be separated with at least these following essencial directories: 

```
awesome-project/
└── src/
   ├── components/
   ├── views/
   └── contexts/
   └── router/
   └── utils/
   └── translations/
   └── settings/
```

### `components/`

Most of the components will be function-based components. The choice between presentation and container implementations depends
of the scenario.

### `GraphQL Structure`

A directory called ``graphql`` should be created inside the component/view directory in order to store the query/mutation files. 

```
awesome-project/
└── src/
   └── components/
       └── Leads  
           └── graphql 
              └── listLeadsQuery.ts 
              └── updateLeadsMutation.ts
```

# Code standards

## Code style

### Use explanatory variables
Bad
```js
const onlyNumbersRegex = /^\d+$/
const validateNumber = message => value => !onlyNumbersRegex.test(value) && message

validateNumber('error message')(1)
```

Good
```js
const onlyNumbersRegex = /^\d+$/
const getNumberValidation = message => value => !onlyNumbersRegex.test(value) && message

const isNumber = getNumberValidation('error message')

isNumber(1)
```

### Use searchable and semantic names
Bad
```js
setTimeout(doSomething, 86400000)
```

Good
```js
const DAY_IN_MILLISECONDS = 86400000

setTimeout(doSomething, DAY_IN_MILLISECONDS)
```

## Components 

### One line props when are more than 2 or big props

Bad
```jsx
<button type="submit" disabled onClick={() => null} className="aLongSpecificClassName">
  Click here
</button>

<button type="submit" className="aLongSpecificClassName">
  Click here
</button>
```


Good
```jsx
<button
  className="aLongSpecificClassNameWithLasers"
  disabled={loading}
  onClick={() => null}
  type="submit"
>
  Click here
</button>
```

## Styles Patterns

### Variables 

It's important to use variables in order to increase reusability of the styles and also to be consistent. 

For instance:

```
// COLORS
$white: #fff;
$gray: #868686;
$light-gray: #E5E9ED;
$medium-gray: #D9D9D9;
$dark-gray: #5D576B;
$facebook-blue: #3B5998;

// BREAKPOINTS
$screen-xs: 400px;
$screen-sm: 575px;
$screen-md: 767px;
$screen-lg: 991px;
$screen-xl: 1199px;
```

```
$aspect-ratio: 1.78;

.media-container {
 right: calc(#{$video-width} * #{$video-position-ratio});
 width: $video-width;
 height: calc(#{$video-width} / 1.78);
 height: calc(#{$video-width} / #{$aspect-ratio});
}
```

### Don't use values that are not specified in the project design system

When applying margin, padding (spaces in general) or lengths, try as much as possible to use the classes from the design system. If there's no class that apply to the case, then extract it to a variable in order to be more verbose. 

Bad 
```
.media-container {
  margin-bottom: 25px; 
}
```

Good 
```
<div className="mb-2 media-container">{...}</div>
```

Bad
```
.media-container {
   width: 250px; 
} 
```

<br />

Good 
```
// COMPONENTS LENGTHS
$media-container-width: 250px;

.media-container {
   width: $media-container-width;
}
```

