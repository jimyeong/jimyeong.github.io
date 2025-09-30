---
layout: single
title: "Composing Middlewares"
date: 2025-09-29 01:35 +0100
categories: [ React, RTK, Design patterns]
tags:
  - RTK
  - "RTK Query"
  - "React.ts"
  - "Design patterns"
  - "Composing Middlewares"
  - "Functional Programming"
---
## Here, we are to figure out how to make expandable structure with Services using RTK


### File Tree
```javascript
> src/
  > app/
    > store.tsx
  > shared/ (for logics | components available globally)
    > ui/
      > Input.tsx (basic components shared across the app)
      > Switch.tsx
    > lib/ (for reuseable utils)
      > hooks/
        > useInput.ts
        > useOutsideClickDetector.ts
        ...
    > styles/
  > entities/
    > videos/
      > apis/
        > videosApi.ts
      > models/  (types with video entity)
        > adapter.ts
        > types.ts
        > slice.ts
        > selectors.ts
      > ui/  (ui component being used with video data)
        > videoSearchFilterBar.tsx
        ...
    > users/
      > apis/
      > models/
      > ui/
    > tags/
      > apis
      > models
      > ui
    ...
  > features/ (ones that have actions changing states, or requests to servers)
    > filters/
      > ui/
          FilterBar.tsx (contains shared/Switch.tsx)
    > lib
      > useSearchQuery.ts
      > useDebounce.ts
      ...
  > widgets/ (lists, sections, ui blocks asembled by various entities ) 
    > videos/
    > tags/
    > users/
  > pages
    > videos
      Video.tsx
      > components/
      > lib/
      > __tests__
```


Principles

At first, you can keep the ui + logic mixture next in a page for maintenance purpose

If it gets bigger, and you feel like you want to break it down into reusable units, then refer to the followings

#### Examples
> shared/ => The ones you can use across the application. ex. Input.tsx, Switch.tsx, Button.tsx  

> feature/ => The ones you might have to attach some actions , requests. ex. Filterbar.tsx, KeywordSearchbar.tsx   

> entities/ => Small, reusable units that represent or edit specific domain data( videos, tags, ratings, etc)  

> widgets/ => Page level blocks that combine multiple entities, features into a single section(section, list, header bar, etc)

--- 


### Decision Tree (Quick)

>
**“Is the core of this component data representation?”** → entities
>
**“Is it assembling multiple pieces into a section?” (filter + sort + list + pagination)** → widgets
>
**“Is it encapsulating domain-neutral actions?” (search / upload / permissions, etc.)** → features
>
**“Is it fully generic UI / hooks / utilities?”** → shared



2. Services composition with RTK

1.Setting ups for composeBaseQuery.ts, wrappers.ts 

* We will leverage middleware composition to achieve benefits similar to those of class inheritance in OOP. 


```javascript
@@@ src/shared/api/composeBaseQuery.ts @@@
import BaseQueryFn from @redux
type Wrapper = (next: BaseQueryFn) => BaseQueryFn

// a function for merging functions, we are going to add up middlewares.
export const composeBaseQuery = (base: BaseQueryFn, ...wrappers: Wrapper[]) =>
  wrappers.reduceRight((acc, w) => w(acc), base);

@@@ src/shared/api/wrappers.ts @@@
// middleware, add logger
const withLogger = ((next: BaseQueryFn)=> BaseQueryFn)=> (next)=> async (args, api, extra)=>{
  const res = await next(args, api, extra)
  return res
}
// middleware, add authentication logic
const withAuth = ((next: BaseQueryFn)=> BaseQueryFn)=> (next)=> async (args, api, extra)=>{
...
}

// middleware, add retrying logic with network errors
const withRetry = ((next: BaseQueryFn)=> BaseQueryFn)=> (next)=> async (retryNumber, intervalMS)=>{
...
}

// create service instance
export const baseFetch = (baseUrl: string) =>
  fetchBaseQuery({ baseUrl, credentials: "include" }); // 필요시 쿠키
```

```javascript
2.Setting ups for videoApi.ts  (a service sample)


@@@ src/entities/videos/api/videosApi.ts @@@

import createApi from @redux
import composeBaseQuery from @redux
import {baseFetch, withLogger} from @wrappers (the reference is above)

// composing middlewares , add more middlewares for specific processes depending on services.
const baseQuery = composeBaseQuery(baseFetch("/api/vidoes"), withLogger("Todos"), withRetry(5, jitter))));

// RTK Query setup
export const videoAPI = createAPI({
  baseQuery: baseQuery // composed middlewares registration
  reducePath: "videoAPI",
  tagTypes: ["video", "videos"],
  endpoints: (builder) => ({
    getVideos: builder.query<Video[]>({
      query: ()=>({url: "/", method: "GET"}) // "/api/videos"
    })
    getVideoWithId: builder.query<video>({
      query: ()=>({url: "/:id" , method: "GET"}) // "/api/videos/:id"
    }),
    createTodo: builder.mutation<Video>({
      query: () => ({url: "/add", method: "POST"}), "/api/videos/add"
      ...
    })
  })
})

```


2.If you need a global middleware that applies to all apis
```javascript
// app/store.ts

export const store = configureStore({
  reducer: {
    [videoApi.reducerPath]: videoAPI.reducer
    ...slices
  },
  // it processes stuff before actions reach the reducer.
  middleware: (getDefault) => 
    getDefault()
    .concat(loggerMiddleware, errorReportMiddleware) // globalMiddleware
    .concat(videoApi.middleware, ...Other serivices' middleware registration)
    .concat() // any other middlewares for error handling, logging, extra listners, injecting tokens
})

```

# Conclusion
By using RTK Query, we can create seperate service branches and apply custom logic to each through middleware composition, enabling a scalable and easily maintainable architecture.

