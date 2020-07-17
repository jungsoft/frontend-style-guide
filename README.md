<img src="https://jungsoft.io/static/media/jungsoft_logo.c44eaf52.png" width="300px"/>

# Frontend Style Guide

## Introduction

The goal of this guide is to help our team to understand and
follow code style best practices, maintaining a pattern.

# :pushpin: Table of Contents

* [Components Definition](#components-definition)
* [Project Organization](#project-organization)
* [Code Standards](#code-standards)
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

The source code of a project should be separated with at least these following essential directories: 

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

[Back to top ⬆️](#pushpin-table-of-contents) 

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

<br />

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

[Back to top ⬆️](#pushpin-table-of-contents)

### Just leave TODOs if it's related to a new issue

Bad
```
  // TODO: Extract this to a component. 
  {...}
```

Good 
```
  // TODO: Support recording voice messages in chat.
  {...}
```

<br />

### Leave comments for workarounds related to issues with a lib/language 

Bad
```
const handleFilePreview = useCallback((event) => {
 const file = Array.from(event.target.files)[0];
 
 // eslint-disable-next-line no-param-reassign
 event.target.value = "";
```

Good
```
const handleFilePreview = useCallback((event) => {
 const file = Array.from(event.target.files)[0];
    
 // Refer to issue https://github.com/ngokevin/react-file-reader-input/issues/11#issuecomment-363484861
 // eslint-disable-next-line no-param-reassign
 event.target.value = "";
```

<br />

### Apply JSDocs to utility functions or constants to improve readability 

Bad
```
const truncateEventName = (
  eventName,
  maxLength = SHORT_EVENT_NAME_LENGTH,
) => (
```

Good
```
/**
 * Truncates one event name to the max length.
 * @param {*} eventName The event name
 * @param {*} maxLength Max text length
 */
const truncateEventName = (
  eventName,
  maxLength = SHORT_EVENT_NAME_LENGTH,
) => (
```
 
<br />

Bad
```
const chatEventsStatuses = {
  CURRENT_USER_LEFT,
  CURRENT_USER_WAS_REMOVED,
  CURRENT_USER_REMOVED_MEMBER,
  CURRENT_USER_WAS_ADDED,
```

Good
```
/**
 * The chat event statuses according to the
 * current user and the chat event member
 */
const chatEventsStatuses = {
  CURRENT_USER_LEFT,
  CURRENT_USER_WAS_REMOVED,
  CURRENT_USER_REMOVED_MEMBER,
  CURRENT_USER_WAS_ADDED,
```

<br />

### Use curried functions to avoid re-creating inline functions on every render of the component 

Bad
```
const handleClick = (item) => {
 {...}
};

<Button onClick={() => handleClick(item)}>
```

Good
```
const handleClick = (item) => () => {
 {...}
};

<Button onClick={handleClick(item)}>
```

<br />

### Break lines for multiple hook dependencies

Bad
```
useEffect(() => {
    if (file) {
      processFile(file)
        .then(({ result }) => {
          setFileSource(result);
        })
        .catch(() => {
          notify.danger(t("errors.an_error_occurred"));
        });
    }
  }, [setFileSource, file, fileSource, isVideo, notify, t]);
```

Good
```
useEffect(() => {
    if (file) {
      processFile(file)
        .then(({ result }) => {
          setFileSource(result);
        })
        .catch(() => {
          notify.danger(t("errors.an_error_occurred"));
        });
    }
  }, [
    setFileSource,
    file,
    fileSource,
    isVideo,
    notify,
    t,
  ]);
```

<br />

### Break lines for multiple conditions and extract to semantic variables

Bad
```
const hasSelectedOffer = !!(
      selectedProduct
      || selectedLeasingOffer
    );

if ((!selectedProduct && !selectedLeasingOffer) || !individualOffer) {
   return;
}

```

Good
```
const hasSelectedOffer = !!(
   selectedProduct
   || selectedLeasingOffer
);

if (!hasSelectedOffer && !individualOffer) {
   return;
}
```

### Break lines between queries and mutations hooks

Bad
```
  const [currentUser] = useCurrentUser();
  const notify = useSnackbar();
  const [t] = useTranslation();
  const [sendRiskAssessment] = useMutation(SEND_RISK_ASSESSMENT_MUTATION);
  const {
    loading: healthCentersLoading,
    error: healthCentersError,
    data: healthCentersData,
    refetch: healthCentersRefetch,
  } = useSafeQuery(GET_LIST_HEALTH_CENTER_QUERY);
```

Good 
```
  const [currentUser] = useCurrentUser();
  const notify = useSnackbar();
  const [t] = useTranslation();
  
  const [sendRiskAssessment] = useMutation(SEND_RISK_ASSESSMENT_MUTATION);
  
  const {
    loading: healthCentersLoading,
    error: healthCentersError,
    data: healthCentersData,
    refetch: healthCentersRefetch,
  } = useSafeQuery(GET_LIST_HEALTH_CENTER_QUERY);
 ```

### Prefer to use nullish coalescing to access object properties instead of destructuring 

Bad
```
  const {
    values: {
      answers,
    },
  } = formikForm;
```

Good
```
  const answers = formikForm?.values?.answers;
```

[Back to top ⬆️](#pushpin-table-of-contents)

## Components 

### Avoid one line props when are more than 2 or big props

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

<br />

### JSX Indentation - Curly Braces 

Bad
```
{!isEnd && (
   <Button
     className={scheduleADemoButtonClasses}
     onClick={handleNavigateToEnd}
   >
     <h4>
       Schedule a Demo
     </h4>
   </Button>
)}
```

Good 
```
{
   !isEnd && (
      <Button
        className={scheduleADemoButtonClasses}
        onClick={handleNavigateToEnd}
      >
        <h4>
          Schedule a Demo
        </h4>
      </Button>
   )
}
```


## Styles Patterns

### Variables 

It's important to use variables in order to increase the reusability of styles and also to be consistent. 

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

<br />

### Don't use values that are not specified in the project design system

When applying margin, padding (spaces in general) or lengths, try as much as possible to use the classes from the design system. If there's no class that applies to the case, then extract it to a variable in order to be more verbose. 

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

[Back to top ⬆️](#pushpin-table-of-contents)

