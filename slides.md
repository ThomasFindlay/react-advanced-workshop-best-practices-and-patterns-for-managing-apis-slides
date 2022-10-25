---
# try also 'default' to start simple
theme: unicorn
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
# background: https://source.unsplash.com/collection/94734566/1920x1080
background: https://images.unsplash.com/photo-1590859808308-3d2d9c515b1a?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1174&q=80
# apply any windi css classes to the current slide
class: "text-center"
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: false
# use UnoCSS
css: unocss
logoHeader: "https://theroadtoenterprise.com/images/logo-400x119.avif"
website: "findlaywebtech.com"
handle: thomasfindlay94
layout: intro
introImage: "https://theroadtoenterprise.com/images/About.avif"
---

# Best Practices and Patterns for Managing API Requests and States

By Thomas Findlay

---

# About instructor - Thomas Findlay

- Full-Stack Web & Mobile Developer with 10 years of programming experience
- Co-Owner of Findlay Web Tech
- Mentor & Consultant at Codementor.io and Toptal
- The author of "Vue - The Road To Enterprise" & "React - The Road To Enterprise" books
- Technical Writer for Telerik and The Road To Enterprise blogs

---

# Workshop Content ( 1 / 3 )

- Fetching & Posting data with Axios - the simple way
- API requests cancellation with Axios
- How to provide meaningful feedback to the users
- Questions

---

# Workshop Content ( 2 / 3 )

- What is the API Layer and what problems does it solve
- Implementing the API Layer
- API requests cancellation with the API Layer
- Questions

---

# Workshop Content ( 3 / 3 )

- APIs at scale with API Layer and React-Query
- Bonus - React-Router Loaders
- Questions

---

# Project Setup

<CodeBlock>
$ git clone https://github.com/ThomasFindlay/react-advanced-workshop-best-practices-and-patterns-for-managing-apis <br />
$ git checkout api-fetch-requests/start <br />
$ npm install <br />
$ npm run dev <br />
</CodeBlock>

---
layout: center
---

# How to Perform API requests in React?

---

# Where can we put the API logic?

- useEffect hook
- callback handlers
- outside of React components
- route loaders and actions (available since React-Router v6.4)
- componentDidMount/componentDidUpdate

---

# Let's start with fetching data

<CodeBlock>
$ git clone https://github.com/ThomasFindlay/react-advanced-workshop-best-practices-and-patterns-for-managing-apis <br />
$ git checkout api-fetch-requests/start <br />
$ npm install <br />
$ npm run dev <br />
</CodeBlock>

---
layout: center
---

# React 18 Strict Mode - Double Requests 

Since React 18 there are double requests in the useEffect

---

# How to deal with double requests in Strict Mode?

1. Leave it as it is, but make sure you cancel requests
2. Move the API request outside of the useEffect - outside of a component or into a callback
3. Deduplicate requests - automatically done by libraries like React-Query, useSWR, RTK Query
4. Turn of the Strict mode

---

# What about sending some data?

<CodeBlock>
$ git checkout api-post-requests/start <br />
</CodeBlock>

---

# Request cancellation with Axios

<CodeBlock>
$ git checkout api-fetch-requests-cancellation/start <br />
</CodeBlock>

---
layout: center
---

# How to provide meaningful feedback to users during API requests?

---

# Boolean flags

```js
const [isLoading, setIsLoading] = useState(false)
const [isError. setIsError] = useState(false)
```

---
layout: center
---

# Let's implement boolean flags

---

# Boolean flags - problems

Each new API request requires at least two additional states:
```js
const [isLoadingQuotes, setIsLoadingQuotes] = useState(false) 
const [isLoadingQuotesError. setIsLoadingQuotesError] = useState(false)

const [isSubmittingQuotes, setIsSubmittingQuotes] = useState(false) 
const [isSubmittingQuotesError. setIsSubmittingQuotesError] = useState(false)
```

---

# Boolean flags - problems

It's possible to have loading and error states active at the same time

```js
try {
  // Oops, forgot to set the isLoadingQuotesError to false
  setIsLoadingQuotes(true)
  const response = await axios.get('...')
  setIsLoadingQuotes(false)
} catch (error) {
  setIsLoadingQuotesError(true)
}
```

