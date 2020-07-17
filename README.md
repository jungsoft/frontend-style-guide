<img src="https://jungsoft.io/static/media/jungsoft_logo.c44eaf52.png" width="300px"/>

# Frontend Style Guide

## Introduction

The goal of this guide is to help our team to understand and
follow code style best practices, maintaining a pattern.

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

The source code of a project should be separated with the following essencial directories: 

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

Each of these directories have special types of components:

### `components/`

Most of the components will be function-based components. The choice between presentation and container implementations depends
of the scenario.


### `views/`

Views components are responsible for returning components and flowing data into the application.

### GraphQL Structure 

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
