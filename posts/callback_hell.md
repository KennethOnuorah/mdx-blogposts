---
title: 'How to Escape from "Callback Hell"'
date: '2024-02-26'
tags: ["General programming", "TypeScript", "JavaScript"]
thumbnailSource: 'https://raw.githubusercontent.com/KennethOnuorah/mdx-blogposts/main/images/thumbnails/callback_hell.png'
---

### **What the Hell is *Callback Hell*?**

**Callback hell**, also known as the **pyramid of doom**, is a common issue in asynchronous JavaScript programming. It occurs when multiple nested callbacks are used, leading to code that's hard to read, understand, and maintain. 

```typescript
getArticlesData(20, (articles) => {  
  console.log("article lists", articles);  
  getUserData(article.username, (name) => {  
    console.log(name);  
    getAddress(name, (item) => {  
      console.log(item);  
      //This continues ad infinitum
    }
  }
}) 
```

Fortunately, both JavaScript and TypeScript offer more beautiful solutions to escape this problem using both the traditional `.then()` approach and the modern `async/await` syntax. This article will explore the different ways we can write cleaner and more manageable asynchronous code.

### **Using .then()**

The .then() method is a fundamental building block of Promises. It allows us to chain asynchronous operations together without nesting callbacks excessively. Let's consider an example where we need to fetch user data from an API first before we can fetch their posts:

```typescript
function fetchUser(userId: string): Promise<User> {
    return fetch(`/api/users/${userId}`).then(res => res.json());
}

function fetchUserPosts(userId: string): Promise<Post[]> {
    return fetch(`/api/posts/${userId}`).then(res => res.json());
}

fetchUser('123')
    .then(user => fetchUserPosts(user.id))
    .then(posts => console.log(posts))
    .catch(error => console.error(error));
```

Here, we first fetch user data using `fetchUserData`. Once that method is complete, then the next method will run, `fetchUserPosts`. This approach avoids the problem of callback nesting and makes the code more easy to digest. The `.catch()` is used to catch and throw any errors during the process.

### **Using Async/Await**

The other way of handling asynchronous code while avoiding callback hell is by using `async/await`. Let's go back to the previous example:

```typescript
async function fetchUser(userId: string): Promise<User> {
    const response = await fetch(`/api/users/${userId}`);
    return await response.json();
}

async function fetchUserPosts(userId: string): Promise<Post[]> {
    const response = await fetch(`/api/posts/${userId}`);
    return await response.json();
}

async function fetchUserPostsAndData(userId: string) {
    try {
        const user = await fetchUser(userId);
        const posts = await fetchUserPosts(user.id);
        console.log(posts);
    } catch (error) {
        console.error(error);
    }
}

fetchUserPostsAndData('123');
```

In this reworked example, the `async` keyword indicates that the function returns a Promise, and `await` is used to pause the execution until the Promise has been resolved.

### Conclusion

Callback hell can make code difficult to understand and maintain, but TypeScript and JavaScript provide solutions to escape it. Whether you prefer the traditional .then() approach or the modern async/await syntax, both methods offer clean and more readable code for handling asynchronous operations.