---
layout: center
---

# What to do instead of boolean flags?

---
layout: center
---

# One API Status

IDLE <br />
PENDING <br />
SUCCESS <br />
ERROR <br />

---

# One API Status

```js{all|1-4|6-7|8-14|10|11|12|14|all}
  const IDLE = 'IDLE'
  const PENDING = 'PENDING'
  const SUCCESS = 'SUCCESS'
  const ERROR = 'ERROR'

  const [fetchQuotesStatus, setFetchQuotesStatus] = useState(IDLE)
  const [quotes, setQuotes] = useState([])

  try {
    setFetchQuotesStatus(PENDING)
    const response = await axios.get('...')
    setFetchQuotesStatus(SUCCESS)
  } catch (error) {
    setFetchQuotesStatus(ERROR)
  }
```

---
layout: center
---

# Let's migrate from boolean flags

---
layout: center
---

# Questions?

---
layout: center
---

# What is the API Layer and what problems does it solve?

---

# Using http clients directly in components results in:

- Lack of standardisation and inconsistent implementation
- Server and third-party URLs are all over the application
- Difficult to update the client-side when the server-side endpoint changes

---

# Let's implement the API layer

<CodeBlock>
$ git checkout api-layer/start
</CodeBlock>

---

# Request cancellation with the API layer

<CodeBlock>
$ git checkout api-layer-cancellation/start
</CodeBlock>

---

# Benefits of the API layer

- The API logic is encapsulated inside of the API layer and hides implementation details of the API layer
- Consumers of the API layer only care about importing methods, providing input and receiving output
- Can easily be enhanced with additional logic without affecting the rest of application code
- Much easier to migrate from one http client to another, e.g. from `axios` to `fetch` 

---
layout: center
---

# Questions?

---
layout: center
---

# APIs at scale with API Layer and React-Query

---

# What is React-Query?

- Data fetching and updating server state
- Caching
- Data synchronising
- Deduping multiple requests
- Background updates
- and more...

---

# Let's incorporate React-Query

<CodeBlock>
$ git checkout react-query/start<br /> 
</CodeBlock>

---

# Benefits of React-Query

- Feature-rich with many tools for managing API state
- Opinionated
- Easy to use

---
layout: center
---

# Bonus Content 
## API Layer + React-Query + React-Router Loaders & Actions

---

# What are route the route loaders and what's the big deal?

- Loaders and Actions are new features introduced in React Router 6.4 (Ported from Remix)
- Data fetching and posting is coupled with the router rather than components
- Allows to start loading data immediately upon entering a route instead of waiting for components to render and load. This helps with avoiding the waterfall effect

--- 
layout: center
---

# Questions?

---

# The End


<div class="flex items-start">

<div class="basis-1/2">

- Twitter <br />
  https://twitter.com/thomasfindlay94
- LinkedIn <br />
  https://www.linkedin.com/in/thomas-findlay/

- Codementor <br />
  https://www.codementor.io/@thomas478

- The Road To Enterprise <br />
  https://theroadtoenterprise.com/blog

<div class="mt-4">
  <div class="mb-2">Slides:</div>
  <a class="leading-relaxed" target="_blank" rel="noreferrer" href="https://react-advanced-workshop-best-practices-and-patterns.vercel.app/">https://react-advanced-workshop-best-practices-and-patterns.vercel.app/</a>
</div>
<div class="mt-3">
   <div class="mb-2">Workshop code:</div>
  <a class="leading-relaxed" target="_blank" rel="noreferrer" href="https://github.com/ThomasFindlay/react-advanced-workshop-best-practices-and-patterns-for-managing-apis-slides">https://github.com/ThomasFindlay/react-advanced-workshop-best-practices-and-patterns-for-managing-apis-slides</a>
</div>
</div>

<div class="basis-1/2">
  <span class="block mb-4"><span class="font-bold">React - The Road To Enterprise</span></span> 
  <span>Special gift for workshop attendees</span>

  <span>35% OFF with the code **REACTADVANCED**</span>

  <div class="flex items-center gap-4">
    <img class="w-48" src="/react-book-cover.png" />
    <img className="w-48 mt-4" src="/react-qrcode.png" />
  </div>
</div>

</div>
---