---
title: Your Fetch Responses Aren't Ok
description: About a common error when using the browser Fetch API
date: 2021-09-12
layout: layouts/post.njk
tags:
  - javascript
---

## The problem
I find it's pretty common when reading tech blogs or looking at code snippets to see people writing examples of `fetch` requests which look like this:

```javascript
// no async/await:
return fetch("https://my-url.com/someAPI")
    .then((response) => response.json());

// async/await:
const response = await fetch("https://my-url.com/someAPI");
return await response.json();
```

This code snippet returns the JSON your server returned, and everything looks snazzy. You might say to yourself, "this code could use some error handling", so let's add some in.

```javascript
return fetch("https://my-url.com/someAPI")
    .then((response) => response.json())
    .catch((e) => {
        // Or log to Sentry or whatever 
        console.error(e);
        // re-throw the error or else anything which waits for this
        // Promise will use the `.then` case instead of a `.catch`.
        throw e;
    });
```

Having a nice promise-based interface means that we've so far been able to separate our "success" and "failure" cases quite nicely, so having pushed this code on a Friday, we will ~~move on with our lives~~ have a really bad Sunday evening due to sudden unexplained errors in production.

See, this code snippet isn't actually handling all of the errors that you think it's handling. It turns out that [`fetch` only rejects if a request could not be completed](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch#checking_that_the_fetch_was_successful) -- generally because of a network error.

<mark>The fetch API <b>does not reject</b> if the server returns a response.</mark>

So for any 301, 400, 404, 403, 500, etc -- if the server responds with one of these, your `fetch` request will visit the `.then` case rather than jumping to `.catch`.

If that happens, then your client code could behave in one of a few ways, **entirely dependent on the server**:
1. If the server only sets an error code without a response body, `response.json()` will fail and your `catch` case will be triggered -- your code will work.
2. If the server sets a status code and responds with a plain string, `response.json()` will fail and your `catch` case will be triggered -- your code will work.
3. If the server sets a status code and responds with _any valid JSON_, `response.json()` will _succeed_, and your code is about to start doing things you probably were not expecting.

Case 3 is the bad one, here. If you hit case 3, it means that your code will treat the server's error response with nicely-provided JSON as a success response, and your nicely written error handling will be missed.

### How does this happen?

Looking at examples from the ExpressJS [res.send](https://expressjs.com/en/4x/api.html#res.send) documentation:
```javascript
res.status(404).send('Sorry, we cannot find that!')
res.status(500).send({ error: 'something blew up' })
```

We see that one of the examples of an error was a string, so we're fine -- that's Case 2. However, for the second example, we've returned a valid JSON object and now our client code will blow up for reasons that are likely challenging to understand or reproduce.

In my experience, a `400` response seems most likely to include error data -- it's quite common to see API responses for things like POST / PUT requests which include field-specific validation errors during data submission. That said, this could theoretically occur for any server response based on the decisions of the API implementation. Your client code should be as resilient to this as possible.

### Typescript to the rescue?

It turns out that TypeScript might actually make this problem _harder_ to debug, rather than easier. Let's imagine that your TypeScript snippet looks like this, which is a not-uncommon suggestion for adding types to async requests:

```typescript
interface SomeAPIResponse {
    // ...
}

return fetch("https://my-url.com/someAPI")
    .then((response) => response.json() as SomeAPIResponse)
    .catch((e) => {
        console.error(e);
        throw e;
    });
```

In this case, the API response being an error is even harder to diagnose, because using `as` has fooled your types into being _extremely_ sure what the type is -- the rest of your system will allow that bad data to flow all the way through.

## The solution: r u ok?

The solution to this is straightforward, even if the syntax does make our previously-clean examples of network fetching a bit more tedious. The `fetch` API exposes on every response the `ok` field, which will be true for any 2XX response and false otherwise. How you utilize this field will depend on whether you care about the server's response.

If you don't care about the server's response, you could handle non-`ok` responses by throwing an error:
```javascript
return fetch("https://my-url.com/someAPI")
    .then((response) => {
        if (!response.ok) {
            throw new Error("Request did not succeed");
        }

        return response.json();
    })
    .catch((e) => {
        console.error(e);
        throw e;
    });
```

If you _do_ care about the server's response, you can use the `ok` response to make your response a bit more able to handle problems. Using this approach, code which wants to use this data will _also_ have to check the `ok` flag, but it lets you handle that branching logic on an as-needed basis. The extra code, in this case, gives you better control.
```javascript
return fetch("https://my-url.com/someAPI")
    .then((response) => {
        // assuming for now that the server always responds with JSON,
        // though you could make this a bit more complicated to handle non-JSON
        // responses as well.
        const json = await response.json();
        return { ok: response.ok, data: json };
    })
    .catch((e) => {
        console.error(e);
        // Now you have a good way to avoid needing to `.catch`
        // failures here
        return { ok: false, data: null };
    });
```

Finally, how could we handle this for TypeScript so that our types work? For that, we'll lean on [Discriminated Unions](https://basarat.gitbook.io/typescript/type-system/discriminated-unions).
```typescript
interface SomeAPIData {
    ok: true,
    data: SomeDataType
}

interface SomeAPIError {
    ok: false,
    data: SomeAPIErrorType | null
}

type SomeAPIResponse = SomeAPIData | SomeAPIError;

// Promise<SomeAPIResponse>
return fetch("https://my-url.com/someAPI")
    .then((response) => {
        const json = await response.json();

        if (response.ok) {
            return { ok: true, data: json as SomeDataType };
        }

        return { ok: false, data: json as SomeErrorType };
    })
    .catch((e) => {
        console.error(e);
        return { ok: false, data: null };
    });
```

Finally, if you use the extremely-excellent [Redux Toolkit](https://redux-toolkit.js.org/), you can write out well-typed `createAsyncThunk` utilizing `rejectWithValue` to make this a bit easier to work with:
```typescript
interface SomeAPIData {
    // ..
}

// Define a specific interface for your 400-error!
interface Some400APIError {
    // ..
}

interface SomeOtherAPIError {
    message: string;
}

const updateSomething = createAsyncThunk<
    SomeAPIData,
    number,
    {
        state: MyStateType,
        rejectValue: Some400APIError | SomeOtherAPIError
    }
>(
    "MyObject/update_something",
    (id: number, { getState, rejectWithValue }) =>
        fetch(`https://my-url.com/someAPI/${id}`)
            .then((response) => 
                response.json().then((json) => {
                    if (response.ok) {
                        return json as SomeAPIData;
                    }

                    if (response.state === 400) {
                        return rejectWithValue(json as Some400APIError);
                    }

                    return rejectWithValue({ message: "Something is not right" });
                })
)
```